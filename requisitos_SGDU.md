# Capa

---

# Requisitos de Software

## Sistema de Gerenciamento Desportivo Universitário (SGDU)

### Versão 1.0

---

## Histórico de revisões

|    Data    | Versão |          Descrição          |                       Autores                        |
| :--------: | :----: | :-------------------------: | :--------------------------------------------------: |
| 26/06/2026 |  1.0   |    Criação do documento     | Arthur Felipe, Davi Holanda, Josef Ian               |

---

## Sumário

- [Capa](#capa)
  - [Histórico de revisões](#histórico-de-revisões)
  - [Sumário](#sumário)
- [Introdução](#introdução)
  - [Definições, Acrônimos e Abreviações](#definições-acrônimos-e-abreviações)
- [Usuários identificados](#usuários-identificados)
- [Requisitos funcionais](#requisitos-funcionais)
- [Requisitos não-funcionais](#requisitos-não-funcionais)
  - [Disponibilidade](#disponibilidade)
  - [Privacidade e segurança](#privacidade-e-segurança)
  - [Usabilidade](#usabilidade)
  - [Suportabilidade](#suportabilidade)
  - [Interoperabilidade](#interoperabilidade)
  - [Manutenibilidade](#manutenibilidade)
  - [Desempenho](#desempenho)
  - [Implementação](#implementação)
  - [Implantação](#implantação)
- [Matriz de rastreabilidade](#matriz-de-rastreabilidade)
  - [Rastreabilidade entre NFs e RNFs](#rastreabilidade-entre-nfs-e-rnfs)

---

# Introdução

O objetivo deste documento é apresentar os requisitos de software do produto **Sistema de Gerenciamento Desportivo Universitário (SGDU)**.

## Definições, Acrônimos e Abreviações

Esta subseção fornece as definições de todos os termos, acrônimos e abreviações necessárias à adequada interpretação do Documento de Requisitos.

- Identificação dos requisitos: por convenção, a referência a requisitos é feita através do identificador de requisitos, de acordo como descrito abaixo:

  `[IDENTIFICADOR DO TIPO DE REQUISITOSidentificador do requisito]`

  O identificador do tipo de requisitos é conforme abaixo:

  - RF – Requisito Funcional
  - RNF – Requisito Não-Funcional
  - NR – Não-Requisito

  O identificador do requisito será uma sequência numérica. Esse número sequencial será único para todo o conjunto de tipos de requisitos.

  **Exemplo**: RF0001, RF1234, RNF1234, NR1212

- Atributos dos Requisitos: os atributos de requisitos estabelecidos são:
  - **Requisitos vinculados**: fornece uma lista dos requisitos que mantém rastreabilidade.
  - **Prioridade**: Essencial, Importante, Desejável
  - **Complexidade**: Complexa, Alta, Média ou Baixa.
  - **Risco**: Alto, Médio, Baixo

# Usuários identificados

Os seguintes usuários foram identificados para o sistema:

- Usuário do sistema
  - Usuários com cargo administrativo
    - Administrador (Nível 3)
    - Moderador (Nível 2)
    - Capitão (Nível 1)
  - Usuário público
    - Atleta / Torcida (acesso público sem autenticação)
  - Usuários institucionais
    - Coordenação do DCE / Reitoria

# Requisitos funcionais

Os requisitos funcionais são descritos a seguir.

- **[RF001]** - Como um usuário com cargo atribuído, eu gostaria de realizar login no sistema utilizando minha matrícula e senha, para que eu possa acessar as funcionalidades conforme o meu nível de permissão. O sistema deve utilizar a matrícula como identificador de acesso (e não nome de usuário), verificar se o atleta possui cargo atribuído e carregar as informações da atlética vinculada ao perfil autenticado.

- **[RF002]** - Como um usuário com cargo recém-atribuído, eu gostaria de ser obrigatoriamente redirecionado para uma página de troca de senha no meu primeiro acesso ao sistema, para que eu possa substituir a senha padrão (igual à minha matrícula) por uma senha pessoal e segura. O acesso ao dashboard somente será liberado após a conclusão da troca.

- **[RF003]** - Como usuário com cargo, eu gostaria de poder alterar minha senha a qualquer momento, para que eu possa manter minha conta segura. A nova senha deve atender a critérios mínimos de segurança: mínimo de 8 caracteres, ao menos uma letra maiúscula, uma minúscula e um número, não podendo ser uma senha comum. A nova senha não pode ser idêntica à senha atual.

- **[RF004]** - Como administrador, moderador ou capitão, eu gostaria de criar novas atléticas no sistema, para que os cursos e campi do IFPB possam ser representados nas competições. Os dados obrigatórios são: nome da atlética (único no sistema, entre 3 e 255 caracteres), campus e curso. Ao criar a atlética, deve ser possível vincular um usuário com cargo de Capitão como responsável. Cada Capitão pode ser responsável por apenas uma atlética.

- **[RF005]** - Como administrador ou moderador, eu gostaria de editar as informações de qualquer atlética cadastrada, para que eu possa manter os dados atualizados. O Capitão poderá editar apenas a sua própria atlética. Os campos editáveis são: nome, campus e curso. A substituição do Capitão responsável não é editável diretamente, requerendo remoção e nova atribuição de cargo.

- **[RF006]** - Como usuário com cargo, eu gostaria de visualizar a lista de atléticas cadastradas no sistema, para que eu possa acompanhar as entidades participantes do evento. O Administrador e o Moderador visualizam todas as atléticas; o Capitão visualiza apenas a sua própria. As informações exibidas devem incluir: nome, campus, capitão responsável, número de atletas e número de times, além das ações disponíveis conforme o cargo.

- **[RF007]** - Como administrador ou moderador, eu gostaria de excluir atléticas do sistema, para que eu possa remover entidades inativas ou criadas incorretamente. A exclusão somente será permitida se a atlética não possuir atletas cadastrados nem times criados, e requer confirmação obrigatória antes de ser efetivada. Após a exclusão, o vínculo do Capitão responsável é liberado.

- **[RF008]** - Como administrador, moderador ou capitão, eu gostaria de cadastrar atletas no sistema, para que os estudantes possam ser vinculados às atléticas e participar das competições. Os dados obrigatórios são: matrícula (6 a 10 dígitos numéricos, única no sistema), nome completo, data de nascimento (idade entre 14 e 100 anos), sexo e atlética. O Administrador e o Moderador podem cadastrar em qualquer atlética; o Capitão somente na sua própria atlética.

- **[RF009]** - Como administrador, moderador ou capitão, eu gostaria de editar as informações cadastrais de atletas, para que eu possa corrigir ou atualizar dados. Os campos editáveis são: nome, data de nascimento e sexo. Matrícula e atlética não são editáveis diretamente, esta última sujeita a processo específico de transferência. O Capitão somente poderá editar atletas da sua própria atlética.

- **[RF010]** - Como administrador ou moderador, eu gostaria de transferir um atleta de uma atlética para outra, para que eu possa corrigir vínculos incorretos ou atender a mudanças organizacionais. A transferência somente será permitida se o atleta não estiver em times ativos, e requer confirmação obrigatória.

- **[RF011]** - Como administrador, moderador ou capitão, eu gostaria de criar times por modalidade esportiva para as atléticas, para que as equipes possam se inscrever nas competições. Os dados obrigatórios são: nome do time (único por atlética e modalidade, entre 3 e 100 caracteres), modalidade, atlética e lista de atletas. Todos os atletas do time devem pertencer à mesma atlética, o número de atletas deve respeitar os limites definidos pela modalidade (ex.: Futsal: mín. 5, máx. 12; Vôlei: mín. 6, máx. 12; Basquete: mín. 5, máx. 12; Xadrez: 1 atleta), e nenhum atleta pode estar em outro time da mesma modalidade. O Capitão pode criar times apenas da sua própria atlética.

- **[RF012]** - Como administrador, moderador ou capitão, eu gostaria de editar a composição e o nome de times criados, para que eu possa manter as equipes atualizadas. As operações disponíveis são: alterar nome do time, adicionar atletas (da mesma atlética) e remover atletas (respeitando o número mínimo da modalidade). O Capitão somente poderá editar times da sua própria atlética.

- **[RF013]** - Como usuário com cargo, eu gostaria de visualizar a lista de times cadastrados, filtráveis por modalidade e atlética, para que eu possa acompanhar as equipes participantes. O Capitão visualiza apenas os times da sua própria atlética. As informações exibidas devem incluir: nome do time, modalidade, atlética, número de atletas e ações disponíveis conforme o cargo.

- **[RF014]** - Como administrador, moderador ou capitão, eu gostaria de excluir times do sistema, para que eu possa remover equipes inativas ou criadas incorretamente. A exclusão requer confirmação obrigatória e não pode ser realizada em times que estejam participando de competições ativas. O Capitão pode excluir apenas times da sua própria atlética.

- **[RF015]** - Como administrador ou moderador, eu gostaria de atribuir cargos administrativos a atletas já cadastrados no sistema, para que eles possam acessar e gerenciar o sistema conforme seu nível de permissão. Os cargos disponíveis são: Capitão (atribuível por Administrador e Moderador, exige vinculação a uma atlética sem capitão) e Moderador (atribuível apenas por Administrador). Ao atribuir um cargo, um perfil de usuário é criado com e-mail válido e senha padrão igual à matrícula, o flag de primeiro_login é ativado, e o atleta não pode já possuir cargo.

- **[RF016]** - Como administrador, eu gostaria de remover cargos de usuários do sistema, para que eu possa revogar acessos quando necessário. A remoção deleta o perfil de usuário e o vínculo associado. Se o usuário removido for Capitão, a atlética é liberada para receber novo responsável. O atleta permanece cadastrado no sistema. A operação requer confirmação obrigatória.

- **[RF017]** - Como usuário com cargo, eu gostaria de acessar um dashboard com informações e estatísticas relevantes ao meu nível de permissão, para que eu possa ter uma visão geral do sistema e acessar rapidamente as funcionalidades disponíveis. O Administrador e o Moderador visualizam estatísticas globais (total de atletas, atléticas, times e usuários). O Capitão visualiza as estatísticas da sua própria atlética (atletas e times).

- **[RF018]** - Como administrador ou moderador, eu gostaria de criar e gerenciar competições por modalidade esportiva vinculadas a uma edição dos jogos, para que os times possam ser inscritos e os jogos possam ser organizados com período de realização definido.

- **[RF019]** - Como administrador ou moderador, eu gostaria de inscrever times em competições, para que as equipes possam participar oficialmente dos jogos. O sistema deve validar se o time possui o número mínimo de atletas exigido pela modalidade antes de confirmar a inscrição.

- **[RF020]** - Como administrador ou moderador, eu gostaria de registrar súmulas digitais das partidas, para que os resultados sejam computados de forma oficial e auditável. O modelo de súmula deve ser adaptado ao tipo de esporte (ex.: pontos no basquete, gols no futsal, tempo no atletismo, sets no vôlei), registrando todos os eventos relevantes da partida.

- **[RF021]** - Como usuário com cargo, eu gostaria de consultar as tabelas de classificação e o ranking das atléticas por modalidade e geral, para que eu possa acompanhar o desempenho das equipes ao longo do evento.

- **[RF022]** - Como administrador, eu gostaria de gerenciar múltiplas edições dos jogos no sistema, para que o histórico das competições anteriores seja preservado e seja possível definir qual é a edição atual ativa.

- **[RF023]** - Como administrador ou moderador, eu gostaria de gerenciar o agendamento das instalações físicas (quadras, ginásios, campos) para as partidas, para que os conflitos de horários entre modalidades sejam evitados, inserindo o Nome do local, a data e hora de uso. O sistema deve verificar a disponibilidade do espaço antes de confirmar um agendamento.

- **[RF024]** - Como administrador ou moderador, eu gostaria de gerar relatórios de participação dos atletas, com estatísticas por partida, modalidade e atlética, para que a Reitoria e a Coordenação de Esportes possam tomar decisões estratégicas com base em dados reais do evento.

- **[RF025]** - Como administrador ou moderador, eu gostaria de exportar os dados do sistema em diferentes formatos, para que as informações possam ser utilizadas em outros contextos institucionais. Os formatos suportados devem incluir PDF e planilhas eletrônicas (XLS, XLSX, CSV).

- **[RF026]** - Como administrador, eu gostaria que o sistema gerenciasse a emissão de certificados de participação para os atletas, para que a contabilização de horas complementares seja realizada de forma automatizada, eliminando os erros e atrasos do processo manual.

- **[RF027]** - Como usuário com cargo, eu gostaria de receber notificações sobre novas competições, inscrições abertas, lembretes de jogos agendados e avisos administrativos, para que eu possa ser informado em tempo hábil das atualizações relevantes ao meu papel no sistema.

- **[RF028]** - Como administrador, eu gostaria que o sistema validasse o vínculo acadêmico dos atletas através de integração com o SUAP (Sistema Unificado de Administração Pública), para que apenas estudantes regularmente matriculados no IFPB possam participar das competições. Caso a integração via SUAP não esteja disponível, o vínculo deve ser validado pelo formato padrão da matrícula institucional.

- **[RF029]** - Como atleta ou membro da comunidade acadêmica, eu gostaria de acessar publicamente os resultados das partidas, tabelas de classificação e horários dos jogos, para que eu possa acompanhar o evento sem necessidade de autenticação no sistema.

# Requisitos não-funcionais

Os requisitos não-funcionais são descritos a seguir.

## Disponibilidade

- **[RNF001]** - O sistema deve estar disponível 24 horas por dia, 7 dias por semana, garantindo estabilidade e acesso contínuo, especialmente durante os períodos de pico de inscrições e de realização das competições.

- **[RNF002]** - O sistema deve ser desenvolvido de forma escalável, suportando o crescimento no volume de atletas, equipes, modalidades e edições dos jogos sem degradação de desempenho ou disponibilidade.

## Privacidade e segurança

- **[RNF003]** - O sistema deve ser desenvolvido sob a metodologia Privacy by Design (PbD), garantindo conformidade total com a Lei Geral de Proteção de Dados (LGPD), com controles de consentimento explícito e uso restrito dos dados pessoais dos estudantes estritamente para fins desportivos.

- **[RNF004]** - Os dados pessoais sensíveis dos estudantes, como e-mail e senha, devem ser criptografados utilizando algoritmos robustos. As senhas devem ser armazenadas com hashing PBKDF2 e salt automático (padrão Django), impossibilitando a recuperação dos dados originais.

- **[RNF005]** - O sistema deve implementar controle de acesso baseado em funções (RBAC — Role-Based Access Control), garantindo que cada usuário acesse apenas as funcionalidades e dados permitidos para o seu cargo (Administrador, Moderador ou Capitão), com verificação de vínculo com atlética para operações restritas do Capitão.

- **[RNF006]** - O sistema deve implementar proteções contra as principais vulnerabilidades web, incluindo: proteção CSRF com tokens em todos os formulários, prevenção de XSS com auto-escaping nos templates e proteção contra SQL Injection por meio do ORM com queries parametrizadas.

- **[RNF007]** - O sistema deve suportar autenticação de dois fatores (2FA) como camada adicional de segurança para usuários com cargos administrativos.

## Usabilidade

- **[RNF008]** - A interface do sistema deve ser intuitiva, responsiva e adaptada para uso em computadores e dispositivos móveis, dispensando treinamentos técnicos para Capitães e Atletas, em conformidade com o perfil de alta fluidez digital dos usuários do IFPB.

- **[RNF009]** - O sistema deve atender aos padrões básicos de acessibilidade web, garantindo o acesso inclusivo a estudantes com diferentes necessidades e habilidades.

## Suportabilidade

- **[RNF010]** - O sistema deve ser compatível com os principais navegadores modernos: Google Chrome, Mozilla Firefox, Microsoft Edge e Safari, em suas versões atualizadas, tanto em desktops quanto em dispositivos móveis com sistemas operacionais Android e iOS.

## Interoperabilidade

- **[RNF011]** - O sistema deve ser capaz de integrar com o ecossistema SUAP (Sistema Unificado de Administração Pública) via API, para validação do vínculo acadêmico dos atletas em tempo real. Na ausência da integração, a validação deve ocorrer pelo formato padrão da matrícula institucional.

- **[RNF012]** - O sistema deve expor APIs de acesso aos dados para permitir integração com outros sistemas institucionais, viabilizando a exportação de informações de forma automatizada e segura.

## Manutenibilidade

- **[RNF013]** - O sistema deve possuir cobertura de testes automatizados (unitários e de integração com Django Test Framework) para garantir a integridade dos dados, facilitar a manutenção e prevenir regressões nas funcionalidades críticas.

- **[RNF014]** - O código-fonte deve ser organizado e documentado seguindo a arquitetura MVT (Model-View-Template) do Django, de forma a permitir auditorias técnicas e futuras expansões por outros desenvolvedores da instituição.

- **[RNF015]** - O sistema deve priorizar o uso de ferramentas, bibliotecas e infraestruturas de código aberto (Open Source), visando a sustentabilidade financeira do projeto enquanto extensão institucional sem fins lucrativos.

## Desempenho

- **[RNF016]** - O sistema deve apresentar tempo de resposta máximo de 5 segundos para qualquer operação, com capacidade de processar múltiplos acessos simultâneos sem degradação, visando uma redução de 90% no tempo operacional em relação aos processos manuais atualmente utilizados.

## Implementação

- **[RNF017]** - O backend do sistema deve ser implementado com o framework Django (Python 3.8+), utilizando PostgreSQL como banco de dados relacional, em conformidade com a arquitetura MVT definida para o projeto.

## Implantação

- **[RNF018]** - O sistema deve ser implantável em ambientes que suportem Python/Django, incluindo servidores institucionais e plataformas de nuvem pública (Vercel, AWS, DigitalOcean), preferencialmente em infraestruturas com planos educacionais gratuitos ou de código aberto.

- **[RNF019]** - O sistema deve ser configurado nativamente para o idioma português (Brasil), utilizando o fuso horário oficial de Brasília (UTC-3) para o registro de logs, prazos de inscrições e horários de partidas.

- **[RNF020]** - O sistema não deve permitir a exclusão de registros de atletas que já participaram de competições homologadas, visando preservar o histórico esportivo institucional e garantir a auditabilidade dos eventos realizados.

# Matriz de rastreabilidade

A matriz de rastreabilidade é apresentada a seguir.

## Rastreabilidade entre NFs e RNFs

| RF / RNF | RF001 | RF002 | RF003 | RF004 | RF005 | RF006 | RF007 | RF008 | RF009 | RF010 | RF011 | RF012 | RF013 | RF014 | RF015 | RF016 | RF017 | RF018 | RF019 | RF020 | RF021 | RF022 | RF023 | RF024 | RF025 | RF026 | RF027 | RF028 | RF029 |
| :------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  RNF001  |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |   X   |       |   X   |       |   X   |       |       |       |       |       |       |       |   X   |
|  RNF002  |       |       |       |       |       |   X   |       |       |       |       |       |       |   X   |       |       |       |   X   |       |   X   |       |   X   |       |       |       |       |       |       |       |   X   |
|  RNF003  |       |       |       |       |       |       |       |   X   |   X   |   X   |       |       |       |       |   X   |   X   |       |       |       |       |       |       |       |   X   |   X   |   X   |       |   X   |       |
|  RNF004  |   X   |   X   |   X   |       |       |       |       |       |       |       |       |       |       |       |   X   |       |       |       |       |       |       |       |       |       |       |       |       |       |       |
|  RNF005  |       |       |       |   X   |   X   |       |   X   |   X   |   X   |   X   |   X   |   X   |       |   X   |   X   |   X   |   X   |   X   |       |       |       |       |   X   |       |       |       |       |       |       |
|  RNF006  |   X   |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |
|  RNF007  |   X   |       |       |       |       |       |       |       |       |       |       |       |       |       |   X   |       |       |       |       |       |       |       |       |       |       |       |       |       |       |
|  RNF008  |       |       |       |       |       |   X   |       |   X   |       |       |   X   |       |   X   |       |       |       |   X   |       |   X   |       |   X   |       |       |       |       |       |       |       |   X   |
|  RNF009  |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |   X   |
|  RNF010  |   X   |       |       |       |       |   X   |       |       |       |       |       |       |   X   |       |       |       |   X   |       |       |       |       |       |       |       |       |       |       |       |   X   |
|  RNF011  |       |       |       |       |       |       |       |   X   |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |   X   |       |
|  RNF012  |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |   X   |   X   |       |       |   X   |       |
|  RNF013  |   X   |       |       |       |       |       |       |   X   |       |       |   X   |       |       |       |   X   |       |       |       |   X   |   X   |       |       |       |       |       |       |       |       |       |
|  RNF014  |       |       |       |   X   |       |       |       |   X   |       |       |   X   |       |       |       |   X   |       |       |   X   |       |   X   |       |       |       |       |       |       |       |       |       |
|  RNF015  |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |
|  RNF016  |       |       |       |       |       |   X   |       |       |       |       |       |       |   X   |       |       |       |   X   |       |   X   |       |   X   |       |       |   X   |       |       |       |       |   X   |
|  RNF017  |   X   |       |       |   X   |       |       |       |   X   |       |       |   X   |       |       |       |   X   |       |       |       |       |   X   |       |   X   |       |       |       |       |       |       |       |
|  RNF018  |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |
|  RNF019  |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |   X   |       |       |       |   X   |   X   |       |       |       |   X   |       |   X   |
|  RNF020  |       |       |       |       |       |       |   X   |       |       |   X   |       |       |       |   X   |       |   X   |       |       |       |       |       |   X   |       |       |       |       |       |       |       |

---

Criado em Junho de 2026 por _Arthur Felipe, Davi Holanda e Josef Ian_
