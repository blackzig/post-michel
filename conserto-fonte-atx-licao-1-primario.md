---
title: "Conserto de Fonte ATX — Lição 1: Entrada, Primário e Diagnóstico Inicial"
seo_title: "Conserto de Fonte ATX Lição 1: primário, fusível, NTC e diodos"
slug: "conserto-fonte-atx-licao-1-primario"
meta_description: "Aprenda os fundamentos do conserto de fonte ATX: segurança, desmontagem, primário, fusível, NTC, ponte retificadora, diodos e diagnóstico inicial com multímetro."
focus_keyphrase: "conserto de fonte ATX"
related_keywords:
  - "consertar fonte ATX"
  - "fonte ATX não liga"
  - "teste de fonte ATX"
  - "fusível fonte ATX"
  - "NTC fonte ATX"
  - "ponte retificadora fonte ATX"
  - "diodo fonte ATX"
  - "multímetro fonte ATX"
category: "Eletrônica"
tags:
  - "Fonte ATX"
  - "Eletrônica"
  - "Conserto de Computadores"
  - "Fonte Chaveada"
  - "Multímetro"
  - "Primário"
  - "Diagnóstico Eletrônico"
excerpt: "Primeira lição de conserto de fontes ATX: conheça a fonte por dentro, entenda o caminho da energia no primário e aprenda a testar fusível, NTC, diodos e ponte retificadora com segurança."
---

# ⚡ Conserto de Fonte ATX — Lição 1: Entrada, Primário e Diagnóstico Inicial

Esta é a primeira lição de uma sequência estruturada para aprender **conserto de fontes ATX de computadores**.

O objetivo não será decorar componentes nem sair trocando peças aleatoriamente.

Vamos aprender a raciocinar desta forma:

```text
sintoma
   ↓
bloco provável
   ↓
teste
   ↓
componente suspeito
   ↓
confirmação
   ↓
reparo
   ↓
teste final
```

Nesta primeira lição, vamos estudar principalmente a **entrada da fonte e o início do circuito primário**, acompanhando o caminho que a energia percorre desde a tomada.

O material-base desta aula apresenta fontes ATX tradicionais, inclusive modelos antigos com seletor manual de `115/230 V`. Ao longo da lição, também vamos diferenciar esses projetos das fontes modernas com entrada automática.

## 1. O que vamos dominar hoje

Nesta primeira lição, você aprenderá:

- o que é uma fonte ATX;
- como identificar suas partes externas;
- como abrir uma fonte de maneira organizada;
- o que significa lado primário e lado secundário;
- como acompanhar trilhas da placa;
- o caminho inicial da energia elétrica;
- a função do fusível;
- a função do NTC;
- a função dos diodos retificadores;
- o que faz uma ponte retificadora;
- o papel dos capacitores do primário;
- como procurar curto-circuito sem energizar a fonte;
- como testar fusível, NTC e diodos com multímetro;
- erros comuns que podem destruir componentes novos.

A meta desta aula é que você consiga olhar para uma fonte aberta e reconhecer o **primeiro bloco de alimentação**.

## 2. Antes de qualquer coisa: segurança

Uma fonte ATX não deve ser tratada como um circuito eletrônico de baixa tensão comum.

Dentro dela existe uma região ligada diretamente à rede elétrica.

Mesmo depois que o cabo de força é removido, os capacitores do primário podem permanecer carregados com **tensão perigosa**.

Em uma fonte ligada a uma rede de aproximadamente `127 V AC`, uma retificação convencional pode produzir perto de:

```text
127 × 1,414 ≈ 180 V DC
```

Em `220 V AC`:

```text
220 × 1,414 ≈ 311 V DC
```

Fontes modernas com PFC ativo podem trabalhar com um barramento próximo da faixa de `380 a 400 V DC`.

> Uma fonte desligada da tomada não deve ser considerada automaticamente descarregada.

Nesta lição, os testes práticos serão feitos **com a fonte desconectada da rede e com o circuito descarregado**.

Medições com o primário energizado exigem conhecimento adicional, instrumentos adequados e procedimentos de bancada que estudaremos separadamente.

## 3. Nunca descarregue capacitor provocando curto

Não use chave de fenda, alicate ou outro objeto metálico para curto-circuitar diretamente os terminais de um capacitor de alta tensão.

Isso pode provocar:

- faísca intensa;
- corrente muito elevada;
- danos às trilhas;
- danos ao capacitor;
- projeção de metal;
- susto e acidente.

O procedimento adequado envolve descarga controlada e confirmação com instrumento de medição.

Nesta fase do curso, a regra será simples:

> Antes de tocar ou medir o primário, confirme que a fonte está fora da tomada e que os capacitores não apresentam tensão perigosa.

## 4. O que é uma fonte ATX

A fonte ATX transforma a energia da rede elétrica em tensões adequadas para os componentes do computador.

De forma simplificada:

```text
Rede elétrica AC
      ↓
Entrada e proteção
      ↓
Retificação
      ↓
Alta tensão DC
      ↓
Chaveamento
      ↓
Transformador
      ↓
Retificação secundária
      ↓
Filtragem
      ↓
12 V / 5 V / 3,3 V e outras tensões
```

Nesta aula estudaremos apenas o começo desse caminho.

## 5. Conhecendo a parte externa

Em uma fonte ATX podemos encontrar:

- entrada para o cabo de força;
- ventoinha;
- etiqueta com especificações;
- cabos e conectores de saída;
- chave liga/desliga em alguns modelos;
- seletor `115/230 V` em fontes antigas.

### Chave liga/desliga

Parece óbvio, mas faz parte do diagnóstico.

Se a fonte possui chave traseira e ela estiver na posição desligada, o computador não receberá alimentação.

Antes de abrir qualquer equipamento, comece pelo básico.

### Seletor 115/230 V

Fontes ATX antigas podem possuir uma chave seletora.

Esse seletor precisa estar configurado corretamente para a rede utilizada.

Uma seleção incorreta pode provocar falha grave.

Fontes modernas normalmente possuem entrada automática, frequentemente indicada por algo semelhante a:

```text
100-240 V~
50-60 Hz
```

Nesse caso, não existe seletor manual.

## 6. Leia a etiqueta antes de abrir a fonte

Antes de desmontar a fonte, observe a etiqueta.

Procure informações como:

- fabricante;
- modelo;
- potência declarada;
- tensão de entrada;
- frequência;
- correntes das saídas;
- presença de entrada universal.

Isso já ajuda a entender que tipo de projeto você encontrará.

Uma fonte antiga de 250 W com seletor manual pode ser completamente diferente de uma fonte moderna de 650 W com PFC ativo.

## 7. Organização durante a desmontagem

Para abrir uma fonte ATX, normalmente removemos os parafusos da tampa.

Uma prática simples evita vários problemas:

> Separe imediatamente todos os parafusos em um recipiente.

Parafusos soltos na bancada podem:

- cair dentro da fonte;
- provocar curto posteriormente;
- desaparecer;
- ser confundidos com outros parafusos.

Também observe a posição da ventoinha, dos cabos e das peças antes de desmontar.

Fotos tiradas antes da desmontagem podem ajudar muito na montagem.

## 8. Limpeza inicial

Fontes antigas frequentemente acumulam muita poeira.

A sujeira dificulta:

- inspeção visual;
- identificação dos componentes;
- leitura das marcações;
- análise das soldas.

Para limpeza inicial, podemos utilizar ferramentas adequadas, como pincel antiestático e equipamentos próprios para remoção de poeira.

Evite produzir eletricidade estática ou danificar componentes mecanicamente.

A limpeza também permite encontrar defeitos visuais como:

- capacitor estufado;
- componente escurecido;
- resistor carbonizado;
- trilha queimada;
- solda trincada;
- conector aquecido.

## 9. Primário e secundário

Uma fonte chaveada é dividida em regiões.

As duas mais importantes para começar são:

### Primário

É o lado ligado à rede elétrica.

Aqui encontramos componentes como:

- fusível;
- filtro de entrada;
- NTC;
- ponte retificadora ou diodos;
- capacitores de alta tensão;
- transistores ou MOSFETs de chaveamento;
- circuito de controle primário.

### Secundário

É o lado eletricamente isolado da rede pelo transformador principal.

Aqui encontramos os circuitos que geram e filtram as tensões utilizadas pelo computador.

Por enquanto, concentre-se nesta ideia:

```text
Tomada → primário → transformador → secundário → computador
```

## 10. O caminho inicial da energia

No material-base, acompanhamos fisicamente as trilhas da placa.

Essa é uma habilidade fundamental.

O caminho simplificado pode ser representado assim:

```text
Rede AC
  ↓
Fusível
  ↓
Proteção / filtro de entrada
  ↓
NTC
  ↓
Ponte retificadora ou quatro diodos
  ↓
Capacitor(es) do primário
  ↓
Circuito de chaveamento
```

Em fontes reais, a ordem exata de alguns componentes pode variar.

Também podem existir:

- MOV;
- bobinas;
- capacitores X;
- capacitores Y;
- filtros EMI;
- relés;
- circuitos de PFC.

Vamos estudá-los em lições posteriores.

## 11. Aprendendo a seguir trilhas

Uma placa de circuito impresso contém trilhas de cobre que interligam os componentes.

Ao observar uma fonte, não pense apenas:

> “Este componente parece ser um fusível.”

Pense:

> “De onde esta trilha vem e para onde ela vai?”

Esse raciocínio permite reconstruir o circuito mesmo sem esquema elétrico.

Exemplo:

```text
entrada AC
   ↓
trilha
   ↓
fusível
   ↓
trilha
   ↓
NTC
   ↓
trilha
   ↓
ponte retificadora
```

Essa habilidade será utilizada durante todo o curso.

## 12. O que é um jumper

Em algumas placas você encontrará pequenos fios rígidos atravessando trechos da placa.

Eles funcionam como ligações elétricas e podem ser chamados de **jumpers**.

Um jumper pode ser usado quando o projetista precisa levar uma conexão de um ponto a outro sem criar uma trilha naquele caminho.

Para o diagnóstico, trate o jumper como qualquer outro condutor:

```text
ponto A ───── jumper ───── ponto B
```

Ele também pode apresentar:

- solda quebrada;
- corrosão;
- rompimento mecânico.

## 13. O fusível

O fusível é um dos primeiros componentes que encontramos no caminho da rede elétrica.

Sua função principal é interromper a corrente quando ocorre uma condição anormal suficientemente intensa.

Se o fusível estiver aberto, a fonte pode ficar completamente morta.

Porém existe uma regra essencial:

> Fusível queimado normalmente é consequência de um problema, não necessariamente a causa principal.

Se você simplesmente trocar o fusível sem descobrir por que ele abriu, o novo fusível pode queimar imediatamente.

## 14. Nunca aumente o valor do fusível por tentativa

Não substitua um fusível por outro de corrente maior simplesmente para evitar que ele queime.

O componente de reposição deve respeitar as especificações do projeto.

Observe:

- corrente nominal;
- tensão nominal;
- tipo rápido ou retardado;
- dimensões;
- características especificadas pelo fabricante.

Exemplo:

```text
T3.15A 250V
```

O `T` normalmente indica fusível de ação retardada.

A escolha do fusível faz parte da proteção do equipamento.

## 15. Testando o fusível

Com a fonte:

- desconectada da tomada;
- descarregada;
- sem alimentação externa;

coloque o multímetro em continuidade ou resistência.

Meça as duas extremidades do fusível.

Um fusível íntegro deve apresentar continuidade e resistência muito baixa.

Um fusível aberto não apresentará continuidade.

Representação:

```text
fusível bom:
[ ponta ]──────[ fusível ]──────[ ponta ]
                  continuidade

fusível aberto:
[ ponta ]────── X ──────────────[ ponta ]
                sem continuidade
```

Se estiver aberto, não coloque outro antes de investigar curto no primário.

## 16. O NTC

O NTC é um resistor cujo valor diminui quando sua temperatura aumenta.

Em fontes ATX ele é frequentemente utilizado para limitar a corrente de partida.

Ao ligar a fonte, os capacitores do primário estão descarregados e inicialmente podem puxar uma corrente elevada.

O NTC ajuda a reduzir esse impacto.

Depois que aquece, sua resistência diminui.

## 17. Como reconhecer um NTC

Ele costuma ter formato de disco.

Pode aparecer em cores como:

- preto;
- verde;
- azul;
- outras, dependendo do fabricante.

Não confie apenas na cor.

Leia a marcação do componente sempre que possível.

## 18. Testando o NTC

Com a fonte desligada e descarregada, podemos medir sua resistência.

Um NTC de entrada normalmente apresenta resistência relativamente baixa quando frio, mas **não existe um valor universal válido para todas as fontes**.

O valor depende de:

- modelo;
- temperatura;
- corrente prevista;
- projeto da fonte.

Portanto, não adote como regra:

```text
"todo NTC deve medir menos de 10 ohms"
```

Alguns podem apresentar poucos ohms, outros valores diferentes.

O melhor procedimento é:

1. ler a marcação;
2. consultar a especificação quando possível;
3. verificar se o componente não está aberto;
4. verificar se há sinais de trinca ou dano térmico;
5. comparar a medição com o valor esperado do componente.

## 19. NTC aberto

Se o NTC estiver eletricamente aberto no caminho de entrada, a alimentação pode não chegar aos estágios seguintes.

O sintoma pode ser:

- fonte completamente morta;
- ausência de funcionamento do circuito de standby;
- nenhuma reação ao comando de partida.

Entretanto, antes de condenar o NTC, verifique também suas soldas.

## 20. Solda trincada pode imitar componente aberto

Um componente bom pode parecer defeituoso se sua solda estiver quebrada.

Isso acontece bastante em componentes:

- pesados;
- que aquecem;
- submetidos a vibração;
- próximos da entrada.

Inspecione com boa iluminação e, se necessário, lupa.

Procure:

- anel ao redor do terminal;
- solda opaca;
- fissura;
- terminal solto.

Não faça ressolda indiscriminada.

Primeiro identifique a falha.

## 21. Ponte retificadora e diodos

Depois da entrada e dos elementos de proteção, encontramos o estágio de retificação.

Algumas fontes utilizam quatro diodos individuais.

Outras utilizam uma ponte retificadora encapsulada.

Eletricamente, a ponte contém quatro diodos organizados para retificação de onda completa.

Representação simplificada:

```text
       AC ~
        │
   ┌────┴────┐
   │  ponte  │
   │retific. │
   └────┬────┘
        │
      + DC -
```

A entrada é corrente alternada.

A saída é uma tensão pulsante com polaridade definida, que será filtrada pelos capacitores do primário.

## 22. O diodo conduz em um sentido

Um diodo de silício em boas condições normalmente conduz em um sentido e bloqueia no sentido inverso.

No multímetro em modo diodo, uma leitura típica pode ficar aproximadamente na faixa de:

```text
0,4 V a 0,8 V
```

dependendo do componente e das condições de teste.

Alguns multímetros mostram isso como números como:

```text
400
500
600
700
```

quando a escala representa milivolts.

O importante não é decorar um número exato.

O importante é entender o comportamento:

```text
sentido direto  → conduz
sentido reverso → bloqueia
```

## 23. Testando um diodo

Com o circuito desligado e descarregado:

1. coloque o multímetro no modo diodo;
2. meça o componente em um sentido;
3. inverta as pontas;
4. compare as leituras.

Comportamento esperado:

```text
sentido 1 → queda de tensão
sentido 2 → OL / aberto
```

Se medir praticamente zero nos dois sentidos, pode existir curto.

Se aparecer aberto nos dois sentidos, o diodo pode estar aberto.

A medição dentro do circuito pode ser influenciada por outros componentes.

Quando houver dúvida, pode ser necessário isolar um terminal.

## 24. A faixa no corpo do diodo

Diodos possuem polaridade.

A faixa impressa no corpo normalmente identifica o cátodo.

Ao substituir um diodo, a orientação precisa ser respeitada.

Antes de remover o componente:

- fotografe;
- marque a posição;
- observe a serigrafia da placa.

Montar um diodo invertido pode impedir o funcionamento e provocar danos.

## 25. Diodo em curto e fusível queimado

Um defeito muito comum no primário é curto em um componente da retificação ou do chaveamento.

Quando um diodo da ponte entra em curto, ele pode provocar corrente muito elevada.

O resultado pode ser:

```text
diodo em curto
      ↓
corrente excessiva
      ↓
fusível abre
      ↓
fonte morta
```

Por isso:

> Depois de encontrar um fusível aberto, investigue curto antes de instalar um fusível novo.

## 26. Testando uma ponte retificadora

Uma ponte retificadora costuma possuir quatro terminais:

```text
~
~
+
-
```

Os dois terminais `~` recebem corrente alternada.

Os terminais `+` e `-` fornecem a saída retificada.

Como internamente existem quatro diodos, o teste é feito verificando os caminhos de condução entre:

- cada terminal AC e `+`;
- `-` e cada terminal AC;

sempre observando condução em um sentido e bloqueio no outro.

Não trate todos os terminais como equivalentes.

Primeiro identifique a marcação da ponte.

## 27. Capacitores do primário

Depois da retificação encontramos um ou mais capacitores eletrolíticos de alta tensão.

Em fontes antigas com seletor manual, é comum encontrar dois capacitores trabalhando em uma configuração associada ao seletor de tensão.

Em projetos modernos, é comum encontrar um grande capacitor de barramento após a retificação e, quando existe PFC ativo, após o estágio de correção do fator de potência.

Esses capacitores armazenam energia.

É justamente por isso que continuam perigosos mesmo depois que a fonte sai da tomada.

## 28. Capacitor pode carregar durante a própria medição

Ao medir resistência em um circuito que contém capacitores, você pode observar a leitura mudar.

Isso acontece porque o multímetro injeta uma pequena corrente e o capacitor começa a carregar.

Por exemplo:

```text
leitura inicialmente baixa
        ↓
valor aumenta
        ↓
tende a uma resistência maior
```

Portanto, não interprete automaticamente uma leitura momentaneamente baixa como curto.

Curto verdadeiro tende a permanecer com resistência muito baixa.

Mesmo assim, circuitos paralelos podem alterar a leitura.

## 29. Não existe a regra "tem que dar infinito"

Em uma placa montada, os capacitores estão ligados a vários outros componentes.

Podem existir:

- resistores de descarga;
- resistores de partida;
- redes de medição;
- semicondutores;
- bobinas;
- circuitos auxiliares.

Por isso, uma medição de resistência no barramento não precisa necessariamente mostrar infinito.

O objetivo é identificar um **curto persistente ou resistência anormalmente baixa**, considerando o circuito.

## 30. Procurando curto antes de energizar

Depois de encontrar fusível queimado, uma sequência inicial mais segura é:

```text
1. Fonte fora da tomada
2. Confirmar descarga do primário
3. Inspeção visual
4. Testar fusível
5. Verificar curto no barramento
6. Testar ponte/diodos
7. Verificar semicondutores do primário
8. Só depois pensar em energização controlada
```

Nesta primeira lição estudamos os itens até a retificação.

MOSFETs e transistores de potência serão estudados mais adiante.

## 31. Por que trocar apenas o fusível é um erro

Imagine:

```text
fusível queimado
      ↓
técnico troca o fusível
      ↓
diodo continua em curto
      ↓
fonte é ligada
      ↓
novo fusível queima
```

Agora imagine um defeito ainda mais grave.

O novo fusível pode não ser a única peça afetada.

Por isso o diagnóstico precisa vir antes da energização.

## 32. Nunca substitua componente por "um maior" sem critério

O material-base sugere em alguns momentos substituir certos componentes por equivalentes "iguais ou maiores".

Essa regra é perigosa quando aplicada sem análise.

Um componente deve ser substituído com base em parâmetros elétricos específicos.

Para um NTC, por exemplo, podem importar:

- resistência a 25 °C;
- corrente nominal;
- energia suportada;
- diâmetro;
- comportamento térmico.

Para um diodo:

- tensão reversa;
- corrente;
- velocidade;
- encapsulamento;
- dissipação.

Para um fusível:

- corrente;
- tensão;
- curva de atuação;
- capacidade de interrupção.

> Em eletrônica, "maior" não significa automaticamente "melhor".

## 33. O que podemos aprender pela inspeção visual

Antes do multímetro, use os olhos.

Procure:

- fusível escurecido;
- NTC trincado;
- diodo rachado;
- ponte queimada;
- capacitor estufado;
- vazamento;
- placa escurecida;
- solda quebrada;
- trilha rompida;
- componente carbonizado.

Mas lembre-se:

> Um componente pode estar eletricamente defeituoso e parecer perfeito externamente.

A inspeção visual é o começo, não o diagnóstico completo.

## 34. Ferramentas úteis nesta etapa

Para esta fase do curso, são úteis:

- chave Phillips adequada;
- pinça;
- multímetro digital;
- lupa;
- iluminação de bancada;
- pincel apropriado;
- recipiente para parafusos;
- câmera ou celular para registrar posições.

Mais adiante adicionaremos outras ferramentas.

## 35. O multímetro será nossa principal ferramenta

Nesta lição usamos principalmente:

### Continuidade

Para verificar caminhos elétricos simples.

Exemplos:

- fusível;
- jumper;
- trilha;
- conexão.

### Resistência

Para procurar:

- componente aberto;
- resistência anormal;
- curto persistente.

### Modo diodo

Para testar:

- diodos;
- junções de semicondutores;
- ponte retificadora.

Nesta etapa, não precisamos energizar a fonte para aprender esses testes.

## 36. Diagnóstico inicial: fonte completamente morta

Imagine uma fonte que não apresenta nenhuma reação.

O raciocínio inicial pode ser:

```text
fonte morta
   ↓
há caminho elétrico de entrada?
   ↓
fusível está bom?
   ↓
NTC está íntegro?
   ↓
ponte retificadora está em curto ou aberta?
   ↓
existe curto no barramento?
```

Observe que ainda não estamos afirmando qual peça queimou.

Estamos eliminando possibilidades.

Esse é o raciocínio correto de diagnóstico.

## 37. Exemplo prático de sequência

Suponha que encontramos:

```text
fusível = aberto
```

Não troque imediatamente.

Faça:

```text
1. confirmar fusível aberto
2. inspecionar visualmente o primário
3. verificar curto no barramento
4. testar os quatro diodos ou a ponte
5. verificar se algum semicondutor está em curto
6. identificar a causa
7. substituir os componentes defeituosos
8. revisar soldas e trilhas
9. somente então preparar o teste de energização
```

É assim que começamos a deixar de ser "trocadores de peças" e passamos a diagnosticar.

## 38. Erros comuns de iniciante

### Erro 1 — trocar o fusível e ligar

Pode queimar novamente.

### Erro 2 — colocar fusível mais forte

Pode comprometer a proteção e aumentar os danos.

### Erro 3 — medir diodo em apenas um sentido

O teste precisa comparar os dois sentidos.

### Erro 4 — esquecer a orientação do diodo

Pode provocar falha ou dano.

### Erro 5 — considerar toda leitura baixa como curto

Capacitores e caminhos paralelos podem enganar.

### Erro 6 — confiar apenas na aparência

Muitos componentes defeituosos não apresentam dano visual.

### Erro 7 — tocar o primário porque a fonte está fora da tomada

O capacitor pode continuar carregado.

### Erro 8 — fazer medições energizadas cedo demais

O primário energizado possui tensão potencialmente letal.

## 39. Mapa mental desta primeira lição

Guarde este mapa:

```text
REDE ELÉTRICA
     │
     ▼
[FUSÍVEL]
     │
     ▼
[FILTRO / PROTEÇÃO]
     │
     ▼
[NTC]
     │
     ▼
[PONTE RETIFICADORA]
     │
     ▼
[CAPACITOR DO PRIMÁRIO]
     │
     ▼
[CHAVEAMENTO]
```

Nesta lição dominamos principalmente os quatro primeiros blocos.

## 40. Exercícios

Resolva antes de consultar novamente o texto.

### 1. Identificação

Explique com suas próprias palavras a diferença entre:

- primário;
- secundário.

### 2. Caminho da energia

Coloque os componentes em uma ordem lógica:

```text
ponte retificadora
fusível
capacitor do primário
NTC
rede elétrica
```

### 3. Fusível queimado

Uma fonte chegou com o fusível aberto.

Qual é a atitude correta?

A) colocar um fusível maior;

B) trocar o fusível e ligar imediatamente;

C) investigar curto e a causa da sobrecorrente antes de energizar;

D) retirar o fusível e colocar um fio.

### 4. Teste de diodo

Um diodo mede praticamente zero nos dois sentidos no modo diodo.

Qual defeito é provável?

### 5. NTC

Explique por que não podemos afirmar que todo NTC de fonte ATX precisa medir exatamente o mesmo valor.

### 6. Trilhas

Pegue uma placa eletrônica desligada e tente acompanhar uma trilha visualmente de um componente a outro.

Não precisa ser uma fonte ATX.

O objetivo é treinar a leitura física da placa.

## 41. Desafio — monte o diagnóstico no papel

Considere o seguinte caso:

```text
Sintoma:
fonte completamente morta

Inspeção:
fusível aberto

Medição:
um diodo da ponte conduz nos dois sentidos
```

Explique:

1. qual componente provavelmente apresenta defeito;
2. por que o fusível pode ter aberto;
3. por que não devemos simplesmente trocar o fusível;
4. o que deve ser verificado antes de energizar a fonte novamente.

## 42. Desafio extra — análise sem esquema

Imagine que você possui uma fonte antiga e nenhum esquema elétrico.

Você identifica:

- entrada AC;
- fusível;
- NTC;
- quatro diodos;
- dois grandes capacitores eletrolíticos.

Desenhe no papel um diagrama em blocos mostrando o caminho provável da energia.

Não precisa desenhar o circuito eletrônico completo.

O objetivo é aprender a transformar a placa física em um **modelo mental do circuito**.

## Meta antes da Lição 2

Antes de avançar, você deve conseguir explicar com suas próprias palavras:

- o que é uma fonte ATX;
- o que é o primário;
- por que o primário é perigoso;
- por que um capacitor pode continuar carregado;
- qual é a função do fusível;
- por que fusível queimado pode indicar outro defeito;
- o que faz um NTC;
- o que faz uma ponte retificadora;
- como um diodo deve se comportar no modo diodo;
- por que precisamos medir um diodo nos dois sentidos;
- por que a orientação do diodo importa;
- como seguir uma trilha da placa;
- por que não devemos aumentar o valor do fusível;
- por que não existe um único valor de resistência válido para todos os NTCs;
- por que devemos procurar curto antes de energizar uma fonte reparada.

Você também deve conseguir entender este fluxo:

```text
fonte morta
   ↓
inspeção visual
   ↓
fusível
   ↓
NTC
   ↓
retificação
   ↓
curto no primário?
   ↓
diagnóstico antes da troca
```

E, mais importante, deve entender a ideia central desta lição:

> **No conserto de uma fonte ATX, primeiro entendemos o caminho da energia. Depois medimos. Somente então substituímos componentes.**

Na **Lição 2**, avançaremos pelo primário para estudar com mais profundidade os **capacitores de alta tensão, transistores/MOSFETs de chaveamento, circuito de partida e os principais motivos que fazem uma fonte tentar ligar e desligar imediatamente**.
