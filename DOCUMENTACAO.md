# Documentação — Sistema de Controle de Frota

## 1. O que é o projeto

O **Controle de Frota** é uma aplicação web para **gerenciar veículos de uma frota e os gastos associados a cada um deles**. Com o sistema é possível:

- Cadastrar e editar veículos (nome/modelo, placa, ano, KM inicial, observações).
- Registrar gastos por veículo (abastecimento, óleo, manutenção, revisão, troca de pneu, etc.), com data, valor, quilometragem e comprovante anexado.
- Acompanhar a quilometragem mensal de cada veículo.
- Gerar relatórios de gastos (por veículo e por tipo), ranking de consumo (km/L) e relatório de quilometragem rodada.
- Receber alertas automáticos quando o consumo de um veículo parece incorreto (acima de 15 km/L).

O objetivo é dar à empresa uma visão centralizada de **quanto cada veículo custa**, **quanto roda** e **qual sua eficiência de combustível**, substituindo controles manuais em planilhas.

O sistema está publicado e acessível em:
- https://controle-frota-mave.web.app
- https://controle-frota-mave.firebaseapp.com

---

## 2. Tecnologias e linguagens utilizadas

O projeto é uma **Single-Page Application (SPA)** simples, sem framework e sem etapa de build. Todo o sistema vive em **um único arquivo HTML** que contém a estrutura, o estilo e a lógica.

| Camada | Tecnologia | Observação |
|--------|-----------|------------|
| Estrutura | **HTML5** | Marcação semântica, idioma `pt-BR`. |
| Estilo | **CSS3 puro** | Sem framework (sem Bootstrap/Tailwind). Usa variáveis CSS (`:root`), grid e flexbox. Paleta slate + laranja como cor de destaque. |
| Lógica | **JavaScript (Vanilla / ES6+)** | Sem React, Vue ou jQuery. Manipulação direta do DOM. |
| Banco de dados | **Google Firebase Firestore** | Banco NoSQL na nuvem. Acessado direto do navegador via SDK. |
| Armazenamento de arquivos | **Firebase Storage** | Guarda os comprovantes (fotos/PDFs) dos gastos. |
| Hospedagem | **Firebase Hosting** | Site estático servido pela pasta `public/`. |
| Fallback offline | **LocalStorage** | Se o Firebase não estiver configurado, os dados ficam salvos no navegador. |
| Fontes / ícones | **Google Fonts (Inter)** e **Material Icons Outlined** | Carregadas via CDN. |

**Resumo:** HTML + CSS + JavaScript puro no frontend, e Firebase (Firestore + Storage + Hosting) como backend/infraestrutura. Não há servidor próprio, API intermediária, ORM ou linguagem de backend (Node, PHP, etc.) — o navegador fala diretamente com o Firebase.

---

## 3. Arquitetura

```
Navegador do usuário
        │
        │  (SDK Firebase carregado via CDN)
        ▼
┌─────────────────────────────┐
│  Firebase                   │
│  ├─ Firestore  (dados)      │
│  ├─ Storage    (arquivos)   │
│  └─ Hosting    (o site)     │
└─────────────────────────────┘
```

- **Não existe backend próprio.** O JavaScript que roda no navegador se conecta diretamente ao Firestore usando o SDK do Firebase.
- O Firestore é **schemaless** (NoSQL): não há migrations SQL, models, DTOs ou validators de servidor. As validações acontecem no frontend.
- Por enquanto **não há autenticação/login** — as regras de segurança do Firestore estão abertas (qualquer pessoa com a URL pode ler e escrever). Isso é adequado para uso interno/restrito, mas precisa ser revisto antes de exposição pública.

---

## 4. Estrutura de dados (coleções do Firestore)

O banco tem **três coleções**:

### `vehicles` — Veículos
| Campo | Descrição |
|-------|-----------|
| `name` | Nome / modelo (ex: "Fiat Strada") |
| `plate` | Placa (ex: "ABC-1D23") |
| `year` | Ano |
| `initialKm` | Quilometragem inicial |
| `obs` | Observações |
| `createdAt` / `updatedAt` | Datas de criação/atualização |

### `expenses` — Gastos
| Campo | Descrição |
|-------|-----------|
| `vehicleId` | Referência ao veículo |
| `type` | Tipo do gasto (ver lista abaixo) |
| `date` | Data |
| `value` | Valor (R$) |
| `km` | Quilometragem no momento do gasto |
| `obs` | Observação |
| `files` | Lista de URLs dos comprovantes anexados |
| `fuelLiters`, `fuelPricePerLiter`, `fuelType`, `fuelKmPerLiter` | Campos extras só para abastecimento |

### `km_readings` — Leituras mensais de quilometragem
| Campo | Descrição |
|-------|-----------|
| `vehicleId` | Referência ao veículo |
| `yearMonth` | Mês/ano da leitura (ex: "2026-04") |
| `km` | Quilometragem registrada naquele mês |

---

## 5. Tipos de gasto suportados

| Valor interno | Nome exibido |
|---------------|--------------|
| `abastecimento` | Abastecimento |
| `oleo` | Óleo |
| `manutencao` | Manutenção |
| `revisao` | Revisão |
| `balde_oleo` | Balde de Óleo |
| `troca_oleo_filtro` | Troca de Óleo e Filtro |
| `troca_pneu` | Troca de Pneu |
| `outro` | Outro (personalizado) |

> Observação: o gasto do tipo **Abastecimento** exige o preenchimento do consumo (**KM/Litro**), pois é a base do cálculo de eficiência da frota.

---

## 6. Telas e funcionalidades

O sistema é dividido em **três abas** no topo:

### Aba "Veículos"
- Formulário de cadastro/edição de veículos.
- Listagem com busca por nome ou placa.
- Mostra a **KM atual** e os **KM rodados** de cada veículo (calculados a partir das leituras e dos gastos).

### Aba "Gastos"
- Formulário de registro de gasto, com campos que mudam conforme o tipo (abastecimento mostra litros, preço por litro, combustível e km/L).
- **Cálculo automático de litros** (valor ÷ preço por litro).
- **Validação de quilometragem**: impede registrar uma KM menor que a última já registrada para o veículo.
- Upload de comprovante (imagem ou PDF, até 5 MB).
- Listagem com busca e filtros (por veículo, por tipo, por quantidade de registros).

### Aba "Relatórios"
- **Relatório de Gastos** (com abas "Por Veículo" e "Por Tipo") e filtro por período.
- **Ranking de Consumo (km/L)** — pódio dos veículos mais econômicos, com medalhas.
- **Relatório de Quilometragem** — KM rodado e média de consumo por mês.
- **Registro de Quilometragem Mensal** — cadastro das leituras de KM de cada veículo.
- **Alerta de consumo** — destaca veículos com consumo acima de 15 km/L (provável erro de digitação nos dados).

---

## 7. Estrutura de arquivos do projeto

```
sistema-controle-frotas/
├── public/
│   └── index.html          → O APP (HTML + CSS + JS num único arquivo). É o que vai ao ar.
├── CLAUDE.md               → Cópia do app usada como referência de desenvolvimento.
├── firebase.json           → Configuração do Firebase Hosting.
├── .firebaserc             → Aponta para o projeto Firebase "controle-frota-mave".
├── firestore.rules         → Regras de segurança do banco (atualmente abertas).
├── cors.json               → Configuração de CORS do Storage (upload de comprovantes).
├── .gitignore
└── dcs/                    → Documentos de decisão (histórico de mudanças do projeto).
```

> Nota técnica: hoje o código do app existe duplicado em `public/index.html` (versão publicada) e em `CLAUDE.md` (referência). Em uma futura organização, o ideal é manter **apenas** `public/index.html` como fonte da verdade.

---

## 8. Como publicar (deploy)

A partir da raiz do projeto, com o Firebase CLI instalado:

```
firebase deploy --only hosting
```

A flag `--only hosting` é importante: ela publica **apenas o site**, sem sobrescrever as regras do Firestore ou do Storage que já estão configuradas no console do Firebase.

---

## 9. Convenções e particularidades do projeto

- **Idioma:** todo o projeto — interface, comentários no código, nomes de funções de domínio e documentação — está em **português (pt-BR)**.
- **Sem framework e sem build:** basta abrir o `index.html` no navegador para rodar; não há `npm install` nem etapa de compilação.
- **Banco NoSQL:** como o Firestore não tem schema fixo, registros antigos podem não ter todos os campos. O código trata isso com valores padrão e, quando necessário, com **funções de migração automática** (ex.: a conversão do antigo tipo "Abastecimento + Óleo" para "Abastecimento").
- **Documentos de decisão (`dcs/`):** cada mudança relevante é registrada num arquivo Markdown explicando o que foi feito, por quê, o que ficou de fora e como testar.
- **Sem autenticação ainda:** o acesso é aberto; recomenda-se adicionar login antes de tornar o sistema público.

---

*Documento gerado para fins de referência e onboarding do projeto Controle de Frota.*
