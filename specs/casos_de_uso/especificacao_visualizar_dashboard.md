
# Especificação de Caso de Uso: Visualizar Dashboard

## 1. Identificação
- **Identificador**: UC07
- **Nome do Caso de Uso**: Visualizar Dashboard
- **Atores Principais**: Administrador, Moderador, Capitão
- **Requisitos Funcionais Associados**: RF017

## 2. Objetivo
Exibir painéis resumidos e widgets analíticos com informações e métricas relevantes ao nível de permissão do usuário, facilitando monitoramento e tomada de decisão operacional.

## 3. Pré-condições
- Usuário autenticado no sistema.
- Usuário possui permissões adequadas (RBAC) para visualizar o escopo de dados solicitado.
- Dados necessários para os widgets (atividades, inscrições, resultados, relatórios) devem existir no banco de dados.

## 4. Pós-condições
- O usuário visualiza os widgets e relatórios no escopo permitido, sem alteração persistente dos dados.
- A tentativa de acesso indevido é registrada em log de segurança quando aplicável.
- Consultas sobre o dashboard são auditadas com `user_id`, `widget_id`, `filtros`, `timestamp`, `IP`.

## 5. Fluxos Principais

### 5.1 Carregar Dashboard
1. Usuário navega até a rota `GET /dashboard`.
2. Backend valida sessão e permissões (RNF005).
3. Backend resolve o conjunto de widgets aplicáveis ao papel do usuário (Admin/Moderador/Capitão) e ao seu escopo (atlética, time, competição).
4. Backend consulta serviços/cache para cada widget (contadores, ranking, próximas partidas, notificações) aplicando filtros padrão (período, modalidade).
5. Backend retorna payload com widgets e metadados; frontend renderiza a interface responsiva com gráficos, tabelas e cards.

### 5.2 Aplicar Filtros e Intervalos de Tempo
1. Usuário ajusta filtros (período, modalidade, competição, atlética, time).
2. Frontend requisita dados filtrados (`GET /dashboard?from=...&to=...&modality=...`).
3. Backend valida filtros, aplica controle de escopo e retorna dados paginados/agrupados.
4. Frontend atualiza apenas os widgets afetados (partial refresh).

### 5.3 Exportar Relatório do Widget
1. Usuário aciona exportação (CSV/PDF) em um widget.
2. Backend enfileira tarefa de geração se o volume for grande; taskworker gera o arquivo e disponibiliza por link seguro temporário.
3. Sistema envia notificação por UI/e-mail com link quando pronto.

### 5.4 Atualização em Tempo Real (Opcional)
1. Se o usuário tiver a opção habilitada, frontend abre WebSocket/SSE com `ws://.../dashboard/stream`.
2. Backend envia eventos incrementais para widgets assinados (novas inscrições, resultados publicados).

## 6. Fluxos Alternativos e Exceções
- **[FA01] Permissão Negada**: Se o usuário pedir dados fora de seu escopo, backend responde `403 Forbidden` e registra auditoria de segurança.
- **[FA02] Dados Incompletos**: Se um widget não puder ser calculado por falta de dados, exibir card com mensagem "Dados indisponíveis" e link para tarefa de correção.
- **[FA03] Erro de Serviço Externo**: Se dependência (ex.: serviço de ranking) falhar, exibir estado degradado no widget e registrar erro para operações de retries.
- **[FA04] Exportação falha**: Se geração do arquivo falhar, retornar erro e permitir re-tentar; não bloquear outros widgets.

## 7. Regras de Negócio
- Visibilidade por escopo: `Administrador` vê todos os dados; `Moderador` vê dados associados às suas atléticas e competições gerenciadas; `Capitão` vê dados do seu time/atlética conforme permissão.
- Widgets devem respeitar paginação, limite máximo de 1000 linhas por consulta direta; exports maiores são processados em background.
- Valores numéricos exibidos devem corresponder às regras de cálculo oficiais (ex.: pontuação por vitória/empate/derrota conforme RF021).
- Dados sensíveis devem ser mascarados ou omitidos conforme RNF004 (LGPD) quando o usuário não tiver consentimento.

## 8. Mensagens de Erro e Feedback
- "Acesso negado: você não tem permissão para visualizar estes dados." (FA01).
- "Dados indisponíveis para o período selecionado." (FA02).
- "Exportação em andamento — você será notificado quando o arquivo estiver pronto." (success for queued exports).
- "Falha ao carregar widget — tente novamente mais tarde." (FA03).

## 9. Segurança e Auditoria
- Aplicar RBAC e verificação de escopo em todas as consultas (RNF005).
- Proteger endpoints com CSRF tokens, validação de entrada e rate-limiting para prevenir scraping e negação de serviço.
- Não incluir dados pessoais sem consentimento; registrar consentimento quando aplicável (RNF004).
- Auditar acessos e ações críticas como exportações e streaming de dados sensíveis.

## 10. Critérios de Aceitação
- Dashboard carrega em < 2s para cargas típicas (caches quentes) e < 5s para consultas filtradas moderadas.
- Widgets exibem dados corretos e consistentes com relatórios oficiais em 99% das amostras de validação.
- Acesso negado é corretamente aplicado para escopos restritos (testar com contas Admin/Mod/Capitão).
- Exportação de dados grandes é enfileirada e entregue por link seguro.
- Eventos em tempo real chegam ao frontend com latência aceitável (< 500ms para eventos críticos).

## 11. Casos de Teste Sugeridos
- Carregar dashboard como `Administrador` → visualizar todos os widgets e métricas.
- Carregar dashboard como `Capitão` → confirmar visibilidade apenas do time/atlética apropriada.
- Aplicar filtro de período e modalidade → verificar atualização de widgets e consistência de totais.
- Tentar acessar relatório fora do escopo → receber `403 Forbidden` e entrada de auditoria.
- Acionar exportação de grande volume → verificar criação de tarefa, notificação e link de download.
- Simular falha em serviço de ranking → widget exibe estado degradado sem impactar outros widgets.

---