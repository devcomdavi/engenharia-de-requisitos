
# Especificação de Caso de Uso: Gerenciar Atleta

## 1. Identificação
- **Identificador**: UC04
- **Nome do Caso de Uso**: Gerenciar Atleta
- **Atores Principais**: Administrador, Moderador, Capitão
- **Requisitos Funcionais Associados**: RF008, RF009, RF010

## 2. Objetivo
Permitir cadastro, edição e transferência de atletas entre atléticas, garantindo validações de integridade (idade, matrícula única) e regras de negócio sobre transferências e participação em times.

## 3. Pré-condições
- Usuário autenticado com papel e permissões adequadas (Admin/Mod para qualquer atlética; Capitão apenas para sua atlética).
- Para transferência, atleta não deve estar em times ativos que impeçam a operação.

## 4. Pós-condições
- Dados do atleta persistidos com integridade referencial.
- Ao transferir atleta, vínculos a times ativos são recusados até que sejam resolvidos; histórico de transferências registrado.
- Auditoria criada para operações críticas (`user_id`, `atleta_id`, ação, timestamp, IP).

## 5. Fluxos Principais

### 5.1 Cadastrar Atleta
1. Ator seleciona "Cadastrar Atleta".
2. Sistema exibe formulário com campos obrigatórios: `matricula`, `nome_completo`, `data_nascimento`, `sexo`, `atlética`.
3. Ator preenche e submete.
4. Sistema valida:
   - `matricula`: 6–10 dígitos numéricos e única no sistema;
   - `data_nascimento`: calcula idade entre 14 e 100 anos;
   - campos obrigatórios preenchidos;
   - matrícula não duplicada.
5. Se válido, sistema cria registro do atleta, marca `vinculo_academico` como pendente (até validação) e exibe confirmação.

### 5.2 Editar Atleta
1. Ator seleciona atleta e escolhe "Editar".
2. Sistema exibe formulário com dados atuais (matrícula somente editável por Admin/Mod mediante justificativa).
3. Ator altera campos permitidos e submete.
4. Sistema revalida regras e atualiza o registro.

### 5.3 Transferir Atleta entre Atléticas
1. Ator (Admin/Mod) inicia processo de transferência selecionando atleta e nova `atlética`.
2. Sistema verifica que o atleta não esteja em times ativos ou, se estiver, exige confirmação de remoção/transferência dos times.
3. Sistema atualiza vínculo da atlética, registra histórico da transferência e notifica os capitães/administradores impactados.

### 5.4 Excluir Atleta
1. Ator solicita exclusão de atleta.
2. Sistema verifica dependências (participação em competições homologadas, histórico, times ativos).
3. Se houver participação em competições homologadas, operação é rejeitada (conforme RNF020). Caso contrário, o sistema realiza exclusão lógica (soft delete) e registra auditoria.

## 6. Fluxos Alternativos e Exceções
- **[FA01] Matrícula duplicada**: Rejeitar cadastro/edição e exibir "Matrícula já cadastrada".
- **[FA02] Idade fora do intervalo**: Rejeitar e indicar o intervalo permitido (14–100 anos).
- **[FA03] Campos ausentes**: Exibir erros por campo obrigatório.
- **[FA04] Transferência bloqueada por vínculo em time ativo**: Informar quais vínculos impedem a transferência e fornecer opções (remover atleta do time, aguardar fim de participação).
- **[FA05] Exclusão proibida por histórico de competições**: Bloquear exclusão e instruir sobre processo de anonimização/arquivamento se necessário.
- **[FA06] Permissão negada**: Capitão tentando operar fora da sua atlética → bloquear e registrar tentativa.

## 7. Regras de Negócio
- Matrícula: 6–10 dígitos, única.
- Idade: entre 14 e 100 anos.
- Somente Administrador/Moderador podem alterar matrícula e forçar transferências; Capitão pode cadastrar/editar atletas apenas na sua atlética.
- Transferência só é permitida se o atleta não participar de times ativos ou mediante remoção prévia desses vínculos.
- Exclusão física de atleta não é permitida quando houver participação em competições homologadas; usar exclusão lógica.

## 8. Mensagens de Erro e Feedback
- "Matrícula inválida ou já cadastrada." (FA01).
- "Idade inválida: o atleta deve ter entre 14 e 100 anos." (FA02).
- "Transferência bloqueada: atleta participa de time ativo X." (FA04).
- "Exclusão não permitida para atletas com participação em competições homologadas." (FA05).

## 9. Segurança e Auditoria
- Registrar todas as operações sensíveis (criação, edição, transferência, exclusão) com `user_id`, `ação`, `atleta_id`, `timestamp` e `IP`.
- Aplicar RBAC (RNF005) para operações administrativas.

## 10. Critérios de Aceitação
- Cadastro válido cria atleta com todos os campos e marcação de vínculo para validação.
- Edição respeita permissões e validações de campo.
- Transferência atualiza vínculo apenas quando regras de negócio são atendidas e registra histórico.
- Exclusão lógica preserva histórico de competições e não permite remoção de atletas que participaram de competições homologadas.
- Testes automatizados cobrindo cadastro, edição, transferência, exclusão e tratamentos de erro.

## 11. Casos de Teste Sugeridos
- Criar atleta com dados válidos → registro criado e visível na atlética.
- Tentar criar com matrícula duplicada → falha com mensagem apropriada.
- Editar atleta como Capitão da mesma atlética → sucesso.
- Transferir atleta com vínculo em time ativo → bloqueio e mensagem indicando o time.
- Tentar excluir atleta com participação homologada → operação negada.

---