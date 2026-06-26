# Especificação de Caso de Uso: Gerenciar Edição dos Jogos

## 1. Identificação
- **Identificador**: UC12
- **Nome do Caso de Uso**: Gerenciar Edição dos Jogos
- **Atores Principais**: Administrador
- **Requisitos Funcionais Associados**: RF022

## 2. Descrição
Permite gerenciar diferentes edições dos jogos, preservando o histórico.

## 3. Pré-condições
- O usuário deve estar autenticado no sistema (exceto para usuários públicos, onde aplicável).
- O usuário deve possuir as permissões adequadas de acordo com seu cargo (Administrador, Moderador ou Capitão).

## 4. Fluxo Principal
1. O ator acessa o menu principal e seleciona a funcionalidade: Gerenciar Competição.
2. O sistema exibe a interface correspondente para interação (painel de competições / formulário de cadastro de competição).
3. O ator seleciona uma competição já cadastrada.
4. O ator visualiza o histórico da competição, com estatísticas e resultados.

## 5. Pós-condições
O estado do sistema reflete a operação realizada de forma persistente, preservando a integridade referencial dos dados entre atléticas, times, competições e atletas.

---