# Especificação de Caso de Uso: Inscrever Time em Competição

## 1. Identificação
- **Identificador**: UC09
- **Nome do Caso de Uso**: Inscrever Time em Competição
- **Atores Principais**: Administrador, Moderador
- **Requisitos Funcionais Associados**: RF019

## 2. Objetivo
Permitir que administradores e moderadores inscrevam times em competições ativas, garantindo que os times atendam às restrições da modalidade e às regras do evento.

## 3. Pré-condições
- Usuário autenticado com papel de Administrador ou Moderador.
- Competição ativa e com período de inscrições aberto.
- Time cadastrado no sistema.

## 4. Pós-condições
- Time inscrito na competição com registro persistido.
- Vagas/contadores atualizados caso haja limite de inscrições.
- Registro de auditoria da operação (user_id, time_id, competicao_id, timestamp, IP).
- Notificação enviada ao capitão responsável pelo time.

## 5. Fluxo Principal
1. Ator acessa a página de gestão da competição e seleciona "Inscrever Time".
2. Sistema exibe lista de times elegíveis ou formulário para busca por time.
3. Ator seleciona o time e confirma a submissão.
4. Sistema valida:
   - competição está com inscrições abertas;
   - o time pertence à atlética correta e à modalidade da competição;
   - o time possui o número mínimo de atletas exigido pela modalidade;
   - o time não está inscrito previamente na mesma competição;
   - não há conflitos de calendário (opcional) e há vagas disponíveis (se aplicável).
5. Se todas as validações passarem, o sistema registra a inscrição, decrementa vagas se necessário e confirma sucesso.
6. Sistema envia notificação por e-mail/sistema ao capitão e registra auditoria.

## 6. Fluxos Alternativos e Exceções
- **[FA01] Inscrições fechadas**: Se a competição não aceitar inscrições no momento → informar e bloquear ação.
- **[FA02] Time incompleto**: Se o time não atingir o mínimo de atletas → rejeitar e explicar a carência (ex.: "Time precisa de 2 atletas a mais").
- **[FA03] Time já inscrito**: Se o time já estiver inscrito → informar e impedir duplicação.
- **[FA04] Modalidade incompatível**: Se o time não corresponder à modalidade da competição → rejeitar explicando o motivo.
- **[FA05] Vagas esgotadas**: Se não houver vagas restantes → oferecer lista de espera (se suportado) ou recusar inscrição.
- **[FA06] Conflito de horário**: Se o time possui jogo/inscrição conflitante no calendário → bloquear e detalhar conflito.
- **[FA07] Permissão negada**: Se o ator não tiver permissão para inscrever o time → bloquear e registrar tentativa.

## 7. Regras de Negócio / Validações
- Inscrição somente durante o período definido pela competição.
- Time deve pertencer à mesma atlética associada ao cadastro (ou conforme política de transferência).
- Número de atletas deve atender ao mínimo da modalidade (validar conforme RF011).
- Evitar duplicação de inscrições para o mesmo time na mesma competição.
- Possibilidade de lista de espera quando vagas esgotadas (implementação opcional).

## 8. Mensagens de Erro e Feedback
- "Inscrições estão fechadas para esta competição." (FA01).
- "Time precisa de X atleta(s) adicionais para cumprir o requisito da modalidade." (FA02).
- "Time já inscrito nesta competição." (FA03).
- "Modalidade do time é incompatível com a competição." (FA04).
- "Vagas esgotadas — sua inscrição foi colocada na lista de espera." (quando aplicável).

## 9. Segurança e Auditoria
- Registrar todas as inscrições com `user_id`, `time_id`, `competicao_id`, `timestamp` e `IP`.
- Aplicar verificação de permissão via RBAC (RNF005).
- Proteger endpoints com CSRF e validação de input para prevenir injeções.

## 10. Critérios de Aceitação
- Sistema valida corretamente número mínimo de atletas antes de confirmar inscrição.
- Sistema impede inscrições fora do período definido.
- Não são permitidas inscrições duplicadas para o mesmo time na mesma competição.
- Auditoria e notificações são geradas após inscrição bem-sucedida.
- Testes automatizados cobrindo cenários: inscrição válida, time incompleto, duplicação, vagas esgotadas e permissões.

## 11. Casos de Teste Sugeridos
- Inscrição válida: competição aberta, time atende requisitos → sucesso e notificação.
- Inscrição com time incompleto: receber erro com quantidade faltante.
- Tentativa de dupla inscrição: registro recusado.
- Inscrição com vagas esgotadas: ser colocado em lista de espera ou receber recusa.
- Inscrição fora do período de inscrições: ação bloqueada.

---