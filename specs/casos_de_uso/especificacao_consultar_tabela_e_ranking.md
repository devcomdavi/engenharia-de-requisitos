# Especificação de Caso de Uso: Consultar Tabela e Ranking

## 1. Identificação
- **Identificador**: UC11
- **Nome do Caso de Uso**: Consultar Tabela e Ranking
- **Atores Principais**: Administrador, Moderador, Capitão, Usuário Público
- **Requisitos Funcionais Associados**: RF021, RF029

## 2. Descrição
Permite consultar as tabelas de classificação e ranking de desempenho das atléticas.

## 3. Pré-condições
- O usuário deve estar autenticado no sistema (exceto para usuários públicos, onde aplicável).
- O usuário deve possuir as permissões adequadas de acordo com seu cargo (Administrador, Moderador ou Capitão).

## 4. Fluxo Principal
1. O ator acessa a página pública do sistema e seleciona o menu de visualizar Tabela e Ranking.
2. O sistema exibe a interface correspondente para interação (Tabela/Dashboard).

## 5. Pós-condições
O estado do sistema reflete a operação realizada de forma persistente, preservando a integridade referencial dos dados entre atléticas, times, competições e atletas.

---