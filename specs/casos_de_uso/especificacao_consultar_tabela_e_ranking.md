
# Especificação de Caso de Uso: Consultar Tabela e Ranking

## 1. Identificação
- **Identificador**: UC11
- **Nome do Caso de Uso**: Consultar Tabela e Ranking
- **Atores Principais**: Administrador, Moderador, Capitão, Usuário Público
- **Requisitos Funcionais Associados**: RF021, RF029

## 2. Objetivo
Permitir a consulta pública e autenticada das tabelas de classificação e rankings por modalidade e geral, fornecendo filtros, histórico por edição dos jogos e exportação de dados.

## 3. Pré-condições
- Dados de partidas, resultados e pontuações devidamente registrados no sistema.
- Para visualizações privadas (ex.: estatísticas de gestão), usuário autenticado com cargo e permissões apropriadas.

## 4. Pós-condições
- Visualização gerada com dados consistentes e atualizados conforme última súmula homologada.
- Exportações (PDF/CSV/XLSX) geradas conforme solicitação do usuário.

## 5. Fluxo Principal
1. Usuário acessa a página de Tabelas/Rankings (pública ou via dashboard para usuários autenticados).
2. Sistema exibe filtros padrão: `edição` (ano/edição), `modalidade`, `campus/categoria`, `período`.
3. Usuário escolhe filtros e solicita visualização.
4. Sistema calcula classificação aplicando regras de pontuação por modalidade e soma para ranking geral.
5. Sistema apresenta tabela/ranking com colunas relevantes (posição, atlética, pontos, vitórias, empates, derrotas, saldo de pontos, jogos, etc.) e opções de ordenação/ordenar por critérios alternativos.
6. Usuário pode exportar a tabela em PDF/CSV/XLSX ou compartilhar link público.

## 6. Regras de Cálculo e Validações
- Cada modalidade possui sua regra de pontuação (ex.: vitória = 3 pontos, empate = 1, derrota = 0) e desempates (saldo de pontos, confronto direto, maior número de vitórias).
- Ranking geral é calculado a partir da agregação de pontos obtidos em todas as modalidades, possivelmente com pesos por modalidade (configurável pelo administrador).
- Apenas resultados homologados (súmulas aprovadas) devem ser considerados no cálculo.
- Histórico por edição: permitir visualizar rankings e tabelas para edições passadas sem afetar a edição atual.

## 7. Filtros e Visualizações
- Filtrar por `modalidade`, `edição`, `atlética`, `campus`, `categoria` e `período`.
- Visualizações disponíveis: tabela por modalidade, ranking geral, gráficos de evolução (linhas), distribuição por campus.
- Indicadores mostrados: pontos, posição atual, variação em relação à rodada anterior, jogos disputados, aproveitamento percentual.

## 8. Acessibilidade e Publicação
- Páginas públicas devem seguir padrões de acessibilidade (WCAG mínimo) e serem responsivas para dispositivos móveis.
- Rankings públicos não devem expor dados sensíveis (e-mails, CPFs, etc.).

## 9. Exportação e API
- Disponibilizar exportação em PDF, CSV e XLSX com opção de incluir cabeçalho e rodapé institucional.
- Expor endpoint API pública para consulta de tabelas e rankings (rate-limited) com parâmetros equivalentes aos filtros da UI.

## 10. Segurança e Controle de Acesso
- Dados sensíveis e endpoints administrativos somente acessíveis por usuários autenticados com permissão adequada.
- Aplicar caching para visualizações públicas e invalidação ao publicar novas súmulas.
- Proteger APIs com rate limiting e autenticação para endpoints não públicos.

## 11. Mensagens de Erro e Feedback
- Se não houver dados para os filtros selecionados: "Nenhum resultado encontrado para os filtros selecionados.".
- Em caso de erro no cálculo (ex.: dados inconsistentes), exibir mensagem e registrar incidente para análise.

## 12. Critérios de Aceitação
- Tabelas por modalidade e ranking geral apresentados corretamente conforme regras de pontuação e considerando apenas súmulas homologadas.
- Exportação em PDF/CSV/XLSX funcional e formatada.
- API pública retorna dados consistentes com a UI e respeita limites de uso.
- Testes automatizados cobrindo cálculo de pontos, desempate, agregação para ranking geral e exportação.

## 13. Casos de Teste Sugeridos
- Gerar tabela de uma modalidade com resultados registrados → conferir posições e desempates.
- Calcular ranking geral com pesos diferentes por modalidade → validar soma ponderada.
- Exportar tabela para CSV/XLSX → validar conteúdo e formatação.
- Visualizar ranking de edição passada → garantir que dados históricos correspondam aos registros dessa edição.
- Acessar API pública com filtros → validar payload e limites de rate.

---