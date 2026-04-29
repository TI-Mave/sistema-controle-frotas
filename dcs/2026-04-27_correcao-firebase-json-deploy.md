# Correção do `firebase.json` e restauração do deploy de Hosting

**Data:** 2026-04-27
**Branch:** main
**Arquivos principais alterados:** `firebase.json`, `.gitignore` (novo), `public/index.html` (novo), `cors.json`, `firestore.rules`

---

## Tarefa solicitada

Ao rodar `firebase deploy`, o Firebase CLI quebrava com:

```
Error: There was an error loading firebase.json:
Parse Error in firebase.json:
Unexpected token '#' at 1:1
# Logs
^
```

O `firebase.json` estava com conteúdo inválido — começava com `# Logs` e o restante de um `.gitignore` típico de projeto Node/Firebase. JSON não aceita `#` como comentário, por isso o parser quebrava na primeira linha.

Objetivo:
1. Reescrever o `firebase.json` com configuração de Hosting válida.
2. Restaurar o `.gitignore` (que era o conteúdo que estava no lugar errado).
3. Fazer o site **funcionar de verdade** após o deploy (não só passar do parser).

---

## Diagnóstico — caos generalizado de arquivos

Investigando, vários arquivos do repositório estavam com conteúdos **trocados entre si** (provavelmente upload manual em lote com troca acidental):

| Arquivo                     | Conteúdo encontrado (errado)                                       | Conteúdo que deveria ter                       |
|-----------------------------|--------------------------------------------------------------------|------------------------------------------------|
| `firebase.json`             | Texto de `.gitignore` (Node/Firebase)                              | JSON de configuração do Firebase CLI           |
| `.gitignore`                | (não existia)                                                      | Padrões de ignore                              |
| `cors.json`                 | `{"projects": {"default": "controle-frota-mave"}}` (de `.firebaserc`) | Array de configuração CORS para Storage     |
| `firestore.rules`           | Array JSON de CORS                                                 | Regras do Firestore (DSL `rules_version = '2'`) |
| `index.html` (raiz)         | 2 linhas de cache de hosting (`404.html,1771...`, `index.html,1774...`) | Era o destino do app, mas hoje o app vive em `CLAUDE.md` |
| `hosting.cHVibGlj.cache`    | `{ "permissions": { "allow": [ "WebSearch", ... ] } }` (settings do Claude Code) | Cache binário do Firebase (regenerado pelo CLI) |
| `CLAUDE.md`                 | HTML completo do app de Controle de Frota (~96 KB, 2357 linhas)    | Instruções para o Claude (curtas)              |

A consequência prática: o **app de verdade** (Painel, Veículos, Gastos, Relatórios) estava dentro do `CLAUDE.md`, e a raiz não tinha um `index.html` deployável.

---

## Mudanças aplicadas

### 1. `firebase.json` — reescrito do zero

```json
{
  "hosting": {
    "public": "public",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```

Decisões:

- **`"public": "public"`** — o cache antigo `hosting.cHVibGlj.cache` tem o sufixo `cHVibGlj` (base64 de `public`), confirmando que essa era a pasta historicamente configurada.
- **Apenas bloco `hosting`** — o app usa Firestore e Storage, mas as regras desses serviços já estão configuradas no console do Firebase. Incluir `firestore` ou `storage` no `firebase.json` faria o `firebase deploy` sobrescrever a config atual com os arquivos locais (que estavam corrompidos), correndo o risco de **derrubar o app em produção**. Deploy de regras fica explícito sob demanda.
- **Rewrite `**` → `/index.html`** — incluído por padrão. O app não é uma SPA com roteador (a navegação interna é por troca de `display` no DOM), mas o rewrite é inofensivo e permite acesso direto a qualquer URL.
- **`ignore`** — padrão do Firebase CLI (oculta dotfiles, evita subir `node_modules` e o próprio `firebase.json`).

### 2. `.gitignore` — criado

Conteúdo: o mesmo `.gitignore` Node/Firebase que estava equivocadamente dentro do `firebase.json`, expandido com:

- `.vscode/`, `.idea/`, `.DS_Store`, `Thumbs.db` (IDE / SO).
- `dist/`, `build/` (saídas de build, caso o projeto venha a adotar bundler no futuro).

### 3. `public/index.html` — criado a partir do `CLAUDE.md`

```bash
mkdir public && cp CLAUDE.md public/index.html
```

Verificações pós-cópia:

- Linha 1: `<!DOCTYPE html>`.
- Linha 2: `<html lang="pt-BR">`.
- Linha final: `</html>`.
- Total: 2357 linhas, 96.816 bytes — idêntico ao conteúdo do `CLAUDE.md`.

A cópia é **byte-exact**; não há perda de dados, só duplicação intencional (o `CLAUDE.md` permanece intacto, ver "O que ficou de fora").

### 4. `cors.json` — restaurado

Conteúdo correto (vinha do `firestore.rules`):

```json
[
  {
    "origin": ["https://controle-frota-mave.web.app", "http://localhost:5000"],
    "method": ["GET", "POST", "PUT", "DELETE", "HEAD"],
    "maxAgeSeconds": 3600,
    "responseHeader": ["Content-Type", "Authorization", "Content-Length", "X-Requested-With"]
  }
]
```

Esse arquivo é consumido por `gsutil cors set cors.json gs://<bucket>` (configuração de CORS do bucket de Storage). Não é tocado pelo `firebase deploy`.

### 5. `firestore.rules` — restaurado com regras válidas

```
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    match /vehicles/{docId}      { allow read, write: if true; }
    match /expenses/{docId}      { allow read, write: if true; }
    match /km_readings/{docId}   { allow read, write: if true; }
  }
}
```

Decisões:

- Regras **abertas** (`if true`), porque o app **não tem autenticação** — o frontend abre conexão direta com o Firestore sem `signInWithEmailAndPassword` ou similar. Restringir por `request.auth != null` quebraria o app imediatamente.
- Match explícito por coleção (`vehicles`, `expenses`, `km_readings`) para evitar permissão acidental em coleções futuras.
- Comentário no topo do arquivo alertando que a regra é insegura para uso público.
- **Não há `firestore` block no `firebase.json`**, então o arquivo não é deployado automaticamente — fica como referência local até alguém decidir aplicar.

---

## O que ficou de fora (e por quê)

| Item                                                           | Status        | Motivo                                                                                                                |
|----------------------------------------------------------------|---------------|-----------------------------------------------------------------------------------------------------------------------|
| Limpeza do `index.html` da raiz (2 linhas de cache antigo)     | Não feito     | Fora do escopo direto. Com `"public": "public"`, esse arquivo **não é deployado** — não atrapalha. Pode ser deletado a qualquer momento. |
| Limpeza do `hosting.cHVibGlj.cache`                            | Não feito     | Mesmo motivo. É cache do CLI; o Firebase regenera no próximo deploy bem-sucedido.                                     |
| Limpeza do `download (1)` (página 404 padrão do Firebase)      | Não feito     | Arquivo órfão sem função no projeto. Pode ser deletado.                                                               |
| Restaurar `CLAUDE.md` para um conteúdo curto de instruções     | Não feito     | O `CLAUDE.md` segue tendo o app inteiro — agora **duplicado** com `public/index.html`. Mexer nisso poderia confundir uma futura entrega que use `CLAUDE.md` como referência. Decisão deixada para o autor. |
| Hardening de `firestore.rules` por auth ou allowlist           | Não feito     | App não tem auth. Mudança requer redesenho do fluxo de login, fora desta tarefa.                                      |
| Adicionar bloco `firestore` ao `firebase.json`                 | Não feito     | Risco de sobrescrever regras já em uso no console. Decisão consciente do autor antes de habilitar.                    |
| Adicionar bloco `storage` ao `firebase.json`                   | Não feito     | Mesmo motivo. Não há `storage.rules` no projeto.                                                                      |
| Deploy automatizado pelo Claude                                | Não feito     | Restrição explícita do solicitante: ele mesmo executa `firebase deploy`.                                              |

---

## Estado final dos arquivos

```
sistema-controle-frotas/
├── .firebaserc                  (intacto — aponta para projeto controle-frota-mave)
├── .gitignore                   ✨ NOVO
├── CLAUDE.md                    (intacto — ainda contém o app HTML)
├── cors.json                    ✏️  conteúdo corrigido
├── firebase.json                ✏️  reescrito (JSON válido)
├── firestore.rules              ✏️  conteúdo corrigido (regras Firestore válidas)
├── public/
│   └── index.html               ✨ NOVO (cópia do CLAUDE.md)
├── dcs/
│   └── 2026-04-27_correcao-firebase-json-deploy.md  ✨ NOVO (este documento)
├── index.html                   (intacto — 2 linhas de cache; órfão, não deployado)
├── hosting.cHVibGlj.cache       (intacto — cache antigo; órfão)
└── download (1)                 (intacto — página 404 padrão; órfão)
```

---

## Como deployar

Da raiz do projeto:

```
firebase deploy --only hosting
```

A flag `--only hosting` é **importante**: ela impede que o CLI tente também subir Firestore Rules ou Storage Rules (que têm arquivos locais agora válidos, mas que não passaram por revisão para uso em produção).

Após o deploy bem-sucedido, o site fica disponível em:

- `https://controle-frota-mave.web.app`
- `https://controle-frota-mave.firebaseapp.com`

---

## Pontos de teste manual sugeridos

1. **Parser do `firebase.json`**: rodar `firebase deploy --only hosting` — não deve mais aparecer `Parse Error`.
2. **Upload concluído**: o CLI deve listar `public/index.html` e finalizar com `Deploy complete!`.
3. **Site carrega**: abrir `https://controle-frota-mave.web.app` — header laranja "Controle de Frota" com 4 abas (Painel, Veículos, Gastos, Relatórios).
4. **Firestore conecta**: console do navegador (F12) — não deve haver erros de permissão (`PERMISSION_DENIED`); listagens de veículos/gastos populam normalmente.
5. **Cadastro funciona**: criar um veículo de teste → aparece na aba Veículos e some na aba Painel (contador "Veículos Cadastrados").
6. **Storage opcional**: anexar comprovante a um gasto. Se o CORS do bucket estiver configurado (via `cors.json`), o upload conclui; caso contrário, o app exibe toast "Storage não configurado. Gasto será salvo sem o anexo." e segue funcionando.
7. **Rewrite SPA**: acessar `https://controle-frota-mave.web.app/qualquer-rota-inexistente` — deve servir o `index.html` (não 404).
8. **Reload preserva estado**: cadastrar veículo → recarregar → veículo continua lá (confirma que o Firestore está persistindo).

---

## Riscos conhecidos

- **Regras do Firestore atualmente em produção são desconhecidas para esta entrega.** O arquivo `firestore.rules` local agora é válido, mas **não foi deployado**. Se alguém rodar `firebase deploy` (sem o `--only hosting`) ou `firebase deploy --only firestore:rules`, as regras locais (abertas) sobrescreverão o que está no console. Em qualquer dúvida, exportar primeiro: `firebase firestore:rules:get`.
- **`CLAUDE.md` e `public/index.html` ficam dessincronizados** assim que houver a próxima edição do app. Tarefas futuras precisam decidir qual é a fonte da verdade. Sugestão: tornar `public/index.html` a fonte e esvaziar o `CLAUDE.md` para apenas instruções do projeto.

---

## Arquivos tocados

- `firebase.json` — reescrito.
- `.gitignore` — criado.
- `public/index.html` — criado (cópia byte-exact de `CLAUDE.md`).
- `cors.json` — conteúdo substituído.
- `firestore.rules` — conteúdo substituído.
- `dcs/2026-04-27_correcao-firebase-json-deploy.md` — este documento.

Nenhum outro arquivo foi alterado nesta tarefa.
