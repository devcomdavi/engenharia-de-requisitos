
# Especificação de Caso de Uso: Gerenciar Atlética

## 1. Identificação
- **Identificador**: UC03
- **Nome do Caso de Uso**: Gerenciar Atlética
- **Atores Principais**: Administrador, Moderador, Capitão
- **Requisitos Funcionais Associados**: RF004, RF005, RF006, RF007

## 2. Objetivo
Cadastrar, editar, listar, atribuir responsável e excluir atléticas, garantindo unicidade de nome, consistência de vínculos e regras de integridade para exclusão.

## 3. Pré-condições
- Usuário autenticado com permissão adequada (Administrador/Moderador para todas as operações; Capitão apenas visualização/edição limitada à sua atlética).
- Para atribuição de Capitão, o usuário indicado deve existir como atleta cadastrado e não possuir cargo anterior incompatível.

## 4. Pós-condições
- Atlética criada/atualizada/excluída conforme a operação, mantendo integridade referencial.
- Ao atribuir Capitão, vínculo entre atleta e cargo é criado, perfil de usuário gerado (quando aplicável) e `primeiro_login` ativado.
- Exclusão somente ocorre se não houver atletas cadastrados nem times criados; caso contrário, operação é impedida.

## 5. Fluxos Principais

### 5.1 Criar Atlética
1. Ator seleciona "Criar Atlética".
2. Sistema exibe formulário com campos: `nome`, `campus`, `curso`, `responsavel_inicial` (opcional).
3. Ator preenche e submete.
4. Sistema valida: `nome` único (3–255 caracteres), `campus` e `curso` válidos.
5. Se válido, sistema cria a atlética; se fornecido `responsavel_inicial`, valida e vincula o Capitão (criando perfil de usuário e marcando `primeiro_login`).
6. Sistema confirma criação e exibe a nova atlética na listagem.

### 5.2 Editar Atlética
1. Ator seleciona uma atlética e escolhe "Editar".
2. Sistema exibe formulário com dados atuais (nome editável conforme regras).
3. Ator altera campos permitidos e submete.
4. Sistema valida unicidade de `nome` e outras regras; em sucesso atualiza registro e grava auditoria.

### 5.3 Atribuir/Remover Capitão
1. Administrador/Moderador escolhe "Atribuir Capitão" para uma atlética.
2. Sistema apresenta lista de atletas elegíveis (sem cargo) ou permite busca por matrícula.
3. Ao selecionar atleta, sistema verifica elegibilidade e, se aprovado, atribui o cargo, gera perfil de usuário com senha padrão igual à matrícula e ativa `primeiro_login`.
4. Remoção de cargo segue processo reverso e libera a atlética para novo responsável.

### 5.4 Listar e Visualizar Atléticas
1. Usuário acessa listagem de atléticas.
2. Administrador/Moderador visualizam todas; Capitão visualiza apenas sua atlética.
3. Cada item exibe: `nome`, `campus`, `capitão_responsavel`, `n_atletas`, `n_times`, e ações permitidas conforme cargo.

### 5.5 Excluir Atlética
1. Ator solicita exclusão de uma atlética.
2. Sistema verifica pré-condições: nenhum atleta cadastrado e nenhum time criado vinculados à atlética.
3. Se condição atendida, solicitar confirmação (modal) e, após confirmação, excluir (soft delete recomendado) e registrar auditoria.
4. Se houver dependências, bloquear exclusão e exibir instruções para remoção das dependências.

## 6. Fluxos Alternativos e Exceções
- **[FA01] Nome duplicado**: Rejeitar criação/edição e exibir "Nome já cadastrado".
- **[FA02] Campos obrigatórios ausentes**: Exibir erros por campo.
- **[FA03] Capitão inválido**: Ao atribuir, se atleta possuir cargo ou vínculo incompatível, rejeitar e indicar motivo.
- **[FA04] Exclusão impedida por dependências**: Informar número de atletas e times que impedem exclusão.
- **[FA05] Permissão negada**: Capitão tentando editar outra atlética → bloquear e registrar tentativa.

## 7. Regras de Negócio
- `nome` da atlética: único no sistema, entre 3 e 255 caracteres.
- Cada Capitão pode ser responsável por apenas uma atlética.
- Ao atribuir cargo, criar perfil de usuário com e-mail válido e senha padrão igual à matrícula; ativar `primeiro_login`.
- Exclusão é permitida apenas quando não existirem atletas cadastrados nem times criados; usar exclusão lógica para preservar histórico quando aplicável.

## 8. Mensagens de Erro e Feedback
- "Nome da atlética já existe." (FA01).
- "Operação bloqueada: existem N atletas e M times vinculados à atlética." (FA04).
- Confirmação clara em operações destrutivas: "Confirma exclusão da atlética X? Esta ação não pode ser desfeita.".

## 9. Segurança e Auditoria
- Registrar todas as operações com `user_id`, `ação`, `atletica_id`, `timestamp` e `IP`.
- Aplicar controle de acesso RBAC (RNF005) para criação, edição, atribuição e exclusão.

## 10. Critérios de Aceitação
- Criar atlética com nome único e dados válidos resulta em novo registro visível na listagem.
- Atribuição de Capitão cria perfil e ativa `primeiro_login`.
- Edição respeita regras de unicidade e permissões.
- Exclusão somente ocorre sem dependências e registra auditoria.
- Testes automatizados cobrindo criação, edição, atribuição de cargo, listagem e exclusão bloqueada por dependências.

## 11. Casos de Teste Sugeridos
- Criar atlética com nome válido → sucesso e visibilidade na listagem.
- Tentar criar com nome duplicado → erro de unicidade.
- Atribuir capitão a uma atlética → perfil criado e `primeiro_login` ativado.
- Tentativa de exclusão com atletas ou times vinculados → operação bloqueada com mensagem clara.

---