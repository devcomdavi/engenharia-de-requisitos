# Especificação de Caso de Uso: Gerenciar Atleta

## 1. Identificação
- **Identificador**: UC04
- **Nome do Caso de Uso**: Gerenciar Atleta
- **Atores Principais**: Administrador, Moderador, Capitão
- **Requisitos Funcionais Associados**: RF008, RF009, RF010

## 2. Descrição
Permite o cadastro, edição de informações cadastrais e transferência de atletas entre atléticas.

## 3. Pré-condições
- O usuário deve estar autenticado no sistema (exceto para usuários públicos, onde aplicável).
- O usuário deve possuir as permissões adequadas de acordo com seu cargo (Administrador, Moderador ou Capitão).

## 4. Fluxo Principal
1. O ator acessa o menu principal e seleciona a funcionalidade Gerenciar Atleta.
2. O sistema exibe a interface correspondente para interação (Painel/Formulário).
3. O ator insere os dados pertinentes à operação: matrícula (6 a 10 dígitos numéricos, única no sistema), nome completo, data de nascimento (idade entre 14 e 100 anos), sexo e atlética.
4. O ator aciona o botão de confirmação.
5. O sistema valida as regras de negócio e os dados informados.
6. O sistema processa a operação e atualiza o banco de dados.
7. O sistema exibe uma notificação de sucesso e atualiza o sistema com as novas informações.

## 5. Fluxos Alternativos e de Exceção
edita, exclui
- **[FA01] Editar Dados**:
  - O ator pode editar os dados pertinentes à operação: matrícula (6 a 10 dígitos numéricos, única no sistema), nome completo, data de nascimento (idade entre 14 e 100 anos), sexo e atlética.
- **[FA02] Excluir Dados**:
  - O ator pode excluir os dados pertinentes à operação: matrícula (6 a 10 dígitos numéricos, única no sistema), nome completo, data de nascimento (idade entre 14 e 100 anos), sexo e atlética.
- **[FA03] Dados Inválidos ou Incompletos**:
  - Se, no passo 5, o sistema detectar que faltam dados obrigatórios ou que regras de negócio foram violadas (ex: matrícula repetida, time abaixo do limite, etc.), o sistema interrompe a operação e exibe uma mensagem de erro indicando o campo a ser corrigido.
- **[FA04] Permissão Negada**:
  - Caso o ator tente modificar registros aos quais não possui escopo (ex: Capitão tentando alterar atleta de outra atlética), o sistema bloqueia a ação, retorna um erro de acesso negado e registra a tentativa em log.
- **[FA05] Dependências Ativas (Exclusão)**:
  - Se o ator tentar excluir uma atlética com times ativos ou um time já inscrito em competições, o sistema exibe uma mensagem de alerta e cancela a exclusão, exigindo que as dependências sejam desfeitas primeiro.

## 6. Pós-condições
O estado do sistema reflete a operação realizada de forma persistente, preservando a integridade referencial dos dados entre atléticas, times, competições e atletas.

---