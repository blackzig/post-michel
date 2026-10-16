---
title: "Engenharia de Software para Mentores — Lição 1: Fundamentos"
seo_title: "Engenharia de Software: fundamentos para atuar como mentor"
slug: "engenharia-software-mentores-licao-1-fundamentos"
meta_description: "Entenda Engenharia de Software além do código e aprenda a explicar requisitos, qualidade e ciclo de vida em uma mentoria prática."
focus_keyphrase: "Engenharia de Software para mentores"
related_keywords:
  - "o que faz um engenheiro de software"
  - "mentoria em programação"
  - "ciclo de vida de software"
category: "Engenharia de Software"
tags:
  - "Engenharia de Software"
  - "Mentoria"
  - "Requisitos"
  - "Qualidade de Software"
excerpt: "Aprenda a conectar problemas, decisões, implementação e evidências de qualidade — e a ensinar esse raciocínio a um mentorado."
---

# Engenharia de Software para Mentores — Lição 1: Fundamentos

Quando alguém diz “quero ser engenheiro de software”, o mentor precisa compreender o que essa pessoa imagina fazer. Ela pode estar pensando em criar aplicativos, trabalhar com backend ou simplesmente conseguir o primeiro emprego em tecnologia.

Nossa formação começa por uma ideia central: **construir software envolve compreender necessidades, tomar decisões e sustentar o resultado ao longo do tempo**. Programar é uma parte essencial desse trabalho.

Esta lição tem dois objetivos simultâneos: desenvolver seu entendimento técnico e ensinar como transformar esse entendimento em orientação útil. Os casos são fictícios e as atividades são propostas didáticas próprias.

## 1. O que você deverá conseguir fazer

Ao concluir, você deverá:

- explicar Engenharia de Software em linguagem acessível;
- distinguir uma ferramenta de uma competência;
- relacionar requisitos, implementação, testes, entrega e manutenção;
- reconhecer decisões que envolvem benefícios e custos;
- conduzir uma conversa inicial sem prescrever uma tecnologia cedo demais;
- avaliar uma pequena entrega por evidências.

Reserve uma sessão de estudo e outra de prática. O tempo efetivo dependerá da experiência anterior.

## 2. O que é Engenharia de Software?

Nesta trilha, entenderemos Engenharia de Software como a aplicação organizada de conhecimentos, métodos e avaliação para desenvolver, operar e modificar software que atenda a necessidades reais.

Essa definição didática coloca cinco perguntas no centro do trabalho:

1. Que problema estamos tentando resolver?
2. Quem será afetado pela solução?
3. Quais restrições precisam ser respeitadas?
4. Como saberemos que a solução funciona?
5. Como vamos mantê-la quando o contexto mudar?

Um programa pode executar sem erros e ainda resolver o problema errado. Também pode atender a uma demonstração e falhar quando duas pessoas o utilizam simultaneamente.

O [SWEBOK](https://www.computer.org/education/bodies-of-knowledge/software-engineering) é uma referência para conhecer a abrangência da disciplina, incluindo requisitos, design, construção, testes e manutenção. Nosso percurso seleciona e organiza temas para fins de aprendizagem; não reproduz uma certificação oficial.

## 3. Programação, engenharia e arquitetura

Esses conceitos se relacionam, mas não são sinônimos.

| Conceito | Pergunta central | Exemplo |
|---|---|---|
| Programação | Como expressar o comportamento em código? | Implementar uma função de reserva |
| Engenharia de Software | Como produzir e sustentar uma solução adequada? | Validar regras, testar, entregar e acompanhar reservas |
| Arquitetura de software | Quais decisões estruturais sustentam as qualidades necessárias? | Definir limites de módulos e responsabilidades |
| Mentoria técnica | Como ajudar a pessoa a desenvolver julgamento e autonomia? | Investigar como ela decidiu e orientar a próxima prática |

Não use essa tabela como uma hierarquia de prestígio. Desenvolvedores também tomam decisões de engenharia; arquitetos precisam compreender consequências concretas de implementação. Títulos e responsabilidades variam entre organizações.

Em uma conversa de carreira, pergunte sobre as atividades desejadas e examine descrições reais de oportunidades. O nome do cargo, sozinho, explica pouco.

## 4. Competência não é uma lista de ferramentas

“Conheço Java, Git e SQL” pode significar muitas coisas. A pessoa talvez tenha visto aulas, executado exemplos ou mantido um sistema real. São experiências diferentes.

Compare:

- **Ferramenta:** Git.
- **Competência:** organizar e comunicar alterações, colaborar e recuperar um estado anterior.
- **Evidência:** um histórico legível, uma alteração revisável e a explicação de como um conflito foi resolvido.

Outro exemplo:

- **Ferramenta:** biblioteca de testes.
- **Competência:** selecionar verificações que detectem riscos relevantes.
- **Evidência:** um teste que falha diante do defeito investigado e passa após a correção.

O mentor deve investigar o que a pessoa consegue fazer, não apenas reconhecer nomes em uma lista.

## 5. Caso de estudo: agendamento de mentorias

Imagine um pequeno grupo que organiza encontros por mensagens. Há horários duplicados e cancelamentos que não chegam a todos.

O pedido inicial é: “Precisamos de um aplicativo”.

Esse pedido ainda não define o problema. Antes de discutir interface ou banco de dados, esclareça:

- Quem disponibiliza horários?
- Cada horário pertence a qual mentor?
- Quantas reservas podem existir por horário?
- Quem pode cancelar?
- O que acontece quando um horário já começou?
- Como os participantes recebem uma confirmação?
- Qual dificuldade é mais urgente: reservar, cancelar ou acompanhar?

Para o laboratório, adotaremos estas hipóteses: cada horário pertence a um mentor, comporta uma reserva ativa e pode ser reservado apenas antes do início. O cancelamento libera o horário se ele ainda não começou.

São regras do exercício. Em um produto real, precisariam ser confirmadas com os responsáveis.

## 6. Do pedido ao critério de aceitação

Uma funcionalidade precisa de comportamento verificável.

**Pedido vago:** permitir reservas de forma eficiente.

**Regra proposta:** não permitir mais de uma reserva ativa para o mesmo horário.

**Exemplo de aceitação:**

~~~text
Dado um horário futuro sem reserva ativa
Quando uma pessoa confirma a reserva
Então o sistema registra a reserva
E o horário deixa de estar disponível
~~~

**Exemplo de conflito:**

~~~text
Dado um horário futuro já reservado
Quando outra pessoa tenta reservá-lo
Então o sistema informa a indisponibilidade
E mantém a reserva existente
~~~

Esses exemplos ajudam a alinhar entendimento. Ainda não resolvem todos os detalhes técnicos. Duas solicitações concorrentes podem passar por uma verificação ingênua de disponibilidade antes de qualquer gravação.

Por isso, uma decisão posterior deverá proteger a regra no mecanismo de persistência ou de coordenação apropriado. Não precisamos ensinar transações em profundidade hoje; precisamos reconhecer o risco.

## 7. Qualidade também precisa de contexto

“Rápido”, “seguro” e “fácil de usar” são intenções importantes, mas insuficientes para orientar uma avaliação.

Uma meta hipotética mais verificável seria: em um ambiente de teste definido, com 20 usuários simultâneos e uma massa de dados especificada, pelo menos 95% das consultas de horários devem responder em até dois segundos.

Esses números são exemplos didáticos, não padrões universais. Para aplicá-los, seria necessário esclarecer carga, ambiente, método de medição e relevância para os usuários.

| Dimensão | Pergunta prática para o projeto |
|---|---|
| Correção | A reserva respeita as regras combinadas? |
| Confiabilidade | O que acontece quando a gravação falha? |
| Segurança | Quem pode cancelar uma reserva? |
| Usabilidade | A pessoa entende por que um horário está indisponível? |
| Manutenibilidade | É possível alterar a regra sem espalhar mudanças? |
| Desempenho | A resposta atende à necessidade sob uma carga definida? |

Não tente otimizar todas as dimensões sem limites. Priorize de acordo com o impacto de falhas e o contexto do produto.

## 8. O ciclo de vida não termina no código

O trabalho envolve compreender, projetar, construir, verificar, entregar, operar e evoluir. Essas atividades podem se repetir e se sobrepor.

Em uma entrega pequena, você pode esclarecer uma regra, implementar uma parte, testar com usuários e ajustar a hipótese. Os [princípios do Manifesto Ágil](https://agilemanifesto.org/iso/ptbr/principles.html) dão importância à colaboração, às entregas frequentes e à reflexão sobre o trabalho.

Para o agendamento, uma evolução possível é:

| Incremento | Entrega | O que aprender |
|---|---|---|
| Primeiro | Consultar horários e reservar em um protótipo | As regras e a linguagem fazem sentido? |
| Segundo | Persistir reservas e impedir conflitos | A integridade dos dados está protegida? |
| Terceiro | Cancelar com autorização | Quem pode executar cada ação? |
| Quarto | Publicar e observar falhas | O sistema funciona no ambiente de uso? |

O protótipo inicial deve deixar suas limitações claras. Um experimento local não demonstra, por si só, prontidão para produção.

## 9. Decisões têm benefícios e custos

Suponha que alguém proponha microsserviços para o sistema. Em vez de aprovar ou rejeitar pelo nome da arquitetura, pergunte:

- Qual problema concreto essa separação resolve?
- Qual alternativa mais simples foi considerada?
- Que dificuldade nova teremos para testar e operar?
- A equipe consegue sustentar essa dificuldade?
- Que evidência nos faria revisar a decisão?

Para um pequeno laboratório, uma aplicação única pode reduzir a complexidade operacional. Isso é uma escolha contextual, não uma regra de que sistemas distribuídos são sempre inadequados.

Registre decisões em quatro campos:

~~~text
Contexto:
Alternativas consideradas:
Decisão e justificativa:
Consequências e condição de revisão:
~~~

Esse hábito torna o raciocínio discutível e ensinável.

## 10. Como explicar tudo isso a um iniciante

Use uma explicação curta antes de aprofundar:

> Programar é escrever as instruções. Trabalhar com Engenharia de Software também envolve entender o problema, combinar o que significa funcionar, escolher uma solução, verificar o resultado e cuidar das mudanças.

Depois, peça à pessoa um exemplo próprio. Se ela só repetir a frase, ainda não sabemos se compreendeu.

Pergunte: “Em um aplicativo de entregas, que problema poderia existir mesmo quando o código executa sem apresentar erro?”

Uma resposta possível é cobrar o valor errado ou permitir uma entrega em um endereço não atendido. A explicação deve ligar o conceito a uma consequência observável.

## 11. Perguntas úteis para uma mentoria

Escolha poucas perguntas e aprofunde as respostas:

- O que você imagina fazer no dia a dia dessa profissão?
- Conte uma situação em que precisou investigar um problema.
- Qual projeto você consegue demonstrar hoje?
- Que parte você fez e que parte veio de um tutorial?
- Como verificou que funcionava?
- O que faria se a regra mudasse amanhã?
- Qual decisão você tomou sem ter certeza?

Evite transformar a conversa em prova de memorização. O objetivo inicial é entender o raciocínio e localizar uma oportunidade de prática.

Experiência em Java, por exemplo, pode fornecer bons casos de manutenção e testes. Mas não justifica recomendar Java automaticamente para toda pessoa ou todo objetivo.

## 12. Exercício do estudante: mapa do problema

Para o sistema de agendamento, entregue uma página contendo:

1. problema e público;
2. três regras de negócio;
3. dois exemplos de aceitação;
4. um risco;
5. uma forma de verificar a solução;
6. uma decisão que depende de esclarecimento.

Não é necessário programar nesta atividade. O resultado deve permitir que outra pessoa faça perguntas e identifique contradições.

**Exemplo parcial:** “Uma pessoa pode cancelar a própria reserva antes do início. Precisamos esclarecer se existe antecedência mínima.” Essa resposta é melhor do que inventar uma regra de 24 horas sem consultar ninguém.

## 13. Exercício do mentor: simulação de 20 minutos

Convide alguém para interpretar um iniciante ou faça a simulação em voz alta.

- Nos primeiros cinco minutos, investigue o objetivo.
- Nos próximos cinco, peça um exemplo do que a pessoa já realizou.
- Use cinco minutos para explicar a ligação entre código, requisitos e qualidade.
- Encerre definindo uma pequena entrega e uma forma de revisá-la.

Depois, registre apenas sua autoavaliação: interrompeu? Recomendou ferramentas antes de entender? Pediu evidências? O estudante participou da escolha?

Não registre relatos pessoais no material público.

## 14. Revisão com respostas orientadoras

**1. Um programa sem erros de execução é necessariamente correto?**

Não. Pode implementar uma regra errada ou atender a uma necessidade diferente da real.

**2. “Aprender um framework” é um objetivo suficiente?**

É pouco específico. Acrescente uma entrega: implementar uma funcionalidade, explicar decisões e demonstrar verificações.

**3. Por que um teste de reserva comum não garante ausência de reservas duplicadas?**

Porque solicitações simultâneas podem interagir de maneira diferente. É preciso investigar o mecanismo que protege a regra e verificar esse cenário.

**4. O mentor deve resolver a tarefa assim que o estudante trava?**

Primeiro deve investigar a dificuldade e oferecer ajuda proporcional: uma pergunta, uma pista, uma demonstração curta ou uma explicação. Depois, devolver ao estudante uma parte da prática.

**5. Qual evidência sugere compreensão?**

Aplicar o conceito em um caso diferente e justificar a escolha, não apenas repetir a definição.

## 15. Critérios para avançar

- [ ] Explico a disciplina em até dois minutos com um exemplo.
- [ ] Diferencio uma ferramenta de uma competência.
- [ ] Formulo uma regra e um exemplo verificável.
- [ ] Identifico um risco além de erro de sintaxe.
- [ ] Reconheço uma decisão com benefícios e custos.
- [ ] Conduzo a simulação e proponho uma entrega proporcional.

Se não conseguir formular exemplos verificáveis, retome o mapa do problema. Se conseguir explicar, mas não aplicar, refaça a atividade com uma biblioteca ou um controle de estoque fictício.

## 16. Próxima lição

Na [Lição 2 — Diagnóstico de competências e plano de desenvolvimento](engenharia-software-mentores-licao-2-diagnostico.md), vamos transformar uma conversa inicial em um plano curto, realista e revisável.

Veja também a [trilha completa de 24 lições](engenharia-software-mentores-trilha.md).

## Fontes e materiais de apoio

- [IEEE Computer Society — SWEBOK](https://www.computer.org/education/bodies-of-knowledge/software-engineering): referência para a abrangência da disciplina.
- [Manifesto Ágil — princípios em português](https://agilemanifesto.org/iso/ptbr/principles.html): referência para colaboração, entregas e melhoria contínua.

*Nota editorial: exemplos, rubricas e simulações são construções didáticas originais. Esta lição não é transcrição de vídeo nem material oficial de uma instituição.*

**Metadescrição:** Entenda Engenharia de Software além do código e aprenda a explicar requisitos, qualidade e ciclo de vida em uma mentoria prática.

**Tags:** Engenharia de Software, Mentoria, Programação, Requisitos, Qualidade, Carreira em TI.
