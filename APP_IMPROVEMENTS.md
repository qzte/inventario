# Propostas de melhoria para a app de inventário

## Objetivo

Com base na avaliação do brief v1, a app deve manter o foco no ciclo essencial de inventário — importar, consultar, movimentar stock, identificar ruturas e exportar — mas precisa de reforços práticos para reduzir riscos de performance, normalização de dados e perceção de lentidão.

As melhorias abaixo estão organizadas por prioridade para proteger o lançamento v1 sem transformar a app num produto demasiado complexo antes da validação com utilizadores reais.

## Prioridade 1 — Melhorias recomendadas antes do v1

### 1. Feedback visível em operações demoradas

**Problema que resolve:** a importação/exportação de ficheiros e operações sobre listas grandes podem parecer bloqueadas, mesmo quando estão a correr corretamente.

**Proposta:**

- Mostrar estados como `A importar ficheiro...`, `A processar dados...` e `A exportar inventário...`.
- Desativar botões enquanto a operação está em curso para evitar cliques repetidos.
- Apresentar uma mensagem final com o resultado da operação: número de artigos importados, linhas ignoradas e eventuais avisos.

**Impacto esperado:** aumenta a confiança do utilizador e reduz a perceção de erro ou bloqueio da app.

### 2. Relatório de importação com avisos

**Problema que resolve:** ficheiros Excel reais podem ter colunas inesperadas, linhas vazias, códigos repetidos ou quantidades inválidas.

**Proposta:**

- Depois de importar, mostrar um resumo com:
  - total de linhas lidas;
  - artigos importados;
  - linhas ignoradas por falta de código;
  - códigos duplicados;
  - quantidades inválidas convertidas para zero.
- Permitir copiar ou exportar este relatório para facilitar correções no ficheiro original.

**Impacto esperado:** torna a normalização transparente e evita que o utilizador descubra problemas apenas mais tarde no inventário.

### 3. Regras explícitas para códigos duplicados

**Problema que resolve:** o código do artigo funciona como identificador principal, mas a app precisa de uma regra clara quando o mesmo código aparece mais do que uma vez.

**Proposta:**

- Definir uma regra v1 simples: manter a primeira ocorrência e reportar duplicados no relatório de importação.
- Numa fase seguinte, permitir ao utilizador escolher entre substituir, somar quantidades ou manter separado.

**Impacto esperado:** evita stock incoerente e reduz ambiguidades durante a importação.

### 4. Guia de formato Excel aceite

**Problema que resolve:** utilizadores podem não saber quais os cabeçalhos e valores esperados pela app.

**Proposta:**

- Adicionar uma pequena secção de ajuda no ecrã de importação com os cabeçalhos recomendados: `CODIGO`, `ARTIGO`, `TIPO`, `COR`, `QUANTIDADE`.
- Disponibilizar um ficheiro modelo ou uma tabela de exemplo.
- Explicar que espaços extra são removidos e que artigos sem código são ignorados.

**Impacto esperado:** diminui erros de importação e reduz necessidade de suporte.

## Prioridade 2 — Melhorias para robustez operacional

### 5. Validação antes de movimentos de stock

**Problema que resolve:** movimentos incorretos podem distorcer rapidamente o inventário.

**Proposta:**

- Impedir saídas que deixem o stock negativo, exceto se existir uma confirmação explícita.
- Validar quantidades nulas, negativas ou não numéricas.
- Pedir motivo obrigatório para ajustes manuais relevantes.

**Impacto esperado:** melhora a qualidade dos dados e cria uma trilha mínima de responsabilidade.

### 6. Pesquisa mais tolerante

**Problema que resolve:** pequenas diferenças de acentuação, maiúsculas/minúsculas ou espaços podem dificultar encontrar artigos.

**Proposta:**

- Normalizar pesquisa removendo acentos, convertendo para minúsculas e ignorando espaços duplicados.
- Aplicar a mesma normalização a código, artigo, tipo e cor.
- Manter o texto original visível na interface, usando a normalização apenas para comparação.

**Impacto esperado:** reduz fricção no uso diário e aumenta a velocidade percebida da app.

### 7. Paginação e limites comunicados

**Problema que resolve:** listas muito grandes podem tornar a interface pesada.

**Proposta:**

- Manter paginação ou carregamento por blocos na lista de inventário.
- Mostrar claramente quantos resultados estão visíveis e quantos existem no total.
- Adicionar uma nota de limite recomendado para ficheiros v1, por exemplo por número de linhas testado.

**Impacto esperado:** protege a performance e define expectativas realistas.

## Prioridade 3 — Melhorias pós-v1

### 8. Cópia de segurança e restauro local

**Problema que resolve:** como a app usa armazenamento local no browser, os dados podem perder-se se o utilizador limpar o navegador ou mudar de dispositivo.

**Proposta:**

- Criar uma opção `Exportar cópia de segurança` com inventário, movimentos e serviços.
- Criar uma opção `Restaurar cópia de segurança` para recuperar o estado completo da app.
- Mostrar aviso claro de que os dados v1 ficam guardados no dispositivo atual.

**Impacto esperado:** reduz risco de perda de dados sem exigir backend no v1.

### 9. Indicadores de saúde do inventário

**Problema que resolve:** o dashboard atual pode evoluir para ajudar na priorização operacional.

**Proposta:**

- Destacar os artigos sem stock por tipo ou serviço.
- Mostrar top artigos com mais saídas recentes.
- Adicionar tendência simples de movimentos dos últimos 7 ou 30 dias.

**Impacto esperado:** transforma a app de consulta numa ferramenta de decisão operacional.

### 10. Preparação para persistência futura

**Problema que resolve:** se a app crescer para múltiplos utilizadores, será necessário mover dados para backend sem reescrever toda a lógica.

**Proposta:**

- Manter funções de leitura/escrita de dados isoladas.
- Documentar o modelo de dados atual: artigos, movimentos e serviços.
- Evitar acoplar regras de negócio diretamente a elementos visuais quando possível.

**Impacto esperado:** reduz custo de evolução para base de dados, autenticação e auditoria.

## Roadmap recomendado

| Fase | Melhorias | Objetivo |
| --- | --- | --- |
| v1 antes do lançamento | feedback de operações, relatório de importação, duplicados, guia Excel | aumentar confiança e reduzir erros básicos |
| v1.1 | validação de movimentos, pesquisa tolerante, paginação comunicada | melhorar uso diário e robustez |
| v2 | backup/restauro, indicadores avançados, preparação para backend | evoluir sem comprometer a simplicidade inicial |

## Métricas de validação

Para confirmar se as melhorias estão a funcionar, recomenda-se acompanhar:

- tempo percebido de importação/exportação reportado pelos utilizadores;
- número de ficheiros importados com avisos;
- frequência de códigos duplicados ou linhas ignoradas;
- tempo médio até encontrar um artigo;
- número de correções manuais feitas após importação;
- pedidos de suporte relacionados com perda de dados ou formato Excel.

## Recomendação final

A app não precisa de mais funcionalidades grandes para o v1. Precisa sobretudo de ser mais previsível, explícita e confiável nas operações críticas. As melhorias prioritárias devem concentrar-se em feedback ao utilizador, validação de dados importados e regras claras para casos ambíguos. Isso protege a experiência atual sem aumentar demasiado a complexidade técnica.
