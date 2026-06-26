# Especificação de Caso de Uso: Gerar Relatório e Exportação

## 1. Identificação
- **Identificador**: UC14
- **Nome do Caso de Uso**: Gerar Relatório e Exportação
- **Atores Principais**: Administrador, Moderador
- **Requisitos Funcionais Associados**: RF024, RF025

## 2. Descrição
Permite gerar relatórios de participação e exportar dados do sistema (PDF, XLS, CSV).

## 3. Pré-condições
- O usuário deve estar autenticado no sistema (exceto para usuários públicos, onde aplicável).
- O usuário deve possuir as permissões adequadas de acordo com seu cargo (Administrador, Moderador ou Capitão).

## 4. Fluxo Principal
1. O ator acessa o menu principal e seleciona a funcionalidade: Estatísticas.
2. O sistema exibe a interface correspondente para interação (dashboards/paineis) e botão de exportação
3. O ator aciona o botão de exportação com as opções de formato de arquivo.
4. O sistema processa a operação e faz o download do arquivo com as estatísticas.

## 5. Pós-condições
O estado do sistema reflete a operação realizada de forma persistente, preservando a integridade referencial dos dados entre atléticas, times, competições e atletas.

---