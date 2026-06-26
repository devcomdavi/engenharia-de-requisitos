
# Especificação de Caso de Uso: Validar Vínculo Acadêmico

## 1. Identificação
- **Identificador**: UC17
- **Nome do Caso de Uso**: Validar Vínculo Acadêmico
- **Atores Principais**: Administrador, Moderador, Capitão, SUAP (serviço externo)
- **Requisitos Funcionais Associados**: RF028

## 2. Objetivo
Validar automaticamente o vínculo acadêmico de atletas com o SUAP durante cadastro ou atualização. Se a integração com o SUAP não estiver disponível, aplicar validação via formato institucional da matrícula como fallback.

## 3. Pré-condições
- O usuário realiza cadastro ou atualização de atleta no sistema.
- O sistema tem conexão com a API do SUAP (quando disponível) ou opera em modo fallback de validação por formato.

## 4. Pós-condições
- O vínculo do atleta é marcado como `validado` (via SUAP) ou `validado_fallback` quando aceito por validação local.
- Em caso de falha de validação, o status `vinculo` permanece como `pendente` e a operação requer intervenção manual.
- Todas as tentativas de validação (sucesso/falha) são registradas em auditoria com `user_id`, `matricula`, `resultado`, `timestamp` e `origem` (SUAP|fallback).

## 5. Fluxo Principal — Validação via SUAP
1. Usuário preenche formulário de cadastro/atualização do atleta com `matricula`, `nome`, `email` e demais dados.
2. Sistema detecta que a validação por SUAP é necessária e invoca a API do SUAP com a matrícula e credenciais do sistema.
3. SUAP responde com o status do vínculo e dados do estudante (ex.: `ativo`, curso, campus).
4. Se SUAP confirmar vínculo ativo, o sistema marca o atleta como `validado` e associa os dados retornados (curso, campus).
5. Sistema registra auditoria e exibe confirmação ao usuário.

## 6. Fluxo Alternativo — Fallback por formato de matrícula
1. Se a integração com SUAP estiver indisponível (timeout, erro de autenticação) ou a resposta for inconclusiva, o sistema aplica validação local pelo formato de matrícula institucional (regex) e regras definidas pela instituição.
2. Se a matrícula atender ao formato e não violar regras (ex.: duplicidade), marcar como `validado_fallback` e permitir cadastro provisório.
3. Registrar no log que a validação foi por fallback e sinalizar o registro para posterior reconciliação automática quando SUAP voltar.

## 7. Fluxos Alternativos e Exceções
- **[FA01] Matrícula inválida**: Rejeitar cadastro/atualização e exibir erro "Matrícula inválida".
- **[FA02] Matrícula duplicada**: Rejeitar e informar que a matrícula já existe no sistema.
- **[FA03] SUAP respondendo negativamente**: Se SUAP indicar vínculo inexistente ou inativo, marcar `vinculo = rejeitado` e notificar o usuário com instruções de recurso.
- **[FA04] Erro de integração SUAP**: Logar erro, acionar fallback e notificar administrador sobre indisponibilidade da integração.
- **[FA05] Validação pendente**: Se nem SUAP nem fallback puderem confirmar (dados incompletos), marcar `pendente` e criar uma fila de verificação manual.

## 8. Regras de Negócio / Validações
- Validação primária: consulta ao SUAP com credenciais de integração do sistema.
- Fallback: validação pelo formato da matrícula institucional (ex.: regex e regras de negócio locais).
- Não permitir cadastro de atleta com matrícula duplicada.
- Quando validado por fallback, re-tentar automaticamente a validação por SUAP em janela definida (ex.: a cada 24h) até sucesso ou número limite de tentativas.
- Somente usuários com cargo (Admin/Mod) podem forçar validação manual ou marcar um vínculo como verificado após revisão documental.

## 9. Segurança e Privacidade
- Transmitir apenas os dados mínimos necessários à SUAP e proteger a comunicação via TLS.
- Armazenar logs de integração sem dados sensíveis desnecessários.
- Cumprir RNF003 e RNF004 referentes à privacidade e hash de dados sensíveis.

## 10. Auditoria e Monitoramento
- Gravar cada tentativa de verificação com `user_id`, `matricula`, `resultado`, `mensagem` (mensagem de erro da SUAP quando aplicável), `timestamp`, `origem`.
- Alertas para administradores em caso de falha contínua da integração com SUAP.

## 11. Critérios de Aceitação
- Sucesso: sistema marca atleta como `validado` quando SUAP confirma vínculo.
- Fallback: quando SUAP indisponível, sistema aceita matrícula válida por formato e marca `validado_fallback` com reconciliação agendada.
- Rejeição: matrícula inválida ou duplicada impede criação e produz mensagens claras ao usuário.
- Auditoria: todas as operações de validação são registradas.

## 12. Casos de Teste Sugeridos
- Validação SUAP bem-sucedida: SUAP retorna vínculo ativo → cadastro completado e marcado `validado`.
- SUAP indisponível: fallback por formato aceita matrícula → marcado `validado_fallback` e enfileirado para reconciliação.
- SUAP responde vínculo inexistente → cadastro rejeitado com instruções.
- Matrícula duplicada → operação rejeitada.
- Reconciliação automática: registro `validado_fallback` é re-tentado e atualizado quando SUAP confirma.

---