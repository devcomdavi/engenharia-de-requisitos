# Método `listar_atleticas()`

Este documento apresenta a explicação e o diagrama de atividades para o método `listar_atleticas()` da classe `Atlética`.

## Descrição
Lista as atléticas cadastradas. Administradores e Moderadores visualizam todas as atléticas; Capitães visualizam apenas a sua própria atlética.

- **Classe:** `Atlética`
- **Requisitos Vinculados:** [RF006](file:///home/ian/Faculdade/APS/engenharia-de-requisitos/requisitos_SGDU.md#L101)
- **Atores Relacionados:** Administrador, Moderador, Capitão

## Assinatura do Método
```python
listar_atleticas() -> List<Atlética>
```

## Regras de Negócio e Fluxo Lógico
O fluxo e as validações descritas a seguir representam o comportamento interno da operação:

1. Solicitar `listar_atleticas()`
2. Verificar cargo do usuário autenticado
3. Buscar todas as atléticas no banco de dados
4. Buscar apenas a atlética onde o usuário é Capitão responsável
5. Para cada atlética na lista
6. Contar número de atletas cadastrados
7. Contar número de times cadastrados
8. Buscar nome do capitão responsável
9. Montar lista de resposta estruturada
10. Retornar lista de atléticas com estatísticas

## Diagrama de Atividades
O diagrama abaixo detalha visualmente o fluxo de decisões, desvios e ações executados pelo método. Ele foi modelado utilizando o formato PlantUML.

![Diagrama de Atividades](listar-atleticas.png)

## Links Relacionados
- **Arquivo de Diagrama:** [listar_atleticas.puml](listar_atleticas.puml)
- **Documento Principal de Visão Lógica:** [Visão Lógica (visao_logica.md)](file:///home/ian/Faculdade/APS/engenharia-de-requisitos/docs/visao_logica/visao_logica.md)
