# Adição do tipo de gasto "Troca de Pneu"

**Data:** 2026-04-27
**Branch:** main
**Arquivo principal alterado:** `CLAUDE.md` (single-page app)

---

## Tarefa solicitada

Adicionar o tipo de gasto **"Troca de Pneu"** (`troca_pneu`) à lista de tipos disponíveis no sistema, integrando-o ao formulário, filtros, listagens, mapas de apresentação e ao agregado de manutenções no Dashboard.

## Observação sobre a arquitetura do projeto

Mesmo padrão das entregas anteriores (`2026-04-27_remocao-tipo-abastecimento-oleo.md`, `2026-04-27_campo-km-em-gastos.md`):

- O app é **single-page** com todo o código (HTML + CSS + JS) em `CLAUDE.md`.
- Backend é **Firestore** (NoSQL, schemaless) — não há migrations SQL, ORM, models, DTOs ou validators server-side.
- Não há suíte de testes nem framework configurado.
- O "enum" efetivo de tipos vive nas `<option>` dos `<select>` do frontend e nas chaves dos mapas `TYPE_NAMES` / `getTypeBadge`.

Como esta é uma **adição** (não renomeação/remoção), **não há migração de dados**: nenhum registro legado precisa ser convertido.

---

## Mudanças aplicadas (em `CLAUDE.md`)

### 1. Formulário "Registrar Gasto" — HTML
- `<select id="expenseType">`: nova opção `<option value="troca_pneu">Troca de Pneu</option>` inserida entre `troca_oleo_filtro` e `outro` (mantém os tipos de manutenção agrupados antes do "Outro personalizado").

### 2. Filtro da seção Gastos — HTML
- `<select id="filterType">`: mesma opção adicionada na mesma posição relativa, garantindo paridade entre o formulário de cadastro e o filtro de busca.

### 3. JS — mapas de apresentação
- `TYPE_NAMES`: chave `troca_pneu: 'Troca de Pneu'` adicionada para que o nome correto seja exibido em tabelas, dashboard, relatórios e modal de detalhes.
- `getTypeBadge`: chave `troca_pneu: 'badge-maintenance'` adicionada — reutiliza o badge marrom de manutenção, sem necessidade de novo CSS.

### 4. Dashboard — agregado "Total Manutenções"
- `renderDashboard`: o array de tipos somados como manutenção foi atualizado de
  `['manutencao', 'revisao', 'troca_oleo_filtro', 'oleo', 'balde_oleo', 'outro']`
  para
  `['manutencao', 'revisao', 'troca_oleo_filtro', 'oleo', 'balde_oleo', 'troca_pneu', 'outro']`.
- Com isso, gastos de troca de pneu passam a compor o card "Total Manutenções" do painel.

---

## O que ficou de fora (e por quê)

| Item                                       | Status         | Motivo                                                                                       |
|--------------------------------------------|----------------|----------------------------------------------------------------------------------------------|
| Migração de dados                          | Não aplicável  | Tipo novo — não há registros legados a converter.                                            |
| Migration SQL / alteração de enum no DB    | Não aplicável  | Firestore é schemaless.                                                                      |
| Backend models / schemas / DTOs            | Não aplicável  | Não há backend próprio; Firestore é acessado direto pelo SDK no browser.                     |
| Hardening em `firestore.rules`             | Não feito      | Pode ser adicionado em PR separado para reforçar a lista de tipos válidos a nível de regras. |
| Testes ajustados                           | Não aplicável  | Projeto não possui runner de testes.                                                         |
| Estilo de badge dedicado (`badge-tire`)    | Não criado     | Optou-se por reutilizar `badge-maintenance` para manter o visual consistente com os demais   |
|                                            |                | tipos de manutenção (revisão, manutenção genérica). Trocar para um badge próprio é trivial.  |

---

## Comportamento esperado

- Ao registrar um novo gasto, o usuário pode selecionar **"Troca de Pneu"** no formulário.
- Na listagem de Gastos, o registro aparece com o badge marrom (`badge-maintenance`) e o nome "Troca de Pneu".
- O filtro de tipo de gasto permite isolar apenas esses registros.
- No Dashboard, o valor é somado em **Total Manutenções** (e em **Gasto Total**, naturalmente).
- No Relatório por Veículo, o tipo aparece como uma linha própria agrupada por nome.

## Pontos de teste manual sugeridos

1. **Form Registrar Gasto**: abrir o `<select>` de Tipo de Gasto — confirmar que "Troca de Pneu" aparece logo após "Troca de Óleo e Filtro".
2. **Salvar gasto**: registrar um gasto desse tipo — verificar que aparece na listagem com badge "Troca de Pneu" e cor de manutenção.
3. **Filtro de Gastos**: filtrar por "Troca de Pneu" — somente os registros desse tipo devem ser exibidos.
4. **Dashboard**: card "Total Manutenções" deve incluir o valor do gasto registrado.
5. **Relatório por Veículo**: agrupamento por tipo deve mostrar uma linha "Troca de Pneu" com a contagem e o total.
6. **Modal de detalhes**: abrir o gasto — campo "Tipo" deve mostrar o badge "Troca de Pneu".
7. **Edição**: editar um gasto desse tipo — `<select>` deve vir pré-selecionado em "Troca de Pneu".

## Arquivos tocados

- `CLAUDE.md` — todas as mudanças de código.
- `dcs/2026-04-27_adicao-tipo-troca-pneu.md` — este documento.

Nenhum outro arquivo foi alterado nesta tarefa.
