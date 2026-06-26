# Especificação de Caso de Uso: Gerenciar Competição

## 1. Identificação
- **Identificador**: UC08
- **Nome do Caso de Uso**: Gerenciar Competição
- **Atores Principais**: Administrador, Moderador
- **Requisitos Funcionais Associados**: RF018

## 2. Descrição
Permite criar e gerenciar competições por modalidade esportiva.

## 3. Pré-condições
- O usuário deve estar autenticado no sistema (exceto para usuários públicos, onde aplicável).
- O usuário deve possuir as permissões adequadas de acordo com seu cargo (Administrador, Moderador ou Capitão).

## 4. Fluxo Principal
1. O ator acessa o menu principal e seleciona a funcionalidade: Gerenciar Competição.
2. O sistema exibe a interface correspondente para interação (painel de competições / formulário de cadastro de competição)
3. O ator insere os dados pertinentes à operação: Modalidade e Edição dos Jogos.
4. O ator aciona o botão de confirmação.
5. O sistema valida os dados informados.
6. O sistema processa a operação e atualiza o banco de dados.
7. O sistema exibe uma notificação de sucesso e atualiza sistema com a nova competição pronta para ser populada.

## 5. Fluxos Alternativos e de Exceção
- **[FA01] Dados Inválidos ou Incompletos**:
  - Se, no passo 5, o sistema detectar que faltam dados obrigatórios ou que regras de negócio foram violadas o sistema interrompe a operação e exibe uma mensagem de erro indicando o campo a ser corrigido.

## 6. Pós-condições
O estado do sistema reflete a operação realizada de forma persistente, preservando a integridade referencial dos dados entre atléticas, times, competições e atletas.

---