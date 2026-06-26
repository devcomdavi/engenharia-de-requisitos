
# Especificação de Caso de Uso: Emitir Certificado

## 1. Identificação
- **Identificador**: UC15
- **Nome do Caso de Uso**: Emitir Certificado
- **Atores Principais**: Administrador, Moderador
- **Requisitos Funcionais Associados**: RF026

## 2. Objetivo
Gerar, validar e emitir certificados de participação (incluindo horas complementares) para atletas participantes, com exportação em PDF assinado digitalmente quando aplicável e envio por e-mail institucional.

## 3. Pré-condições
- Partida/competição e participação do atleta devidamente registrada e homologada (súmula aprovada).
- Atleta possui vínculo acadêmico validado e e-mail institucional cadastrado.
- Template de certificado configurado (modelo, campos dinâmicos, assinatura/instituição).

## 4. Pós-condições
- Certificados gerados e armazenados (PDF) com metadados: `atleta_id`, `competicao_id`, `horas`, `timestamp`, `assinatura` (se aplicável).
- Notificações de envio registradas; entregas e falhas logadas.
- Auditoria de emissão com `user_id` que acionou a geração, lista de destinatários, e hash/assinatura dos PDFs.

## 5. Modelos e Campos do Certificado
- Campos dinâmicos: `nome_completo`, `matricula`, `atlética`, `competicao`, `data_evento`, `horas_complementares`, `descrição`.
- Template deve suportar variações por tipo de evento (participação, organização, comissão técnica) e idioma (pt-BR padrão).
- Opção de inclusão de QR code contendo link para verificação/validação online do certificado.

## 6. Fluxo Principal — Emissão em Lote (Administrativo)
1. Administrador/Moderador navega até a funcionalidade "Emitir Certificados" para uma competição/edição.
2. Sistema exibe lista de participantes elegíveis (com participações validadas) e configurações (template, horas a atribuir, assinatura digital, incluir QR code).
3. Ator seleciona participantes ou escolhe "Selecionar todos" e confirma emissão.
4. Sistema gera PDFs conforme template, aplica assinatura digital/hash (quando configurado), grava arquivos no repositório e cria registros de emissão.
5. Sistema envia e-mail institucional para cada atleta com o PDF anexado e link de verificação; registra status de entrega.
6. Sistema exibe resumo de emissão com contagem de sucesso/falhas e detalhes das falhas.

## 7. Fluxo Alternativo — Emissão Individual
1. Usuário (Admin/Mod) ou atleta (quando permitido) solicita emissão individual via perfil do atleta.
2. Sistema valida elegibilidade e gera certificado para o atleta solicitado.

## 8. Fluxos Alternativos e Exceções
- **[FA01] Atleta sem vínculo validado**: O sistema não gera certificado e marca o registro como `pendente`; notifica administrador para revisão documental.
- **[FA02] E-mail inválido/entrega falhada**: Registrar falha e permitir reenvio manual ou exportação para download.
- **[FA03] Template ausente/inválido**: Bloquear emissão até corrigir template; indicar erro ao usuário.
- **[FA04] Participação não homologada**: Impedir emissão até homologação da súmula.
- **[FA05] Assinatura digital indisponível**: Gerar certificado sem assinatura, marcar como `sem_assinatura` e registrar log para reprocessamento quando serviço voltar.

## 9. Regras de Negócio
- Só emitir certificados para participantes com presença/participação comprovada conforme regras da competição.
- As horas complementares atribuídas devem seguir política institucional e ser configuráveis por tipo de participação.
- Cada certificado possui ID único e código de verificação (ex.: UUID + QR code) para permitir validação pública.
- Emissões em lote devem ser compatíveis com limites de envio por minuto/hora para respeitar provedores de e-mail.

## 10. Mensagens de Erro e Feedback
- "Nenhum participante elegível encontrado." quando lista vazia.
- "Emissão parcial: X certificados enviados, Y falhas." com link para detalhes das falhas.
- Erros claros para problemas de template, assinatura ou validação de vínculo.

## 11. Segurança, Privacidade e Auditoria
- Proteger geração e armazenamento de certificados (acesso controlado, criptografia em repouso opcional).
- Não expor dados sensíveis no certificado sem consentimento.
- Auditoria completa contendo `user_id` que gerou, `lista_atletas`, `competicao_id`, `timestamp`, IP e hash/assinatura dos arquivos gerados.

## 12. Critérios de Aceitação
- Certificados gerados para participantes válidos contendo campos dinâmicos corretos.
- PDFs armazenados com metadados e código de verificação que permitem validação pública.
- Envio por e-mail registrado com estatísticas de entrega e possibilidade de reenvio manual.
- Testes automatizados cobrindo geração de PDF, aplicação de QR code, assinatura digital/ fallback e tratamento de falhas.

## 13. Casos de Teste Sugeridos
- Emissão em lote com 100 participantes: verificar geração, anexos e entregas (simuladas) e relatório de falhas.
- Emissão para atleta sem vínculo validado: operação deve falhar e criar registro pendente.
- Simular falha do serviço de assinatura: gerar PDFs sem assinatura e registrar para reprocessamento.
- Verificar QR code: escaneamento direciona para página de validação com dados do certificado.

---