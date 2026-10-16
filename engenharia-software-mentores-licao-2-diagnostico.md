---
title: "Engenharia de Software para Mentores — Lição 2: Diagnóstico e Plano de Desenvolvimento"
seo_title: "Mentoria em software: diagnóstico e plano de desenvolvimento"
slug: "engenharia-software-mentores-licao-2-diagnostico"
meta_description: "Aprenda a diagnosticar competências em software, avaliar evidências e construir um plano de estudos viável com seu mentorado."
focus_keyphrase: "mentoria em software"
related_keywords:
  - "diagnóstico de competências"
  - "plano de estudos"
  - "mentoria de carreira em TI"
category: "Engenharia de Software"
tags:
  - "Engenharia de Software"
  - "Mentoria"
  - "Plano de Desenvolvimento"
  - "Aprendizagem"
excerpt: "Transforme objetivos vagos em pequenos experimentos de aprendizagem com evidências, feedback e revisão."
---

# Engenharia de Software para Mentores — Lição 2: Diagnóstico e Plano de Desenvolvimento

Na [Lição 1](engenharia-software-mentores-licao-1-fundamentos.md), relacionamos Engenharia de Software a problemas, decisões e qualidade. Agora vamos aplicar esse olhar à formação de uma pessoa.

Uma orientação como “estude lógica, Java, banco de dados e cloud” pode ser tecnicamente plausível e pouco útil. Sem conhecer o objetivo, as condições de estudo e as competências existentes, o mentor não sabe por onde começar.

Nesta lição, construiremos um diagnóstico inicial e um plano de duas semanas. Trata-se de um método didático proposto para a trilha, ajustável à pessoa e ao contexto.

## 1. Objetivos

Ao final, você deverá conseguir:

- esclarecer um objetivo de carreira sem prometer resultados;
- distinguir relato, evidência e hipótese;
- propor uma tarefa diagnóstica proporcional;
- registrar competências sem rotular a pessoa;
- escolher uma prioridade de aprendizagem;
- formular um plano com entrega, critério de conclusão e revisão;
- oferecer feedback que ajude a próxima tentativa.

A atividade final será uma simulação. Não é necessário ter um mentorado real para praticar.

## 2. Diagnóstico é uma hipótese de trabalho

O diagnóstico inicial não mede toda a capacidade de alguém. Uma tarefa curta é uma amostra, influenciada por familiaridade com o problema, ambiente e experiência.

Considere três tipos de informação:

| Tipo | Exemplo | Uso adequado |
|---|---|---|
| Relato | “Já estudei SQL” | Investigar experiências anteriores |
| Evidência | Explica uma consulta que escreveu | Identificar uma capacidade demonstrada |
| Hipótese | Talvez tenha dificuldade com agrupamentos | Propor uma tarefa para verificar |

Não transforme uma hipótese em sentença. “Não explicou esta consulta hoje” é diferente de “não sabe banco de dados”.

Também não reduza a pessoa ao código que produziu em poucos minutos. Peça explicações, permita consulta quando isso fizer sentido e observe como ela investiga.

## 3. Esclareça a direção

“Quero ser engenheiro de software” é um ponto de partida. Explore:

- Que tipo de problema gostaria de resolver?
- Que atividades já experimentou?
- Está buscando primeiro contato, estágio, primeira vaga ou evolução no trabalho?
- Tem preferência por web, mobile, backend, qualidade ou outra área?
- Quais condições de estudo consegue sustentar?

Se a pessoa ainda não conhece as áreas, o primeiro plano pode ser exploratório. Não é necessário forçar uma especialização antes de qualquer experiência.

Um objetivo inicial possível é: “Experimentar desenvolvimento backend e construir uma pequena funcionalidade para decidir se quero aprofundar essa área”.

Mais adiante, pode evoluir para: “Preparar evidências das competências exigidas em oportunidades de desenvolvimento backend compatíveis com meu momento”.

Quando usar vagas como referência, consulte anúncios atuais, registre a data e procure padrões. Uma vaga isolada não representa todo o mercado.

## 4. Investigue condições sem invadir a privacidade

Pergunte apenas o necessário para tornar o plano executável:

- Quantos períodos de estudo cabem na semana?
- Há acesso regular a um computador?
- Instalar ferramentas é possível?
- Que formato de material facilita o estudo?
- Existe alguma adaptação que a pessoa queira solicitar?

Não é necessário coletar renda, diagnóstico médico, documentos ou detalhes familiares para montar um primeiro exercício. A pessoa pode dizer apenas que tem duas horas disponíveis por semana.

Combine o que será registrado e para qual finalidade. Em materiais públicos, use exclusivamente casos fictícios e exemplos sem identificação.

## 5. Use uma matriz pequena de competências

Comece com dimensões relacionadas ao objetivo:

| Dimensão | Evidência possível |
|---|---|
| Decomposição | Divide uma tarefa em partes e identifica entradas e saídas |
| Programação | Implementa e explica uma regra simples |
| Depuração | Formula hipóteses e verifica uma falha |
| Dados | Explica como representar e preservar informação |
| Qualidade | Escolhe exemplos que poderiam revelar defeitos |
| Colaboração | Comunica uma alteração e considera feedback |
| Autonomia | Usa documentação e reconhece quando precisa de ajuda |

Não aplique todas as dimensões como um exame obrigatório no primeiro encontro. Selecione duas ou três para a tarefa escolhida.

Use uma escala descritiva:

- **Ainda não observado:** falta evidência suficiente.
- **Com apoio:** realiza com perguntas, exemplos ou orientação.
- **Com autonomia no caso observado:** realiza e explica a tarefa.
- **Com transferência:** adapta a ideia a uma situação diferente e explica consequências.

Essa escala não classifica junioridade, senioridade ou empregabilidade. Uma pessoa pode demonstrar autonomia em uma tarefa simples e precisar de apoio em outra mais complexa.

## 6. Tarefa diagnóstica para quem está começando

Retome o sistema de agendamento. Apresente a regra:

> Uma reserva só pode ser criada se o horário estiver disponível e ainda não tiver começado.

Peça que a pessoa descreva, em linguagem natural ou pseudocódigo:

1. quais informações precisa receber;
2. quais verificações fará;
3. o que devolverá em cada situação;
4. quais exemplos usará para conferir a regra.

Se a pessoa ainda não programa, não transforme sintaxe em barreira. Observe primeiro a lógica.

Uma resposta inicial possível:

~~~text
Se o horário já começou:
    informar que a reserva não é permitida
Senão, se o horário não está disponível:
    informar indisponibilidade
Senão:
    confirmar a reserva
~~~

Esse pseudocódigo demonstra organização básica da decisão. Ele não resolve persistência, autorização ou concorrência. Essas limitações devem ser reconhecidas de acordo com o nível esperado, sem exigir conhecimento avançado de quem está iniciando.

## 7. Tarefa para quem já programa

Peça uma implementação pequena na linguagem que a pessoa conhece. Forneça a hora atual como entrada para facilitar exemplos reproduzíveis.

~~~text
podeReservar(disponivel, inicio, agora)

Retorna verdadeiro apenas se:
- disponivel for verdadeiro;
- inicio for posterior a agora.
~~~

Solicite exemplos para:

| Disponibilidade | Início em relação a agora | Resultado esperado |
|---|---|---|
| Disponível | Futuro | Verdadeiro |
| Indisponível | Futuro | Falso |
| Disponível | Passado | Falso |
| Disponível | Exatamente agora | Falso |

Declare que início e agora usam a mesma referência temporal. Fuso horário e horário de verão serão temas posteriores quando o domínio exigir.

Depois, mude a regra: “A reserva deve ocorrer com pelo menos 30 minutos de antecedência”. Para este exercício, exatamente 30 minutos é permitido.

Observe se a pessoa altera a lógica e acrescenta exemplos abaixo, no limite e acima desse intervalo. A mudança testa transferência de conhecimento.

Se usar IA, peça que explique o resultado, confira os exemplos e faça a alteração de regra. A autoria manual de cada linha não é a única evidência relevante; compreensão e verificação continuam necessárias.

## 8. Como observar sem assumir a tarefa

Quando surgir uma dificuldade, aumente a ajuda gradualmente:

1. peça que a pessoa explique o resultado esperado;
2. pergunte qual valor ela gostaria de inspecionar;
3. sugira um exemplo menor;
4. mostre uma técnica em outro problema;
5. retorne à tarefa para que ela aplique.

Registre o apoio oferecido. “Concluiu após uma pergunta sobre o limite de horário” informa mais do que uma nota isolada.

Se o bloqueio for configurar o ambiente, isso não prova dificuldade de lógica. Um editor simples ou pseudocódigo pode permitir que a avaliação continue, enquanto a configuração vira uma ação separada.

## 9. Caso fictício: estudante que depende de tutoriais

Considere uma pessoa fictícia que completou aulas introdutórias, consegue reproduzir exemplos e dispõe de quatro horas semanais para estudar.

Na tarefa, ela escreveu uma condição, mas precisou de apoio para diferenciar horário futuro de horário igual ao atual. Também não havia criado exemplos de teste.

Um registro adequado seria:

| Observação | Interpretação provisória | Próxima ação |
|---|---|---|
| Explicou entradas e saída | Há compreensão inicial da função | Manter uma tarefa pequena |
| Precisou de ajuda no limite temporal | Precisa praticar condições de fronteira | Criar exemplos no limite |
| Não verificou entradas alternativas | Falta hábito de testar hipóteses | Escrever casos antes de concluir |
| Explicou a alteração após uma pista | Há possibilidade de avançar com apoio leve | Repetir com outra regra |

Evite concluir que ela precisa começar toda a formação novamente. O plano deve atacar a lacuna observada.

## 10. Plano de duas semanas

Para o caso fictício, proponha oito horas totais, distribuídas de modo negociado.

**Objetivo:** implementar e explicar a regra de reserva, verificar situações de fronteira e adaptar a antecedência mínima.

| Período | Atividade | Tempo sugerido | Evidência |
|---|---|---|---|
| Semana 1 | Escrever entradas, saída e exemplos | 1 hora | Tabela de casos |
| Semana 1 | Implementar a função | 2 horas | Código executável |
| Semana 1 | Explicar e registrar uma dúvida | 1 hora | Explicação curta |
| Semana 2 | Alterar a antecedência para 30 minutos | 2 horas | Código e novos exemplos |
| Semana 2 | Investigar uma falha encontrada | 1 hora | Hipótese e verificação |
| Semana 2 | Revisar com o mentor | 1 hora | Feedback e próximo passo |

Esses tempos são estimativas de planejamento. Se a tarefa exigir mais esforço, reduza o escopo ou revise o prazo. Não use a estimativa como julgamento de capacidade.

**Critério de conclusão:** a pessoa demonstra os quatro casos iniciais, inclui os três casos de fronteira da nova regra, explica a comparação temporal e identifica uma limitação da solução.

**Se houver bloqueio:** levar um exemplo que não funciona, resultado esperado, resultado observado e uma tentativa realizada. Não exigir que a pessoa fique travada por dias antes de pedir ajuda.

## 11. Feedback específico

Compare duas formas de comentar a entrega.

**Pouco acionável:** “Você precisa melhorar sua lógica”.

**Mais útil:** “Na situação em que o horário é exatamente agora, sua função permite a reserva. A regra pede que o início seja posterior. Compare os operadores usados e acrescente um exemplo para esse limite.”

Esse feedback descreve observação, consequência e próximo passo. Em seguida, pergunte como a pessoa entendeu a mudança.

Também reconheça escolhas adequadas: “Você passou a hora atual como parâmetro; isso tornou os exemplos reproduzíveis”. Um elogio específico ajuda a identificar o comportamento que merece ser mantido.

## 12. Como revisar o plano

Na revisão, pergunte:

- O que foi concluído e demonstrado?
- Que parte exigiu mais ajuda?
- A dificuldade estava no conceito, na ferramenta ou no tamanho da tarefa?
- A disponibilidade prevista se confirmou?
- Que atividade produziu mais aprendizado?
- Qual próximo passo a pessoa considera viável?

Se os objetivos foram alcançados, avance para uma variação ou integração pequena. Se houve dificuldade, ajuste o exercício. Se o interesse mudou, reavalie a direção.

Não interprete todo atraso como falta de dedicação. Investigue o que aconteceu antes de aconselhar.

## 13. Modelo reutilizável

~~~text
Objetivo do ciclo:
Contexto necessário, sem dados pessoais:
Competências observadas:
Evidências:
Apoio utilizado:
Hipóteses a verificar:
Prioridade escolhida:
Entrega:
Critérios de conclusão:
Disponibilidade combinada:
Como pedir ajuda:
Data ou momento de revisão:
Decisão após a revisão:
~~~

Para praticar publicamente, preencha apenas com personagens e dados inventados. Registros reais de acompanhamento precisam de tratamento próprio e acesso adequado.

## 14. Exercícios do mentor

**Exercício 1 — Separar observação de julgamento**

Reescreva: “O estudante é fraco em programação”.

Resposta orientadora: “Na tarefa de reserva, precisou de apoio para combinar duas condições; vamos observar essa competência em outro exemplo”.

**Exercício 2 — Reduzir um plano excessivo**

Uma pessoa com duas horas semanais recebeu a tarefa de aprender uma linguagem, criar uma API, configurar banco e publicar na nuvem em uma semana. Refaça o plano.

Resposta orientadora: escolher uma única regra, descrevê-la, implementá-la se houver condições e verificar alguns exemplos. A infraestrutura pode esperar.

**Exercício 3 — Não confundir consulta com incapacidade**

O estudante consultou documentação para lembrar um operador, mas explicou corretamente a regra e seus testes. Como registrar?

Resposta orientadora: autonomia na solução observada com consulta à documentação. Memorizar toda a sintaxe não é requisito para demonstrar raciocínio.

**Exercício 4 — Simular a revisão**

Faça uma conversa de dez minutos sobre uma entrega incompleta. Termine com uma alteração concreta no plano e peça que a pessoa explique o compromisso assumido.

## 15. Checklist de conclusão

- [ ] Diferencio relato, evidência e hipótese.
- [ ] Escolho uma tarefa compatível com o objetivo.
- [ ] Registro o apoio necessário sem rotular a pessoa.
- [ ] Defino uma entrega que cabe na disponibilidade combinada.
- [ ] Especifico como verificar a conclusão.
- [ ] Dou feedback sobre um comportamento observável.
- [ ] Incluo uma forma de pedir ajuda e revisar o plano.
- [ ] Preservo autonomia e privacidade.

## 16. Próxima etapa

A Lição 3, prevista na [trilha de formação](engenharia-software-mentores-trilha.md), aprofundará lógica, decomposição e depuração: como investigar um problema, formular hipóteses e ensinar sem entregar todas as respostas.

Antes de avançar, conclua uma simulação de diagnóstico e preencha o modelo com um caso fictício.

## Fontes e materiais de apoio

- [IEEE Computer Society — SWEBOK](https://www.computer.org/education/bodies-of-knowledge/software-engineering): contexto das competências técnicas da disciplina.
- [Princípios do Manifesto Ágil](https://agilemanifesto.org/iso/ptbr/principles.html): referência geral para reflexão e adaptação do trabalho.
- [Lição 1 — Fundamentos](engenharia-software-mentores-licao-1-fundamentos.md): conceitos utilizados no projeto fictício.

*Nota editorial: a matriz, o plano de duas semanas e a sequência de ajuda são propostas didáticas próprias, não instrumentos psicométricos ou critérios oficiais de contratação. Devem ser adaptados e revistos com o mentorado.*

**Metadescrição:** Aprenda a diagnosticar competências em software, avaliar evidências e construir um plano de estudos viável com seu mentorado.

**Tags:** Mentoria em Software, Engenharia de Software, Diagnóstico de Competências, Plano de Estudos, Feedback.
