---
title: "Maven no Java: o que é, como funciona e como usar na prática"
seo_title: "Maven no Java: POM, dependências, build e comandos essenciais"
slug: "maven-java-guia-pratico"
meta_description: "Aprenda Maven no Java na prática: entenda o pom.xml, dependências, ciclo de build, repositórios, plugins e os principais comandos com um projeto completo."
focus_keyphrase: "Maven no Java"
related_keywords:
  - "o que é Maven"
  - "Maven Java"
  - "como usar Maven"
  - "pom.xml"
  - "dependências Maven"
  - "ciclo de vida Maven"
  - "Maven Central"
  - "comandos Maven"
category: "Java"
tags:
  - "Java"
  - "Maven"
  - "Build"
  - "POM"
  - "Dependências"
  - "Ferramentas Java"
  - "Maven Central"
excerpt: "Guia prático de Maven no Java: entenda o POM, estrutura de projeto, ciclo de vida, dependências, repositórios, plugins e comandos essenciais."
---

# Maven no Java: o que é, como funciona e como usar na prática

O **Maven no Java** é uma das ferramentas mais importantes para organizar, construir, testar e distribuir projetos.

Quem começa a programar em Java geralmente consegue criar pequenos exemplos apenas com a IDE e o compilador. Quando o projeto cresce, porém, surgem problemas que não aparecem nos primeiros exercícios:

- onde colocar o código-fonte;
- onde colocar os testes;
- como adicionar bibliotecas externas;
- como controlar versões dessas bibliotecas;
- como compilar o projeto sempre da mesma maneira;
- como gerar um arquivo JAR;
- como executar testes automaticamente;
- como compartilhar o projeto sem enviar dezenas de arquivos JAR;
- como reproduzir o mesmo build em outra máquina ou em um servidor de integração contínua.

É nesse cenário que o Maven se torna útil.

Ele oferece convenções para a estrutura do projeto, um modelo padronizado de build e um mecanismo para trabalhar com dependências, plugins e repositórios.

O ponto central de um projeto Maven é o arquivo <code>pom.xml</code>, que descreve o projeto e informa ao Maven como ele deve ser construído.

Neste guia, vamos estudar Maven de forma prática. Além de entender sua origem e seus objetivos, criaremos um projeto, veremos o POM, o ciclo de vida, as dependências, os repositórios, os plugins e os comandos mais usados no dia a dia.

> O objetivo não é decorar comandos. É entender o modelo que o Maven aplica aos projetos Java.

## 1. O que é Maven?

Apache Maven é uma ferramenta de **gerenciamento e compreensão de projetos**.

No ecossistema Java, ele é usado principalmente para:

- organizar a estrutura do projeto;
- compilar código;
- executar testes;
- empacotar aplicações e bibliotecas;
- gerenciar dependências;
- executar plugins;
- gerar relatórios;
- instalar artefatos em repositórios locais;
- publicar artefatos em repositórios remotos.

A história do Maven ajuda a entender por que ele existe.

O projeto começou como uma tentativa de simplificar o processo de build do Jakarta Turbine. Havia vários projetos com arquivos Ant diferentes entre si, e arquivos JAR eram mantidos no controle de versão.

A equipe queria uma forma mais uniforme de construir os projetos, definir claramente o que fazia parte deles, publicar informações e compartilhar bibliotecas.

Essa necessidade levou à criação de um modelo de projeto padronizado.

A documentação oficial resume os objetivos do Maven em pontos como:

- tornar o processo de build mais simples;
- fornecer um sistema uniforme de build;
- disponibilizar informações de qualidade sobre o projeto;
- incentivar boas práticas de desenvolvimento.

O Maven não elimina a necessidade de entender Java, compilação, testes ou dependências. Ele organiza e automatiza grande parte desse trabalho.

## 2. Maven não serve apenas para baixar bibliotecas

É muito comum ouvir:

> Maven serve para baixar dependências.

Isso é apenas uma parte do que ele faz.

Imagine um projeto Java sem uma ferramenta de gerenciamento. Você poderia precisar:

1. procurar uma biblioteca na internet;
2. baixar o arquivo JAR;
3. copiar esse arquivo para uma pasta do projeto;
4. configurar o classpath;
5. descobrir manualmente quais outras bibliotecas aquela biblioteca precisa;
6. repetir tudo quando uma versão for atualizada;
7. garantir que todos da equipe tenham exatamente as mesmas versões.

Com Maven, você normalmente declara a dependência no POM:

~~~xml
<dependency>
    <groupId>org.exemplo</groupId>
    <artifactId>biblioteca</artifactId>
    <version>1.2.0</version>
</dependency>
~~~

O Maven pode resolver o artefato em um repositório configurado e incluí-lo no build.

Mas ele também cuida de outras partes do processo:

~~~text
código + pom.xml + dependências
              ↓
            Maven
              ↓
   valida → compila → testa
              ↓
           empacota
              ↓
          JAR / WAR
~~~

Por isso, pense no Maven como um **orquestrador do build do projeto**, e não apenas como um gerenciador de bibliotecas.

## 3. O que significa build?

A palavra build aparece constantemente em projetos Java.

Build é o processo de transformar código-fonte e recursos em um resultado utilizável.

Um fluxo simplificado pode ser:

~~~text
código-fonte
    ↓
compilação
    ↓
testes
    ↓
recursos
    ↓
empacotamento
    ↓
arquivo JAR
~~~

Em aplicações reais, o build pode incluir ainda:

- geração de código;
- análise estática;
- cobertura de testes;
- testes de integração;
- empacotamento adicional;
- assinatura de artefatos;
- publicação.

O Maven organiza essas etapas usando **ciclos de vida, fases e plugins**.

## 4. O coração do Maven: pom.xml

POM significa:

**Project Object Model**

O arquivo <code>pom.xml</code> é a unidade fundamental de um projeto Maven.

Ele contém informações sobre o projeto e configurações usadas no build.

Um POM mínimo pode ser:

~~~xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="
             http://maven.apache.org/POM/4.0.0
             https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.michel</groupId>
    <artifactId>exemplo-maven</artifactId>
    <version>1.0.0-SNAPSHOT</version>

</project>
~~~

As três coordenadas mais conhecidas são:

- <code>groupId</code>;
- <code>artifactId</code>;
- <code>version</code>.

Elas ajudam a identificar um artefato.

### groupId

Representa o grupo, organização ou domínio lógico responsável pelo projeto.

Exemplo:

~~~xml
<groupId>com.michel</groupId>
~~~

É comum usar nomes baseados em domínio invertido:

~~~text
br.com.empresa
com.exemplo
org.projeto
~~~

### artifactId

É o identificador do projeto ou módulo.

~~~xml
<artifactId>sistema-clientes</artifactId>
~~~

### version

Representa a versão:

~~~xml
<version>1.0.0</version>
~~~

Durante o desenvolvimento, também é comum encontrar:

~~~xml
<version>1.0.0-SNAPSHOT</version>
~~~

Uma forma simples de visualizar a identidade de um artefato é:

~~~text
com.michel:exemplo-maven:1.0.0-SNAPSHOT
~~~

## 5. O que significa SNAPSHOT?

O sufixo <code>SNAPSHOT</code> indica normalmente uma versão em desenvolvimento.

Exemplo:

~~~text
1.0.0-SNAPSHOT
~~~

Ela não representa necessariamente um lançamento final e imutável.

Quando uma versão está pronta para lançamento, um projeto pode usar algo como:

~~~text
1.0.0
~~~

Essa distinção é importante em ambientes de desenvolvimento e em repositórios de artefatos.

## 6. Packaging: o que o projeto produz?

O POM pode informar o tipo de empacotamento:

~~~xml
<packaging>jar</packaging>
~~~

Alguns valores comuns são:

- <code>jar</code>;
- <code>war</code>;
- <code>pom</code>.

### JAR

É muito usado para bibliotecas e aplicações Java.

### WAR

É tradicionalmente usado para aplicações web implantadas em containers Servlet ou servidores compatíveis.

### POM

Aparece bastante em projetos pais, agregadores e estruturas multi-módulo.

Quando o packaging não é informado, o padrão é normalmente JAR.

## 7. Estrutura padrão de um projeto Maven

Uma das grandes vantagens do Maven é a convenção de diretórios.

Uma estrutura típica é:

~~~text
meu-projeto/
├── pom.xml
└── src/
    ├── main/
    │   ├── java/
    │   └── resources/
    └── test/
        ├── java/
        └── resources/
~~~

### src/main/java

Contém o código Java principal.

### src/main/resources

Contém recursos da aplicação, como:

- arquivos de propriedades;
- XML;
- templates;
- configurações;
- outros arquivos carregados pela aplicação.

### src/test/java

Contém testes.

### src/test/resources

Contém recursos usados pelos testes.

Essa padronização oferece uma vantagem prática enorme: ao abrir um projeto Maven desconhecido, você já sabe onde procurar as principais partes.

## 8. Criando o primeiro projeto Maven manualmente

Vamos criar um projeto simples para entender o que o Maven faz.

Estrutura:

~~~text
exemplo-maven/
├── pom.xml
└── src/
    └── main/
        └── java/
            └── com/
                └── michel/
                    └── App.java
~~~

Use este POM:

~~~xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="
             http://maven.apache.org/POM/4.0.0
             https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.michel</groupId>
    <artifactId>exemplo-maven</artifactId>
    <version>1.0.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.release>21</maven.compiler.release>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

</project>
~~~

Agora crie <code>App.java</code>:

~~~java
package com.michel;

public class App {

    public static void main(String[] args) {
        System.out.println("Olá, Maven!");
    }
}
~~~

Esse projeto ainda não possui nenhuma biblioteca externa.

Mesmo assim, Maven já pode aplicar sua estrutura e seu ciclo de build.

## 9. Verificando o ambiente

Abra o terminal dentro da pasta do projeto e execute:

~~~bash
mvn --version
~~~

ou:

~~~bash
mvn -v
~~~

O comando mostra informações como:

- versão do Maven;
- versão do Java usada pelo Maven;
- caminho da instalação;
- sistema operacional.

Essa verificação é muito importante.

Sua IDE pode estar configurada com um JDK e o Maven no terminal pode estar usando outro.

Quando um projeto funciona na IDE e falha no terminal, conferir <code>mvn --version</code> é uma das primeiras coisas a fazer.

## 10. Qual versão do Maven utilizar?

Na revisão deste artigo, em outubro de 2026, o projeto Apache lista o **Maven 3.10.0** como a versão GA mais recente da linha 3.10.x.

A documentação também informa que Maven 3.10.0 e 3.9.16 são as duas séries GA mantidas naquele momento.

O Maven 4 ainda aparece como **não GA**, com versões release candidate.

Esse detalhe é importante porque artigos antigos podem recomendar versões que já chegaram ao fim do suporte.

Não trate o número presente neste artigo como eterno. Antes de instalar Maven em uma máquina nova, consulte o histórico oficial de releases.

## 11. Compilando o projeto

Execute:

~~~bash
mvn compile
~~~

O Maven localizará o POM, aplicará a configuração e compilará o código principal.

Depois disso, aparecerá a pasta:

~~~text
target/
~~~

Por exemplo:

~~~text
target/
└── classes/
    └── com/
        └── michel/
            └── App.class
~~~

A pasta <code>target</code> contém resultados gerados pelo build.

Não coloque código-fonte importante dentro dela.

## 12. Gerando um arquivo JAR

Execute:

~~~bash
mvn package
~~~

O Maven executará as fases necessárias até chegar a <code>package</code>.

O resultado pode ser:

~~~text
target/exemplo-maven-1.0.0-SNAPSHOT.jar
~~~

Observe que o nome do artefato vem das informações do POM.

Esse comportamento mostra uma característica essencial do ciclo de vida:

> Ao solicitar uma fase, Maven executa as fases anteriores necessárias.

Você não precisa rodar manualmente cada etapa uma por uma.

## 13. O ciclo de vida do Maven

O Maven possui ciclos de vida definidos.

Os mais conhecidos são:

- <code>clean</code>;
- <code>default</code>;
- <code>site</code>.

No desenvolvimento cotidiano, o ciclo default é o principal.

Algumas de suas fases são:

~~~text
validate
   ↓
compile
   ↓
test
   ↓
package
   ↓
verify
   ↓
install
   ↓
deploy
~~~

### validate

Verifica se o projeto possui as informações necessárias para continuar.

### compile

Compila o código-fonte principal.

### test

Executa os testes apropriados.

### package

Empacota o projeto, por exemplo em JAR ou WAR.

### verify

Executa verificações necessárias para validar o pacote.

### install

Instala o artefato no repositório local.

### deploy

Publica o artefato em um repositório remoto configurado.

A documentação oficial recomenda escolher a fase de acordo com o resultado desejado.

Quer gerar o pacote?

~~~bash
mvn package
~~~

Quer validar o build de maneira mais abrangente?

~~~bash
mvn verify
~~~

Precisa que outro projeto local consuma o artefato?

~~~bash
mvn install
~~~

## 14. O que faz mvn clean?

Durante o build, Maven produz arquivos dentro de <code>target</code>.

Para remover resultados do build anterior:

~~~bash
mvn clean
~~~

É comum combinar ciclos:

~~~bash
mvn clean package
~~~

ou:

~~~bash
mvn clean verify
~~~

O primeiro comando limpa o resultado anterior antes de gerar um novo pacote.

O segundo limpa e executa a validação do build até a fase verify.

## 15. O que faz mvn install?

O comando:

~~~bash
mvn install
~~~

executa as fases necessárias do ciclo default até <code>install</code>.

Além de compilar, testar e empacotar, o artefato é instalado no repositório Maven local.

Isso não significa "instalar o programa no Windows".

Imagine:

~~~text
Projeto A
    ↓
mvn install
    ↓
repositório Maven local
    ↓
Projeto B pode resolver A como dependência local
~~~

Essa diferença evita uma confusão bastante comum entre iniciantes.

## 16. Repositório local

Maven mantém um repositório local na máquina.

Em uma configuração padrão, ele costuma ficar em:

### Windows

~~~text
C:\Users\SEU_USUARIO\.m2\repository
~~~

### Linux e macOS

~~~text
~/.m2/repository
~~~

Ali podem ficar:

- dependências baixadas;
- plugins;
- metadados;
- artefatos instalados localmente.

Isso evita copiar a mesma biblioteca manualmente para cada projeto.

## 17. Maven Central e repositórios remotos

Quando Maven precisa de um artefato que ainda não está disponível localmente, ele pode consultá-lo em repositórios remotos configurados.

O repositório público mais conhecido é o **Maven Central**.

Fluxo simplificado:

~~~text
pom.xml declara uma dependência
             ↓
Maven procura localmente
             ↓
não encontrou?
             ↓
consulta repositório remoto
             ↓
baixa o artefato
             ↓
armazena no repositório local
             ↓
usa no build
~~~

Empresas também podem utilizar repositórios privados, por exemplo com gerenciadores como Nexus ou Artifactory.

Isso permite controlar:

- artefatos internos;
- acesso;
- segurança;
- auditoria;
- versões;
- disponibilidade.

## 18. Como declarar uma dependência

Dependências normalmente ficam dentro de:

~~~xml
<dependencies>
    ...
</dependencies>
~~~

Exemplo:

~~~xml
<dependencies>

    <dependency>
        <groupId>org.exemplo</groupId>
        <artifactId>biblioteca-exemplo</artifactId>
        <version>2.0.0</version>
    </dependency>

</dependencies>
~~~

As coordenadas permitem identificar exatamente qual artefato o projeto precisa.

## 19. Dependências transitivas

Um dos recursos mais importantes do Maven é a resolução de dependências transitivas.

Imagine:

~~~text
seu projeto
     ↓
Biblioteca A
     ↓
Biblioteca B
~~~

Seu POM declara A.

Se A depende de B, Maven pode resolver B automaticamente.

Isso reduz o trabalho manual, mas também pode criar conflitos entre versões.

Um comando essencial para investigar o que realmente entrou no projeto é:

~~~bash
mvn dependency:tree
~~~

Saída conceitual:

~~~text
meu-projeto
+- biblioteca-a:1.0
|  \- biblioteca-c:2.0
\- biblioteca-b:3.0
   \- biblioteca-d:1.4
~~~

Quando surgir a pergunta:

> Quem trouxe esta biblioteca?

A árvore de dependências é um excelente ponto de partida.

## 20. Escopos de dependência

Maven permite definir em quais contextos uma dependência é necessária.

Os scopes principais incluem:

### compile

É o padrão.

A dependência fica disponível conforme as regras normais de compilação e execução.

### provided

É necessária para compilar, mas espera-se que o ambiente de execução a forneça.

### runtime

Não é necessária para compilar o código principal, mas é necessária durante a execução.

### test

É usada apenas para testes.

Exemplo:

~~~xml
<dependency>
    <groupId>org.exemplo</groupId>
    <artifactId>biblioteca-de-testes</artifactId>
    <version>1.0.0</version>
    <scope>test</scope>
</dependency>
~~~

Existem ainda scopes como <code>system</code> e <code>import</code>, usados em situações específicas.

A própria documentação do Maven desaconselha tratar <code>system</code> como solução normal, pois ele prende a dependência a um caminho físico da máquina.

## 21. Dependência, plugin, fase e goal são coisas diferentes

Esses conceitos costumam ser misturados.

### Dependência

É normalmente uma biblioteca utilizada pelo projeto.

### Plugin

É uma extensão usada pelo Maven para executar tarefas de build.

Plugins podem:

- compilar;
- executar testes;
- gerar JAR;
- gerar documentação;
- analisar código;
- publicar artefatos.

Uma forma simples de pensar:

~~~text
dependência = algo que seu projeto usa

plugin = algo que Maven usa para executar o build
~~~

### Fase

É um estágio do ciclo de vida:

~~~text
compile
test
package
verify
install
~~~

### Goal

É uma tarefa específica fornecida por um plugin.

Quando você executa:

~~~bash
mvn package
~~~

está chamando uma fase.

Quando executa:

~~~bash
mvn dependency:tree
~~~

está chamando o goal <code>tree</code> de um plugin identificado pelo prefixo <code>dependency</code>.

## 22. Configurando a versão do Java

No exemplo deste artigo usamos:

~~~xml
<properties>
    <maven.compiler.release>21</maven.compiler.release>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
</properties>
~~~

Isso informa a release Java desejada para compilação conforme a configuração do plugin envolvido.

Mas existe um detalhe importante:

> O JDK realmente utilizado pelo Maven precisa ser compatível com o projeto.

Por isso, verifique:

~~~bash
mvn --version
~~~

Não confie apenas no Java selecionado na IDE.

## 23. O que é o POM efetivo?

Um POM pode herdar configurações de outros POMs e do próprio Super POM do Maven.

Por isso, nem tudo que influencia o build aparece explicitamente no arquivo que você está olhando.

Para visualizar o POM efetivo:

~~~bash
mvn help:effective-pom
~~~

Esse comando é muito útil quando:

- uma versão parece surgir sem estar declarada;
- um plugin foi herdado;
- existe um POM pai;
- você quer compreender propriedades efetivas;
- está investigando um comportamento inesperado.

É uma ferramenta de diagnóstico, não apenas uma curiosidade.

## 24. Maven Wrapper

Em equipes, pode acontecer:

~~~text
desenvolvedor A → Maven versão X
desenvolvedor B → Maven versão Y
servidor CI     → Maven versão Z
~~~

O **Maven Wrapper** ajuda a padronizar a execução.

Projetos configurados com Wrapper costumam possuir:

~~~text
mvnw
mvnw.cmd
.mvn/
~~~

Em vez de depender exclusivamente de uma instalação Maven global, você pode executar:

### Windows

~~~powershell
.\mvnw.cmd verify
~~~

### Linux e macOS

~~~bash
./mvnw verify
~~~

Isso facilita a reprodução do build por desenvolvedores e pipelines.

## 25. dependencyManagement

Projetos maiores frequentemente utilizam <code>dependencyManagement</code>.

Ele não é o mesmo que <code>dependencies</code>.

### dependencies

Declara as bibliotecas que o módulo utiliza.

### dependencyManagement

Centraliza regras e versões de dependências.

Exemplo:

~~~xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.exemplo</groupId>
            <artifactId>biblioteca</artifactId>
            <version>2.5.0</version>
        </dependency>
    </dependencies>
</dependencyManagement>
~~~

Um módulo filho pode então declarar:

~~~xml
<dependency>
    <groupId>org.exemplo</groupId>
    <artifactId>biblioteca</artifactId>
</dependency>
~~~

e receber a versão gerenciada, dependendo da hierarquia do projeto.

Esse recurso aparece bastante em aplicações corporativas.

## 26. O que é um BOM?

BOM significa:

**Bill of Materials**

No Maven, um BOM permite trabalhar com um conjunto coordenado de versões.

Em vez de escolher manualmente a versão de cada biblioteca de um ecossistema, o projeto pode importar um BOM responsável por manter versões compatíveis.

Esse conceito aparece bastante em frameworks modernos.

O importante para o iniciante é compreender:

> BOM não é uma biblioteca comum; ele é usado para gerenciamento consistente de versões.

## 27. Projetos multi-módulo

Maven também trabalha com projetos formados por vários módulos.

Exemplo:

~~~text
sistema/
├── pom.xml
├── dominio/
│   └── pom.xml
├── persistencia/
│   └── pom.xml
└── api/
    └── pom.xml
~~~

O POM agregador pode declarar:

~~~xml
<modules>
    <module>dominio</module>
    <module>persistencia</module>
    <module>api</module>
</modules>
~~~

Isso permite construir vários módulos de forma coordenada.

Não é necessário começar os estudos por multi-módulo, mas é importante saber que Maven vai muito além de um único JAR.

## 28. Maven e Spring Boot

Quem estuda Spring Boot encontra Maven rapidamente.

Em um projeto Spring Boot, Maven pode cuidar de:

- starters;
- dependências;
- compilação;
- testes;
- empacotamento;
- plugins;
- geração de aplicação executável.

Por isso, entender Maven torna mais fácil compreender um projeto Spring.

Quando abrir um <code>pom.xml</code> de Spring Boot, procure identificar:

- parent;
- properties;
- dependencies;
- dependencyManagement;
- build;
- plugins.

Não trate o POM como um arquivo mágico criado pela IDE.

Leia o arquivo e tente entender de onde vêm as configurações.

## 29. Maven versus Gradle

Maven não é a única ferramenta de build do ecossistema Java.

Gradle é outra opção bastante conhecida.

De forma simplificada:

### Maven

- forte uso de convenções;
- configuração tradicional em XML;
- enorme presença em projetos Java;
- ecossistema maduro;
- modelo bastante previsível.

### Gradle

- scripts em Kotlin ou Groovy;
- alta flexibilidade;
- forte presença em projetos JVM e Android;
- modelo de build diferente.

Não existe uma resposta universal para "qual é melhor".

Uma pergunta mais útil é:

> Qual ferramenta este projeto usa e eu consigo entender como o build funciona?

Para quem trabalha com Java profissionalmente, conhecer Maven continua sendo extremamente útil.

## 30. Maven substitui Git?

Não.

São ferramentas para problemas diferentes.

### Git

Controla versões do código.

### Maven

Gerencia o build e diversos aspectos do projeto.

Fluxo comum:

~~~text
Git
 ↓
código + pom.xml
 ↓
Maven
 ↓
compila + testa + empacota
 ↓
artefato
~~~

É normal usar os dois juntos.

## 31. Maven substitui a IDE?

Também não.

Você pode executar Maven:

- pela linha de comando;
- pelo IntelliJ IDEA;
- pelo Eclipse;
- pelo VS Code;
- em um servidor CI.

Um projeto bem configurado não deve depender exclusivamente de uma sequência de cliques em uma IDE específica.

Ter um build reproduzível pelo terminal é uma vantagem importante.

## 32. É uma boa ideia colocar JARs dentro do Git?

Na maioria dos projetos Maven, não.

Em vez de versionar arquivos binários manualmente, o projeto normalmente registra as coordenadas das dependências no POM.

Exemplo conceitual:

~~~text
abordagem manual

projeto/
├── lib/
│   ├── biblioteca-a.jar
│   ├── biblioteca-b.jar
│   └── biblioteca-c.jar
└── src/


abordagem Maven

projeto/
├── pom.xml
└── src/
~~~

Existem exceções, mas copiar JARs manualmente não deve ser a solução padrão quando a biblioteca está disponível em um repositório apropriado.

## 33. Comandos Maven essenciais

| Comando | Para que serve |
|---|---|
| <code>mvn --version</code> | Mostra Maven, Java e ambiente |
| <code>mvn clean</code> | Remove resultados do build anterior |
| <code>mvn compile</code> | Compila o código principal |
| <code>mvn test</code> | Executa testes |
| <code>mvn package</code> | Gera o pacote |
| <code>mvn verify</code> | Executa verificações até a fase verify |
| <code>mvn install</code> | Instala o artefato no repositório local |
| <code>mvn deploy</code> | Publica em repositório remoto configurado |
| <code>mvn dependency:tree</code> | Exibe a árvore de dependências |
| <code>mvn help:effective-pom</code> | Mostra o POM efetivo |

Você não precisa decorar todos no primeiro dia.

Uma base útil é conhecer:

~~~text
clean
test
package
verify
install
dependency:tree
~~~

## 34. Um fluxo de trabalho real

Imagine que você clonou um projeto Maven.

Primeiro, verifique o ambiente:

~~~bash
mvn --version
~~~

Depois tente validar o build:

~~~bash
mvn clean verify
~~~

Se houver problema de dependência:

~~~bash
mvn dependency:tree
~~~

Se não entender de onde veio uma configuração:

~~~bash
mvn help:effective-pom
~~~

Se precisar gerar apenas o pacote:

~~~bash
mvn package
~~~

Se outro projeto local precisar consumir aquele artefato:

~~~bash
mvn install
~~~

Esse pequeno conjunto cobre grande parte das situações comuns.

## 35. Como investigar um build Maven quebrado

Quando o build falhar, não adote a estratégia de apagar arquivos aleatoriamente até funcionar.

Use um processo.

### Passo 1 — leia o primeiro erro relevante

Muitas mensagens posteriores são apenas consequência da primeira falha.

### Passo 2 — confira o ambiente

~~~bash
mvn --version
~~~

Confira Maven e Java.

### Passo 3 — faça um build limpo

~~~bash
mvn clean verify
~~~

### Passo 4 — analise dependências

~~~bash
mvn dependency:tree
~~~

### Passo 5 — veja o POM efetivo

~~~bash
mvn help:effective-pom
~~~

### Passo 6 — aumente o nível de detalhes apenas se necessário

Logs muito extensos podem dificultar a análise.

Primeiro entenda a mensagem principal e só então aumente a verbosidade.

## 36. Erros comuns de quem está começando

### Erro 1 — editar arquivos dentro de target

A pasta <code>target</code> contém resultados gerados.

Ela pode ser removida por:

~~~bash
mvn clean
~~~

Não coloque código-fonte importante ali.

### Erro 2 — não conferir o Java usado pelo Maven

Execute:

~~~bash
mvn --version
~~~

A IDE e o terminal podem estar usando JDKs diferentes.

### Erro 3 — copiar JAR manualmente sem necessidade

Antes de criar uma pasta lib, verifique se a dependência possui coordenadas em um repositório confiável.

### Erro 4 — ignorar dependency:tree

Conflitos de versão e dependências transitivas ficam mais claros com:

~~~bash
mvn dependency:tree
~~~

### Erro 5 — confundir install com instalação do programa

<code>mvn install</code> instala o artefato no repositório Maven local.

### Erro 6 — atualizar tudo cegamente

Versão mais nova não significa atualização automática sem análise.

Leia notas de versão, valide compatibilidade e execute testes.

### Erro 7 — usar somente a IDE

Aprenda a validar o projeto pelo terminal.

Isso ajuda a separar problemas da IDE de problemas reais do build.

### Erro 8 — não entender herança de POM

Se uma versão não aparece no seu POM, ela pode ter vindo de parent, dependencyManagement, BOM ou Super POM.

Use:

~~~bash
mvn help:effective-pom
~~~

## 37. O que mudou em relação a projetos Java antigos?

Em projetos antigos, era comum encontrar:

- arquivos JAR versionados junto com o código;
- scripts Ant específicos para cada projeto;
- estruturas de diretórios diferentes;
- configurações dependentes da máquina;
- documentação incompleta sobre como construir o sistema.

Maven ajudou a popularizar uma abordagem baseada em:

- convenções;
- metadados;
- coordenadas;
- repositórios;
- plugins reutilizáveis;
- ciclo de vida padronizado.

Isso não significa que Maven seja perfeito.

POMs grandes podem ficar extensos e builds complexos exigem conhecimento.

Mesmo assim, a previsibilidade oferecida pelas convenções continua sendo uma das maiores vantagens.

## 38. Quando vale a pena usar Maven?

Maven é especialmente útil quando você precisa:

- gerenciar dependências;
- automatizar build;
- padronizar diretórios;
- executar testes;
- gerar JAR ou WAR;
- trabalhar em equipe;
- integrar com CI/CD;
- publicar bibliotecas;
- usar frameworks com forte integração Maven.

Para um único arquivo Java de poucas linhas, Maven pode ser desnecessário.

Mas quando aparecem testes, dependências e empacotamento, seu valor fica evidente.

## 39. Exercício prático

Crie um projeto chamado:

~~~text
calculadora-maven
~~~

Use:

~~~text
groupId: com.seunome
artifactId: calculadora-maven
version: 1.0.0-SNAPSHOT
Java: 21
~~~

Crie:

~~~java
package com.seunome;

public class Calculadora {

    public int somar(int a, int b) {
        return a + b;
    }

    public int subtrair(int a, int b) {
        return a - b;
    }
}
~~~

Depois execute:

~~~bash
mvn clean compile
~~~

Em seguida:

~~~bash
mvn package
~~~

Agora investigue a pasta <code>target</code>.

Responda:

1. onde ficou o arquivo <code>.class</code>?
2. qual JAR foi gerado?
3. de onde veio o nome do JAR?
4. o que acontece depois de <code>mvn clean</code>?
5. o que muda no nome do artefato se você alterar a versão no POM?

Esse exercício ensina mais do que simplesmente decorar comandos.

## 40. Desafio: leia um POM real

Abra um projeto Java real que use Maven.

Tente localizar:

~~~text
groupId
artifactId
version
packaging
properties
dependencies
dependencyManagement
build
plugins
parent
modules
~~~

Nem todo projeto terá todos esses elementos.

O objetivo é começar a enxergar o POM como uma descrição do projeto, e não como um grande bloco de XML misterioso.

## 41. Perguntas frequentes sobre Maven

### Maven é obrigatório para programar em Java?

Não.

É possível compilar e executar Java sem Maven.

Entretanto, Maven é muito usado profissionalmente porque resolve problemas de build, dependências e padronização.

### Maven é uma linguagem de programação?

Não.

Maven é uma ferramenta.

O POM é um documento XML declarativo.

### O que é pom.xml?

É o Project Object Model.

Ele contém informações do projeto e configurações usadas pelo Maven.

### O que é a pasta .m2?

Ela é usada para configurações do usuário Maven e, normalmente, para o repositório local.

### Posso apagar .m2/repository?

Artefatos remotos podem ser baixados novamente quando necessários, mas apagar todo o repositório local não deve ser a resposta automática para qualquer erro.

Primeiro investigue a causa.

### O que é Maven Central?

É um dos principais repositórios públicos de artefatos usados pelo ecossistema Maven.

### O que faz mvn clean install?

Executa o ciclo clean e depois as fases necessárias até install, conforme o build configurado.

### Devo usar package, verify ou install?

Depende do objetivo:

- quer gerar o pacote: <code>package</code>;
- quer validar o build até as verificações configuradas: <code>verify</code>;
- precisa instalar o artefato no repositório local: <code>install</code>.

Não use install apenas por hábito quando verify já atende sua necessidade.

### Maven funciona apenas com Java?

Seu foco histórico é o ecossistema Java/JVM, mas o sistema de plugins permite integrar outras ferramentas e tarefas ao build.

## 42. Checklist: você entendeu o básico de Maven?

Antes de considerar este assunto concluído, tente explicar sem consultar o artigo:

- o que é Maven;
- o que significa build;
- para que serve o POM;
- o que são groupId, artifactId e version;
- o que significa SNAPSHOT;
- onde fica o código principal;
- onde ficam os testes;
- para que serve target;
- diferença entre clean, package, verify e install;
- o que é repositório local;
- o que é repositório remoto;
- o que é Maven Central;
- o que é dependência transitiva;
- para que serve dependency:tree;
- diferença entre dependência e plugin;
- diferença entre fase e goal;
- o que é POM efetivo;
- por que Maven Wrapper é útil;
- o que dependencyManagement faz;
- o que é um BOM.

Se você consegue explicar esses pontos com suas próprias palavras, já possui uma base de Maven muito mais útil do que apenas saber executar <code>mvn install</code>.

## 43. Mapa mental

Guarde este fluxo:

~~~text
            pom.xml
               │
               ├── identidade
               ├── propriedades
               ├── dependências
               └── plugins
               │
               ▼
             Maven
               │
      ┌────────┼─────────┐
      ▼        ▼         ▼
   compile    test     package
      │        │         │
      └────────┴─────────┘
               │
               ▼
            verify
               │
               ▼
            install
               │
               ▼
             deploy
~~~

A ideia central é:

> Maven fornece um modelo padronizado para descrever, construir, testar e distribuir projetos.

## Conclusão

O Maven nasceu para reduzir a desorganização do processo de build e criar uma maneira mais uniforme de trabalhar com projetos Java.

Sua importância não está apenas em baixar bibliotecas.

Ele conecta:

~~~text
estrutura do projeto
       +
     pom.xml
       +
   dependências
       +
     plugins
       +
 ciclo de vida
       ↓
 build reproduzível
~~~

A melhor maneira de aprender Maven é utilizá-lo.

Crie um projeto pequeno, execute os comandos pelo terminal, examine a pasta <code>target</code>, veja a árvore de dependências e leia o POM.

Depois faça o mesmo em um projeto Spring Boot.

Quando você deixa de enxergar Maven como "aquele XML enorme" e passa a compreender seu modelo, o funcionamento de muitos projetos Java profissionais começa a fazer sentido.

## Referências oficiais

- Apache Maven — Introduction: https://maven.apache.org/what-is-maven
- Apache Maven — Getting Started Guide: https://maven.apache.org/guides/getting-started/index.html
- Apache Maven — Maven in 5 Minutes: https://maven.apache.org/guides/getting-started/maven-in-five-minutes
- Apache Maven — Introduction to the POM: https://maven.apache.org/guides/introduction/introduction-to-the-pom.html
- Apache Maven — Build Lifecycle: https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html
- Apache Maven — Dependency Mechanism: https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html
- Apache Maven — Standard Directory Layout: https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html
- Apache Maven — Release History: https://maven.apache.org/docs/history.html

## Próximos estudos recomendados

Depois deste conteúdo, avance para:

1. criar um projeto Maven com testes JUnit;
2. estudar Maven Surefire Plugin;
3. aprofundar dependencyManagement e BOM;
4. criar um projeto multi-módulo;
5. utilizar Maven Wrapper;
6. analisar o POM de um projeto Spring Boot;
7. integrar Maven em pipelines de CI/CD.

> **Ideia central:** Maven vale mais quando você entende o modelo de projeto e o ciclo de build, em vez de apenas decorar uma lista de comandos.
