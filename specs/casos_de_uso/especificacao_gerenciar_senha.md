# Especificação de Caso de Uso: Gerenciar Senha

## 1. Identificação
- **Identificador**: UC02
- **Nome do Caso de Uso**: Gerenciar Senha
- **Atores Principais**: Administrador, Moderador, Capitão (usuário com cargo)
- **Requisitos Funcionais Associados**: RF002, RF003

## 2. Objetivo
Permitir que usuários com cargo alterem sua senha voluntariamente e sejam obrigados a trocá-la no primeiro acesso após receberem um cargo, garantindo requisitos mínimos de segurança e auditoria.

## 3. Pré-condições
- O usuário possui uma conta ativa e credenciais iniciais (senha padrão igual à matrícula quando aplicável).
- O usuário está autenticado para operações de troca voluntária.
- Para primeiro acesso, o flag `primeiro_login = true` está ativo no perfil do usuário.

## 4. Pós-condições
- A nova senha é persistida com hashing seguro (PBKDF2 + salt).
- Se aplicável, o flag `primeiro_login` é removido e o usuário obtém acesso ao dashboard.
- Um registro de auditoria da alteração é gravado (user_id, timestamp, IP, origem).

## 5. Fluxo Principal — Troca obrigatória (primeiro acesso)
1. Usuário realiza login com matrícula e senha padrão.
2. O sistema detecta `primeiro_login = true` e redireciona para a tela de troca de senha.
3. O usuário fornece `nova_senha` e `confirmar_nova_senha` (o campo `senha_atual` pode ser omitido se a senha atual for a padrão).
4. O sistema valida as regras de senha e verifica que `nova_senha != senha_atual`.
5. Se as validações forem aprovadas, o sistema atualiza a senha (hash + salt), remove `primeiro_login` e redireciona o usuário ao dashboard.
6. O sistema registra o evento de alteração em log e mostra confirmação de sucesso.

## 6. Fluxo Principal — Troca voluntária
1. Usuário autenticado acessa a opção "Alterar senha" no menu de perfil.
2. O usuário informa `senha_atual`, `nova_senha` e `confirmar_nova_senha`.
3. O sistema valida `senha_atual` e as regras para `nova_senha`.
4. Em caso de sucesso, o sistema atualiza a senha, registra o evento de auditoria e exibe mensagem de confirmação.

## 7. Fluxos Alternativos e Exceções
- **[FA01] Campos obrigatórios ausentes**: Exibir erro indicando os campos faltantes.
- **[FA02] Confirmação divergente**: `nova_senha` e `confirmar_nova_senha` não coincidem → rejeitar e exibir mensagem.
- **[FA03] Senha atual incorreta**: Ao validar `senha_atual` falha → exibir "Senha atual incorreta".
- **[FA04] Senha viola regras**: `nova_senha` não atende critérios de segurança → listar regras faltantes.
- **[FA05] Nova senha igual à atual**: Bloquear alteração e exibir erro específico.
- **[FA06] Limite de tentativas**: Após N tentativas falhas, aplicar bloqueio temporário e registrar no log.

## 8. Regras de Negócio / Validações
- Senha mínima: 8 caracteres.
- Pelo menos uma letra maiúscula, uma minúscula e um dígito.
- Não pode ser uma senha comum (ver lista de senhas proibidas).
- Não pode ser idêntica à senha atual.
- Novas senhas devem ser passadas por verificador de força e fornecer feedback em tempo real.
- Para contas administrativas, exigir confirmação por 2FA conforme política de segurança (opcional/por perfil).

## 9. Mensagens de Erro e Feedback
- Erros específicos por violação (ex.: "A senha deve ter ao menos 8 caracteres").
- Mensagem clara para senha atual incorreta.
- Indicação visual da força da senha e quais critérios faltam.
- Em caso de sucesso: "Senha atualizada com sucesso." (para primeiro login também incluir: "Acesso liberado ao dashboard").

## 10. Segurança e Auditoria
- Armazenamento de senha com PBKDF2 + salt (conforme RNF004).
- Formularios protegidos contra CSRF.
- Rate limiting para tentativas de alteração e login.
- Todas as alterações devem gerar event log com `user_id`, `timestamp`, `IP` e `origem` (primeiro_login|manual). Não registrar senhas em claro.

## 11. Critérios de Aceitação
- Usuário com `primeiro_login = true` é impedido de acessar o dashboard até trocar a senha.
- Troca voluntária requer validação da `senha_atual` e aplicação das regras de senha.
- Nova senha diferente da atual e persistida com hashing verificável.
- Eventos de alteração são gravados com os campos obrigatórios de auditoria.
- Cobertura de testes automatizados: fluxo de primeiro acesso, troca voluntária, validações de regras e tratamento de erros.

## 12. Casos de Teste Sugeridos
- Primeiro acesso: login com senha padrão → redirecionamento → troca → acesso ao dashboard.
- Troca voluntária: senha atual correta → nova senha válida → sucesso.
- Validação: novas senhas que falham cada regra individual devem retornar mensagens específicas.
- Segurança: repetidas tentativas falhas resultam em bloqueio temporário.

---