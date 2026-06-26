
# Especificação de Caso de Uso: Gerenciar Cargo Administrativo

## 1. Identificação
- **Identificador**: UC06
- **Nome do Caso de Uso**: Gerenciar Cargo Administrativo
- **Atores Principais**: Administrador, Moderador
- **Requisitos Funcionais Associados**: RF015, RF016

## 2. Objetivo
Permitir a atribuição e remoção de cargos administrativos (`Capitão`, `Moderador`) a atletas cadastrados, gerando perfis de usuário quando necessário, respeitando regras de elegibilidade e garantindo rastreabilidade e segurança.

## 3. Pré-condições
- Usuário executando a operação deve estar autenticado e ter permissão (`Administrador` ou `Moderador` conforme operação).
- Atleta alvo deve estar cadastrado no sistema.
- Para atribuir `Capitão`, a atlética alvo deve existir e não possuir capitão responsavel.

## 4. Pós-condições
- Cargo atribuído ou removido com persistência no sistema.
- Ao atribuir cargo: se não existir perfil de usuário, criar perfil com e-mail válido, senha padrão igual à matrícula e marcar `primeiro_login = true`.
- Ao remover cargo: excluir perfil de usuário associado (ou desativar), liberar vínculo e manter registro do atleta.
- Todas as operações geram registro de auditoria (`user_id`, `target_atleta_id`, `cargo`, `ação`, `timestamp`, `IP`).

## 5. Fluxos Principais

### 5.1 Atribuir Capitão
1. Ator (Admin ou Moderador) seleciona "Atribuir Capitão" para uma atlética.
2. Sistema exibe lista de atletas elegíveis (sem cargo) ou busca por matrícula.
3. Ator seleciona atleta e confirma atribuição.
4. Sistema valida:
  - ator tem permissão para atribuir Capitão (Admin/Mod);
  - atleta não possui cargo existente;
  - atlética não possui capitão responsável;
  - atleta possui matrícula válida e e-mail institucional.
5. Se válido, sistema atribui o cargo `Capitão`, cria perfil de usuário se necessário (senha padrão = matrícula), ativa `primeiro_login` e envia notificação por e-mail com instruções de primeiro acesso.
6. Sistema registra auditoria e exibe confirmação.

### 5.2 Atribuir Moderador
1. Ator (Administrador) seleciona "Atribuir Moderador".
2. Sistema lista atletas elegíveis (sem cargo) ou permite busca por matrícula.
3. Ator seleciona e confirma.
4. Sistema valida:
  - ator é Administrador;
  - atleta não possui cargo existente;
  - atleta possui e-mail institucional válido.
5. Se válido, sistema cria/atribui cargo `Moderador`, cria perfil de usuário se necessário (senha padrão = matrícula), ativa `primeiro_login` e envia notificação por e-mail.
6. Sistema registra auditoria e exibe confirmação.

### 5.3 Remover Cargo Administrativo
1. Ator (Administrador para qualquer cargo; Moderador pode solicitar remoção de Capitão conforme política) seleciona usuário com cargo e escolhe "Remover Cargo".
2. Sistema valida permissões e solicita confirmação.
3. Sistema remove o cargo do usuário: desativa ou exclui o perfil de usuário, libera vínculo com a atlética (se Capitão) e registra auditoria.
4. Sistema exibe confirmação e notifica as partes afetadas.

## 6. Fluxos Alternativos e Exceções
- **[FA01] Atleta já possui cargo**: Rejeitar atribuição e sugerir remoção prévia do cargo existente.
- **[FA02] Atlética já tem Capitão**: Rejeitar atribuição e indicar capitão atual; oferecer opção de transferência mediante processo adicional.
- **[FA03] E-mail inválido ou ausente**: Exigir correção do cadastro antes de criar perfil; permitir provisoriamente atribuição sem criação de perfil (marcar para ação posterior).
- **[FA04] Falha na criação de perfil**: Se falha no serviço de criação (e-mail/senha), reverter atribuição e registrar erro para suporte.
- **[FA05] Permissão negada**: Se ator não tiver permissão para a operação, bloquear e registrar tentativa.

## 7. Regras de Negócio
- `Capitão` pode ser atribuído por Administrador e Moderador, e somente se a atlética estiver sem capitão.
- `Moderador` pode ser atribuído apenas por Administrador.
- Ao atribuir cargo, gerar perfil de usuário com senha padrão igual à matrícula e `primeiro_login=true`.
- Um atleta não pode possuir mais de um cargo administrativo ao mesmo tempo.
- Remoção de cargo deve liberar vínculos e permitir nova atribuição posterior.

## 8. Mensagens de Erro e Feedback
- "Atleta já possui um cargo administrativo." (FA01).
- "Atlética já possui Capitão: [nome do capitão atual]." (FA02).
- "E-mail institucional inválido ou ausente — corrija o cadastro antes de prosseguir." (FA03).
- Mensagem de sucesso: "Cargo atribuído com sucesso." / "Cargo removido com sucesso.".

## 9. Segurança e Auditoria
- Todas as ações críticas gravam auditoria com `user_id`, `target_atleta_id`, `cargo`, `ação`, `timestamp`, `IP` e `motivo` quando aplicável.
- Aplicar RBAC (RNF005): apenas perfis autorizados podem atribuir ou remover cargos.
- Proteger endpoints contra CSRF e validar inputs para prevenir injeção.

## 10. Critérios de Aceitação
- Atribuição de Capitão/Moderador só ocorre quando as regras de elegibilidade são atendidas.
- Perfil de usuário é criado com senha padrão igual à matrícula e `primeiro_login` ativado quando aplicável.
- Remoção de cargo libera vínculo e não remove o registro do atleta.
- Auditoria completa presente para todas as operações de atribuição e remoção.
- Testes automatizados cobrindo atribuição, remoção, conflitos de cargo e falhas na criação de perfil.

## 11. Casos de Teste Sugeridos
- Atribuir Capitão a atlética sem capitão → perfil gerado e e-mail enviado.
- Atribuir Moderador por usuário não-Admin → operação negada.
- Tentar atribuir cargo a atleta que já possui cargo → operação rejeitada.
- Remover cargo e verificar que atlética ficou sem capitão e perfil do usuário foi desativado.

---