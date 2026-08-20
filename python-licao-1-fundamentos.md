---
title: "Python — Lição 1: Fundamentos da Programação"
seo_title: "Python Lição 1: variáveis, tipos, input, print e operadores"
slug: "python-licao-1-fundamentos"
meta_description: "Aprenda os fundamentos de Python: variáveis, tipos de dados, entrada e saída, conversão de tipos e operadores matemáticos."
focus_keyphrase: "python lição 1"
related_keywords:
  - "aprender python"
  - "python para iniciantes"
  - "variáveis em python"
  - "tipos de dados python"
  - "input python"
  - "print python"
  - "operadores python"
category: "Python"
tags:
  - "Python"
  - "Programação"
  - "Iniciantes"
  - "Variáveis"
  - "Tipos de Dados"
  - "Entrada e Saída"
  - "Operadores"
excerpt: "Primeira lição de Python: aprenda variáveis, tipos de dados, entrada e saída, conversão de valores e operadores básicos."
---

# 🐍 Python — Lição 1: Fundamentos da Programação

Esta é a primeira lição de uma sequência estruturada para aprender Python de forma progressiva, trabalhando **lógica, sintaxe, leitura de código, resolução de problemas e prática**.

O objetivo não é apenas decorar comandos, mas entender como um programa funciona e como transformar ideias em instruções que o computador consegue executar.

## 1. O que vamos dominar hoje

Nesta primeira lição, você aprenderá:

- o que é uma **variável**;
- os principais **tipos de dados**;
- como mostrar informações com `print()`;
- como receber informações com `input()`;
- como converter texto para números;
- como fazer operações matemáticas;
- como criar seu primeiro programa interativo.

## 2. O que é uma variável

Uma variável é um nome usado para armazenar um valor.

Exemplo:

```python
nome = "Carlos"
idade = 25
altura = 1.78
```

Aqui temos três variáveis:

- `nome`
- `idade`
- `altura`

Cada variável guarda um valor diferente.

Podemos imaginar uma variável como uma caixa com uma etiqueta:

```text
[ nome ]   -> "Carlos"
[ idade ]  -> 25
[ altura ] -> 1.78
```

Em Python, usamos o sinal:

```python
=
```

para fazer uma **atribuição**.

Por exemplo:

```python
idade = 25
```

Isso significa:

> Guarde o valor `25` dentro da variável `idade`.

É importante entender que, em programação, `=` normalmente representa atribuição, e não uma igualdade matemática.

## 3. Tipos de dados

Os valores armazenados em um programa podem ter tipos diferentes.

No começo, os quatro tipos mais importantes são:

```python
nome = "Ana"       # str
idade = 30         # int
altura = 1.65      # float
estudando = True   # bool
```

### `str` — texto

`str` vem de **string**.

Representa textos.

Exemplos:

```python
"Olá"
"Python"
"123"
```

Mesmo:

```python
"123"
```

é texto, porque está entre aspas.

### `int` — número inteiro

Representa números inteiros.

Exemplos:

```python
10
-5
200
0
```

### `float` — número decimal

Representa números com casas decimais.

Exemplos:

```python
3.14
1.75
-0.5
```

Em Python, usamos ponto para separar a parte decimal.

Correto:

```python
altura = 1.75
```

Não:

```python
altura = 1,75
```

### `bool` — verdadeiro ou falso

Representa valores lógicos.

Existem dois:

```python
True
False
```

Eles serão muito importantes quando começarmos a trabalhar com decisões e condições.

## 4. Descobrindo o tipo de um valor

Python possui a função:

```python
type()
```

Ela permite descobrir o tipo de um valor ou variável.

Exemplo:

```python
idade = 25

print(type(idade))
```

Resultado:

```text
<class 'int'>
```

Outro exemplo:

```python
nome = "Maria"

print(type(nome))
```

Resultado:

```text
<class 'str'>
```

## 5. Mostrando informações com `print()`

Para mostrar algo na tela, usamos:

```python
print()
```

Exemplo:

```python
print("Olá, mundo!")
```

Resultado:

```text
Olá, mundo!
```

Também podemos mostrar o conteúdo de uma variável:

```python
nome = "Lucas"

print(nome)
```

Resultado:

```text
Lucas
```

Podemos combinar texto e variáveis:

```python
nome = "Lucas"
idade = 20

print("Nome:", nome)
print("Idade:", idade)
```

Resultado:

```text
Nome: Lucas
Idade: 20
```

## 6. F-strings

Uma forma moderna e muito usada de combinar texto e variáveis é a **f-string**.

Exemplo:

```python
nome = "Lucas"
idade = 20

print(f"Meu nome é {nome} e tenho {idade} anos.")
```

Resultado:

```text
Meu nome é Lucas e tenho 20 anos.
```

Observe o `f` antes das aspas:

```python
f"..."
```

Dentro do texto, colocamos variáveis entre chaves:

```python
{nome}
{idade}
```

Python substitui essas expressões pelos respectivos valores.

## 7. Recebendo informações com `input()`

Para receber informações digitadas pelo usuário, usamos:

```python
input()
```

Exemplo:

```python
nome = input("Digite seu nome: ")

print(f"Olá, {nome}!")
```

Se o usuário digitar:

```text
Maria
```

o programa mostrará:

```text
Olá, Maria!
```

A função `input()` permite tornar o programa interativo.

## 8. Um detalhe fundamental sobre `input()`

O valor retornado por:

```python
input()
```

é sempre uma `str`.

Exemplo:

```python
idade = input("Digite sua idade: ")

print(type(idade))
```

Mesmo que o usuário digite:

```text
20
```

Python recebe:

```python
"20"
```

Ou seja, um texto.

Isso faz diferença quando queremos realizar cálculos.

## 9. Convertendo texto para número

Para transformar um texto em número inteiro, usamos:

```python
int()
```

Exemplo:

```python
idade = int(input("Digite sua idade: "))
```

Agora `idade` será um número inteiro.

Para números decimais, usamos:

```python
float()
```

Exemplo:

```python
altura = float(input("Digite sua altura: "))
```

Assim:

```text
"20" -> int() -> 20
```

e:

```text
"1.75" -> float() -> 1.75
```

Esse processo é chamado de **conversão de tipo**.

## 10. Operadores matemáticos

Python consegue realizar operações matemáticas diretamente.

Considere:

```python
a = 10
b = 3
```

### Adição

```python
print(a + b)
```

Resultado:

```text
13
```

### Subtração

```python
print(a - b)
```

Resultado:

```text
7
```

### Multiplicação

```python
print(a * b)
```

Resultado:

```text
30
```

### Divisão

```python
print(a / b)
```

Resultado aproximado:

```text
3.3333333333333335
```

### Potência

Usamos:

```python
**
```

Exemplo:

```python
print(a ** b)
```

Resultado:

```text
1000
```

Porque:

```text
10³ = 1000
```

### Divisão inteira

Usamos:

```python
//
```

Exemplo:

```python
print(a // b)
```

Resultado:

```text
3
```

A divisão inteira descarta a parte decimal do resultado.

### Resto da divisão

Usamos:

```python
%
```

Exemplo:

```python
print(a % b)
```

Resultado:

```text
1
```

Isso significa que, ao dividir `10` por `3`, sobra `1`.

## 11. O operador `%` e números pares

O operador `%` aparece bastante em programação.

Por exemplo:

```python
numero = 10

print(numero % 2)
```

Resultado:

```text
0
```

Isso acontece porque `10` pode ser dividido por `2` sem deixar resto.

Portanto:

```python
numero % 2
```

é uma forma importante de descobrir se um número é par ou ímpar.

Se o resto for:

```text
0
```

o número é par.

Se o resto for:

```text
1
```

o número é ímpar.

Mais adiante, usaremos essa ideia dentro de condições com `if`.

## 12. Seu primeiro programa completo

Agora podemos juntar tudo o que aprendemos.

```python
nome = input("Digite seu nome: ")
idade = int(input("Digite sua idade: "))

print(f"Olá, {nome}!")
print(f"Você tem {idade} anos.")
print(f"Daqui a 5 anos você terá {idade + 5} anos.")
```

Se o usuário digitar:

```text
João
20
```

o resultado será:

```text
Olá, João!
Você tem 20 anos.
Daqui a 5 anos você terá 25 anos.
```

Esse pequeno programa já utiliza:

- variáveis;
- `input()`;
- `int()`;
- `print()`;
- f-strings;
- operações matemáticas.

## 13. Entendendo o fluxo do programa

Observe:

```python
nome = input("Digite seu nome: ")
idade = int(input("Digite sua idade: "))

print(f"Olá, {nome}!")
print(f"Você tem {idade} anos.")
```

Python executa o código de cima para baixo.

Primeiro:

```python
nome = input(...)
```

Depois:

```python
idade = int(input(...))
```

Depois:

```python
print(...)
```

Isso parece simples, mas entender o **fluxo de execução** será essencial quando começarmos a criar programas maiores.

## 14. Erro comum: somar texto e número

Observe este código:

```python
idade = input("Digite sua idade: ")

print(idade + 5)
```

Ele gera erro.

O motivo é que `idade` é uma `str`, enquanto `5` é um `int`.

Python não pode somar diretamente:

```text
"20" + 5
```

Precisamos converter:

```python
idade = int(input("Digite sua idade: "))

print(idade + 5)
```

Agora funciona.

Essa é uma das primeiras diferenças importantes entre **texto** e **número**.

## 15. Exercícios

Resolva os exercícios antes de consultar exemplos ou pedir a resposta.

### 1. Apresentação pessoal

Crie um programa que peça:

- nome;
- idade;
- cidade.

Depois mostre uma frase semelhante a:

```text
Olá, meu nome é João, tenho 20 anos e moro em São Paulo.
```

### 2. Calculadora básica

Peça dois números ao usuário.

Depois mostre:

- soma;
- subtração;
- multiplicação;
- divisão.

Você pode começar com:

```python
numero1 = float(input("Digite o primeiro número: "))
numero2 = float(input("Digite o segundo número: "))
```

Complete o restante sozinho.

### 3. Descobrindo o dobro

Peça um número ao usuário e mostre seu dobro.

Exemplo:

```text
Digite um número: 8
O dobro de 8 é 16.
```

### 4. Descobrindo o resto

Peça dois números inteiros e mostre o resto da divisão do primeiro pelo segundo.

Use:

```python
%
```

### 5. Tipos de dados

Sem executar o código primeiro, tente descobrir o tipo de cada variável:

```python
nome = "Python"
idade = 30
preco = 19.90
ativo = True
codigo = "123"
```

Depois confira usando:

```python
type()
```

## 16. Desafio

Crie um programa que receba o **ano de nascimento** de uma pessoa e calcule aproximadamente sua idade.

Considere:

```python
ano_atual = 2026
```

Exemplo:

```text
Digite seu ano de nascimento: 2000
Você tem aproximadamente 26 anos.
```

Uma possível ideia:

```text
idade = ano atual - ano de nascimento
```

Tente transformar essa ideia em código sem consultar uma solução pronta.

## 17. Desafio extra

Crie um programa que peça:

- nome;
- ano de nascimento;
- altura.

Depois mostre algo semelhante a:

```text
Olá, Maria!
Você tem aproximadamente 26 anos.
Sua altura é 1.68 m.
```

Aqui você precisará combinar:

- `input()`;
- `int()`;
- `float()`;
- variáveis;
- operações matemáticas;
- f-strings.

## Meta antes da Lição 2

Antes de avançar, você deve conseguir explicar com suas próprias palavras:

- o que é uma variável;
- o que são `str`, `int`, `float` e `bool`;
- para que serve `print()`;
- para que serve `input()`;
- por que `input()` retorna texto;
- quando usar `int()` e `float()`;
- como usar `+`, `-`, `*`, `/`, `//`, `%` e `**`;
- como inserir variáveis dentro de uma f-string.

E, mais importante, deve conseguir entender este programa:

```python
nome = input("Nome: ")
idade = int(input("Idade: "))

print(f"Olá, {nome}.")
print(f"Daqui a 10 anos você terá {idade + 10} anos.")
```

Quando esses fundamentos estiverem sólidos, a próxima etapa será a **Lição 2**, com **comparações, valores booleanos, `if`, `elif`, `else` e tomada de decisões em Python**.
