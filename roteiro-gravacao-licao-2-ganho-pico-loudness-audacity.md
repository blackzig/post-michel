# Roteiro de gravação — Lição 2
## Ganho, pico e loudness no Audacity: entenda o volume do áudio

> **Formato:** fala natural de professor
>
> **Objetivo:** explicar ganho, pico, dBFS, clipping, headroom e loudness sem transformar números de referência em regras universais.
>
> **Observação:** os nomes dos menus podem variar um pouco conforme a versão e o idioma do Audacity.

---

## Abertura

Olá! Seja bem-vindo à segunda lição da nossa trilha de masterização no Audacity.

Na aula anterior, nós vimos que masterizar não significa simplesmente deixar o áudio mais alto. Antes de aplicar qualquer efeito, precisamos ouvir, diagnosticar e entender qual problema realmente está presente.

Hoje vamos aprofundar justamente uma das dúvidas mais comuns de quem começa:

**por que um áudio pode chegar perto do limite e, mesmo assim, parecer baixo?**

Essa situação parece contraditória. Você olha a forma de onda, percebe alguns picos altos, abre o efeito Amplificar e descobre que quase não existe espaço para aumentar. Mesmo assim, quando aperta o play, a voz continua fraca.

A explicação é que **pico e volume percebido não são a mesma coisa**.

O pico mostra o instante mais alto do sinal. O loudness tenta representar melhor a intensidade percebida durante um trecho ou ao longo do programa inteiro.

Entre esses dois conceitos aparecem outros termos importantes: ganho, amplitude, dBFS, headroom, clipping, RMS e LUFS.

Nesta lição, vamos organizar tudo isso de maneira prática. A ideia não é decorar siglas. A ideia é aprender a olhar para um áudio e entender o que está acontecendo antes de escolher uma ferramenta.

Ao final, você deverá saber quando usar Amplificar, quando usar Normalizar, quando usar Normalização de Loudness e quando nenhuma dessas opções, sozinha, resolverá o problema.

---

## 1. Ganho não é sinônimo de qualidade

Vamos começar pelo ganho.

Ganho é uma alteração aplicada ao nível do sinal. Se você acrescenta ganho, a amplitude do áudio aumenta. Se reduz o ganho, a amplitude diminui.

Isso parece simples, e realmente é. Mas existe um detalhe essencial:

**aumentar o ganho não melhora automaticamente o áudio.**

Imagine uma gravação de voz com ruído de fundo. Se você aumentar toda a faixa em 6 decibéis, a voz sobe, mas o ruído também sobe.

Agora imagine que essa mesma gravação possui frases muito baixas e alguns picos muito fortes. Ao acrescentar ganho, você aumenta tudo na mesma proporção. As frases baixas ficam mais audíveis, mas os picos também se aproximam ainda mais do limite.

Portanto, ganho é uma ferramenta de nível. Ele não corrige sozinho:

ruído,

timbre,

dicção,

dinâmica,

eco,

nem distorção já gravada.

Antes de aumentar o ganho, faça algumas perguntas:

O áudio está baixo de maneira uniforme?

Existem picos isolados ocupando quase toda a margem?

A voz parece fraca porque foi gravada longe do microfone?

O problema é volume ou falta de clareza?

O ruído ficará incômodo quando tudo subir?

Se o áudio estiver uniformemente baixo e houver espaço disponível, um aumento global pode ser suficiente.

Mas, se a fala estiver baixa enquanto poucos picos chegam perto do teto, o problema principal é a dinâmica. Nesse caso, será preciso corrigir eventos isolados, automatizar o volume ou usar compressão em uma etapa posterior.

---

## 2. Amplitude e decibéis

A forma de onda exibida no Audacity representa a amplitude do sinal ao longo do tempo.

Quanto mais a onda se afasta da linha central, maior é a amplitude naquele instante.

No áudio digital, o nível costuma ser apresentado em decibéis relativos à escala completa, conhecidos como dBFS.

A escala de decibéis é logarítmica. Isso significa que ela não funciona como uma porcentagem comum.

Aumentar 3 decibéis não significa aumentar 3 por cento. Da mesma forma, acrescentar 6 decibéis à amplitude não garante que o ouvinte perceberá o som como exatamente duas vezes mais alto.

A percepção depende de vários fatores:

frequência,

duração,

dinâmica,

equipamento,

ambiente,

e até do áudio que foi ouvido imediatamente antes.

Para trabalhar no Audacity, você não precisa fazer cálculos manualmente. O mais importante é entender que os decibéis representam relações de nível.

Na prática, faça mudanças pequenas, ouça e compare.

---

## 3. O que significa 0 dBFS

Na escala digital usada pelos medidores, zero dBFS representa o teto de referência da escala completa.

Por isso, os valores normalmente aparecem negativos:

menos 24,

menos 18,

menos 12,

menos 6,

menos 1,

e zero dBFS.

Quanto mais próximo de zero, maior é o nível da amostra.

Então, menos 3 dBFS é mais alto do que menos 12 dBFS.

Agora vem uma precisão importante.

O Audacity trabalha normalmente com áudio interno em ponto flutuante. Esse formato pode armazenar temporariamente valores acima de zero dBFS durante o processamento. Portanto, uma onda acima de zero dentro do projeto não significa necessariamente que os dados já foram destruídos naquele momento.

Mas isso não torna seguro ignorar o limite.

Na gravação, o conversor de entrada pode clipar. E, ao exportar para formatos comuns de ponto fixo, valores acima da escala precisam ser reduzidos ou serão cortados.

Além disso, um sinal acima do limite pode distorcer durante a reprodução ou em etapas posteriores.

A regra prática é esta:

**não use a capacidade interna de ponto flutuante como desculpa para trabalhar sem controle.**

Antes da exportação, mantenha os picos dentro da margem adequada ao destino.

---

## 4. O que é pico de áudio

O pico é o ponto de maior amplitude encontrado no áudio ou na seleção analisada.

Se o maior pico está em menos 1 dBFS, isso significa que o instante mais alto ficou um decibel abaixo do teto de referência.

O pico é muito importante para prevenir sobrecarga. Mas ele não descreve sozinho a experiência de escuta.

Vamos comparar duas narrações.

A primeira tem pico em menos 1 dBFS, mas quase toda a fala permanece muito baixa.

A segunda também tem pico em menos 1 dBFS, porém a maior parte da voz permanece mais próxima desse nível.

As duas têm o mesmo pico. Mesmo assim, a segunda provavelmente parecerá mais forte e consistente.

Por quê?

Porque, na primeira, o pico pode ter sido causado por um evento muito curto:

uma batida no microfone,

um clique,

uma plosiva,

uma sílaba excepcionalmente forte,

ou outro acidente.

Esse instante ocupa a margem disponível, mas não representa o restante da fala.

É por isso que normalizar dois arquivos para o mesmo pico não garante que eles terão o mesmo volume percebido.

---

## 5. Pico de amostra e true peak

O pico que observamos diretamente nas amostras é chamado de pico de amostra.

Mas, durante a reprodução, o conversor reconstrói uma onda contínua entre essas amostras. Essa reconstrução pode criar um ponto máximo ligeiramente acima do maior valor armazenado.

A estimativa desse máximo reconstruído é chamada de true peak, geralmente expressa em dBTP.

Essa diferença é importante porque um arquivo pode ficar abaixo de zero no pico de amostra e, ainda assim, produzir um pico reconstruído mais alto durante conversão ou reprodução.

Para quem está começando, a consequência prática é simples:

não empurre todo arquivo exatamente até zero.

Deixar o pico de amostra em torno de menos 1 dBFS oferece uma pequena margem prática em muitos trabalhos simples. Mas atenção: isso **não é uma garantia de true peak** e não substitui um medidor dedicado quando o cliente ou a plataforma exige um limite em dBTP.

Então, use menos 1 dBFS como uma referência conservadora de teste, não como lei universal.

---

## 6. Clipping: quando o limite é ultrapassado

Clipping acontece quando o sinal ultrapassa o que pode ser representado corretamente na etapa em questão.

Na forma de onda, ele pode aparecer como regiões achatadas no topo ou na parte inferior.

No som, pode gerar aspereza, estalos, perda de definição e distorção.

No Audacity, existe uma opção para mostrar possíveis amostras clipadas na forma de onda. O caminho costuma ficar no menu Exibir, mas o nome exato pode variar conforme a versão e a tradução.

É importante distinguir algumas situações.

Primeira situação: o áudio está alto, mas não está clipado. Nesse caso, ainda podemos reduzir o nível normalmente.

Segunda situação: o projeto em ponto flutuante contém valores temporariamente acima de zero. Se os picos não foram destruídos antes, pode ser possível reduzir o nível antes da exportação e preservar a forma da onda.

Terceira situação: a gravação já entrou clipada pelo conversor ou pelo equipamento. Nesse caso, parte da informação foi perdida na captura. Reduzir o volume depois apenas deixa a distorção mais baixa; não reconstrói automaticamente a forma original.

Quarta situação: o clipping foi criado por um efeito ou pela exportação. A solução correta é desfazer, reduzir o nível e processar novamente.

A melhor estratégia continua sendo a prevenção.

---

## 7. Headroom: a margem antes do teto

Headroom é a margem existente entre os picos atuais e o teto de referência.

Se o pico está em menos 6 dBFS, existem aproximadamente 6 decibéis de margem até zero.

Se está em menos 1 dBFS, a margem é muito pequena.

Essa reserva é útil porque outros processos podem aumentar o nível.

Uma equalização pode elevar certas frequências.

A compressão pode usar ganho de compensação.

A soma de várias faixas pode gerar picos maiores.

A conversão para formatos com perdas pode alterar os picos reconstruídos.

Por isso, durante a edição, não existe necessidade de deixar a forma de onda encostada no teto o tempo todo.

Você não ganha qualidade apenas porque a onda ficou visualmente maior.

Trabalhar com alguma margem facilita as próximas decisões e reduz o risco de distorção.

---

## 8. Por que o ouvido não segue apenas o pico

O volume percebido depende da distribuição de energia ao longo do tempo, do conteúdo de frequências e do contraste entre partes fortes e fracas.

Uma voz estável pode parecer mais alta do que outra cheia de picos curtos, mesmo que as duas tenham o mesmo pico máximo.

Uma voz com melhor presença na região de inteligibilidade também pode parecer mais próxima sem aumentar o pico.

O contexto interfere muito.

Uma fala moderada depois de um trecho silencioso parece forte.

A mesma fala depois de um trecho intenso pode parecer fraca.

O fone, o alto-falante, o ruído do ambiente e o volume do dispositivo também mudam a experiência.

Por isso, a frase “meu áudio está baixo” deve iniciar o diagnóstico, e não encerrá-lo.

Pergunte:

Qual é o pico máximo?

A maior parte da fala está muito distante desse pico?

Existem longos silêncios?

A dinâmica está ampla demais?

O timbre está abafado?

O ruído está mascarando a voz?

A comparação foi feita no mesmo volume de reprodução?

A plataforma alterou o volume durante a reprodução?

Essas perguntas ajudam a evitar o aumento indiscriminado de ganho.

---

## 9. RMS e loudness

RMS é uma medida relacionada à energia média do sinal.

Ela oferece mais informação sobre a densidade do áudio do que o pico isolado. Duas faixas podem ter o mesmo pico, mas a faixa com maior energia média tende a parecer mais forte.

Mesmo assim, RMS não representa completamente a percepção humana. Ele não aplica o mesmo tipo de ponderação usado pelos algoritmos modernos de loudness.

É aí que entram as medições em LUFS.

LUFS representa uma medição de loudness relativa à escala completa. Ela segue algoritmos da família ITU-R BS.1770 e é usada em recomendações como a EBU R 128.

Essas medições aplicam ponderação de frequência e, no loudness integrado, usam regras para reduzir a influência de trechos muito silenciosos.

Você pode encontrar diferentes leituras:

loudness integrado, que representa o programa completo;

loudness de curto prazo, calculado em uma janela de poucos segundos;

loudness momentâneo, calculado em uma janela menor;

além do Loudness Range, que descreve a variação de loudness do material.

Para esta lição, concentre-se principalmente no loudness integrado. Ele é útil para comparar capítulos, episódios ou programas completos.

---

## 10. LU e LUFS

LUFS apresenta um valor absoluto de loudness relacionado à escala digital.

LU, ou Loudness Unit, expressa uma diferença relativa.

Se um programa mede menos 23 LUFS e outro mede menos 20 LUFS, a diferença é de 3 LU.

Na prática, uma mudança de 3 LU no alvo corresponde aproximadamente a uma mudança de 3 decibéis no ganho global aplicado ao programa.

Então, pense assim:

LUFS responde: “qual é o loudness medido?”

LU responde: “qual é a diferença de loudness?”

---

## 11. Não existe um alvo universal de LUFS

A EBU R 128 usa menos 23 LUFS como referência para determinados fluxos de radiodifusão.

Isso não significa que todo vídeo, podcast, audiobook ou aula deva obrigatoriamente ser entregue em menos 23 LUFS.

Cada destino pode ter regras próprias.

Além disso, algumas plataformas normalizam o volume durante a reprodução, enquanto outras apenas recomendam faixas de trabalho.

Antes de escolher um alvo, verifique:

qual é o destino final;

se existe uma especificação publicada;

se o arquivo é mono ou estéreo;

como o medidor trata mono e dual mono;

se existe limite de true peak;

se todos os capítulos precisam manter consistência;

se o cliente definiu um padrão de entrega.

Durante o estudo, você pode testar alvos diferentes. Mas trate esses valores como experiências controladas, não como normas universais.

---

## 12. A opção “Tratar mono como dual mono”

Na Normalização de Loudness do Audacity existe a opção “Tratar mono como dual mono”.

Um arquivo dual mono possui o mesmo sinal nos canais esquerdo e direito.

Na medição baseada na soma de canais, essa versão pode apresentar aproximadamente 3 LU a mais do que um arquivo de um único canal com o mesmo conteúdo, embora ambos possam soar iguais quando o sistema de reprodução duplica o mono nos dois alto-falantes.

A opção do Audacity procura compensar essa diferença durante o cálculo.

O manual informa que ela fica ativada por padrão, mas também alerta que muitos medidores externos tratam mono de outra maneira.

Por isso, duas ferramentas podem apresentar resultados diferentes para o mesmo arquivo mono.

O mais importante é manter o procedimento consistente.

Se você está preparando vários capítulos, use a mesma configuração em todos eles. Se precisa atender a uma especificação externa, confirme como o medidor de referência trata sinais mono.

---

## 13. Amplificar, Normalizar e Normalização de Loudness

Agora vamos separar três ferramentas que parecem semelhantes, mas cumprem funções diferentes.

### Amplificar

O efeito Amplificar acrescenta ou remove a mesma quantidade de ganho em tudo o que foi selecionado.

Se você aplicar mais 3 decibéis em várias faixas, todas sobem 3 decibéis. As diferenças relativas entre elas são preservadas.

Ao abrir a janela, o Audacity calcula a alteração necessária para levar o maior pico da seleção até zero dBFS. Isso também permite estimar rapidamente o pico atual.

Se a ferramenta sugerir mais 5 decibéis para chegar a zero, o pico atual está aproximadamente em menos 5 dBFS.

Use Amplificar quando você sabe quanto deseja alterar ou quando precisa preservar as diferenças relativas entre faixas e canais.

Evite ativar a opção que permite clipping apenas para conseguir uma forma de onda maior.

### Normalizar

O efeito Normalizar pode definir um pico máximo e também remover deslocamento DC.

Em uma única faixa mono, Amplificar até determinado pico e Normalizar para o mesmo pico podem produzir resultados equivalentes, desde que a correção de deslocamento DC não altere o sinal.

A diferença aparece com mais clareza quando existem várias faixas ou canais.

Amplificar aplica a mesma mudança a todos.

Normalizar pode ajustar faixas separadamente para o mesmo pico.

Se existem diferenças intencionais entre as faixas, não normalize cada uma isoladamente. Isso pode destruir o equilíbrio planejado.

Em estéreo, mantenha os canais vinculados quando o balanço esquerdo e direito já estiver correto. Normalizar os canais de forma independente pode alterar a imagem estéreo.

### Normalização de Loudness

A Normalização de Loudness ajusta o nível com base na medição de loudness ou, opcionalmente, RMS.

Ela é mais adequada quando o objetivo é aproximar capítulos ou programas de um mesmo nível percebido.

No Audacity, o caminho costuma ser:

Efeito, depois Volume e Compressão, e então Normalização de Loudness.

Ela deve ser usada perto do final do fluxo, depois das correções que alteram timbre e dinâmica.

Mas atenção: normalização de loudness não substitui compressão.

Ela mede e aplica uma alteração global de nível. A dinâmica interna permanece essencialmente a mesma.

Depois de normalizar, confira novamente os picos. Se o alvo fizer o áudio ultrapassar a margem adequada à exportação, desfaça e controle primeiro os eventos isolados ou a dinâmica.

---

## 14. Por que normalização não substitui compressão

Imagine uma narração com estas características:

loudness integrado baixo;

pico máximo em menos 1 dBFS;

várias frases muito fracas;

e uma batida forte no microfone.

Normalizar por pico não resolve, porque o arquivo já está perto do teto.

Aumentar o ganho também não resolve, porque a batida ultrapassará o limite.

A normalização de loudness pode elevar o nível médio, mas também pode empurrar esse pico além da margem segura.

O problema verdadeiro é a distância entre o pico isolado e o corpo principal da fala.

Nesse caso, você pode:

reduzir manualmente a batida;

corrigir a plosiva ou o clique;

usar automação de volume;

ou aplicar compressão adequada.

Depois de organizar a dinâmica, a normalização final consegue elevar o programa com mais eficiência.

Guarde esta diferença:

**normalização ajusta o nível global.**

**compressão modifica a relação entre trechos fortes e fracos.**

---

## 15. Fluxo prático para diagnosticar áudio baixo

Agora vamos montar um procedimento simples.

### Primeiro passo: preserve o original

Salve o projeto e mantenha uma cópia do arquivo bruto. Não dependa apenas do botão desfazer.

### Segundo passo: ouça começo, meio e fim

Escolha trechos com fala normal, partes mais baixas e momentos fortes. Não tire conclusões usando apenas alguns segundos.

### Terceiro passo: procure clipping

Ative a visualização de clipping e observe regiões achatadas. Escute os pontos suspeitos.

### Quarto passo: estime o pico

Selecione a faixa e abra Amplificar sem aplicar. Observe quanto o Audacity sugere para chegar a zero.

### Quinto passo: compare pico e sensação de volume

Se o pico estiver baixo e a fala for uniforme, um ajuste global pode bastar.

Se o pico estiver alto, mas a voz continuar baixa, investigue a dinâmica.

### Sexto passo: localize eventos isolados

Procure batidas, cliques, plosivas e palavras excepcionalmente fortes. Um único defeito pode estar ocupando toda a margem.

### Sétimo passo: avalie o loudness

Use uma medição confiável ou faça uma cópia do áudio e teste a Normalização de Loudness. Registre o alvo e a configuração usada.

### Oitavo passo: mude uma coisa por vez

Aplique um ajuste, ouça, compare e desfaça se a naturalidade piorar.

---

## 16. Exemplo prático

Imagine dois arquivos de narração.

O arquivo A possui pico em menos 1 dBFS, mas a fala parece baixa. Existe uma batida muito forte no meio da gravação.

O arquivo B possui pico em menos 3 dBFS, mas a fala é mais estável e parece mais forte.

O arquivo A tem o pico mais alto, porém o arquivo B possui maior consistência.

No arquivo A, o procedimento correto é:

localizar a batida;

reduzir ou reparar o evento;

medir novamente;

controlar outras variações, se necessário;

normalizar o loudness no final.

No arquivo B, talvez seja suficiente aplicar um ajuste global moderado.

Esse exemplo mostra por que o pico, sozinho, não descreve o volume percebido.

---

## 17. Como comparar antes e depois

Existe um erro de percepção muito comum: a versão mais alta quase sempre parece melhor em uma comparação rápida.

Isso acontece porque o aumento de nível pode dar a impressão de mais detalhe e presença, mesmo quando o processamento piorou o timbre.

Para comparar de maneira mais justa:

duplique um trecho;

aplique o efeito apenas em uma cópia;

ajuste os níveis para que as duas versões pareçam aproximadamente igualmente altas;

alterne entre elas;

compare clareza, naturalidade, ruído e fadiga;

escute em fones e em alto-falantes comuns;

e faça pausas para não cansar o ouvido.

Não escolha automaticamente a maior forma de onda. Escolha a versão que permanece clara, estável e natural.

---

## 18. Erros comuns

Vamos revisar os erros mais frequentes.

Primeiro: tentar manter o áudio em zero dBFS durante todas as etapas.

Isso elimina margem para o processamento seguinte.

Segundo: confundir pico com loudness.

Dois arquivos podem ter o mesmo pico e volumes percebidos muito diferentes.

Terceiro: normalizar todos os capítulos apenas pelo pico.

Um estalo ou uma batida pode determinar o pico e deixar o restante da voz baixo.

Quarto: permitir clipping para “ganhar volume”.

Clipping não é uma forma transparente de aumentar loudness. Ele produz distorção.

Quinto: reduzir o nível de uma gravação já clipada e considerar o problema resolvido.

A distorção continua presente.

Sexto: escolher um alvo LUFS sem verificar o destino.

Não existe um único alvo correto para todos os usos.

Sétimo: ignorar a diferença entre mono, estéreo e dual mono.

A configuração dos canais influencia a medição.

Oitavo: aplicar a normalização final antes de equalizar ou comprimir.

Esses efeitos podem alterar o nível novamente.

Nono: julgar apenas pela aparência da forma de onda.

Uma onda grande não garante qualidade.

Décimo: comparar versões em volumes diferentes.

A versão mais alta pode parecer melhor apenas por estar mais alta.

---

## 19. Exercício da lição

Agora você vai fazer um teste prático.

Escolha um trecho de voz com cerca de um minuto e duplique a faixa duas vezes. Você ficará com três versões idênticas.

Na primeira, use Amplificar para deixar o pico em torno de menos 1 dBFS.

Na segunda, use Normalizar para o mesmo pico.

Na terceira, teste a Normalização de Loudness com um alvo escolhido apenas para comparação.

Depois, reduza temporariamente o volume das versões mais altas até que as três pareçam aproximadamente igualmente fortes.

Compare:

A dinâmica mudou?

O ruído ficou mais evidente?

Alguma versão clipou?

Amplificar e Normalizar ficaram iguais na faixa mono?

A versão baseada em loudness ficou mais próxima do volume esperado?

Algum pico isolado impediu o aumento?

Anote as respostas.

O objetivo não é encontrar uma configuração universal. É entender que cada ferramenta responde a uma pergunta diferente.

---

## 20. Checklist antes de exportar

Antes de considerar o volume concluído, confirme:

não existe clipping criado pelo processamento;

o pico final respeita a margem exigida pelo destino;

o loudness está coerente com a especificação escolhida;

todos os capítulos foram medidos com a mesma configuração;

mono e estéreo foram tratados conscientemente;

o áudio não ficou agressivo depois do aumento;

o ruído de fundo continua aceitável;

as partes baixas continuam inteligíveis;

as partes fortes não causam sustos;

a exportação foi ouvida novamente;

e o arquivo original foi preservado.

---

## Encerramento

Nesta lição, você aprendeu que pico e loudness respondem a perguntas diferentes.

O pico mostra o ponto mais alto do sinal e ajuda a prevenir sobrecarga.

O loudness oferece uma representação mais útil da intensidade percebida ao longo do programa.

O ganho aumenta ou reduz todo o sinal, mas também eleva ruído e picos.

A normalização de pico organiza o valor máximo.

A normalização de loudness ajusta o nível global com base em uma medição perceptiva.

E a compressão, que estudaremos mais adiante, controla a relação entre partes fortes e fracas.

A pergunta correta não é apenas “como deixar o áudio mais alto?”.

Pergunte:

Qual é o pico?

Quanto headroom existe?

A fala está muito distante dos picos?

Existem eventos isolados ocupando a margem?

Qual é o destino do arquivo?

O processamento preservou a naturalidade?

Quando você consegue responder a essas perguntas, Amplificar, Normalizar e Normalização de Loudness deixam de parecer três botões parecidos. Cada ferramenta passa a ter uma função clara.

Na próxima lição, vamos estudar a limpeza inicial da voz: ruído constante, cliques, respirações, silêncios, plosivas e os limites da restauração.

Até a próxima aula.

---

## Referências técnicas utilizadas na revisão

- Audacity Manual — Amplify
- Audacity Manual — Normalize
- Audacity Manual — Amplify and Normalize
- Audacity Manual — Loudness Normalization
- Audacity Manual — Perceived Loudness, EBU R 128
- ITU-R BS.1770-5 — Algorithms to measure audio programme loudness and true-peak audio level
- EBU R 128 — Loudness normalisation and permitted maximum level of audio signals
