
# Especificação de Caso de Uso: Gerar Relatório e Exportação

## 1. Identificação
- **Identificador**: UC14
- **Nome do Caso de Uso**: Gerar Relatório e Exportação
- **Atores Principais**: Administrador, Moderador
- **Requisitos Funcionais Associados**: RF024, RF025

## 2. Objetivo
Fornecer relatórios e exportações de dados do sistema para análises institucionais e operacionais (PDF, CSV, XLSX), com filtros, agregações e opções de agendamento e automação.

## 3. Pré-condições
- Usuário autenticado com papel adequado (Admin/Moderador) e permissões para acessar dados solicitados.
- Dados subjacentes (participantes, partidas, resultados, inscrições) estão atualizados e homologados.

## 4. Pós-condições
- Arquivo de relatório gerado e disponibilizado para download ou enviado por e-mail se agendado.
- Exportações respeitam políticas de privacidade (ex.: remover dados sensíveis quando necessário).
- Logs de geração exportação gravados com `user_id`, parâmetros usados, formato, timestamp e status.

## 5. Tipos de Relatórios e Exportações
- Relatórios de participação por competição, modalidade, atlética, atleta.
- Relatórios de presenças e horas complementares por atleta/atlética.
- Relatórios de inscrições e desistências por competição.
- Estatísticas agregadas: total de atletas, atléticas, times, usuários ativos, taxa de participação.
- Exportações de dados brutos para integração com outras ferramentas (CSV/XLSX) e relatórios formatados para apresentação (PDF).

## 6. Fluxo Principal — Geração Manual de Relatório
1. Ator acessa painel de relatórios e escolhe tipo de relatório.
2. Ator configura filtros: `edição`, `modalidade`, `periodo`, `atlética`, `campus`, `formatos` e opções de agregação.
3. Ator opta por gerar imediatamente ou agendar (email) e confirma.
4. Sistema valida parâmetros e executa consulta/geração.
5. Sistema renderiza relatório e disponibiliza para download; se agendado, envia por e-mail com anexo ou link seguro.
6. Sistema registra evento de geração com parâmetros e resultado.

## 7. Fluxo Principal — Exportação de Dados Brutos
1. Ator seleciona exportação de dados brutos e define colunas e filtros.
2. Sistema valida volume e aplica limites de paginação/segmentação para evitar geração massiva em uma única operação.
3. Sistema gera arquivo CSV/XLSX e disponibiliza para download ou envia por link seguro.

## 8. Agendamento e Automação
- Permitir agendar relatórios recorrentes (diário, semanal, mensal) com envio por e-mail para lista de destinatários.
- Gerenciar histórico de relatórios agendados e permitir cancelamento/edição.

## 9. Privacidade e Anonimização
- Para exportações públicas ou quando solicitado, oferecer opção de anonimizar dados (remover e-mails, matricula parcial, etc.).
- Respeitar LGPD: apenas exportar dados pessoais quando autorizado ou necessário; aplicar consentimento quando aplicável.

## 10. Performance e Limites
- Relatórios pesados devem ser processados em background (fila) e notificar o usuário quando prontos.
- Aplicar limites por usuário/periodo e políticas de rate limiting para evitar sobrecarga.

## 11. Formatos e Layouts
- Suportar PDF (formatado para impressão), CSV (dados brutos), XLSX (planilha com múltiplas abas).
- Incluir metadados no arquivo (gerado_por, filtros, data_hora, hash do arquivo).

## 12. Segurança e Auditoria
- Controlar acesso via RBAC; only Admin/Moderador podem acessar relatórios sensíveis.
- Registrar geracoes com `user_id`, `parametros`, `formatos`, `tamanho_arquivo`, `timestamp` e `IP`.
- Links de download temporários e autenticados com expiração configurável.

## 13. Mensagens de Erro e Feedback
- "Nenhum dado encontrado para os filtros selecionados." para conjuntos vazios.
- "Relatório agendado com sucesso; você será notificado por e-mail quando estiver pronto." para jobs agendados.
- Em caso de falha: apresentar motivo e instruções (ex.: "Erro ao gerar relatório: timeout de consulta; tente reduzir o intervalo de datas").

## 14. Critérios de Aceitação
- Relatórios gerados corretamente com filtros aplicados.
- Exportações em CSV/XLSX contêm colunas solicitadas e são compatíveis com ferramentas externas.
- Agendamento funciona e entrega relatórios por e-mail conforme configurado.
- Logs e auditoria registram todas as operações de geração e exportação.
- Testes automatizados cobrindo geração síncrona, jobs assíncronos e exportação/anonimização.

## 15. Casos de Teste Sugeridos
- Gerar relatório de participação para uma edição específica → checar contagem e colunas.
- Exportar dados brutos com filtro por atlética → validar CSV/XLSX gerado.
- Agendar relatório semanal → receber e-mail com anexo/link corretamente.
- Gerar relatório com grande volume → processo em background, notificação e arquivo disponível.
- Gerar relatório público com anonimização → verificar remoção de dados sensíveis.

---