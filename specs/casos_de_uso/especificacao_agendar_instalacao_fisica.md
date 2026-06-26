# Especificação de Caso de Uso: Agendar Instalação Física

## 1. Identificação
- **Identificador**: UC13
- **Nome do Caso de Uso**: Agendar Instalação Física
- **Atores Principais**: Administrador, Moderador
- **Requisitos Funcionais Associados**: RF023

## 2. Descrição
Permite o agendamento de espaços (quadras, ginásios) para evitar conflito de horários de partidas.

## 3. Pré-condições
- O usuário deve estar autenticado no sistema (exceto para usuários públicos, onde aplicável).
- O usuário deve possuir as permissões adequadas de acordo com seu cargo (Administrador, Moderador ou Capitão).

## 4. Fluxo Principal
1. O ator acessa o menu principal e seleciona a funcionalidade: Agendar Instalação Física.
2. O sistema exibe a interface correspondente para interação (Formulário/painel).
3. O ator insere os dados pertinentes à operação: Nome do local, data de uso, horário de uso
4. O ator aciona o botão de confirmação.
5. O sistema valida as regras de negócio (Se o nome e o horário estão disponíveis e existem) e os dados informados.
6. O sistema processa a operação e atualiza o banco de dados.
7. O sistema exibe uma notificação de sucesso e atualiza a interface com as novas informações.

## 5. Fluxos Alternativos e de Exceção
editar dados, selecionar dados
- **[FA01] Editar Dados Pertinentes**:
  - O ator pode editar os dados (Nome do Local, data e hora de uso) já fornecidos.
- **[FA02] Excluir Dados Pertinentes**:
 - O ator pode excluir os dados de agendamento (Nome do Local, data e hora de uso) fornecidos anteriormente.
- **[FA03] Dados Inválidos ou Incompletos**:
  - Se, no passo 5, o sistema detectar que faltam dados obrigatórios ou que regras de negócio foram violadas (ex: matrícula repetida, time abaixo do limite, etc.), o sistema interrompe a operação e exibe uma mensagem de erro indicando o campo a ser corrigido.
- **[FA04] Permissão Negada**:
  - Caso o ator tente modificar registros aos quais não possui escopo (ex: Capitão tentando alterar atleta de outra atlética), o sistema bloqueia a ação, retorna um erro de acesso negado e registra a tentativa em log.
- **[FA05] Dependências Ativas (Exclusão)**:
  - Se o ator tentar excluir uma atlética com times ativos ou um time já inscrito em competições, o sistema exibe uma mensagem de alerta e cancela a exclusão, exigindo que as dependências sejam desfeitas primeiro.

## 6. Pós-condições
O estado do sistema reflete a operação realizada de forma persistente, preservando a integridade referencial dos dados entre atléticas, times, competições e atletas.

---

### Diagrama de Atividades Opcional (Mermaid)

```mermaid
flowchart TD
    A[Acessar funcionalidade] --> B[Preencher/Selecionar dados]
    B --> C{Dados válidos?}
    C -- Sim --> D[Processar requisição]
    D --> E[Exibir sucesso e atualizar tela]
    C -- Não --> F[Exibir mensagem de erro]
    F --> B
```
