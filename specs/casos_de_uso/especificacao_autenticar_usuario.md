
# Especificação de Caso de Uso: Autenticar Usuário

## 1. Identificação
- **Identificador**: UC01
- **Nome do Caso de Uso**: Autenticar Usuário
- **Atores Principais**: Administrador, Moderador, Capitão (usuário com cargo)
- **Requisitos Funcionais Associados**: RF001

## 2. Objetivo
Permitir que usuários com cargo façam login no sistema utilizando a matrícula como identificador e senha, verificando vínculo e carregando o contexto da atlética associada ao perfil autenticado.

## 3. Pré-condições
- Conta de usuário criada com matrícula e senha (ou senha padrão igual à matrícula quando aplicável).
- Perfil do usuário contém informação de cargo e vínculo à atlética quando aplicável.
- Serviço de autenticação (hashing/DB) disponível.

## 4. Pós-condições
- Sessão autenticada criada para o usuário com escopo de permissões apropriado (RBAC).
- `last_login` atualizado e auditoria de login gravada (user_id, timestamp, IP, sucesso/fracasso).
- Caso `primeiro_login = true`, usuário é redirecionado para troca obrigatória de senha (RF002).

## 5. Fluxo Principal
1. Usuário abre a tela de login e insere `matricula` e `senha`.
2. Sistema normaliza entrada (trim, case-sensitivity controlada) e busca usuário por `matricula`.
3. Sistema valida a senha comparando hash armazenado com o hash da senha informada.
4. Se credenciais corretas, sistema verifica se o usuário possui cargo atribuído.
   - Se não tiver cargo atribuído, acesso ao dashboard é negado e usuário recebe instrução para solicitar atribuição.
5. Se possuir cargo, o sistema carrega o contexto do usuário (atlética vinculada, nível de permissão) e cria sessão com token seguro.
6. Sistema atualiza `last_login`, registra auditoria de sucesso e redireciona para o dashboard (ou para troca de senha se `primeiro_login=true`).

## 6. Fluxos Alternativos e Exceções
- **[FA01] Credenciais inválidas**: Se matrícula inexistente ou senha incorreta → incrementar contador de tentativas, exibir erro genérico "Matrícula ou senha incorreta".
- **[FA02] Conta bloqueada**: Após X tentativas falhas, bloquear temporariamente a conta e exibir instruções de recuperação.
- **[FA03] Sem cargo atribuído**: Usuário autenticado mas sem cargo → negar acesso ao dashboard e indicar procedimento para atribuição de cargo.
- **[FA04] Primeiro acesso**: Se `primeiro_login = true` → redirecionar para fluxo de troca de senha obrigatório (RF002) antes de liberar o dashboard.
- **[FA05] Sessão inválida/expirada**: Forçar reautenticação quando token expirado ou revogado.
- **[FA06] Erro do serviço de autenticação**: Em caso de falha do banco ou do serviço de hashing, exibir erro amigável e logar o incidente para operações.

## 7. Regras de Negócio / Validações
- Autenticação utiliza `matricula` como identificador único (não nome de usuário).
- Senhas armazenadas com hashing PBKDF2 + salt (conforme RNF004).
- Implementar rate limiting e lockout após N tentativas falhas para prevenir brute-force.
- Aplicar 2FA opcional/obrigatório para cargos administrativos (conforme RNF007).
- Sessões devem usar cookies seguros (`Secure`, `HttpOnly`, `SameSite=strict`) e expirar após período inativo configurável.

## 8. Mensagens de Erro e Feedback
- Mensagem genérica para credenciais inválidas: "Matrícula ou senha incorreta." (não revelar qual dos dois).
- Mensagem para conta bloqueada: "Conta temporariamente bloqueada por excesso de tentativas. Consulte suporte.".
- Mensagem para usuário sem cargo: "Seu perfil não possui cargo atribuído. Solicite atribuição ao administrador.".
- Indicação clara quando redirecionado para troca obrigatória de senha.

## 9. Segurança e Auditoria
- Registrar tentativas de login (sucesso/fracasso) com `user_id` (quando conhecido), `matricula`, `timestamp`, `IP` e `user_agent`.
- Proteger rota de autenticação contra CSRF e validação de input para prevenir injeções.
- Aplicar proteção contra replay e session fixation; regenerar identificador de sessão após login.
- Não gravar nem retornar senhas em claro em logs ou respostas.

## 10. Critérios de Aceitação
- Usuário com matrícula e senha válidas e cargo atribuído consegue acessar o dashboard.
- Usuário sem cargo autenticado recebe mensagem apropriada e não tem acesso ao dashboard.
- Conta é bloqueada após N tentativas falhas e desbloqueio ocorre via procedimento administrativo ou timeout configurado.
- Suporte a 2FA para cargos administrativos está implementado e exigido quando configurado.
- Testes automatizados cobrindo: login válido, login inválido, bloqueio por tentativas, primeiro login redirecionamento e falta de cargo.

## 11. Casos de Teste Sugeridos
- Login válido com cargo atribuído → acesso ao dashboard e `last_login` atualizado.
- Login válido com `primeiro_login = true` → redirecionamento para troca de senha.
- Login com senha incorreta → erro e incremento do contador de tentativas.
- Repetir tentativas falhas até bloqueio → conta bloqueada e recusa de login.
- Usuário sem cargo tenta logar → autenticação pode ocorrer mas acesso ao dashboard é negado com mensagem apropriada.

---