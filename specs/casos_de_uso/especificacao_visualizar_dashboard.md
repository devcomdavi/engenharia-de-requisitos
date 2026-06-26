# Especificação de Caso de Uso: Visualizar Dashboard

## 1. Identificação
- **Identificador**: UC07
- **Nome do Caso de Uso**: Visualizar Dashboard
- **Atores Principais**: Administrador, Moderador, Capitão
- **Requisitos Funcionais Associados**: RF017

## 2. Descrição
Exibe informações e estatísticas relevantes ao nível de permissão do usuário.

## 3. Pré-condições
- O usuário deve estar autenticado no sistema (exceto para usuários públicos, onde aplicável).
- O usuário deve possuir as permissões adequadas de acordo com seu cargo (Administrador, Moderador ou Capitão).

## 4. Fluxo Principal
1. O ator acessa o menu principal e seleciona a funcionalidade desejada.
2. O sistema exibe a interface correspondente para interação (formulário, listagem ou painel).
3. O ator insere, edita ou seleciona os dados pertinentes à operação.
4. O ator aciona o botão de confirmação.
5. O sistema valida as regras de negócio e os dados informados.
6. O sistema processa a operação e atualiza o banco de dados.
7. O sistema exibe uma notificação de sucesso e atualiza a interface com as novas informações.

## 5. Fluxos Alternativos e de Exceção
- **[FA01] Dados Inválidos ou Incompletos**:
  - Se, no passo 5, o sistema detectar que faltam dados obrigatórios ou que regras de negócio foram violadas (ex: matrícula repetida, time abaixo do limite, etc.), o sistema interrompe a operação e exibe uma mensagem de erro indicando o campo a ser corrigido.
- **[FA02] Permissão Negada**:
  - Caso o ator tente modificar registros aos quais não possui escopo (ex: Capitão tentando alterar atleta de outra atlética), o sistema bloqueia a ação, retorna um erro de acesso negado e registra a tentativa em log.
- **[FA03] Dependências Ativas (Exclusão)**:
  - Se o ator tentar excluir uma atlética com times ativos ou um time já inscrito em competições, o sistema exibe uma mensagem de alerta e cancela a exclusão, exigindo que as dependências sejam desfeitas primeiro.

## 6. Pós-condições
O estado do sistema reflete a operação realizada de forma persistente, preservando a integridade referencial dos dados entre atléticas, times, competições e atletas.

---