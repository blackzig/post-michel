---
title: "Formação DEV — HTML: Lição 1 — Primeiro Exemplo na Prática"
seo_title: "Primeiro Exemplo de HTML: Texto, Salvamento e Navegador"
slug: "formacao-dev-html-licao-1-primeiro-exemplo"
meta_description: "Aprenda seu primeiro exemplo de HTML: escreva texto no VS Code, salve o arquivo, atualize o navegador e descubra como funcionam espaços e quebras de linha."
focus_keyphrase: "primeiro exemplo de HTML"
related_keywords:
  - "HTML para iniciantes"
  - "primeiro exemplo HTML"
  - "aprender HTML do zero"
  - "Visual Studio Code HTML"
  - "como salvar arquivo HTML"
  - "como atualizar página HTML"
  - "quebra de linha HTML"
  - "espaços em branco HTML"
  - "HTML navegador"
  - "Formação DEV"
category: "HTML"
tags:
  - "HTML"
  - "Formação DEV"
  - "Desenvolvimento Web"
  - "Programação"
  - "Iniciantes"
  - "Visual Studio Code"
  - "Navegadores"
  - "Chrome"
  - "Aprendizado Prático"
excerpt: "Na primeira prática de HTML, descubra como um texto aparece no navegador, por que salvar o arquivo importa e por que pressionar Enter não cria uma nova linha na página."
---

# 🌐 Formação DEV — HTML: Lição 1 — Primeiro Exemplo na Prática

O que acontece se você criar um arquivo HTML, escrever uma frase comum e abri-lo no navegador, **sem usar nenhuma tag**?

Essa é a proposta da aula **“Primeiro Exemplo #01”**, da sequência de estudos de HTML da Formação DEV. Em vez de começar decorando comandos, o professor Leonardo convida o aluno a observar o comportamento do navegador diante de algo muito simples: **texto puro**.

O resultado ajuda a compreender três ideias fundamentais:

1. O navegador consegue apresentar texto de um documento HTML.
2. Alterar o arquivo no editor não é o mesmo que salvar a alteração.
3. As quebras de linha e os espaços escritos no código-fonte não são necessariamente reproduzidos na página.

Nesta lição, vamos reconstruir o experimento passo a passo, entender o que ele revela sobre a web e realizar pequenos desafios para consolidar o aprendizado.

> **Sobre o conteúdo:** a explicação do experimento acompanha a transcrição da aula. A organização em etapas, os exercícios, as tabelas e as observações técnicas complementares deste artigo são contribuições didáticas próprias, não falas literais do professor.

## 1. O que vamos aprender

Ao terminar o estudo, você deverá conseguir:

- criar um arquivo simples para iniciar seus estudos de HTML;
- escrever texto no Visual Studio Code;
- identificar visualmente uma alteração ainda não salva;
- salvar um arquivo usando o teclado;
- atualizar a página no navegador para ver mudanças;
- diferenciar o zoom do editor do zoom do navegador;
- observar o que acontece ao inserir uma segunda frase;
- explicar por que várias linhas em branco não geram, sozinhas, várias linhas na página;
- distinguir o conteúdo digitado no arquivo da forma como ele é apresentado pelo navegador;
- compreender por que as marcações HTML serão necessárias nas próximas aulas.

**Pré-requisito:** ter acesso a um editor de texto, como o Visual Studio Code, e a um navegador. Não é necessário conhecer tags HTML para acompanhar este primeiro exercício.

## 2. Por que começar com um exemplo de “quase HTML”?

Em aulas introdutórias, é comum apresentar imediatamente uma estrutura completa de página, com várias marcações diferentes. Essa abordagem pode funcionar, mas o aluno também pode acabar copiando símbolos sem compreender por que eles existem.

O experimento da aula começa por uma pergunta anterior:

**O que o navegador faz com o conteúdo de um arquivo HTML antes de adicionarmos marcações?**

Vamos começar apenas com:

~~~text
Bom dia, meu nome é Leonardo
~~~

Não há uma tag nesse exemplo. É uma frase comum.

Essa escolha é deliberada: antes de aprender como controlar a apresentação do conteúdo, precisamos observar **como ele aparece sem essas instruções adicionais**.

Isso não significa que páginas profissionais devam ser construídas apenas com texto solto. Trata-se de um primeiro experimento, não de um modelo completo de documento.

## 3. Entenda o caminho entre editor, arquivo e navegador

Três elementos participam da atividade:

| Elemento | Para que serve neste exercício |
|---|---|
| Visual Studio Code | Permite escrever e modificar o conteúdo |
| Arquivo HTML | Guarda o texto que será lido pelo navegador |
| Navegador | Interpreta o documento e exibe a página |

O fluxo que vamos praticar é:

~~~text
Escrever no editor
        ↓
Salvar o arquivo HTML
        ↓
Abrir ou voltar ao navegador
        ↓
Atualizar a página
        ↓
Observar o resultado
~~~

Um detalhe faz muita diferença: **o navegador não lê pensamentos nem alterações que ainda estão apenas no editor**.

Se você modificou o conteúdo, mas não salvou o arquivo, a versão armazenada em disco continua sendo a anterior. Por isso, ao recarregar uma página aberta diretamente desse arquivo, o navegador pode mostrar o conteúdo antigo.

*Observação complementar:* ambientes de desenvolvimento com atualização automática podem seguir outro fluxo. Aqui estamos reproduzindo o exercício simples de salvar e recarregar manualmente.

## 4. Prepare seu primeiro arquivo

Para refazer o laboratório por conta própria:

1. Abra o Visual Studio Code.
2. Crie um arquivo de texto.
3. Salve-o com o nome sugerido `index.html`.
4. Digite a frase abaixo:

~~~text
Bom dia, meu nome é Leonardo
~~~

O nome `index.html` é uma **sugestão deste artigo** para reproduzir a prática; a transcrição não exige esse nome específico.

A extensão `.html` identifica o documento como um arquivo HTML. Mais tarde, aprenderemos como organizar esse documento com os elementos apropriados.

Por enquanto, o arquivo não precisa de mais conteúdo para o experimento que estamos estudando.

### Resultado esperado

Ao abrir esse arquivo em um navegador, você deverá conseguir visualizar a frase escrita.

O ponto principal não é a aparência da página, mas perceber que **o texto já pode ser exibido mesmo sem termos introduzido comandos de marcação**.

## 5. Salvar o arquivo não é um detalhe

Uma parte importante da aula é dedicada a um erro comum de iniciantes: editar um arquivo e esquecer de salvá-lo.

O professor destaca um indicador visual do Visual Studio Code. No exemplo mostrado, aparece um pequeno círculo quando o arquivo possui modificações que ainda não foram salvas.

Depois de salvar, esse indicador desaparece e a aba volta a apresentar seu estado normal.

A aparência exata da aba pode variar com a versão, as configurações e o sistema operacional. O princípio, porém, é sempre o mesmo: **uma edição pendente precisa ser gravada para atualizar o arquivo no disco**.

### Atalhos de salvamento

| Sistema | Atalho utilizado na aula |
|---|---|
| Windows | `Ctrl + S` |
| Linux | `Ctrl + S` |
| macOS | `Command + S` |

Experimente agora: acrescente um ponto de exclamação à frase e use o atalho correspondente.

~~~text
Bom dia, meu nome é Leonardo!
~~~

O importante é desenvolver um hábito simples: **alterou, salvou, verificou**.

## 6. Como visualizar a alteração no navegador

Com o arquivo salvo:

1. Abra o arquivo `index.html` no navegador, se ainda não estiver aberto.
2. Volte ao Visual Studio Code.
3. Modifique alguma palavra ou pontuação.
4. Salve o arquivo.
5. Retorne ao navegador.
6. Use o botão de recarregar a página.

Na demonstração, o professor faz exatamente esse movimento: salva a frase e recarrega a página para observar o texto.

### Um teste que vale a pena fazer

Primeiro, escreva e salve:

~~~text
Bom dia, meu nome é Leonardo
~~~

Abra a página no navegador.

Agora, troque o conteúdo no editor por:

~~~text
Bom dia, estou aprendendo HTML!
~~~

**Antes de salvar**, atualize o navegador. Em um fluxo comum de arquivo local, a página ainda mostrará o conteúdo que foi salvo anteriormente.

Depois, salve e atualize novamente. A nova frase deverá aparecer.

Esse pequeno contraste ajuda a identificar a diferença entre **editar** e **persistir a alteração**.

## 7. Zoom no editor e zoom no navegador são coisas diferentes

Durante a demonstração, o professor aumenta o zoom para facilitar a visualização.

Isso ocorre em dois ambientes diferentes:

- **No Visual Studio Code:** o zoom ajuda a enxergar melhor o conteúdo que está sendo editado.
- **No navegador:** o zoom ajuda a enxergar melhor a página apresentada.

Em geral, no Windows e no Linux, os atalhos `Ctrl + +` e `Ctrl + -` ajustam o zoom; no macOS, costuma-se usar `Command + +` e `Command + -`. Os comandos disponíveis podem variar com o aplicativo, o teclado e suas configurações.

O Chrome também oferece uma opção de zoom em seu menu.

**Atenção:** aumentar o zoom não significa adicionar texto, modificar seu conteúdo ou criar uma nova estrutura HTML. Você apenas muda a escala de visualização.

Esse detalhe evita uma confusão frequente: o que mudou foi a forma de visualizar, não necessariamente o arquivo.

## 8. O segundo experimento: duas frases

Chegamos ao momento mais interessante da aula.

Depois da primeira frase, o professor acrescenta outra:

~~~text
Bom dia, meu nome é Leonardo
Até logo!
~~~

No editor, parecem duas linhas.

O que você espera encontrar ao atualizar o navegador?

Talvez imagine:

~~~text
Bom dia, meu nome é Leonardo
Até logo!
~~~

Mas o experimento mostra que as frases aparecem como parte do mesmo fluxo de texto. Em uma janela com espaço suficiente, o resultado se parece com:

~~~text
Bom dia, meu nome é Leonardo Até logo!
~~~

**Por que pressionar Enter no arquivo não obrigou o navegador a começar outra linha?**

Essa pergunta prepara o conceito central da próxima etapa do curso.

## 9. E se deixarmos várias linhas em branco?

Na aula, o professor amplia o teste: insere várias linhas vazias entre as duas frases.

Algo assim:

~~~text
Bom dia, meu nome é Leonardo




Até logo!
~~~

Depois salva e recarrega a página.

O resultado continua sem reproduzir aquelas linhas vazias como separações verticais.

O professor ainda modifica a quantidade de pontos de exclamação para verificar que a atualização realmente ocorreu. A pontuação muda; a separação visual continua não funcionando como ele esperava.

Essa é uma boa prática de investigação: **mudar algo verificável para distinguir um problema de atualização de um comportamento do navegador**.

### O que isso demonstra?

No conteúdo textual HTML comum, sequências de espaços e quebras de linha do código-fonte são geralmente tratadas como espaços em branco que não preservam, na apresentação, a quantidade originalmente digitada.

Por isso:

- apertar Enter não garante uma quebra de linha visual;
- adicionar cinco linhas vazias não cria, por si só, cinco linhas vazias na página;
- salvar corretamente não obriga o navegador a preservar a formatação do texto-fonte.

*Precisão técnica complementar:* existem contextos e recursos específicos que preservam espaços ou controlam sua apresentação. Eles não são abordados nesta primeira prática, que observa o comportamento normal do texto HTML.

## 10. Código-fonte e página apresentada não são a mesma coisa

É importante distinguir duas representações do mesmo documento.

### No arquivo

~~~text
Bom dia, meu nome é Leonardo

Até logo!
~~~

### Na página, em uma janela suficientemente larga

~~~text
Bom dia, meu nome é Leonardo Até logo!
~~~

O arquivo contém caracteres de quebra de linha. Isso não obriga o navegador a convertê-los em separações visuais equivalentes.

A página é o **resultado da interpretação do documento**, não uma fotografia literal de cada espaço ou Enter digitado.

Essa distinção acompanhará todo o estudo do desenvolvimento web.

Mais adiante, você poderá escrever um código-fonte com uma organização legível para quem programa e, ao mesmo tempo, definir a estrutura visual que o navegador deve apresentar para quem utiliza a página.

## 11. Por que o HTML precisa de marcações?

A aula termina deixando um desafio para o próximo encontro: como fazer as duas frases aparecerem realmente em linhas diferentes?

O professor explica que será necessário introduzir um **comando de marcação HTML**.

Esse é um dos motivos pelos quais HTML não é simplesmente um editor de texto: a linguagem permite identificar partes e relações do conteúdo, dando estrutura a uma página.

A sigla **HTML** vem de *HyperText Markup Language*, ou **Linguagem de Marcação de Hipertexto**.

Vamos separar as ideias:

| Termo | Ideia inicial |
|---|---|
| HyperText (hipertexto) | Conteúdo que pode estabelecer conexões e navegação entre documentos ou recursos |
| Markup (marcação) | Indicações que descrevem a estrutura e o significado de partes do documento |
| Language (linguagem) | Conjunto de regras e convenções para expressar essas marcações |

A transcrição anuncia que o significado detalhado da sigla será discutido em outra aula. As definições da tabela são um complemento introdutório deste artigo.

**Importante:** para preservar a sequência didática da gravação, ainda não vamos resolver o desafio com tags específicas. Primeiro precisamos compreender **por que elas são necessárias**.

## 12. HTML, CSS e JavaScript: como essas tecnologias se relacionam?

Ao final da aula, o professor lembra que páginas e aplicações populares da internet podem utilizar HTML junto de CSS e JavaScript.

Uma distinção introdutória útil é:

| Tecnologia | Papel comum em uma aplicação web |
|---|---|
| HTML | Estruturar o conteúdo e seus elementos |
| CSS | Definir a apresentação visual |
| JavaScript | Implementar comportamentos e interações |

Isso não significa que qualquer sistema moderno seja construído apenas com essas três tecnologias. Aplicações complexas também podem envolver servidores, bancos de dados, APIs, processamento e infraestrutura.

O objetivo do comentário do professor é mostrar que o mesmo HTML usado em um exercício muito simples também participa de experiências web bastante mais elaboradas.

**Começar com uma frase no navegador não é perder tempo.** É conhecer uma das peças fundamentais de sistemas muito maiores.

## 13. Laboratório guiado: observe antes de concluir

Reproduza os testes abaixo com seu arquivo `index.html`.

### Experimento A — Primeiro texto

Digite:

~~~text
Estou começando a aprender HTML.
~~~

Salve e atualize a página.

**Pergunta:** a frase apareceu?

**O que observar:** o navegador consegue apresentar o texto inserido no arquivo.

### Experimento B — Alteração sem salvar

No editor, substitua o conteúdo por:

~~~text
Estou aprendendo a salvar meus arquivos.
~~~

Não salve imediatamente. Atualize o navegador.

Depois, salve e atualize mais uma vez.

**Pergunta:** em qual momento o novo conteúdo apareceu?

**O que observar:** a diferença entre o conteúdo em edição e o documento efetivamente salvo.

### Experimento C — Quebras de linha

Digite:

~~~text
Primeira frase.
Segunda frase.
Terceira frase.
~~~

Salve e atualize.

**Pergunta:** as três frases foram obrigatoriamente apresentadas como três linhas independentes?

**O que observar:** o navegador trata as quebras do arquivo-fonte de maneira diferente de um editor de texto comum.

### Experimento D — Espaços em branco

Digite:

~~~text
Primeira frase.


Segunda frase.
~~~

Depois acrescente mais linhas vazias, salve e atualize.

**Pergunta:** aumentar a quantidade de linhas vazias alterou a separação como você imaginava?

**O que observar:** acrescentar espaços no arquivo não é, por si só, uma instrução de apresentação da página.

Esses quatro experimentos são propostas didáticas complementares baseadas nos princípios demonstrados pela gravação.

## 14. Como diagnosticar os erros mais comuns

| Situação | Causa possível | O que conferir |
|---|---|---|
| Editei o texto, mas a página não mudou | Arquivo ainda não salvo | Salve e atualize o navegador |
| Salvei, mas continuo vendo outra frase | Navegador abriu outro arquivo ou aba | Confira o caminho do arquivo aberto |
| Apertei Enter e a segunda frase não foi para outra linha | Comportamento normal do texto HTML | Não confunda quebra no código com quebra na apresentação |
| Coloquei cinco linhas vazias, mas elas sumiram | Espaços em branco foram tratados como parte do fluxo do texto | Observe o comportamento antes de aprender as marcações |
| O texto ficou enorme | Zoom foi ampliado | Revise o zoom do navegador ou do editor |
| O texto ficou em outra linha sozinho | Janela estreita ou zoom elevado pode causar quebra automática | Aumente a largura da janela e compare |

A última situação merece atenção especial: **quebra automática por falta de espaço não é a mesma coisa que uma quebra de linha intencional criada pela estrutura ou apresentação**.

Um texto pode ocupar duas linhas porque a janela ficou estreita, mesmo quando seu conteúdo não determinou uma divisão entre as frases.

## 15. Desafio: investigue sem usar tags HTML

Vamos transformar a curiosidade da aula em um procedimento de investigação.

Crie o arquivo com este conteúdo:

~~~text
Meu primeiro contato com HTML.

Quero aprender a construir páginas.

Até a próxima lição!
~~~

Siga a sequência:

1. Registre como o texto aparece no editor.
2. Salve o arquivo.
3. Observe como aparece no navegador.
4. Reduza a largura da janela.
5. Aumente a largura novamente.
6. Insira novas linhas vazias no arquivo.
7. Salve e atualize.
8. Descreva o que mudou e o que permaneceu igual.

Responda às perguntas:

- As quebras do código-fonte foram preservadas?
- O texto ocupou mais linhas quando a janela ficou estreita?
- Essa mudança foi causada por novas instruções HTML?
- Por que salvar era necessário antes da verificação?
- O que ainda falta aprender para controlar a separação entre os trechos?

**Critério de conclusão:** você consegue explicar, com suas palavras, a diferença entre uma quebra de linha digitada no editor e uma quebra de linha apresentada pelo navegador.

Não há necessidade de antecipar a próxima aula para terminar o exercício.

## 16. Revise o que você aprendeu

**1. Posso ver texto no navegador mesmo sem usar uma tag HTML neste primeiro exercício?**

Sim. Esse é justamente o ponto de partida da demonstração. Trata-se de um experimento introdutório, e não de uma recomendação para construir páginas completas sem estrutura.

**2. Por que a alteração não apareceu quando atualizei a página?**

Uma explicação comum é que o arquivo não foi salvo. Também pode acontecer de você estar observando outro arquivo ou outra aba.

**3. Para que serve o atalho `Ctrl + S`?**

Para salvar o documento no editor, no Windows e no Linux. No macOS, o atalho apresentado é `Command + S`.

**4. O navegador preserva qualquer quantidade de linhas em branco que eu digite no texto HTML comum?**

Não. No contexto observado pela aula, esses espaços e quebras são tratados como espaços em branco do fluxo do texto.

**5. Se uma frase aparecer na linha seguinte quando eu estreitar a janela, resolvi o desafio da aula?**

Não. Uma quebra automática de texto por falta de espaço não equivale a determinar estruturalmente que duas frases devem aparecer separadas.

**6. Por que ainda não usamos uma tag para resolver a separação?**

Porque a proposta desta aula é perceber o problema primeiro. A marcação que resolverá o desafio é apresentada como assunto da próxima aula.

**7. HTML sozinho explica toda a tecnologia usada em grandes aplicações web?**

Não. HTML é uma parte importante, mas aplicações maiores costumam reunir outras tecnologias, incluindo CSS, JavaScript e componentes de backend.

## 17. Checklist para avançar

Antes da próxima lição, confira o que você já consegue fazer:

- [ ] Criei um arquivo com extensão `.html`.
- [ ] Escrevi uma frase simples no arquivo.
- [ ] Salvei usando o atalho do meu sistema operacional.
- [ ] Abri ou atualizei o arquivo no navegador.
- [ ] Observei uma alteração aparecer após salvar.
- [ ] Identifiquei a diferença entre zoom do editor e zoom da página.
- [ ] Acrescentei uma segunda frase.
- [ ] Testei várias linhas em branco entre as frases.
- [ ] Entendi que o navegador não reproduz literalmente toda a organização do código-fonte.
- [ ] Consigo formular o desafio que será resolvido com marcações HTML.

Se algum item ainda não estiver claro, retome o laboratório. A meta desta etapa é a **observação consciente**, não a velocidade.

## 18. Conclusão: o primeiro passo é compreender o navegador

O grande aprendizado da aula “Primeiro Exemplo #01” não é um comando novo. É uma descoberta sobre a relação entre **arquivo, editor e navegador**.

Você escreve um texto, salva o documento e o navegador o apresenta. Em seguida, altera o arquivo e percebe que precisa salvar e recarregar a página para verificar o resultado naquele fluxo de trabalho.

Ao tentar separar duas frases, encontra um comportamento inesperado: as linhas em branco que pareciam evidentes no editor não se transformam automaticamente em espaços visuais na página.

É essa diferença que cria a necessidade das **marcações HTML**.

Na próxima lição, o caminho natural será aprender como introduzir uma marcação que permita controlar a separação entre os conteúdos, avançando do texto simples para a estruturação da página.

**Ideia para guardar:** antes de decorar sintaxes, observe o que o navegador faz, formule uma hipótese, teste e compare os resultados.

## Fonte da aula e nota editorial

**Curso indicado no contexto do estudo:** Formação DEV — introdução a HTML.

**Aula-base:** “Primeiro Exemplo #01”.

**Material analisado:** legenda/transcrição enviada em formato WebVTT (`Primeiro Exemplo #01.vtt`).

**Professor identificado pela fala:** Leonardo.

**Duração aproximada:** 6 minutos e 27 segundos.

**Momentos centrais da gravação:**

- **00:00–00:38:** proposta do primeiro exercício e ajuste do zoom no editor;
- **00:38–01:48:** escrita da primeira frase e importância de salvar o documento;
- **01:49–02:55:** atualização da página e demonstração de zoom no navegador;
- **02:56–04:25:** inserção da segunda frase, testes com linhas vazias e observação do comportamento;
- **04:25–05:15:** introdução à ideia de HTML como linguagem de marcação;
- **05:15–06:27:** desafio da próxima aula e relação com páginas e aplicações web.

*Nota editorial: este é um artigo educacional independente, elaborado a partir da transcrição fornecida. Não é uma reprodução integral da aula nem uma publicação oficial da Formação DEV. O nome sugerido para o arquivo, os laboratórios adicionais, as explicações complementares, as tabelas, as perguntas de revisão e o checklist foram criados para fins de aprendizagem. O artigo não atribui esses complementos ao professor e não antecipa a demonstração das tags reservada para a aula seguinte.*

**Metadescrição:** Aprenda seu primeiro exemplo de HTML: escreva texto no VS Code, salve o arquivo, atualize o navegador e descubra como funcionam espaços e quebras de linha.

**Tags:** HTML, Formação DEV, Desenvolvimento Web, Programação, Iniciantes, Visual Studio Code, Navegadores, Chrome, Aprendizado Prático.
