
# Especificação de Caso de Uso: Agendar Instalação Física

## 1. Identificação
- **Identificador**: UC13
- **Nome do Caso de Uso**: Agendar Instalação Física
- **Atores Principais**: Administrador, Moderador, Capitão (quando autorizado)
- **Requisitos Funcionais Associados**: RF023

## 2. Objetivo
Permitir agendamento de espaços físicos (quadras, ginásios, campos) para partidas e atividades, evitando conflitos de horário e fornecendo verificações de disponibilidade, notificações e histórico de agendamentos.

## 3. Pré-condições
- Usuário autenticado com permissão para agendar (Admin/Moderador; Capitão nas suas instalações quando autorizado).
- O local (instalação) já está cadastrado no sistema com atributos: nome, tipo, capacidade, horários disponíveis e restrições.

## 4. Pós-condições
- Agendamento persistido com status `confirmado` ou `pendente` conforme regras de aprovação.
- Conflitos evitados: nenhum outro agendamento confirmado para o mesmo local e intervalo de tempo.
- Notificações enviadas aos responsáveis e partes interessadas.
- Auditoria gravada com `user_id`, `local_id`, `inicio`, `fim`, `timestamp` e `IP`.

## 5. Fluxos Principais

### 5.1 Agendar Instalação
1. Ator seleciona "Agendar Instalação Física".
2. Sistema exibe formulário com campos obrigatórios: `local`, `data_inicio`, `hora_inicio`, `data_fim` (ou duração), `hora_fim`, `finalidade` e `responsável`.
3. Ator preenche os dados e submete solicitação.
4. Sistema valida:
   - `local` existe e está ativo;
   - intervalo (`data_inicio`+`hora_inicio` a `data_fim`+`hora_fim`) é válido (início < fim);
   - não há agendamento confirmado no mesmo `local` com interseção de tempo;
   - respeita horários de abertura/fechamento e restrições do local (capacidade, tipo de atividade);
   - se houver limite de reservas por usuário, respeitar a cota.
5. Se as validações passarem, o sistema cria a solicitação com status `confirmado` (ou `pendente` se requer aprovação manual) e envia confirmação.

### 5.2 Editar Agendamento
1. Ator seleciona agendamento existente e escolhe "Editar".
2. Sistema exibe formulário com dados atuais.
3. Ator altera campos permitidos.
4. Sistema revalida regras de disponibilidade e restrições.
5. Em sucesso, atualiza o agendamento, registra auditoria e notifica interessados.

### 5.3 Cancelar Agendamento
1. Ator solicita cancelamento.
2. Sistema verifica permissão e janela mínima para cancelamento (se aplicável).
3. Se permitido, sistema marca agendamento como `cancelado`, atualiza disponibilidade e notifica partes.

### 5.4 Consultar Disponibilidade
1. Ator consulta calendário/agenda do `local` com filtros por data e horário.
2. Sistema exibe blocos ocupados, vagas e eventos pendentes.

## 6. Fluxos Alternativos e Exceções
- **[FA01] Local inexistente/inativo**: Rejeitar solicitação e sugerir locais alternativos.
- **[FA02] Conflito de horário**: Se existir interseção com agendamento confirmado, rejeitar e oferecer opções próximas (horários/lugares disponíveis).
- **[FA03] Solicitação fora do horário permitido**: Rejeitar se fora do horário de funcionamento ou em período bloqueado.
- **[FA04] Cota excedida**: Rejeitar se o usuário ultrapassou o número máximo de reservas simultâneas.
- **[FA05] Aprovação necessária**: Se a reserva requer aprovação manual, criar status `pendente` e notificar aprovadores; permitir confirmação/rejeição com justificativa.
- **[FA06] Erro de validação de dados**: Campos obrigatórios ausentes ou formatos inválidos → exibir erros por campo.

## 7. Regras de Negócio
- Agendamento não pode sobrepor agendamentos confirmados para o mesmo local.
- Cada local tem horário de funcionamento e restrições (ex.: só treinos, só competições, necessidade de técnico).
- Alguns agendamentos exigem aprovação manual (ex.: uso para eventos externos, uso fora do horário padrão).
- Cancelamentos com pouca antecedência podem gerar penalidades ou bloqueios temporários.
- Sistema deve suportar reservas recorrentes (opcional), respeitando limites e verificando conflitos em cada ocorrência.

## 8. Mensagens de Erro e Feedback
- "Local indisponível no período selecionado." (FA02).
- "Horário inválido: data/hora de término anterior ao início." (FA06).
- "Reserva criada e pendente de aprovação." (quando aplicável).
- "Agendamento cancelado com sucesso." (após cancelamento).

## 9. Notificações e Integrações
- Enviar confirmação por e-mail/in-app para o responsável e aprovadores quando aplicável.
- Integração com calendários (iCal) e exportação de eventos (opcional).
- Painel de administradores com fila de solicitações pendentes e histórico.

## 10. Segurança e Auditoria
- Registrar todas as ações com `user_id`, `ação`, `local_id`, `inicio`, `fim`, `timestamp` e `IP`.
- Aplicar controle de acesso RBAC (RNF005) para criação/edição/cancelamento.
- Validar entradas para prevenir injeção e proteger endpoints com CSRF.

## 11. Critérios de Aceitação
- Não permitir criação de agendamento que cause conflito com agendamento confirmado existente.
- Reservas que exigem aprovação ficam em `pendente` até decisão de aprovador.
- Notificações são enviadas aos responsáveis após criação/alteração/cancelamento.
- Consultas de disponibilidade exibem os blocos corretos em calendário.
- Testes automatizados cobrindo criação, edição, cancelamento, conflito de horários e fluxo de aprovação.

## 12. Casos de Teste Sugeridos
- Criar agendamento válido: sucesso e bloco no calendário.
- Tentar agendar em intervalo já ocupado: receber alternativa ou erro de conflito.
- Editar agendamento mudando para horário conflituoso: operação rejeitada.
- Cancelar agendamento dentro da janela permitida: cancelamento e notificação.
- Criar reserva que requer aprovação: status `pendente` e notificação ao aprovador.

---