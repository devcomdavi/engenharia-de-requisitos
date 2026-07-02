# Visão Lógica do Sistema (SGDU)

Este documento apresenta a **Visão Lógica** do **Sistema de Gerenciamento Desportivo Universitário (SGDU)**. Ele foi desenvolvido com base nas diretrizes e heurísticas de projeto disponibilizadas pelo professor em seu [website de Visão Lógica](https://maxwellamaral.github.io/lessons/softeng/design/views_logic/), aplicando-as sistematicamente sobre os requisitos contivos em [requisitos_SGDU.md](file:///home/ian/Faculdade/APS/engenharia-de-requisitos/requisitos_SGDU.md) e nos Casos de Uso contidos na pasta [specs/casos_de_uso](file:///home/ian/Faculdade/APS/engenharia-de-requisitos/specs/casos_de_uso).

---

## 1. Introdução

A visão lógica exibe a estrutura do sistema e como seus componentes interagem entre si, demonstrando **como** as funcionalidades descritas nos requisitos e casos de uso são projetadas e promovidas. 

Este documento divide-se em:
* **Estrutura Estática**: Representada por um Diagrama de Classes robusto que modela o domínio do SGDU.
* **Comportamento Dinâmico**: Detalhado por meio de Diagramas de Atividades para métodos/operações chave das classes e um Diagrama de Sequência de interação de objetos para um fluxo de negócio crítico.
* **Heurísticas de Extração**: Demonstração do mapeamento de substantivos (classes/atributos) e verbos (métodos/operações) extraídos das especificações de casos de uso.

---

## 2. Heurística para Extração de Classes e Métodos

Conforme a metodologia do professor, a identificação das classes e métodos deve ser extraída de forma natural a partir das especificações de casos de uso. A seguir, demonstra-se a aplicação da heurística em casos de uso centrais do sistema.

### 2.1 Extração de Classes e Atributos (Substantivos)

Foram analisadas as especificações de [Gerenciar Atlética (UC03)](file:///home/ian/Faculdade/APS/engenharia-de-requisitos/specs/casos_de_uso/especificacao_gerenciar_atletica.md), [Gerenciar Atleta (UC04)](file:///home/ian/Faculdade/APS/engenharia-de-requisitos/specs/casos_de_uso/especificacao_gerenciar_atleta.md) e [Gerenciar Time (UC05)](file:///home/ian/Faculdade/APS/engenharia-de-requisitos/specs/casos_de_uso/especificacao_gerenciar_time.md) para extrair os substantivos candidatos.

| Substantivo | Classificação | Justificativa |
| :--- | :--- | :--- |
| **Atlética** | Classe | Entidade do domínio. Possui ações como criar, editar, listar e excluir. |
| **nome** (da atlética) | Atributo de `Atlética` | Característica identificadora da atlética. Única e obrigatória. |
| **campus** | Atributo de `Atlética` | Característica física associada à atlética. |
| **curso** | Atributo de `Atlética` | Vínculo acadêmico de representação da atlética. |
| **Atleta** | Classe | Entidade que representa os estudantes competidores no sistema. |
| **matrícula** | Atributo de `Atleta` / `Usuario` | Identificador único numérico (6 a 10 dígitos). Usado também para autenticação. |
| **nome completo** | Atributo de `Atleta` | Dado cadastral do competidor. |
| **data de nascimento** | Atributo de `Atleta` | Usado para validação de idade (14 a 100 anos). |
| **sexo** | Atributo de `Atleta` | Dado biológico para regras de categorização esportiva. |
| **vínculo acadêmico** | Atributo de `Atleta` | Estado de validação do aluno junto ao SUAP (Pendente, Validado, Fallback, etc.). |
| **Time** | Classe | Grupo de atletas que disputam modalidades específicas. |
| **nome** (do time) | Atributo de `Time` | Nome identificador do time (único por modalidade + atlética). |
| **Modalidade** | Classe / Enum | Tipo de esporte (Futsal, Vôlei, etc.) com regras de limites de atletas. |
| **Usuário** | Classe | Entidade de acesso e segurança, contendo hash de senha, e-mail e cargo. |
| **cargo** | Atributo de `Usuario` | Nível de permissão (Administrador, Moderador ou Capitão). |
| **Instalação Física** | Classe | Local físico do campus (quadra, campo, etc.) agendável. |
| **Agendamento** | Classe | Registro de reserva de uma instalação física para partidas/treinos. |
| **Súmula** | Classe | Registro oficial e digital dos eventos e placar de uma partida. |
| **Certificado** | Classe | Documento de participação digital emitido para horas complementares. |

### 2.2 Extração de Métodos e Operações (Verbos)

Para identificar os comportamentos, os verbos presentes nas especificações de caso de uso foram listados e classificados:

| Verbo Extraído | Tipo | Operação Atribuída | Classe Destino |
| :--- | :--- | :--- | :--- |
| **Criar / Cadastrar** Atlética | Operação | `criar_atletica()` | `Atlética` |
| **Editar / Atualizar** Atlética | Operação | `editar_atletica()` | `Atlética` |
| **Excluir / Remover** Atlética | Operação | `excluir_atletica()` | `Atlética` |
| **Listar / Buscar** Atlética | Operação | `listar_atleticas()` | `Atlética` |
| **Cadastrar** Atleta | Operação | `cadastrar_atleta()` | `Atleta` |
| **Transferir** Atleta | Operação | `transferir_atleta(nova_atletica)` | `Atleta` |
| **Validar vínculo** com SUAP | Operação | `validar_vinculo_academico()` | `Atleta` |
| **Autenticar** Usuário | Operação | `autenticar()` | `Usuario` |
| **Alterar** Senha | Operação | `alterar_senha(nova_senha)` | `Usuario` |
| **Inscrever** Time | Operação | `inscrever_time()` | `Inscrição` |
| **Registrar / Iniciar** Súmula | Operação | `iniciar_sumula()` / `registrar_evento()` | `Súmula` |
| **Agendar** Instalação | Operação | `criar_agendamento()` | `Agendamento` |
| **Emitir** Certificado | Operação | `gerar_pdf()` / `enviar_por_email()` | `Certificado` |
| *Preencher formulário* | Descartar | - (Ação de interface/UI) | - |
| *Confirmar exclusão* | Redundante | - (Validado na própria exclusão) | - |
| *Verificar se cadastrou* | Redundante | - (Retorno booleano/exceção) | - |

---

## 3. Modelo de Estrutura Estática: Diagrama de Classes

Abaixo está o diagrama de classes completo do SGDU, modelado com tipagem adequada para persistência em PostgreSQL e backend Django (Python 3.8+), conforme os requisitos técnicos [RNF017](file:///home/ian/Faculdade/APS/engenharia-de-requisitos/requisitos_SGDU.md#L200) e segurança RBAC [RNF005](file:///home/ian/Faculdade/APS/engenharia-de-requisitos/requisitos_SGDU.md#L165).

```plantuml
@startuml
skinparam classAttributeIconSize 0
skinparam monochrome false
skinparam packageStyle rectangle
skinparam shadowing false
skinparam defaultFontName "Helvetica"
skinparam ArrowColor #2C3E50
skinparam BorderColor #2C3E50
skinparam ClassBackgroundColor #EAEDED

class Usuario {
  - id: Integer
  - matricula: String
  - senha_hash: String
  - email: String
  - primeiro_login: Boolean
  - last_login: DateTime
  - cargo: CargoEnum
  + autenticar(): Boolean
  + alterar_senha(nova_senha: String): Boolean
  + gerenciar_senha(): Boolean
  + visualizar_dashboard(): DashboardStats
}

class Atlética {
  - id: Integer
  - nome: String
  - campus: String
  - curso: String
  + criar_atletica(): Atlética
  + editar_atletica(): Boolean
  + excluir_atletica(): Boolean
  + listar_atleticas(): List<Atlética>
}

class Atleta {
  - id: Integer
  - matricula: String
  - nome_completo: String
  - data_nascimento: Date
  - sexo: Char
  - vinculo_status: VinculoEnum
  + cadastrar_atleta(): Atleta
  + editar_atleta(): Boolean
  + transferir_atleta(nova_atletica: Atlética): Boolean
  + excluir_atleta(): Boolean
  + validar_vinculo_academico(): Boolean
}

class Time {
  - id: Integer
  - nome: String
  + criar_time(): Time
  + editar_time(): Boolean
  + listar_times(): List<Time>
  + excluir_time(): Boolean
}

class Modalidade {
  - id: Integer
  - nome: String
  - min_atletas: Integer
  - max_atletas: Integer
}

class Competição {
  - id: Integer
  - data_inicio: Date
  - data_fim: Date
  - status: CompeticaoStatusEnum
  + criar_competicao(): Competição
  + gerenciar_competicao(): Boolean
}

class Inscrição {
  - id: Integer
  - data_inscricao: DateTime
  - status: InscricaoStatusEnum
  + inscrever_time(): Inscrição
}

class Partida {
  - id: Integer
  - data_hora: DateTime
  - placar_casa: Integer
  - placar_visitante: Integer
  - status: PartidaStatusEnum
}

class Súmula {
  - id: Integer
  - resultado_detalhado: Text
  - aprovada: Boolean
  - versao: Integer
  + iniciar_sumula()
  + registrar_evento(evento: EventoPartida)
  + confirmar_sumula()
  + reabrir_sumula(justificativa: String)
}

class EventoPartida {
  - id: Integer
  - tipo: EventoTipoEnum
  - minuto: Integer
  - descricao: String
}

class InstalaçãoFísica {
  - id: Integer
  - nome: String
  - tipo: String
  - capacidade: Integer
  - horarios_disponiveis: Text
  - restricoes: Text
}

class Agendamento {
  - id: Integer
  - data_inicio: Date
  - hora_inicio: Time
  - data_fim: Date
  - hora_fim: Time
  - finalidade: String
  - status: AgendamentoStatusEnum
  + criar_agendamento(): Agendamento
  + editar_agendamento(): Boolean
  + cancelar_agendamento(): Boolean
  + consultar_disponibilidade(): List<TimeSlot>
}

class EdiçãoDosJogos {
  - id: Integer
  - nome: String
  - ano: Integer
  - ativa: Boolean
  + criar_edicao(): EdiçãoDosJogos
  + definir_edicao_atual(): Boolean
}

class Certificado {
  - id: Integer
  - horas_complementares: Integer
  - data_emissao: DateTime
  - codigo_verificacao: UUID
  - status_assinatura: String
  + gerar_pdf()
  + validar_certificado(codigo: UUID): Boolean
  + enviar_por_email()
}

class LogAuditoria {
  - id: Integer
  - acao: String
  - objeto_id: String
  - timestamp: DateTime
  - ip: String
}

' Enums
enum CargoEnum {
  ADMINISTRADOR
  MODERADOR
  CAPITAO
}

enum VinculoEnum {
  PENDENTE
  VALIDADO
  VALIDADO_FALLBACK
  REJEITADO
}

enum CompeticaoStatusEnum {
  ABERTA
  EM_ANDAMENTO
  FINALIZADA
}

enum InscricaoStatusEnum {
  CONFIRMADA
  PENDENTE
  LISTA_DE_ESPERA
}

enum PartidaStatusEnum {
  AGENDADA
  EM_ANDAMENTO
  FINALIZADA
}

enum EventoTipoEnum {
  GOL
  PONTO
  CARTAO_AMARELO
  CARTAO_VERMELHO
  SUBSTITUICAO
  SET_PONTOS
  MOVIMENTO
  TEMPO
}

enum AgendamentoStatusEnum {
  CONFIRMADO
  PENDENTE
  CANCELADO
}

' Relacionamentos e Multiplicidades
Usuario "1" -- "0..1" Atleta : "associa-se a"
Usuario "1" --> "1" CargoEnum : "possui"
Atlética "1" -- "0..1" Usuario : "tem como Capitão responsável"
Atleta "0..*" --> "1" Atlética : "pertence a"
Atleta "0..*" -- "0..*" Time : "composto por"
Time "0..*" --> "1" Atlética : "pertence a"
Time "0..*" --> "1" Modalidade : "joga na"
Competição "0..*" --> "1" Modalidade : "específica para"
Competição "0..*" --> "1" EdiçãoDosJogos : "vinculada a"
Inscrição "0..*" --> "1" Time : "inscreve"
Inscrição "0..*" --> "1" Competição : "na"
Partida "0..*" --> "1" Competição : "realizada na"
Partida "1" -- "1" Súmula : "contém"
Partida "0..*" --> "1" Time : "time_casa"
Partida "0..*" --> "1" Time : "time_visitante"
Partida "0..*" --> "1" InstalaçãoFísica : "localizada em"
Súmula "1" *-- "0..*" EventoPartida : "contém eventos"
EventoPartida "0..*" --> "1" Atleta : "envolve"
Agendamento "0..*" --> "1" InstalaçãoFísica : "reserva"
Agendamento "0..*" --> "1" Usuario : "agendado por"
Certificado "0..*" --> "1" Atleta : "emitido para"
Certificado "0..*" --> "1" Competição : "referente a"
LogAuditoria "0..*" --> "1" Usuario : "realizado por"

@enduml
```

---

## 4. Modelo de Comportamento Dinâmico: Diagramas de Atividades

> [!NOTE]
> De acordo com as notas de aula do professor, na Visão Lógica, o Diagrama de Atividades é utilizado sob o ponto de vista do desenvolvedor, detalhando o fluxo interno e as regras de uma operação ou método de classe específico.

### 4.1 Validação de Vínculo Acadêmico (`Atleta.validar_vinculo_academico()`)
Este fluxo reflete os requisitos do [UC17 (Validar Vínculo Acadêmico)](file:///home/ian/Faculdade/APS/engenharia-de-requisitos/specs/casos_de_uso/especificacao_validar_vinculo_academico.md) e [RF028](file:///home/ian/Faculdade/APS/engenharia-de-requisitos/requisitos_SGDU.md#L145), implementando a integração com o SUAP e o fallback local via formato de matrícula.

```plantuml
@startuml
skinparam shadowing false
skinparam monochrome false
skinparam ActivityBackgroundColor #F8F9F9
skinparam ActivityBorderColor #2C3E50

start
:Iniciar `validar_vinculo_academico()`;
:Obter matrícula do atleta;

if (Serviço externo SUAP está conectado?) then (Sim)
  :Chamar API SUAP (matricula);
  if (SUAP responde com estudante ativo?) then (Sim)
    :Extrair curso e campus da resposta do SUAP;
    :Atualizar status do Atleta para VALIDADO;
    :Gravar log de auditoria (origem = SUAP, resultado = SUCESSO);
    :Retornar true;
  else (Não / Estudante inativo ou inexistente)
    :Atualizar status do Atleta para REJEITADO;
    :Gravar log de auditoria (origem = SUAP, resultado = REJEITADO);
    :Lançar erro de vínculo inativo;
    :Retornar false;
  endif
else (Não / Timeout ou falha de conexão)
  :Registrar indisponibilidade do SUAP no log de sistema;
  :Acionar Fallback por Formato de Matrícula;
  if (Matrícula atende expressão regular (regex) institucional?) then (Sim)
    if (Matrícula já está cadastrada para outro atleta?) then (Sim)
      :Gravar log de auditoria (origem = FALLBACK, resultado = DUPLICADO);
      :Lançar erro de duplicidade de matrícula;
      :Retornar false;
    else (Não)
      :Atualizar status do Atleta para VALIDADO_FALLBACK;
      :Enfileirar ID do Atleta para fila de reconciliação assíncrona;
      :Gravar log de auditoria (origem = FALLBACK, resultado = OK_PROVISORIO);
      :Retornar true;
    endif
  else (Não)
    :Atualizar status do Atleta para REJEITADO;
    :Gravar log de auditoria (origem = FALLBACK, resultado = REGEX_FALHOU);
    :Lançar erro de formato de matrícula inválida;
    :Retornar false;
  endif
end
stop
@enduml
```

### 4.2 Criação de Reserva (`Agendamento.criar_agendamento()`)
Este fluxo detalha as validações de disponibilidade física e conflitos de horário exigidas no [UC13 (Agendar Instalação Física)](file:///home/ian/Faculdade/APS/engenharia-de-requisitos/specs/casos_de_uso/especificacao_agendar_instalacao_fisica.md) e [RF023](file:///home/ian/Faculdade/APS/engenharia-de-requisitos/requisitos_SGDU.md#L135).

```plantuml
@startuml
skinparam shadowing false
skinparam monochrome false
skinparam ActivityBackgroundColor #F8F9F9
skinparam ActivityBorderColor #2C3E50

start
:Solicitar `criar_agendamento(local, data_inicio, hora_inicio, data_fim, hora_fim, finalidade)`;
:Buscar `InstalaçãoFísica` no banco de dados;

if (Instalação existe e está ativa?) then (Não)
  :Lançar erro "Instalação física inexistente ou inativa";
  stop
else (Sim)
  :Buscar agendamentos com status CONFIRMADO no mesmo local e no mesmo intervalo de tempo;
  if (Existe sobreposição de horário?) then (Sim)
    :Consultar slots livres sugeridos próximos;
    :Lançar erro "Conflito de horário: local ocupado";
    stop
  else (Não)
    :Validar se o intervalo solicitado está dentro dos limites de funcionamento do local;
    if (Fora do horário de funcionamento?) then (Sim)
      :Lançar erro "Local fechado para reservas no horário solicitado";
      stop
    else (Não)
      :Contabilizar reservas do Usuário no mesmo mês;
      if (Quantidade excede cota permitida?) then (Sim)
        :Lançar erro "Cota de agendamentos excedida";
        stop
      else (Não)
        if (Finalidade exige aprovação manual?) then (Sim)
          :Criar registro com status = PENDENTE;
          :Enviar notificação de aprovação pendente para Moderadores;
          :Gravar LogAuditoria (acao = SOLICITACAO_RESERVA, status = PENDENTE);
        else (Não)
          :Criar registro com status = CONFIRMADO;
          :Enviar notificação de sucesso ao Capitão/Responsável;
          :Gravar LogAuditoria (acao = CRIACAO_RESERVA, status = CONFIRMADO);
        endif
        :Retornar registro de Agendamento;
      endif
    endif
  endif
endif
stop
@enduml
```

### 4.3 Registro de Evento em Partida (`Súmula.registrar_evento()`)
Este fluxo apresenta o registro dinâmico de eventos (gols, pontos, cartões) durante uma partida em tempo real, conforme [UC10 (Registrar Súmula)](file:///home/ian/Faculdade/APS/engenharia-de-requisitos/specs/casos_de_uso/especificacao_registrar_sumula.md) e [RF020](file:///home/ian/Faculdade/APS/engenharia-de-requisitos/requisitos_SGDU.md#L129).

```plantuml
@startuml
skinparam shadowing false
skinparam monochrome false
skinparam ActivityBackgroundColor #F8F9F9
skinparam ActivityBorderColor #2C3E50

start
:Executar `registrar_evento(tipo_evento, minuto, atleta, time)`;
:Verificar se a Súmula está com status 'Aprovada';

if (Súmula aprovada/homologada?) then (Sim)
  :Lançar erro "Súmula finalizada. Requer reabertura com justificativa.";
  stop
else (Não / Aberta para edições)
  :Validar se o atleta está inscrito no time participante;
  if (Atleta pertence ao time?) then (Não)
    :Lançar erro "Atleta não pertence ao time escalado";
    stop
  else (Sim)
    :Verificar restrições da Modalidade (Ex: limite de substituições, cartões);
    if (Evento excede limites da modalidade?) then (Sim)
      :Lançar erro "Ação excede os limites permitidos para a modalidade";
      stop
    else (Não)
      :Gravar novo objeto EventoPartida vinculado à Súmula;
      if (Tipo de evento altera o placar (Gol ou Ponto)?) then (Sim)
        :Incrementar pontuação correspondente na Partida;
      else (Não)
      endif
      :Registrar LogAuditoria (acao = REGISTRO_EVENTO_SUMULA, id_evento);
      :Enviar notificação de atualização em tempo real para os espectadores;
    endif
  endif
endif
stop
@enduml
```

---

## 5. Diagrama de Sequência de Objeto

O diagrama a seguir detalha o fluxo de interação e mensagens entre os objetos para a realização da inscrição de um time em uma competição, implementando o caso de uso [UC09 (Inscrever Time em Competição)](file:///home/ian/Faculdade/APS/engenharia-de-requisitos/specs/casos_de_uso/especificacao_inscrever_time_em_competicao.md) e o requisito [RF019](file:///home/ian/Faculdade/APS/engenharia-de-requisitos/requisitos_SGDU.md#L127).

```plantuml
@startuml
autonumber
skinparam shadowing false
skinparam monochrome false
skinparam defaultFontName "Helvetica"
skinparam ParticipantBackgroundColor #EAEDED
skinparam ParticipantBorderColor #2C3E50
skinparam ControlBackgroundColor #FADBD8
skinparam ControlBorderColor #E74C3C
skinparam EntityBackgroundColor #D4EFDF
skinparam EntityBorderColor #27AE60
skinparam DatabaseBackgroundColor #FCF3CF
skinparam DatabaseBorderColor #F1C40F

actor "Moderador / Administrador" as User
boundary "PainelInscriçãoView" as View
control "InscriçãoService" as Service
entity "Competição" as Competicao
entity "Time" as Time
entity "Inscrição" as Inscricao
database "Banco de Dados" as DB
control "NotificationService" as Notif

User -> View: Selecionar Time e solicitar inscrição na Competição
activate View
View -> Service: inscreverTime(time_id, competicao_id)
activate Service

Service -> Competicao: verificarInscricoesAbertas()
activate Competicao
Competicao --> Service: esta_aberta (Boolean)
deactivate Competicao

alt Inscrições Fechadas
  Service --> View: erro "Período de inscrições fechado"
else Inscrições Abertas
  Service -> Time: obterDetalhesInscricao()
  activate Time
  Time --> Service: dados_time (modalidade, atletica, lista_atletas)
  deactivate Time

  Service -> Competicao: validarRegrasInscricao(dados_time)
  activate Competicao
  Competicao -> Competicao: validarModalidade(time_modalidade)
  Competicao -> Competicao: validarMinimoAtletas(n_atletas)
  Competicao -> Competicao: verificarInscricaoDuplicada(time_id)
  Competicao --> Service: regras_atendidas (Boolean)
  deactivate Competicao

  alt Regras Violadas (Ex: Time Incompleto)
    Service --> View: erro "Time não atende os requisitos mínimos da modalidade"
  else Regras Atendidas
    create Inscricao
    Service -> Inscricao: new Inscrição(time_id, competicao_id, CONFIRMADA)
    activate Inscricao
    Inscricao -> DB: persistirInscrição()
    activate DB
    DB --> Inscricao: gravado
    deactivate DB
    Inscricao --> Service: objeto_inscrição
    deactivate Inscricao

    Service -> DB: gravarLogAuditoria(user_id, "INSCREVER_TIME", inscricao_id)
    activate DB
    DB --> Service: log_gravado
    deactivate DB

    Service -> Notif: dispararEmailInscricao(capitao_id, competicao_id)
    activate Notif
    Notif --> Service: email_enviado
    deactivate Notif

    Service --> View: confirmacao_inscricao
  end
end

deactivate Service
View --> User: Apresentar notificação de sucesso ou mensagem de erro
deactivate View

@enduml
```

---

## 6. Considerações Finais e Auditoria

A Visão Lógica apresentada assegura:
1. **Rastreabilidade completa**: Todas as regras e validações estabelecidas na [especificação de requisitos](file:///home/ian/Faculdade/APS/engenharia-de-requisitos/requisitos_SGDU.md) foram refletidas nos atributos e métodos das classes.
2. **Segurança (RBAC e Logs)**: As classes `Usuario` e `LogAuditoria` asseguram que cada operação de alteração de estado no banco seja devidamente autorizada e rastreada.
3. **Robustez na Integração**: Os fluxos dinâmicos preveem contingências de rede (como o fallback de validação do SUAP), tornando o SGDU resiliente a falhas de infraestrutura.
