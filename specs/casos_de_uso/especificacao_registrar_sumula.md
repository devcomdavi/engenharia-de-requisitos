
# Especificação de Caso de Uso: Registrar Súmula

## 1. Identificação
- **Identificador**: UC10
- **Nome do Caso de Uso**: Registrar Súmula
- **Atores Principais**: Administrador, Moderador, Capitão (quando autorizado)
- **Requisitos Funcionais Associados**: RF020

## 2. Objetivo
Registrar súmulas digitais das partidas de forma auditável e adaptada à modalidade esportiva, garantindo que todos os eventos relevantes sejam capturados e que os resultados sejam computados oficialmente.

## 3. Pré-condições
- Usuário autenticado com permissão para registrar súmula (Administrador/Moderador; Capitão se autorizado pelo papel/competição).
- Partida/agendamento previamente criado e com participantes definidos.
- Modelo de súmula configurado para a modalidade (template de campos e eventos).

## 4. Pós-condições
- Súmula persistida como documento oficial da partida, com versão e marca temporal.
- Resultado agregado (placar, tempo, pontos, sets, etc.) computado e disponível nos rankings/tabelas.
- Auditoria registrada com `user_id`, `partida_id`, `timestamp`, `IP` e hash/assinatura digital (se aplicável).

## 5. Modelos de Súmula (exemplos por modalidade)
- Futsal / Futebol: gols por tempo, autores dos gols, substituições, cartões (amarelo/vermelho), escanteios, faltas relevantes.
- Vôlei: sets (placares por set), pontuação por set, substituições, faltas e cartões.
- Basquete: pontos por jogador por período, faltas, tempo jogado, eficiência, prorrogações.
- Atletismo / natação: tempos por atleta, ordem de chegada, recordes.
- Xadrez: resultado (1-0, 0-1, ½-½), movimentos e observações relevantes.

## 6. Fluxo Principal — Registro durante a partida (tempo real)
1. Usuário inicia a súmula vinculada à `partida_id`.
2. Sistema carrega o template adequado à modalidade.
3. Usuário registra eventos conforme ocorrem (gols, pontos, cartões, substituições, tempos, resultados parciais).
4. Sistema valida eventos em tempo real (atleta pertence ao time, atleta não excede limite de substituições, etc.).
5. Ao término da partida, usuário confirma e submete a súmula.
6. Sistema calcula resultado final, atualiza classificações/tabelas e grava a súmula com versão e registro de auditoria.

## 7. Fluxo Principal — Registro pós-partida (upload / import)
1. Usuário seleciona partida e carrega súmula em formato suportado (JSON, CSV, XML) ou preenche formulário com dados finais.
2. Sistema valida integridade e consistência dos dados importados.
3. Em caso de validação, sistema grava súmula e atualiza resultados e rankings.

## 8. Fluxos Alternativos e Exceções
- **[FA01] Evento inválido**: Evento rejeitado se referência inválida (atleta inexistente ou não participante) — exibir erro e permitir correção.
- **[FA02] Conflito de registros**: Se já existir súmula aprovada para a partida, bloquear alteração direta e exigir processo de contestação/reabertura.
- **[FA03] Discrepância de pontuação**: Se total calculado divergir do informado, destacar inconsistência e exigir revisão manual.
- **[FA04] Registro incompleto**: Não permitir aprovação final se campos obrigatórios estiverem em falta.

## 9. Regras de Negócio / Validações
- A súmula aprovada é considerada documento oficial e não pode ser alterada sem processo de reabertura com justificativa e auditoria.
- Eventos devem referenciar atletas e times válidos inscritos na partida.
- Limites específicos por modalidade (substituições, tempo de jogo, número de atletas em campo) devem ser aplicados e validados.
- Sistema deve suportar anexos (evidências, fotos, vídeos) vinculados à súmula.

## 10. Mensagens de Erro e Feedback
- "Atleta X não pertence ao time Y." (FA01).
- "Já existe uma súmula aprovada para esta partida. Solicite reabertura para correções." (FA02).
- "Súmula incompleta: faltam dados obrigatórios: [campo1, campo2]." (FA04).

## 11. Segurança, Assinatura e Auditoria
- Registrar `user_id`, `timestamp`, `IP`, `ação` e `versão` a cada submissão/alteração.
- Implementar trilha de auditoria para reaberturas e correções, armazenando justificativa e usuário que aprovou.
- Para garantir integridade, considerar assinatura digital/hash da súmula armazenada (opcional, conforme política institucional).
- Controlar acesso via RBAC (RNF005) e proteger uploads contra XSS/CSRF e content-type spoofing.

## 12. Critérios de Aceitação
- Súmula aceita só se todos os campos obrigatórios estiverem preenchidos e validados.
- Resultado final refletido nas tabelas/rákings corretamente.
- Registro de auditoria e versão criado ao submeter a súmula.
- Processo de reabertura gera entrada de auditoria com justificativa e não apaga histórico anterior.
- Testes automatizados cobrindo registro em tempo real, upload/import e processo de reabertura.

## 13. Casos de Teste Sugeridos
- Registrar súmula de futsal em tempo real: gols, cartões e substituições → resultado correto.
- Importar súmula via CSV com dados corretos → súmula aceita e rankings atualizados.
- Tentar registrar evento com atleta não participante → rejeição e erro claro.
- Tentar submeter súmula incompleta → bloqueio até completar campos obrigatórios.
- Reabrir súmula aprovada, alterar e garantir que a nova versão seja auditada sem apagar a anterior.

---