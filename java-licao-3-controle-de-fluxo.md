---
title: "Java — Lição 3: Controle de Fluxo com if, else e switch"
seo_title: "Java Lição 3: if, else, switch e operador ternário"
slug: "java-licao-3-controle-de-fluxo"
meta_description: "Aprenda controle de fluxo em Java com if, else, else if, switch, operadores lógicos e ternário, com exemplos, exercícios e desafios."
focus_keyphrase: "java lição 3"
related_keywords:
  - "aprender java"
  - "java para iniciantes"
  - "if else java"
  - "switch java"
  - "controle de fluxo java"
  - "operador ternário java"
  - "condicionais java"
  - "switch expression java"
category: "Java"
tags:
  - "Java"
  - "Programação"
  - "Iniciantes"
  - "Controle de Fluxo"
  - "If Else"
  - "Switch"
  - "Operador Ternário"
  - "Lógica de Programação"
excerpt: "Terceira lição de Java: aprenda a criar decisões com if, else, else if, switch, operadores lógicos e operador ternário."
---

# ☕ Java — Lição 3: Controle de Fluxo

Nesta terceira lição da nossa sequência para dominar Java, vamos aprender a fazer o programa **tomar decisões**.

Até agora trabalhamos com:

- variáveis;
- tipos primitivos;
- operadores aritméticos;
- operadores relacionais;
- operadores lógicos.

Esses conhecimentos serão usados agora para controlar quais instruções devem ou não ser executadas.

O objetivo não é apenas decorar `if`, `else` e `switch`, mas aprender a transformar **regras e decisões do mundo real em código Java**.

## 1. O que vamos dominar hoje

Nesta lição, você aprenderá:

- como funciona o `if`;
- como usar `if` e `else`;
- como criar várias decisões com `else if`;
- por que a ordem das condições importa;
- como combinar condições com `&&`, `||` e `!`;
- como usar condições aninhadas;
- como funciona o `switch`;
- para que serve o `break`;
- para que serve o `default`;
- como utilizar o `switch` moderno;
- como um `switch` pode produzir um valor;
- como usar o operador ternário;
- quando escolher `if` ou `switch`.

Ao final, criaremos pequenos programas para praticar esses conceitos.

## 2. O que é controle de fluxo

Um programa Java normalmente executa suas instruções de cima para baixo.

Por exemplo:

```java
System.out.println("Linha 1");
System.out.println("Linha 2");
System.out.println("Linha 3");
```

O resultado será:

```text
Linha 1
Linha 2
Linha 3
```

Mas programas reais precisam tomar decisões.

Por exemplo:

> Se a pessoa tiver 18 anos ou mais, permita a entrada.

Ou:

> Se o saldo for suficiente, realize o pagamento.

Ou:

> Se a nota for maior ou igual a 7, o aluno está aprovado.

É nesse momento que entram as **estruturas condicionais**.

## 3. A estrutura `if`

A palavra `if` significa:

> se.

A estrutura básica é:

```java
if (condicao) {
    // código executado se a condição for verdadeira
}
```

Por exemplo:

```java
int idade = 20;

if (idade >= 18) {
    System.out.println("Maior de idade");
}
```

A expressão:

```java
idade >= 18
```

produz um valor booleano:

```text
true
```

ou:

```text
false
```

Se o resultado for `true`, o código entre as chaves será executado.

Se for `false`, o bloco será ignorado.

## 4. Primeiro exemplo com `if`

Observe:

```java
public class TesteIdade {

    public static void main(String[] args) {

        int idade = 25;

        if (idade >= 18) {
            System.out.println("Entrada permitida.");
        }
    }
}
```

Como:

```java
idade = 25;
```

e:

```java
25 >= 18
```

é verdadeiro, o resultado será:

```text
Entrada permitida.
```

Agora altere:

```java
int idade = 15;
```

A condição:

```java
idade >= 18
```

passará a ser falsa.

Nesse caso, a mensagem não será exibida.

## 5. `if` com `else`

Muitas vezes queremos executar uma ação quando a condição for verdadeira e outra quando for falsa.

Para isso usamos:

```java
if (condicao) {

} else {

}
```

Exemplo:

```java
int idade = 16;

if (idade >= 18) {
    System.out.println("Maior de idade");
} else {
    System.out.println("Menor de idade");
}
```

Resultado:

```text
Menor de idade
```

Nesse caso, sempre um dos dois blocos será executado.

Podemos pensar da seguinte forma:

```text
SE condição for verdadeira
    execute o primeiro bloco
SENÃO
    execute o segundo bloco
```

## 6. Várias possibilidades com `else if`

Nem sempre temos apenas duas possibilidades.

Imagine uma classificação baseada em nota:

```java
double nota = 7.5;

if (nota >= 9) {
    System.out.println("Excelente");
} else if (nota >= 7) {
    System.out.println("Aprovado");
} else if (nota >= 5) {
    System.out.println("Recuperação");
} else {
    System.out.println("Reprovado");
}
```

Como:

```java
nota = 7.5;
```

a primeira condição:

```java
nota >= 9
```

é falsa.

Java passa para:

```java
nota >= 7
```

Essa condição é verdadeira.

Portanto:

```text
Aprovado
```

será exibido.

Depois disso, Java não continua testando os demais blocos daquela cadeia.

## 7. A ordem das condições importa

Este é um detalhe muito importante.

Observe:

```java
int idade = 65;

if (idade >= 18) {
    System.out.println("Adulto");
} else if (idade >= 60) {
    System.out.println("Idoso");
}
```

Qual será o resultado?

```text
Adulto
```

Mas uma pessoa com 65 anos deveria ser classificada como idosa segundo nossa regra.

O problema está na ordem.

A primeira condição:

```java
idade >= 18
```

já é verdadeira.

Java executa esse bloco e não testa o próximo `else if`.

Uma solução seria:

```java
int idade = 65;

if (idade >= 60) {
    System.out.println("Idoso");
} else if (idade >= 18) {
    System.out.println("Adulto");
} else {
    System.out.println("Menor de idade");
}
```

Agora o resultado será:

```text
Idoso
```

Essa situação ensina uma das ideias mais importantes da programação:

> Um código pode compilar corretamente e ainda possuir um erro de lógica.

## 8. Combinando condições com `&&`

Na lição anterior aprendemos que:

```java
&&
```

representa o operador lógico **E**.

Imagine que uma pessoa somente possa dirigir se:

- tiver pelo menos 18 anos;
- possuir habilitação.

Podemos escrever:

```java
int idade = 25;
boolean possuiHabilitacao = true;

if (idade >= 18 && possuiHabilitacao) {
    System.out.println("Pode dirigir.");
} else {
    System.out.println("Não pode dirigir.");
}
```

Para que o primeiro bloco seja executado, as duas condições precisam ser verdadeiras.

Podemos visualizar assim:

```text
idade >= 18              -> true
possuiHabilitacao        -> true

true && true             -> true
```

Portanto:

```text
Pode dirigir.
```

## 9. Não compare `boolean` com `true` sem necessidade

Você poderá encontrar códigos assim:

```java
if (possuiHabilitacao == true) {
    System.out.println("Possui habilitação.");
}
```

Isso funciona.

Mas a variável já contém um valor booleano.

Portanto, podemos escrever simplesmente:

```java
if (possuiHabilitacao) {
    System.out.println("Possui habilitação.");
}
```

Para verificar o contrário:

```java
if (!possuiHabilitacao) {
    System.out.println("Não possui habilitação.");
}
```

O operador:

```java
!
```

significa **negação**.

Se:

```java
possuiHabilitacao = false;
```

então:

```java
!possuiHabilitacao
```

será:

```text
true
```

## 10. Combinando condições com `||`

O operador:

```java
||
```

representa **OU**.

Imagine que uma pessoa tenha direito a determinado desconto se for:

- estudante;
- ou tiver 60 anos ou mais.

```java
boolean estudante = false;
int idade = 70;

if (estudante || idade >= 60) {
    System.out.println("Possui desconto.");
}
```

Temos:

```text
estudante       -> false
idade >= 60     -> true
```

Portanto:

```text
false || true
```

resulta em:

```text
true
```

A mensagem será exibida.

Com `||`, basta uma das condições ser verdadeira.

## 11. Condições aninhadas

Podemos colocar um `if` dentro de outro `if`.

Exemplo:

```java
int idade = 25;
boolean possuiHabilitacao = true;

if (idade >= 18) {

    if (possuiHabilitacao) {
        System.out.println("Pode dirigir.");
    }

}
```

Esse código funciona.

Chamamos isso de **condição aninhada**.

Porém, nesse exemplo podemos simplificar:

```java
if (idade >= 18 && possuiHabilitacao) {
    System.out.println("Pode dirigir.");
}
```

As duas formas são válidas.

Porém, devemos evitar níveis de aninhamento desnecessários porque eles podem tornar o código mais difícil de entender.

## 12. A estrutura `switch`

Imagine que temos um número representando um dia da semana.

Poderíamos fazer:

```java
int dia = 2;

if (dia == 1) {
    System.out.println("Domingo");
} else if (dia == 2) {
    System.out.println("Segunda-feira");
} else if (dia == 3) {
    System.out.println("Terça-feira");
}
```

Funciona.

Mas Java possui uma estrutura muito útil para situações em que comparamos um mesmo valor contra várias possibilidades:

```java
switch
```

## 13. `switch` tradicional

Exemplo:

```java
int dia = 2;

switch (dia) {

    case 1:
        System.out.println("Domingo");
        break;

    case 2:
        System.out.println("Segunda-feira");
        break;

    case 3:
        System.out.println("Terça-feira");
        break;

    default:
        System.out.println("Dia inválido");
}
```

Como:

```java
dia = 2;
```

Java encontrará:

```java
case 2:
```

e exibirá:

```text
Segunda-feira
```

## 14. O que significa `case`

Cada `case` representa uma possibilidade.

Por exemplo:

```java
case 1:
```

significa:

> Caso o valor do `switch` seja 1.

Já:

```java
case 2:
```

significa:

> Caso o valor seja 2.

Assim podemos tratar várias alternativas de forma organizada.

## 15. Para que serve o `break`

Observe:

```java
case 1:
    System.out.println("Domingo");
    break;
```

O comando:

```java
break;
```

interrompe a execução daquele `switch`.

Sem ele, no `switch` tradicional, Java pode continuar executando os próximos casos.

Observe:

```java
int numero = 1;

switch (numero) {

    case 1:
        System.out.println("Um");

    case 2:
        System.out.println("Dois");

    case 3:
        System.out.println("Três");
}
```

O resultado será:

```text
Um
Dois
Três
```

Isso acontece porque não colocamos `break`.

Esse comportamento é chamado de:

**fall-through**.

Em algumas situações ele pode ser intencional.

Porém, muitas vezes representa um erro.

## 16. Para que serve `default`

O `default` funciona de maneira semelhante ao `else`.

Exemplo:

```java
int dia = 10;

switch (dia) {

    case 1:
        System.out.println("Domingo");
        break;

    case 2:
        System.out.println("Segunda-feira");
        break;

    default:
        System.out.println("Dia inválido");
}
```

Como nenhum `case` corresponde ao valor:

```java
10
```

Java executará:

```java
default:
```

Resultado:

```text
Dia inválido
```

## 17. `switch` moderno

Java também possui uma sintaxe moderna de `switch`.

Exemplo:

```java
int dia = 2;

switch (dia) {
    case 1 -> System.out.println("Domingo");
    case 2 -> System.out.println("Segunda-feira");
    case 3 -> System.out.println("Terça-feira");
    default -> System.out.println("Dia inválido");
}
```

Observe o uso de:

```java
->
```

Nessa forma não precisamos escrever:

```java
break;
```

para impedir que o próximo caso seja executado.

Essa sintaxe deixa muitos `switch` mais simples e legíveis.

## 18. `switch` produzindo um valor

O `switch` moderno também pode ser usado como uma expressão.

Isso significa que ele pode gerar um valor.

Exemplo:

```java
int dia = 2;

String nomeDia = switch (dia) {
    case 1 -> "Domingo";
    case 2 -> "Segunda-feira";
    case 3 -> "Terça-feira";
    default -> "Dia inválido";
};

System.out.println(nomeDia);
```

Resultado:

```text
Segunda-feira
```

Observe esta parte:

```java
String nomeDia = switch (dia) {
```

O resultado produzido pelo `switch` será armazenado em:

```java
nomeDia
```

## 19. Vários valores no mesmo `case`

Também podemos agrupar valores.

Exemplo:

```java
int dia = 6;

String tipoDia = switch (dia) {
    case 1, 7 -> "Fim de semana";
    case 2, 3, 4, 5, 6 -> "Dia útil";
    default -> "Dia inválido";
};

System.out.println(tipoDia);
```

Resultado:

```text
Dia útil
```

Aqui:

```java
case 1, 7
```

significa que tanto o valor `1` quanto o valor `7` produzirão:

```text
Fim de semana
```

## 20. Operador ternário

Java possui uma forma curta de representar uma decisão simples.

Chamamos de **operador ternário**.

A estrutura é:

```java
condicao ? valorSeVerdadeiro : valorSeFalso
```

Exemplo:

```java
int idade = 20;

String resultado = idade >= 18
        ? "Maior de idade"
        : "Menor de idade";

System.out.println(resultado);
```

Resultado:

```text
Maior de idade
```

Podemos ler:

```text
idade é maior ou igual a 18?

SIM -> "Maior de idade"
NÃO -> "Menor de idade"
```

## 21. Comparando ternário com `if`

Este código:

```java
String resultado;

if (idade >= 18) {
    resultado = "Maior de idade";
} else {
    resultado = "Menor de idade";
}
```

pode ser escrito como:

```java
String resultado = idade >= 18
        ? "Maior de idade"
        : "Menor de idade";
```

As duas versões possuem o mesmo objetivo.

## 22. Quando utilizar o operador ternário

O operador ternário funciona muito bem para decisões pequenas.

Por exemplo:

```java
String status = ativo ? "Ativo" : "Inativo";
```

Isso é simples de entender.

Porém, não devemos transformar toda condição em ternário.

Por exemplo:

```java
String resultado = nota >= 9
        ? "Excelente"
        : nota >= 7
        ? "Bom"
        : nota >= 5
        ? "Regular"
        : "Ruim";
```

Esse código funciona.

Mas fica mais difícil de ler.

Nesse caso, `if`, `else if` e `else` seriam provavelmente uma escolha melhor.

A clareza do código é mais importante do que escrever menos linhas.

## 23. Quando utilizar `if` ou `switch`

Uma regra prática é utilizar `if` quando trabalhamos com condições como:

```java
idade >= 18
```

```java
nota > 7
```

```java
saldo >= valorCompra
```

ou:

```java
idade >= 18 && possuiHabilitacao
```

Ou seja, situações envolvendo:

- comparações;
- intervalos;
- múltiplas condições;
- expressões booleanas.

Já o `switch` costuma ser interessante quando temos um valor e várias opções bem definidas.

Por exemplo:

```text
1 -> Janeiro
2 -> Fevereiro
3 -> Março
```

ou:

```text
ADMIN
USUARIO
VISITANTE
```

Não existe uma regra absoluta.

O objetivo é escolher a estrutura que torne a intenção do código mais clara.

## 24. Mini projeto — Sistema de aprovação

Vamos juntar vários conhecimentos.

```java
public class SistemaAprovacao {

    public static void main(String[] args) {

        double nota = 7.8;
        int faltas = 5;

        if (nota >= 7 && faltas <= 10) {
            System.out.println("Aluno aprovado.");
        } else if (nota >= 5 && faltas <= 10) {
            System.out.println("Aluno em recuperação.");
        } else {
            System.out.println("Aluno reprovado.");
        }
    }
}
```

Esse pequeno programa utiliza:

- `double`;
- `int`;
- operadores relacionais;
- operador lógico `&&`;
- `if`;
- `else if`;
- `else`.

Começamos, portanto, a combinar diferentes fundamentos da linguagem para representar uma regra de negócio.

## 25. Exercícios

Tente resolver os exercícios antes de consultar as soluções.

A prática é fundamental para realmente aprender programação.

### 1. Positivo, negativo ou zero

Crie uma variável:

```java
int numero = -5;
```

Seu programa deve informar se o número é:

```text
Positivo
```

```text
Negativo
```

ou:

```text
Zero
```

### 2. Par ou ímpar

Crie um programa que determine se um número inteiro é par ou ímpar.

Lembre-se do operador:

```java
%
```

### 3. Descobrindo o maior número

Considere:

```java
int a = 15;
int b = 30;
```

Seu programa deve informar:

- se `a` é maior;
- se `b` é maior;
- se os números são iguais.

### 4. Faixa etária

Crie um programa que classifique uma pessoa segundo estas regras:

```text
Menos de 12 anos -> Criança
12 até 17 anos -> Adolescente
18 até 59 anos -> Adulto
60 anos ou mais -> Idoso
```

### 5. Calculadora com `switch`

Considere:

```java
double numero1 = 10;
double numero2 = 5;
char operacao = '+';
```

Dependendo do valor de:

```java
operacao
```

realize:

```text
+
-
*
/
```

Use `switch`.

Não permita divisão por zero.

## 26. Soluções comentadas

Só consulte esta seção depois de tentar resolver os exercícios.

### Exercício 1 — Positivo, negativo ou zero

```java
int numero = -5;

if (numero > 0) {
    System.out.println("Positivo");
} else if (numero < 0) {
    System.out.println("Negativo");
} else {
    System.out.println("Zero");
}
```

Observe que o `else` representa o único caso restante.

Se o número:

- não é maior que zero;
- não é menor que zero;

então ele só pode ser zero.

### Exercício 2 — Par ou ímpar

```java
int numero = 17;

if (numero % 2 == 0) {
    System.out.println("Par");
} else {
    System.out.println("Ímpar");
}
```

O operador:

```java
%
```

retorna o resto da divisão.

Se:

```java
numero % 2
```

for:

```text
0
```

o número pode ser dividido por dois sem deixar resto.

Portanto, ele é par.

### Exercício 3 — Maior número

```java
int a = 15;
int b = 30;

if (a > b) {
    System.out.println("A é maior");
} else if (b > a) {
    System.out.println("B é maior");
} else {
    System.out.println("Os números são iguais");
}
```

Precisamos considerar três possibilidades:

```text
a > b
b > a
a == b
```

O último caso pode ser representado simplesmente pelo `else`.

### Exercício 4 — Faixa etária

```java
int idade = 35;

if (idade < 12) {
    System.out.println("Criança");
} else if (idade < 18) {
    System.out.println("Adolescente");
} else if (idade < 60) {
    System.out.println("Adulto");
} else {
    System.out.println("Idoso");
}
```

Observe que não precisamos escrever:

```java
idade >= 12 && idade < 18
```

Quando Java chega ao segundo teste, já sabemos que:

```java
idade < 12
```

era falso.

Portanto, a pessoa já possui pelo menos 12 anos.

Esse é mais um exemplo de como a ordem das condições influencia nossa lógica.

### Exercício 5 — Calculadora

Uma solução utilizando o `switch` tradicional:

```java
double numero1 = 10;
double numero2 = 5;
char operacao = '*';

switch (operacao) {

    case '+':
        System.out.println(numero1 + numero2);
        break;

    case '-':
        System.out.println(numero1 - numero2);
        break;

    case '*':
        System.out.println(numero1 * numero2);
        break;

    case '/':
        if (numero2 != 0) {
            System.out.println(numero1 / numero2);
        } else {
            System.out.println("Não é possível dividir por zero.");
        }
        break;

    default:
        System.out.println("Operação inválida.");
}
```

Observe que utilizamos um:

```java
if
```

dentro de:

```java
case '/'
```

Isso mostra que estruturas de controle podem ser combinadas.

## 27. Desafio — Sistema de descontos

Uma loja fornece descontos segundo estas regras:

```text
Compra abaixo de R$ 100 -> sem desconto
R$ 100 até R$ 499,99 -> 5%
R$ 500 até R$ 999,99 -> 10%
R$ 1.000 ou mais -> 15%
```

Crie uma variável:

```java
double valorCompra = 750;
```

O programa deverá mostrar:

- valor original;
- percentual de desconto;
- valor do desconto;
- valor final.

Tente resolver antes de continuar.

## 28. Solução do desafio

Uma possível solução é:

```java
double valorCompra = 750;
double percentualDesconto;

if (valorCompra >= 1000) {
    percentualDesconto = 0.15;
} else if (valorCompra >= 500) {
    percentualDesconto = 0.10;
} else if (valorCompra >= 100) {
    percentualDesconto = 0.05;
} else {
    percentualDesconto = 0;
}

double valorDesconto = valorCompra * percentualDesconto;
double valorFinal = valorCompra - valorDesconto;

System.out.println("Valor original: " + valorCompra);
System.out.println("Desconto: " + percentualDesconto * 100 + "%");
System.out.println("Valor do desconto: " + valorDesconto);
System.out.println("Valor final: " + valorFinal);
```

Se:

```java
valorCompra = 750;
```

a condição:

```java
valorCompra >= 1000
```

é falsa.

A próxima:

```java
valorCompra >= 500
```

é verdadeira.

Portanto:

```java
percentualDesconto = 0.10;
```

O desconto será de 10%.

## 29. Desafio extra — Sistema de acesso

Crie um programa com:

```java
int idade = 20;
boolean possuiIngresso = true;
boolean bloqueado = false;
```

Uma pessoa poderá entrar somente se:

- possuir pelo menos 18 anos;
- possuir ingresso;
- não estiver bloqueada.

O resultado deverá ser:

```text
Acesso permitido.
```

ou:

```text
Acesso negado.
```

Tente montar uma única expressão utilizando:

```java
&&
```

e:

```java
!
```

## 30. Desafio extra — Classificação de mês

Crie:

```java
int mes = 9;
```

Use `switch` para transformar o número no nome correspondente:

```text
1 -> Janeiro
2 -> Fevereiro
3 -> Março
...
12 -> Dezembro
```

Para qualquer outro número, mostre:

```text
Mês inválido
```

Depois tente escrever duas versões:

1. usando o `switch` tradicional;
2. usando o `switch` moderno com `->`.

Esse exercício ajuda a perceber as diferenças entre as duas sintaxes.

## Meta antes da Lição 4

Antes de avançar, você deve conseguir explicar com suas próprias palavras:

- para que serve o `if`;
- quando o bloco de um `if` é executado;
- para que serve o `else`;
- para que serve o `else if`;
- por que a ordem das condições importa;
- qual é a diferença entre `=` e `==`;
- como funciona o operador `&&`;
- como funciona o operador `||`;
- como funciona o operador `!`;
- o que é um `if` aninhado;
- quando um `switch` pode ser mais adequado que vários `if`;
- para que serve `case`;
- para que serve `break` no `switch` tradicional;
- o que é fall-through;
- para que serve `default`;
- como funciona o `switch` moderno com `->`;
- como um `switch` pode produzir um valor;
- como funciona o operador ternário `? :`.

Também deve conseguir entender este programa sem executá-lo:

```java
public class VerificadorAcesso {

    public static void main(String[] args) {

        int idade = 25;
        boolean possuiIngresso = true;
        boolean bloqueado = false;

        if (idade >= 18 && possuiIngresso && !bloqueado) {
            System.out.println("Acesso permitido.");
        } else {
            System.out.println("Acesso negado.");
        }
    }
}
```

E deve conseguir prever que, com esses valores, o resultado será:

```text
Acesso permitido.
```

Mais importante do que decorar a sintaxe é conseguir olhar para uma regra escrita em português e transformá-la em condições Java.

Quando essas estruturas estiverem naturais, estaremos preparados para a **Lição 4**, na qual aprenderemos **laços de repetição com `for`, `while`, `do-while`, além de `break` e `continue`**.
