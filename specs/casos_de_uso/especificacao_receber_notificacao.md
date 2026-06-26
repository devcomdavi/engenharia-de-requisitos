
# Especificação de Caso de Uso: Receber Notificação

## 1. Identificação
- **Identificador**: UC16
- **Nome do Caso de Uso**: Receber Notificação
- **Atores Principais**: Administrador, Moderador, Capitão
- **Requisitos Funcionais Associados**: RF027

## 2. Objetivo
Notificar usuários com cargo sobre eventos relevantes (novas competições, inscrições abertas, lembretes de jogos, avisos administrativos), entregando informações no canal e com a frequência apropriada ao papel do usuário.

## 3. Pré-condições
- Usuário com cargo (Administrador, Moderador ou Capitão) possui perfil ativo.
- Preferências de notificação configuradas pelo usuário ou padrão do sistema.
- Integração de envio (e-mail, push, in-app) configurada e disponível.

## 4. Pós-condições
- Notificações registradas como enviadas/pendentes/entregues/falhadas conforme canal.
- Usuários recebem aviso através dos canais habilitados conforme suas preferências.
- Eventos de notificação são auditados (user_id responsável, tipo de notificação, destinatários, timestamp, resultado).

## 5. Tipos de Notificações
- Novas competições criadas.
- Abertura de inscrições para competições.
- Lembretes de jogos agendados (configuráveis: 24h, 1h antes, etc.).
- Avisos administrativos (manutenção, alteração de regras, comunicados gerais).
- Atualizações de inscrições (confirmação, rejeição, lista de espera).

## 6. Fluxo Principal — Envio automático (sistema desencadeado)
1. Evento gerador ocorre (ex.: competição criada, jogo agendado, prazo de inscrição se aproximando).
2. Sistema identifica público-alvo (todos usuários com cargo, capitães das atléticas afetadas, etc.).
3. Sistema consulta preferências de notificação e canais disponíveis.
4. Sistema compõe mensagem substituindo template com dados do evento.
5. Sistema envia notificações nos canais habilitados (in-app, e-mail, push) e registra resultados.
6. Sistema re-tenta envio em caso de falha segundo política de retry e marca eventuais falhas para análise.

## 7. Fluxo Principal — Envio manual (ator desencadeado)
1. Administrador ou Moderador seleciona criar um comunicado/aviso.
2. Ator define público-alvo, canal(es) e conteúdo ou seleciona template.
3. Sistema realiza validações (conteúdo, público, permissões) e solicita confirmação.
4. Após confirmação, sistema envia notificações conforme preferências e registra auditoria.

## 8. Preferências do Usuário
- Usuário pode habilitar/desabilitar canais: `in-app`, `e-mail`, `push`.
- Configurar nível de detalhe: `todas`, `apenas críticas`, `somente administrativas`.
- Configurar lembretes temporais (ex.: 24h antes, 1h antes).
- Opt-out global disponível, respeitando avisos obrigatórios do sistema.

## 9. Fluxos Alternativos e Exceções
- **[FA01] Canal indisponível**: Se e-mail/push indisponível, marcar como falha e notificar por canal alternativo ou re-tentar.
- **[FA02] Preferência de usuário bloqueia envio**: Se usuário optou por não receber determinado tipo, pular envio para esse usuário.
- **[FA03] Template inválido**: Rejeitar envio manual até correção do template.
- **[FA04] Volume elevado**: Ao enviar para grande massa, usar filas e rate limiting para evitar sobrecarga e spam.

## 10. Regras de Negócio
- Notificações de prazo e administrativas podem ser marcadas como obrigatórias (não permitidas para opt-out).
- Mensagens devem ser localizadas para português (Brasil).
- Sistema deve evitar envios duplicados salvo quando confirmado pelo usuário.
- Logs de entrega devem reter status por X dias conforme política de auditoria.

## 11. Mensagens de Erro e Feedback
- Feedback claro em caso de falha de envio (ex.: "Falha ao enviar e-mail para user@exemplo.com").
- Informar sucesso quando notificações forem agendadas/enviadas.
- Para envios manuais, apresentar resumo com contagem de destinatários, canais e falhas.

## 12. Segurança e Privacidade
- Respeitar RNF003 e RNF004: não incluir dados sensíveis em notificações sem consentimento.
- Limitar exposição de dados pessoais em notificações públicas.
- Proteger endpoints de envio com RBAC (RNF005) e CSRF.

## 13. Critérios de Aceitação
- Usuários com cargo recebem notificações conforme preferência configurada.
- Lembretes de jogos são disparados nos intervalos configurados.
- Sistema registra entregas e falhas, com retry implementado para falhas transitórias.
- Envio massivo não impacta negativamente a disponibilidade do sistema.
- Testes automatizados cobrindo envios automáticos, envios manuais, preferências e tratamento de falhas.

## 14. Casos de Teste Sugeridos
- Notificação automática de nova competição: destinatários corretos e mensagem template preenchida.
- Lembrete de jogo 24h antes: envio para capitães com canal habilitado.
- Envio manual com público selecionado: resumo de envio e auditoria gravada.
- Usuário com opt-out não recebe notificações não obrigatórias.
- Simular falha de e-mail e validar retry e fallback para in-app.

---