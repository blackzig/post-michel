---
title: "Ganho, pico e loudness no Audacity: entenda o volume do áudio"
seo_title: "Ganho, pico e loudness no Audacity: guia completo"
slug: "ganho-pico-loudness-audacity"
meta_description: "Entenda ganho, pico, dBFS, clipping, headroom e loudness no Audacity para ajustar o volume sem distorcer nem depender de tentativas."
focus_keyphrase: "ganho, pico e loudness no Audacity"
related_keywords:
  - "volume percebido no Audacity"
  - "dBFS no Audacity"
  - "normalização de loudness"
  - "pico de áudio"
  - "clipping no Audacity"
  - "amplificar áudio no Audacity"
  - "normalizar áudio no Audacity"
category: "Áudio e Produção"
tags:
  - "Audacity"
  - "Masterização"
  - "Ganho"
  - "Loudness"
  - "LUFS"
  - "dBFS"
  - "Clipping"
  - "Normalização"
excerpt: "Aprenda por que um áudio pode ter picos altos e ainda parecer baixo, e use ganho, dBFS, headroom e loudness com segurança no Audacity."
featured_image_url: ""
featured_image_alt: "Forma de onda e medidores representando ganho, pico e loudness no Audacity"
featured_image_title: "Ganho, pico e loudness no Audacity"
featured_image_caption: "Conceitos de nível digital e volume percebido aplicados à masterização de voz no Audacity."
---

# Ganho, pico e loudness no Audacity: entenda o volume do áudio

Um áudio pode chegar perto do limite digital e, mesmo assim, parecer baixo. Também pode apresentar uma forma de onda grande, mas soar menos presente do que outra gravação visualmente menor. Essas situações confundem quem está começando porque parecem contradizer uma ideia intuitiva: se o pico está alto, o áudio deveria soar alto.

A explicação é que **pico e volume percebido não são a mesma coisa**. O pico mostra até onde os instantes mais fortes do sinal chegaram. O loudness procura representar a intensidade percebida ao longo de um trecho ou de todo o programa. Entre esses dois conceitos aparecem o ganho, o headroom, a dinâmica, a normalização e o clipping.

Nesta segunda lição da trilha de masterização, vamos estudar **ganho, pico e loudness no Audacity**. O objetivo não é apenas memorizar números, mas aprender a interpretar o áudio antes de aplicar qualquer efeito. Ao final, você saberá por que aumentar o ganho nem sempre resolve um som baixo, quando usar Amplificar, quando usar Normalizar e quando a Normalização de Loudness é a ferramenta mais adequada.

Você também aprenderá a identificar clipping, preservar margem de segurança e evitar um erro frequente: tentar corrigir um problema de dinâmica usando apenas aumento de volume.

## O que você aprenderá nesta lição

Ao concluir este conteúdo, você deverá ser capaz de:

- distinguir ganho, amplitude, pico e volume percebido;
- entender a escala dBFS usada no áudio digital;
- reconhecer clipping na forma de onda e nos medidores;
- compreender o significado de headroom;
- diferenciar pico de amostra, true peak, RMS e LUFS;
- interpretar por que duas faixas com o mesmo pico podem soar diferentes;
- escolher entre Amplificar, Normalizar e Normalização de Loudness;
- aplicar um fluxo seguro para ajustar voz no Audacity;
- evitar metas de volume arbitrárias e processamento destrutivo;
- preparar o áudio para as próximas etapas de equalização e compressão.

## 1. Ganho não é sinônimo de qualidade

O ganho representa uma alteração aplicada ao nível do sinal. Quando você aumenta o ganho, multiplica a amplitude do áudio. Quando reduz, diminui essa amplitude. Esse processo torna o sinal numericamente maior ou menor, mas não corrige automaticamente ruído, timbre, dinâmica, dicção ou distorção.

Imagine uma gravação de voz com ruído de fundo e variações intensas entre frases. Se você adicionar 6 dB de ganho a toda a faixa, a voz ficará mais forte, mas o ruído também aumentará. As frases baixas subirão, porém os picos altos subirão na mesma proporção. Se esses picos já estiverem próximos de 0 dBFS, o resultado poderá ultrapassar o limite digital e produzir clipping.

Portanto, ganho é uma ferramenta de nível, não uma ferramenta universal de melhoria.

Essa distinção muda a forma de pensar. Em vez de perguntar apenas “quanto posso aumentar?”, pergunte:

- o áudio está uniformemente baixo?
- existem picos isolados limitando o aumento?
- a voz está baixa porque foi gravada longe?
- a sensação de fraqueza vem da dinâmica ou do timbre?
- o ruído ficará excessivo quando o nível subir?
- o áudio já contém distorção na origem?

Se todo o material estiver baixo de maneira relativamente uniforme e houver espaço até o teto digital, aumentar o ganho pode ser suficiente. Se a média estiver baixa, mas existirem picos muito altos, será necessário controlar a dinâmica em outra etapa, normalmente com compressão ou automação de volume.

## 2. Amplitude e decibéis

A forma de onda exibida no Audacity representa a amplitude do sinal ao longo do tempo. Quanto mais distante a onda aparece da linha central, maior é a amplitude naquele instante. No áudio digital, essa amplitude costuma ser apresentada em decibéis relativos à escala completa, ou **dBFS**.

A relação entre amplitude e decibéis é logarítmica. Para comparar duas amplitudes, utiliza-se a expressão:

```text
diferença em dB = 20 × log10(amplitude final / amplitude inicial)
```

Você não precisa calcular isso manualmente durante a edição. O próprio Audacity trabalha com valores em decibéis. Entretanto, entender que a escala é logarítmica ajuda a interpretar os controles.

Um aumento de alguns decibéis pode ser bastante perceptível, mas os decibéis não devem ser confundidos diretamente com uma porcentagem simples. Somar 3 dB não significa “aumentar 3%”, e somar 6 dB não significa necessariamente que o ouvinte perceberá o conteúdo como duas vezes mais alto. A sensação de volume depende de frequência, duração, dinâmica, ambiente, equipamento e sensibilidade auditiva.

Na prática, use decibéis como uma unidade de comparação e ajuste. Teste mudanças pequenas, compare antes e depois e observe se o áudio continua natural.

## 3. O que significa 0 dBFS

Na escala digital, **0 dBFS representa o maior nível de amostra permitido antes que o sinal exceda a escala disponível**. Diferentemente de alguns contextos analógicos, em que valores acima de zero podem representar margem operacional, no dBFS o zero fica no topo.

Por isso, os valores normais aparecem como números negativos:

```text
-24 dBFS
-18 dBFS
-12 dBFS
-6 dBFS
-1 dBFS
0 dBFS
```

Quanto mais próximo de zero, maior é o nível da amostra. Assim, -3 dBFS é mais alto do que -12 dBFS.

Se um processo tentar criar amostras acima de 0 dBFS em um formato digital de ponto fixo, os valores que excederiam o limite não poderão ser representados corretamente. O topo da forma de onda é cortado, e surge o clipping.

Esse teto explica por que simplesmente aumentar o ganho possui limite. Um arquivo pode ter fala média baixa, mas um único estalo, uma batida no microfone ou uma sílaba muito forte pode chegar a -1 dBFS. Nesse caso, resta pouco espaço para ampliar toda a faixa sem ultrapassar o zero.

## 4. O que é pico de áudio

O pico é o ponto de maior amplitude encontrado no sinal ou na seleção analisada. Quando o Audacity informa que o novo pico será -1 dB, está indicando que o instante mais forte ficará um decibel abaixo do teto digital.

O pico é importante para evitar sobrecarga. Porém, ele não descreve sozinho a experiência de escuta.

Considere duas narrações:

- a primeira possui pico em -1 dBFS, mas quase toda a fala permanece em nível muito baixo;
- a segunda também possui pico em -1 dBFS, mas a maior parte da fala permanece mais próxima desse pico.

As duas têm o mesmo valor máximo, mas a segunda provavelmente parecerá mais forte e consistente.

O pico da primeira faixa pode ter sido causado por um evento muito curto: uma consoante explosiva, uma batida, um clique ou uma palavra dita com intensidade excepcional. Esse instante ocupa o espaço disponível, embora não represente a energia média da narração.

Essa é uma das razões pelas quais a normalização de pico não garante igualdade de volume percebido entre arquivos.

## 5. Pico de amostra e true peak

O pico mostrado diretamente pelas amostras é chamado de pico de amostra. Cada amostra digital registra um valor em um instante específico. No entanto, durante a reprodução, o conversor reconstrói uma forma de onda contínua entre essas amostras.

Essa reconstrução pode produzir um ponto máximo ligeiramente superior aos valores individuais armazenados. O nível estimado dessa onda reconstruída é chamado de **true peak**, normalmente expresso em dBTP.

Isso significa que um arquivo cujas amostras não ultrapassam 0 dBFS ainda pode apresentar picos entre amostras muito próximos ou, em determinadas condições de conversão e codificação, acima do limite seguro do sistema de reprodução.

Para iniciantes, a consequência prática é simples: deixar alguma margem abaixo de 0 dBFS é mais prudente do que empurrar todos os arquivos exatamente até o teto. Normalizar para -1 dBFS oferece uma pequena folga, embora não substitua uma medição dedicada de true peak quando uma especificação profissional exigir dBTP.

O Audacity permite controlar o pico de amostra com Amplificar e Normalizar. Quando o trabalho exigir conformidade rigorosa de true peak, utilize medição compatível com a especificação de entrega e verifique se o fluxo inteiro, incluindo a codificação final, preserva a margem necessária.

## 6. Clipping: quando o sinal ultrapassa o limite

Clipping ocorre quando o sinal excede o nível que pode ser representado. Os picos são cortados, e a forma de onda pode apresentar regiões achatadas no topo ou na parte inferior.

No Audacity, você pode ativar a visualização de amostras cortadas em:

```text
Exibir > Mostrar clipping na forma de onda
```

Dependendo da tradução e da versão, o nome pode aparecer de forma ligeiramente diferente. Quando ativado, o programa destaca em vermelho as amostras que tocam ou excedem o limite reconhecido pela visualização.

O clipping pode produzir distorção áspera, estalos ou perda de definição. Um trecho intensamente cortado perde informação. Reduzir o ganho depois não reconstrói automaticamente a forma original; apenas torna a distorção mais baixa.

Observe a diferença entre estas situações:

1. **Áudio alto, mas sem clipping:** ainda pode ser reduzido ou tratado normalmente.
2. **Processamento interno acima do nível final, sem renderização destrutiva:** pode existir margem no fluxo de ponto flutuante, mas é necessário baixar o nível antes de exportar para um formato limitado.
3. **Gravação já capturada com clipping:** a informação foi perdida na entrada, e a recuperação será limitada.
4. **Clipping criado ao exportar ou aplicar efeito:** pode ser evitado desfazendo a operação e reduzindo o nível.

A melhor estratégia é prevenir. Grave com margem, observe os medidores e não marque opções que permitem clipping sem entender a consequência.

## 7. Headroom: a margem antes do teto

Headroom é a distância entre os picos atuais e o limite máximo do sistema. Se o maior pico está em -6 dBFS, existem aproximadamente 6 dB de margem até 0 dBFS. Se está em -1 dBFS, a margem é muito pequena.

Essa reserva é útil por vários motivos:

- a equalização pode aumentar determinadas frequências e elevar os picos;
- a compressão com ganho de compensação pode aumentar o nível final;
- a soma de várias faixas pode produzir picos maiores;
- a conversão para formatos com perdas pode gerar picos entre amostras;
- pequenos ajustes posteriores ficam mais seguros.

Durante a gravação e as etapas intermediárias, não existe necessidade de manter o sinal permanentemente encostado no zero. O áudio digital moderno permite trabalhar com margem suficiente sem a obrigação de “encher” toda a forma de onda.

Na masterização, a margem final dependerá do destino e da medição usada. Para um fluxo simples de voz, deixar o pico de amostra em torno de -1 dBFS costuma ser uma escolha conservadora para testes. Entretanto, não trate esse número como regra universal nem como substituto de requisitos específicos da plataforma.

## 8. Volume percebido: por que o ouvido não segue apenas o pico

A percepção de intensidade considera muito mais do que o instante mais alto. O ouvido responde à distribuição de energia ao longo do tempo, ao conteúdo de frequências, à duração dos sons e ao contraste entre partes fortes e fracas.

Uma voz com nível estável pode parecer mais alta do que outra cheia de picos curtos, embora ambas tenham o mesmo pico máximo. Da mesma forma, uma voz com boa presença na região de inteligibilidade pode parecer mais próxima sem que o pico aumente.

O contexto também interfere. Uma fala moderada depois de um trecho muito baixo parece forte. A mesma fala depois de um trecho intenso pode parecer fraca. O volume do dispositivo, o tipo de fone e o ruído do ambiente alteram a experiência.

Por isso, “meu áudio parece baixo” é o começo do diagnóstico, não a conclusão. É preciso investigar:

- qual é o pico máximo?
- como está a média ao longo da fala?
- existem longos silêncios?
- a dinâmica é muito ampla?
- o timbre está abafado?
- há um ruído que mascara a voz?
- o material de comparação foi reproduzido no mesmo volume?
- a plataforma aplicou normalização na reprodução?

Ao separar essas perguntas, você evita aumentar o ganho de forma indiscriminada.

## 9. RMS: uma visão média da energia

RMS é uma medida relacionada à energia média do sinal. Ela oferece mais informação sobre a densidade do áudio do que o pico isolado. Duas faixas com o mesmo pico podem apresentar valores RMS diferentes; a que mantém mais energia ao longo do tempo tende a soar mais forte.

Apesar de útil, RMS não modela completamente a audição humana. Ele não pondera as frequências e os canais da mesma maneira que os algoritmos modernos de loudness. Por isso, foi complementado em muitos fluxos por medições baseadas em LUFS.

No Audacity, a Normalização de Loudness permite escolher entre loudness percebido e RMS. Para estudar o comportamento, você pode duplicar um trecho e normalizar uma cópia por pico e outra por RMS ou LUFS. Depois, compare:

- o tamanho da forma de onda;
- o pico final;
- a sensação de volume;
- a presença de dinâmica;
- a margem restante.

Esse exercício mostra que “normalizar” pode significar objetivos diferentes conforme a métrica escolhida.

## 10. LUFS e a medição de loudness

**LUFS** significa Loudness Units relative to Full Scale. A unidade foi criada para representar melhor o loudness de programas de áudio no domínio digital. A medição segue algoritmos da família ITU-R BS.1770, usados por recomendações como a EBU R 128.

Diferentemente do pico, o LUFS considera o conteúdo ao longo do tempo e aplica ponderações relacionadas à percepção. Por essa razão, é mais útil para comparar a intensidade global de programas diferentes.

É comum encontrar estas formas de medição:

- **loudness integrado:** representa o programa completo ou toda a seleção;
- **loudness de curto prazo:** observa uma janela de alguns segundos;
- **loudness momentâneo:** acompanha uma janela ainda menor;
- **Loudness Range, ou LRA:** descreve a variação de loudness ao longo do material.

A ferramenta **Normalização de Loudness** do Audacity permite definir um alvo em LUFS. Ela é baseada na lógica da EBU R 128 e é adequada quando você precisa ajustar o nível global com base em loudness, e não apenas no maior pico.

É importante compreender o que essa ferramenta não faz: ela não substitui a compressão. A normalização mede o arquivo e aplica uma mudança de nível. A relação dinâmica interna permanece essencialmente a mesma. Se um pico isolado já ocupa quase toda a margem, elevar o loudness poderá levar o pico ao limite. Nesse caso, será necessário controlar a dinâmica antes da normalização final.

## 11. LU e LUFS não são exatamente a mesma coisa

LUFS representa um valor absoluto relacionado à escala digital. LU, ou Loudness Unit, representa uma diferença relativa.

Por exemplo, se um programa mede -23 LUFS e outro mede -20 LUFS, a diferença é de 3 LU. Essa linguagem é útil para dizer quanto o nível precisa mudar sem repetir a referência absoluta.

Na prática:

- LUFS responde “qual é o loudness medido?”;
- LU responde “qual é a diferença de loudness?”.

Ao normalizar de -21 LUFS para -18 LUFS, você solicita um aumento de 3 LU, que na aplicação de ganho corresponde a uma mudança de aproximadamente 3 dB no nível global.

## 12. Por que não existe um alvo universal

A EBU R 128 adota -23 LUFS como referência para determinados fluxos de radiodifusão europeus. Outros sistemas profissionais usam referências diferentes. Conteúdo para internet, podcast, audiobook, televisão, rádio e cinema pode ter exigências próprias.

Isso significa que não existe um único número ideal para todo áudio.

Antes de escolher um alvo, verifique:

- qual é o destino final?
- a plataforma publica uma especificação técnica?
- o conteúdo é mono, estéreo ou multicanal?
- a medição trata mono como dual mono?
- existe limite de true peak?
- a plataforma altera o volume durante a reprodução?
- o arquivo será comparado a outros episódios ou capítulos?
- o ouvinte usará fone, celular, carro ou sistema de som?

Para exercícios de voz, você pode testar diferentes alvos e observar o comportamento, mas deve registrá-los como pontos de partida, não como normas universais. Quando houver especificação do cliente ou da plataforma, ela tem prioridade.

## 13. O tratamento de mono como dual mono no Audacity

A janela de Normalização de Loudness do Audacity oferece a opção **Tratar mono como dual mono**. Esse ajuste existe porque uma faixa mono reproduzida igualmente nos dois canais pode receber interpretação diferente de uma medição que considere apenas um canal.

Com a opção ativada, o Audacity calcula a faixa mono de modo a aproximá-la do comportamento de uma versão dual mono. Isso ajuda a manter consistência entre um arquivo de um canal e outro arquivo com o mesmo sinal duplicado nos canais esquerdo e direito.

Entretanto, medidores externos podem adotar outra convenção. Se você precisa comparar o resultado do Audacity com uma ferramenta específica ou atender uma entrega profissional, confirme se os dois sistemas tratam mono da mesma forma. Uma diferença de configuração pode explicar leituras divergentes mesmo quando o áudio é idêntico.

Para estudos internos, mantenha a opção consistente entre todos os capítulos. Não altere o tratamento de mono no meio de uma série sem registrar a mudança.

## 14. Amplificar no Audacity

O efeito **Amplificar** altera todos os trechos selecionados pela mesma quantidade de decibéis. Ele preserva as diferenças relativas entre faixas e canais selecionados.

Ao abrir a janela, o Audacity calcula quanto seria necessário aumentar para levar o maior pico até 0 dBFS. Isso também pode ser usado como uma forma rápida de estimar o pico atual. Se a janela sugerir 5 dB para atingir zero, o pico atual está aproximadamente em -5 dBFS.

A ferramenta apresenta dois campos relacionados:

- **Amplificação (dB):** quanto será somado ou subtraído;
- **Nova amplitude de pico (dB):** onde o pico ficará depois da mudança.

Use Amplificar quando você sabe quanto deseja alterar ou quando precisa preservar a diferença relativa entre várias faixas.

Tenha cuidado com a opção que permite clipping. Se ela estiver habilitada, o programa pode aceitar uma amplificação que leve o novo pico acima de 0 dBFS. Para uma masterização segura, não permita clipping apenas para obter uma forma de onda maior.

Amplificar é apropriado para correções uniformes. Ele não nivela frases individuais, não controla picos e não torna duas gravações igualmente altas por percepção.

## 15. Normalizar no Audacity

O efeito **Normalizar** pode:

- definir a amplitude máxima de pico;
- remover deslocamento DC;
- ajustar faixas selecionadas a um mesmo pico;
- opcionalmente tratar canais estéreo de forma independente.

Em uma faixa mono isolada, Amplificar até determinado pico e Normalizar para o mesmo pico podem produzir resultados equivalentes, desde que não seja feita correção de deslocamento DC. A diferença torna-se mais importante quando existem várias faixas ou canais.

O Normalize pode ajustar cada faixa ao mesmo pico, enquanto Amplificar aplica a mesma quantidade de ganho a todas. Se duas faixas precisam preservar uma diferença intencional, selecioná-las e aplicar o mesmo ganho é mais adequado do que normalizar cada uma separadamente.

Outro cuidado aparece nos canais estéreo. Se os canais já estão corretamente equilibrados, normalize-os como par. Normalizá-los independentemente pode alterar a imagem estéreo ao elevar cada lado por uma quantidade diferente.

A normalização de pico é útil como etapa final para criar margem, mas não garante loudness consistente entre capítulos.

## 16. Normalização de Loudness no Audacity

A **Normalização de Loudness** ajusta o nível com base no loudness medido. Em vez de perguntar apenas qual é o pico máximo, ela considera a intensidade global da seleção.

No Audacity, acesse:

```text
Efeito > Volume e compressão > Normalização de Loudness
```

A organização dos menus pode variar entre versões.

Na janela, você poderá escolher a normalização por loudness percebido ou por RMS, definir o alvo e decidir como tratar canais estéreo e faixas mono.

Use essa ferramenta quando o objetivo for aproximar capítulos ou programas de um mesmo nível percebido. Ela é especialmente útil para séries de narração, aulas e podcasts, desde que as faixas tenham sido tratadas de maneira consistente.

Aplique-a normalmente perto do final do fluxo, depois das correções que alteram timbre e dinâmica. Se você normalizar antes da equalização ou da compressão, os efeitos seguintes poderão modificar o pico e o loudness novamente.

## 17. Por que normalização não substitui compressão

Suponha que uma narração tenha:

- loudness integrado muito baixo;
- pico máximo em -1 dBFS;
- várias frases em nível baixo;
- uma única batida forte no microfone.

A Normalização de Loudness tentará aumentar o nível global para atingir o alvo, mas quase não existe headroom. O pico isolado impede o aumento seguro.

Normalizar por pico também não resolve. O arquivo já está perto de -1 dBFS.

O problema é a diferença entre o pico e o corpo principal da fala. Essa diferença faz parte da dinâmica. Para elevar o volume percebido, será necessário corrigir o evento isolado manualmente, usar automação de volume ou aplicar compressão adequada.

A compressão, estudada em uma lição própria, reduz a distância entre trechos fortes e fracos. Depois disso, a normalização final consegue elevar o programa de forma mais eficiente.

Portanto:

```text
normalização = ajuste global de nível
compressão = controle da variação dinâmica
```

Confundir as duas ferramentas é uma das principais causas de áudio baixo mesmo depois de “normalizado”.

## 18. Crest factor: a distância entre pico e corpo do áudio

Crest factor é uma forma de descrever a diferença entre o pico e um nível médio, frequentemente RMS. Uma faixa com picos muito acima da média possui crest factor elevado. Uma faixa muito densa e comprimida possui distância menor.

Você não precisa calcular esse valor para iniciar no Audacity. Entretanto, o conceito ajuda a interpretar a forma de onda:

- picos finos e muito altos sobre um corpo pequeno indicam grande distância;
- forma de onda mais consistente indica menor distância;
- bloco quase uniforme e constantemente próximo do teto pode indicar compressão ou limitação excessiva.

Para voz, o objetivo não é eliminar todos os picos. Consoantes, ataques e variações naturais contribuem para a inteligibilidade e a expressão. A meta é evitar que poucos eventos dominem toda a margem disponível.

## 19. Um fluxo seguro para diagnosticar volume baixo

Use este procedimento antes de aumentar o áudio:

### Passo 1 — Salve o projeto e preserve o original

Mantenha uma cópia do arquivo bruto. Trabalhe em um projeto do Audacity ou duplique a faixa antes de experimentar.

### Passo 2 — Ouça começo, meio e fim

Escolha trechos com fala normal, frases baixas e momentos fortes. Não tire conclusões usando apenas cinco segundos.

### Passo 3 — Ative a visualização de clipping

Procure linhas vermelhas e regiões achatadas. Se o áudio já estiver cortado, anote que o problema nasceu antes da masterização ou em uma etapa anterior.

### Passo 4 — Verifique o pico

Selecione a faixa e abra Amplificar sem aplicar. Observe quanto o programa sugere para chegar a 0 dB. O valor permite estimar o pico atual.

### Passo 5 — Compare pico e sensação de volume

Se o pico estiver baixo e a fala for relativamente uniforme, um ganho global pode resolver. Se o pico estiver alto, mas a voz continuar baixa, investigue a dinâmica.

### Passo 6 — Identifique eventos isolados

Amplie a forma de onda e procure batidas, cliques, plosivas ou palavras excepcionalmente fortes. Corrigir um evento isolado pode liberar margem sem comprimir toda a narração.

### Passo 7 — Analise o loudness

Use uma ferramenta de medição disponível no seu fluxo ou faça uma cópia e teste a Normalização de Loudness. Registre o alvo usado e compare a sensação auditiva.

### Passo 8 — Ajuste apenas o necessário

Evite aplicar vários efeitos ao mesmo tempo. Faça uma mudança, compare e desfaça se a naturalidade piorar.

## 20. Exemplo prático com duas narrações

Imagine dois arquivos de cinco minutos.

### Narração A

- pico máximo: -1 dBFS;
- fala média visualmente pequena;
- uma batida forte aos dois minutos;
- sensação geral de volume baixo.

### Narração B

- pico máximo: -3 dBFS;
- fala estável;
- poucas variações bruscas;
- sensação geral de volume mais forte.

O pico da narração A está mais próximo do zero, mas ela parece mais baixa. A batida ocupa a margem sem representar o restante do conteúdo.

O procedimento correto seria:

1. localizar a batida;
2. reduzi-la ou repará-la, se for um defeito;
3. reavaliar o pico;
4. controlar outras variações, se necessário;
5. aplicar normalização de loudness no final.

Na narração B, talvez baste um ajuste uniforme de ganho ou loudness.

Esse exemplo demonstra por que o diagnóstico precisa considerar o programa inteiro.

## 21. Como comparar antes e depois sem ser enganado

Quando uma versão fica mais alta, o cérebro tende a interpretá-la como melhor. Esse viés dificulta a avaliação de equalização, compressão e redução de ruído.

Para comparar processamento de forma honesta:

- duplique um trecho;
- aplique o efeito apenas em uma cópia;
- ajuste os níveis para que as duas versões soem aproximadamente igualmente altas;
- alterne entre elas;
- avalie clareza, naturalidade, ruído e fadiga;
- use fones e alto-falantes comuns;
- faça uma pausa se o ouvido estiver cansado.

Comparar em níveis muito diferentes faz a versão mais alta parecer mais detalhada, mesmo quando o processamento prejudicou o timbre.

Na lição atual, use a comparação para perceber a diferença entre normalização de pico e loudness. Não escolha automaticamente a maior forma de onda. Escolha a versão mais inteligível, estável e confortável.

## 22. Erros comuns ao ajustar volume

### Aumentar até 0 dBFS em todas as etapas

Não é necessário manter o áudio no teto durante a edição. Preserve headroom para os efeitos seguintes.

### Confundir pico com loudness

Mesmo pico não significa mesma intensidade percebida.

### Normalizar cada capítulo apenas pelo pico

Um espirro ou estalo pode determinar o pico e deixar a fala média baixa. Capítulos diferentes podem terminar com o mesmo pico e volumes percebidos desiguais.

### Permitir clipping para “ganhar volume”

Clipping não é uma forma transparente de aumentar loudness. Ele cria distorção.

### Reduzir o nível de um arquivo já clipado e considerar resolvido

A distorção gravada continua presente, apenas mais baixa.

### Escolher um alvo LUFS sem verificar o destino

Um valor adequado para um fluxo pode estar incorreto para outro.

### Ignorar mono, estéreo e dual mono

A configuração de canais influencia a medição.

### Aplicar loudness normalization no início

Equalização, compressão e limiter podem alterar novamente o nível. Faça a normalização final perto da exportação.

### Julgar só pela aparência

Forma de onda grande não garante clareza nem qualidade.

### Comparar com referências em volumes diferentes

Iguale o nível de reprodução antes de decidir qual versão soa melhor.

## 23. Exercício prático da Lição 2

Escolha um áudio de voz com pelo menos dois minutos e faça o seguinte:

1. importe o arquivo no Audacity;
2. salve o projeto com outro nome;
3. duplique a faixa;
4. ative a exibição de clipping;
5. abra Amplificar e anote o pico estimado;
6. normalize a primeira cópia para um pico com margem;
7. aplique Normalização de Loudness na segunda cópia;
8. não use compressor neste exercício;
9. compare as duas versões;
10. anote qual parece mais estável e por quê.

Use esta ficha:

```text
Tipo de áudio:
Duração:
Pico estimado antes do tratamento:
Havia clipping?
Alvo usado na normalização de pico:
Alvo usado na normalização de loudness:
Qual versão pareceu mais alta?
Qual versão pareceu mais natural?
Algum pico impediu o aumento?
O ruído ficou mais evidente?
Conclusão:
```

O objetivo não é descobrir uma configuração universal. É perceber que cada métrica responde a uma pergunta diferente.

## 24. Checklist antes de exportar

Antes de considerar o volume concluído, confirme:

- não há clipping audível ou visual criado pelo processamento;
- o pico final possui a margem exigida pelo destino;
- o loudness está coerente com a especificação escolhida;
- todos os capítulos foram medidos com a mesma configuração;
- mono e estéreo foram tratados conscientemente;
- o áudio não ficou agressivo após o aumento;
- o ruído de fundo continua aceitável;
- partes baixas permanecem inteligíveis;
- partes fortes não causam sustos;
- a exportação foi ouvida do começo ao fim ou por amostragem criteriosa;
- o original sem processamento foi preservado.

## 25. Perguntas de revisão

### 1. O que o ganho altera?

Ele aumenta ou reduz o nível de todo o sinal selecionado pela mesma proporção.

### 2. Por que um arquivo com pico em -1 dBFS pode parecer baixo?

Porque o pico pode ser muito curto, enquanto a maior parte do áudio permanece em nível baixo.

### 3. O que representa 0 dBFS?

O teto da escala de amostras digitais. Ultrapassá-lo em um formato limitado pode causar clipping.

### 4. Reduzir o volume remove clipping gravado?

Não. A distorção permanece, apenas fica mais baixa.

### 5. Qual é a diferença entre normalização de pico e loudness?

A primeira usa o maior pico como referência. A segunda usa uma medição da intensidade global percebida.

### 6. A Normalização de Loudness substitui o compressor?

Não. Ela aplica um ajuste global de nível; o compressor modifica a dinâmica.

### 7. Para que serve o headroom?

Para manter margem antes do teto e permitir processamento, conversão e reprodução com mais segurança.

### 8. Por que não existe um alvo LUFS universal?

Porque destinos, formatos, canais e especificações de entrega são diferentes.

### 9. Quando usar Amplificar?

Quando você deseja aplicar a mesma mudança de ganho e preservar as diferenças relativas entre faixas ou canais.

### 10. Quando usar Normalizar?

Quando precisa definir um pico, remover deslocamento DC ou ajustar faixas ao mesmo valor máximo, observando o equilíbrio entre canais.

## Conclusão

Dominar **ganho, pico e loudness no Audacity** é fundamental para deixar de tratar volume por tentativa. O pico protege contra sobrecarga, mas não representa sozinho a intensidade percebida. O loudness ajuda a comparar programas completos, mas não corrige automaticamente uma dinâmica desorganizada. O ganho altera todo o sinal, porém também eleva ruído e picos.

A pergunta correta não é apenas “como deixar mais alto?”. Pergunte:

- qual é o pico?
- quanto headroom existe?
- o corpo da fala está distante do pico?
- há eventos isolados ocupando a margem?
- qual métrica corresponde ao destino?
- o processamento preservou naturalidade?

Quando essas respostas estão claras, Amplificar, Normalizar e Normalização de Loudness deixam de ser botões semelhantes e passam a cumprir funções específicas.

Na próxima lição, estudaremos a limpeza inicial da voz: ruído constante, cliques, respirações, silêncios, plosivas e os limites da restauração. Essa preparação evitará que equalização e compressão aumentem defeitos que deveriam ter sido tratados antes.

## Referências

- [Audacity Manual — Amplify](https://manual.audacityteam.org/man/amplify.html)
- [Audacity Manual — Normalize](https://manual.audacityteam.org/man/normalize.html)
- [Audacity Manual — Amplify and Normalize](https://manual.audacityteam.org/man/amplify_and_normalize.html)
- [Audacity Manual — Loudness Normalization](https://manual.audacityteam.org/man/loudness_normalization.html)
- [Audacity Manual — Show Clipping in Waveform](https://manual.audacityteam.org/man/view_menu.html)
- [ITU-R BS.1770-5 — Algorithms to measure audio programme loudness and true-peak audio level](https://www.itu.int/rec/R-REC-BS.1770-5-202311-I)
- [EBU R 128 — Loudness normalisation and permitted maximum level of audio signals](https://tech.ebu.ch/docs/r/r128.pdf)
