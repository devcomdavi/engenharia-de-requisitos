# Especificação de Caso de Uso: Gerenciar Time

## 1. Identificação
- **Identificador**: UC05
- **Nome do Caso de Uso**: Gerenciar Time
- **Atores Principais**: Administrador, Moderador, Capitão
- **Requisitos Funcionais Associados**: RF011, RF012, RF013, RF014

## 2. Objetivo
Criar, editar, listar e excluir times por modalidade esportiva e atlética, garantindo consistência das composições e regras da modalidade.

## 3. Pré-condições
- O usuário está autenticado no sistema.
- O usuário possui cargo com permissão para a operação: Administrador/Moderador podem operar em qualquer atlética; Capitão apenas na sua atlética.
- Atletas referenciados já devem estar cadastrados no sistema.

## 4. Pós-condições
- Operação persistida no banco de dados mantendo integridade referencial.
- Em ações de alteração, a composição final do time respeita os limites da modalidade.
- Exclusões liberam vínculos quando permitidas e são registradas em auditoria.

## 5. Fluxos Principais

### 5.1 Criar Time
1. Ator seleciona "Criar Time".
2. Sistema exibe formulário com campos: `nome`, `modalidade`, `atlética`, `lista_de_atletas`.
3. Ator preenche e submete o formulário.
4. Sistema valida:
   - `nome` único por `atlética`+`modalidade`, entre 3 e 100 caracteres;
   - todos os atletas pertencem à mesma `atlética` selecionada;
   - número de atletas está dentro do mínimo e máximo definidos pela modalidade;
   - nenhum atleta já está em outro time da mesma modalidade.
5. Se válido, sistema cria o time e confirma sucesso.

### 5.2 Editar Time (nome e composição)
1. Ator seleciona um time existente e escolhe "Editar".
2. Sistema exibe formulário com dados atuais.
3. Ator altera `nome` (opcional), adiciona ou remove atletas.
4. Sistema revalida as regras do item 5.1.
5. Em sucesso, sistema atualiza o time e confirma.

### 5.3 Listar Times
1. Ator acessa a lista de times, com filtros por `modalidade` e `atlética`.
2. Sistema exibe: `nome`, `modalidade`, `atlética`, `número_de_atletas` e ações disponíveis conforme cargo.

### 5.4 Excluir Time
1. Ator solicita exclusão de um time.
2. Sistema verifica:
   - time não está inscrito em competições ativas;
   - confirmação do usuário (modal de confirmação).
3. Se permitido e confirmado, o sistema exclui o time, libera vínculos e registra auditoria.

## 6. Fluxos Alternativos e Exceções
- **[FA01] Campos inválidos ou ausentes**: Rejeitar submissão e exibir erros por campo.
- **[FA02] Nome duplicado**: Se `nome` já existir para mesma `atlética`+`modalidade`, sugerir alteração.
- **[FA03] Atleta em outra atlética**: Ao tentar adicionar atleta de outra atlética, rejeitar e indicar o atleta problemático.
- **[FA04] Atleta já em outro time da mesma modalidade**: Rejeitar adição e informar qual time já contém o atleta.
- **[FA05] Número fora do permitido**: Se quantidade de atletas < mínimo ou > máximo da modalidade, rejeitar com mensagem específica.
- **[FA06] Permissão negada**: Capitão atuando fora da sua atlética → bloqueio e registro em log.
- **[FA07] Time em competição ativa**: Tentativa de exclusão é impedida e instruções são apresentadas para desinscrição prévia.

## 7. Regras de Negócio
- `nome` do time: único por `atlética`+`modalidade`, 3–100 caracteres.
- Atletas do time devem pertencer à mesma `atlética` do time.
- Cada modalidade define `min` e `max` de atletas (ex.: Futsal min 5 max 12, Vôlei min 6 max 12, Basquete min 5 max 12, Xadrez min 1 max 1).
- Um atleta não pode estar em mais de um time da mesma modalidade.
- Apenas Administrador/Moderador podem manipular times de qualquer atlética; Capitão apenas da sua atlética.
- Exclusão de time requer confirmação e não pode ocorrer se o time participa de competições ativas.

## 8. Mensagens de Erro e Feedback
- Erros por campo com descrições claras (ex.: "O atleta X pertence a outra atlética").
- Ao criar/editar com sucesso: "Time salvo com sucesso.".
- Ao tentar excluir time em competição ativa: "Não é possível excluir time inscrito em competição ativa.".

## 9. Segurança e Auditoria
- Operações críticas gravam logs de auditoria com `user_id`, `ação`, `objeto_id`, `timestamp` e `IP`.
- Aplicar controle de acesso RBAC (RNF005) para todas as ações.

## 10. Critérios de Aceitação
- Criação: validar unicidade de nome, pertencimento dos atletas e limites da modalidade.
- Edição: permitir alterar nome e composição mantendo todas as validações.
- Listagem: filtros por modalidade e atlética com visualização adequada do número de atletas.
- Exclusão: só ocorrer quando time não estiver em competição ativa e após confirmação.
- Testes automatizados cobrindo criação, edição (adicionar/remover atletas), validações e exclusão bloqueada quando aplicável.

## 11. Casos de Teste Sugeridos
- Criar time válido: preencher dados corretos → time criado.
- Criar com nome duplicado: receber erro de unicidade.
- Adicionar atleta de outra atlética: receber erro indicando o atleta.
- Adicionar atleta já alocado em mesma modalidade: erro indicando conflito.
- Remover atleta abaixo do mínimo: operação rejeitada.
- Excluir time em competição ativa: operação impedida.

---