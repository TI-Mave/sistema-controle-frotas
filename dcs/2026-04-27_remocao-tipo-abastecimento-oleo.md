# Remoção do tipo de gasto "Abastecimento + Óleo"

**Data:** 2026-04-27
**Branch:** main
**Arquivo principal alterado:** `CLAUDE.md` (single-page app)

---

## Tarefa solicitada

Remover a opção **"Abastecimento + Óleo"** (`abastecimento_oleo`) da lista de tipos de gasto disponíveis no sistema, ajustando formulário, filtros, listagens, relatórios e a persistência. Registros históricos com esse tipo devem ser **migrados** para `Abastecimento`, sem perda de dados.

## Observação sobre a arquitetura do projeto

Mesmo padrão da entrega anterior (`2026-04-27_campo-km-em-gastos.md`):

- O app é **single-page** com todo o código (HTML + CSS + JS) em `CLAUDE.md`.
- Backend é **Firestore** (NoSQL, schemaless) — não há migrations SQL, ORM, models, DTOs ou validators server-side.
- Não há suíte de testes nem framework configurado.
- Não há `enum` de tipos em backend; o "enum" efetivo são as `<option>` do `<select>` no frontend e as chaves dos mapas `TYPE_NAMES` / badges.

A entrega foi adaptada ao stack real, mantendo o objetivo funcional dos requisitos.

---

## Mudanças aplicadas (em `CLAUDE.md`)

### 1. Formulário "Registrar Gasto" — HTML
- `<select id="expenseType">`: opção `<option value="abastecimento_oleo">Abastecimento + Óleo</option>` removida.
- Demais opções intactas.

### 2. Filtro da seção Gastos — HTML
- `<select id="filterType">`: opção `<option value="abastecimento_oleo">Abastecimento + Óleo</option>` removida.

### 3. JS — handlers de tipo
Onde havia `['abastecimento', 'abastecimento_oleo'].includes(...)`, foi simplificado para `=== 'abastecimento'`:
- `saveExpense` — bloco que escreve `fuelLiters`, `fuelPricePerLiter`, `fuelType` no expense.
- `editExpense` — preenchimento dos campos de combustível ao editar.
- `onExpenseTypeChange` — exibição do bloco `#fuelFields`.
- `calcLiters` — guard inicial.
- `renderDashboard` — agregado `fuelExpenses`.
- `renderMileageReport` — filtros mensal e do período (cálculo de km/L).

### 4. JS — mapas de apresentação
- `TYPE_NAMES`: chave `abastecimento_oleo: 'Abastecimento + Óleo'` removida.
- `getTypeBadge`: chave `abastecimento_oleo: 'badge-fuel'` removida.

### 5. Migração de dados — `migrateAbastecimentoOleoExpenses()`
Nova função executada automaticamente em `loadData()` logo após o carregamento (e antes do `renderAll()`):

- Filtra `expenses` por `type === 'abastecimento_oleo'`.
- Se a lista estiver vazia, **retorna imediatamente** (idempotente, custo zero em runs subsequentes).
- Caso contrário:
  - **Firestore**: para cada doc legado, faz `db.collection('expenses').doc(id).update({ type: 'abastecimento', updatedAt: <ISO> })` e atualiza o objeto em memória.
  - **LocalStorage** (fallback quando `firebaseConfigured === false`): muta `e.type = 'abastecimento'` em memória e chama `saveToLocalStorage()`.
- Falhas individuais (erro de rede, doc indisponível) são logadas no console e **não interrompem** os demais — a migração tenta de novo no próximo load.
- Todos os demais campos (`fuelLiters`, `fuelPricePerLiter`, `fuelType`, `value`, `km`, `obs`, `files`, `createdAt`, etc.) são **preservados**.

### 6. Referências remanescentes ao termo
A única ocorrência de `abastecimento_oleo` no código vive **dentro** de `migrateAbastecimentoOleoExpenses` — necessária para localizar e converter registros legados. Em todos os outros pontos (form, filtros, JS de fluxo, mapas) o termo foi eliminado.

---

## O que ficou de fora (e por quê)

| Item dos requisitos                       | Status              | Motivo                                                                                       |
|-------------------------------------------|---------------------|----------------------------------------------------------------------------------------------|
| Migration SQL / alteração de enum no DB   | Não aplicável       | Firestore é schemaless. A "migração" foi entregue como função client-side idempotente.       |
| Backend models / schemas / DTOs           | Não aplicável       | Não há backend próprio; Firestore é acessado direto pelo SDK no browser.                     |
| Validators server-side rejeitando o valor | Não aplicável (1)   | Sem backend. Em runtime, o valor não pode mais ser produzido pelo frontend (option removida).|
| Hardening em `firestore.rules`            | Não feito           | Pode ser adicionado em PR separado para reforçar a lista de tipos válidos a nível de regras. |
| Testes ajustados                          | Não aplicável       | Projeto não tem runner de testes (mesmo motivo da entrega anterior).                         |

(1) Como o `<option>` foi removido do `<select>`, o frontend não consegue mais submeter `type === 'abastecimento_oleo'` por fluxo normal. Para defesa server-side, ver linha de `firestore.rules`.

---

## Comportamento com dados legados

- Na primeira vez que o app for carregado por qualquer cliente após esta mudança, registros antigos com `type === 'abastecimento_oleo'` são reescritos para `type === 'abastecimento'` no Firestore (ou no localStorage no modo offline).
- A migração é idempotente: nas cargas seguintes, `legacy.length === 0` e a função retorna imediatamente.
- Após a migração, os registros aparecem corretamente:
  - Listagem de Gastos: badge "Abastecimento" (`badge-fuel`) e nome via `TYPE_NAMES`.
  - Dashboard: somam em **Total Abastecimentos**.
  - Relatório por Veículo: agrupados como "Abastecimento".
  - Relatório de Quilometragem: continuam contando como combustível para o cálculo de km/L (mantêm `fuelLiters` original).
- Caso a migração falhe para um doc específico (ex.: erro transitório no Firestore), o registro segue como `abastecimento_oleo` em memória — o `getTypeName` cairá no fallback `expense.type` (texto cru) e o badge ficará `badge-other`. Próximo load tenta migrar de novo.

## Pontos de teste manual sugeridos

1. **Form Registrar Gasto**: abrir o `<select>` de Tipo de Gasto — confirmar que "Abastecimento + Óleo" não aparece.
2. **Filtro de Gastos**: confirmar que o filtro de tipo também não tem mais a opção.
3. **Banco com registros legados**: antes do load, ter pelo menos um expense com `type: 'abastecimento_oleo'` no Firestore. Após carregar, verificar que o doc foi atualizado para `type: 'abastecimento'`, mantendo `fuelLiters`, `fuelPricePerLiter`, `fuelType`, `value`, `km` e demais campos.
4. **Dashboard**: somatório "Total Abastecimentos" deve incluir os registros migrados.
5. **Relatório por Veículo**: agrupamento por tipo mostra "Abastecimento" englobando os legados.
6. **Relatório de Quilometragem**: `Litros Abastecidos` e `km/L` continuam considerando os registros migrados.
7. **Edição de gasto migrado**: abrir um expense que era `abastecimento_oleo` — agora aparece como `Abastecimento` no `<select>`, com bloco `#fuelFields` populado.
8. **Idempotência**: recarregar o app — console não deve mais logar a mensagem de migração.

## Arquivos tocados

- `CLAUDE.md` — todas as mudanças de código.
- `dcs/2026-04-27_remocao-tipo-abastecimento-oleo.md` — este documento.

Nenhum outro arquivo foi alterado nesta tarefa.
