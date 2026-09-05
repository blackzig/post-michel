---
title: "Python — Lição 2: Condições e Tomada de Decisões"
seo_title: "Python Lição 2: if, elif, else e operadores lógicos"
slug: "python-licao-2-condicoes"
meta_description: "Aprenda condições em Python com if, elif, else, operadores de comparação, and, or, not e exercícios práticos para tomada de decisões."
focus_keyphrase: "python lição 2"
related_keywords:
  - "if python"
  - "elif python"
  - "else python"
  - "operadores lógicos python"
  - "condições em python"
  - "python para iniciantes"
category: "Python"
tags:
  - "Python"
  - "Programação"
  - "Iniciantes"
  - "Condicionais"
  - "If"
  - "Operadores Lógicos"
  - "Lógica de Programação"
excerpt: "Segunda lição de Python: aprenda comparações, valores booleanos, if, elif, else, operadores lógicos e como fazer programas tomarem decisões."
---

# 🐍 Python — Lição 2: Condições e Tomada de Decisões

Nesta lição, você vai aprender a fazer o programa **tomar decisões**.

Até agora, nosso código executava uma sequência fixa. Agora vamos ensinar o Python a responder perguntas como:

- o usuário é maior de idade?
- a senha está correta?
- o número é positivo?
- a nota foi suficiente para aprovação?
- o número é par ou ímpar?

## 1. O que vamos dominar hoje

Nesta segunda lição, você aprenderá:

- operadores de comparação;
- valores booleanos;
- `if`;
- `elif`;
- `else`;
- operadores lógicos;
- condições compostas;
- condições aninhadas;
- validação de entradas;
- exercícios práticos.

## 2. Valores booleanos

Na lição anterior, vimos o tipo:

```python
bool
```

Ele possui apenas dois valores:

```python
True
False
```

Exemplo:

```python
idade = 20

print(idade >= 18)
```

Resultado:

```text
True
```

Porque `20` realmente é maior ou igual a `18`.

Agora:

```python
idade = 15

print(idade >= 18)
```

Resultado:

```text
False
```

Esses valores são a base das decisões em programação.

## 3. Operadores de comparação

Python possui operadores para comparar valores.

| Operador | Significado |
|---|---|
| `==` | igual |
| `!=` | diferente |
| `>` | maior que |
| `<` | menor que |
| `>=` | maior ou igual |
| `<=` | menor ou igual |

Exemplo:

```python
idade = 20

print(idade == 20)
print(idade != 20)
print(idade > 18)
print(idade < 18)
print(idade >= 20)
print(idade <= 20)
```

Resultado:

```text
True
False
True
False
True
True
```

## 4. Atenção: `=` não é `==`

Esse é um dos erros mais comuns de iniciantes.

Um único `=` significa:

```python
idade = 20
```

> atribua `20` à variável `idade`.

Já:

```python
idade == 20
```

significa:

> idade é igual a 20?

Portanto:

```text
=   atribuição
==  comparação
```

Essa diferença é fundamental.

## 5. Conhecendo o `if`

`if` significa aproximadamente:

> se

Exemplo:

```python
idade = 20

if idade >= 18:
    print("Você é maior de idade.")
```

Python verifica:

```python
idade >= 18
```

Se o resultado for:

```python
True
```

executa:

```python
print("Você é maior de idade.")
```

Se for `False`, essa linha é ignorada.

## 6. A indentação é obrigatória

Observe:

```python
if idade >= 18:
    print("Você é maior de idade.")
```

O espaço antes de:

```python
print()
```

é chamado de **indentação**.

Python usa a indentação para identificar quais linhas pertencem ao `if`.

Errado:

```python
if idade >= 18:
print("Você é maior de idade.")
```

Correto:

```python
if idade >= 18:
    print("Você é maior de idade.")
```

Normalmente usamos quatro espaços por nível de indentação.

## 7. `if` + `else`

Agora podemos dizer:

> se isso acontecer, faça uma coisa; caso contrário, faça outra.

```python
idade = int(input("Digite sua idade: "))

if idade >= 18:
    print("Você é maior de idade.")
else:
    print("Você é menor de idade.")
```

Se o usuário digitar `20`, o resultado será:

```text
Você é maior de idade.
```

Se digitar `15`:

```text
Você é menor de idade.
```

## 8. Fluxo mental do `if`

Leia este código:

```python
idade = 17

if idade >= 18:
    print("Adulto")
else:
    print("Menor")
```

O computador pensa aproximadamente assim:

```text
idade é maior ou igual a 18?
        |
       não
        |
      Menor
```

Se a idade fosse `25`:

```text
idade é maior ou igual a 18?
        |
       sim
        |
      Adulto
```

## 9. Conhecendo o `elif`

Às vezes existem mais de duas possibilidades.

```python
nota = 7

if nota >= 9:
    print("Excelente")
elif nota >= 7:
    print("Aprovado")
elif nota >= 5:
    print("Recuperação")
else:
    print("Reprovado")
```

Resultado:

```text
Aprovado
```

`elif` significa aproximadamente:

> caso contrário, se...

Python testa as condições de cima para baixo.

## 10. A ordem das condições importa

Observe:

```python
nota = 10

if nota >= 5:
    print("Aprovado")
elif nota >= 9:
    print("Excelente")
```

Resultado:

```text
Aprovado
```

Mesmo a nota sendo `10`.

Isso acontece porque Python encontrou primeiro `nota >= 5`, que é verdadeiro. Depois disso, ignora os demais `elif`.

Uma versão melhor:

```python
if nota >= 9:
    print("Excelente")
elif nota >= 7:
    print("Aprovado")
elif nota >= 5:
    print("Recuperação")
else:
    print("Reprovado")
```

> Coloque condições mais específicas antes das mais gerais.

## 11. Verificando se um número é par

Na lição anterior, aprendemos o operador `%`.

Agora podemos usá-lo com `if`:

```python
numero = int(input("Digite um número: "))

if numero % 2 == 0:
    print("O número é par.")
else:
    print("O número é ímpar.")
```

Se o usuário digitar `8`, o resto de `8 % 2` será `0`. Logo, o número é par.

## 12. Comparando textos

Também podemos comparar strings.

```python
senha = input("Digite a senha: ")

if senha == "python123":
    print("Acesso permitido.")
else:
    print("Senha incorreta.")
```

Python diferencia letras maiúsculas de minúsculas. Portanto:

```text
python123
```

é diferente de:

```text
Python123
```

## 13. Operador `and`

`and` significa:

> e

As duas condições precisam ser verdadeiras.

```python
idade = 25
tem_carteira = True

if idade >= 18 and tem_carteira:
    print("Pode dirigir.")
```

## 14. Operador `or`

`or` significa:

> ou

Basta uma condição ser verdadeira.

```python
dia = "sábado"

if dia == "sábado" or dia == "domingo":
    print("É fim de semana.")
```

## 15. Operador `not`

`not` significa:

> não

Ele inverte um valor booleano.

```python
logado = False

print(not logado)
```

Resultado:

```text
True
```

Também podemos escrever:

```python
logado = False

if not logado:
    print("Faça login.")
```

## 16. Tabela lógica do `and`

```text
True  and True  -> True
True  and False -> False
False and True  -> False
False and False -> False
```

Com `and`, tudo precisa ser verdadeiro.

## 17. Tabela lógica do `or`

```text
True  or True  -> True
True  or False -> True
False or True  -> True
False or False -> False
```

Com `or`, basta uma condição verdadeira.

## 18. Exemplo completo: entrada em um evento

```python
idade = int(input("Digite sua idade: "))
tem_ingresso = input("Possui ingresso? (sim/não): ")

if idade >= 18 and tem_ingresso == "sim":
    print("Entrada liberada.")
else:
    print("Entrada negada.")
```

Para entrar, as duas condições precisam ser atendidas:

- ter 18 anos ou mais;
- possuir ingresso.

## 19. Condições dentro de condições

Também podemos colocar um `if` dentro de outro.

Isso é chamado de **condição aninhada**.

```python
idade = int(input("Digite sua idade: "))

if idade >= 18:
    possui_carteira = input("Possui carteira? ")

    if possui_carteira == "sim":
        print("Você pode dirigir.")
    else:
        print("Você é maior de idade, mas não possui carteira.")
else:
    print("Você ainda é menor de idade.")
```

Isso funciona, mas nem sempre é a forma mais simples.

Muitas vezes podemos substituir condições aninhadas por condições compostas usando `and`.

## 20. Programa: classificador de número

```python
numero = float(input("Digite um número: "))

if numero > 0:
    print("Número positivo.")
elif numero < 0:
    print("Número negativo.")
else:
    print("O número é zero.")
```

Aqui temos três possibilidades:

- positivo;
- negativo;
- zero.

## 21. Programa: sistema de notas

```python
nota = float(input("Digite sua nota: "))

if nota >= 9:
    print("Excelente!")
elif nota >= 7:
    print("Aprovado.")
elif nota >= 5:
    print("Recuperação.")
else:
    print("Reprovado.")
```

Mas existe um problema: o usuário poderia digitar `200` ou `-50`.

Podemos melhorar:

```python
nota = float(input("Digite sua nota: "))

if nota < 0 or nota > 10:
    print("Nota inválida.")
elif nota >= 9:
    print("Excelente!")
elif nota >= 7:
    print("Aprovado.")
elif nota >= 5:
    print("Recuperação.")
else:
    print("Reprovado.")
```

Agora o programa trata entradas fora do intervalo permitido.

> Não pense apenas no caminho correto; pense também no que pode dar errado.

## 22. Programa: login simples

```python
usuario = input("Usuário: ")
senha = input("Senha: ")

if usuario == "admin" and senha == "1234":
    print("Login realizado com sucesso.")
else:
    print("Usuário ou senha incorretos.")
```

As duas condições precisam ser verdadeiras.

## 23. Um detalhe sobre booleanos

Você não precisa escrever:

```python
if ativo == True:
    print("Ativo")
```

Pode escrever:

```python
if ativo:
    print("Ativo")
```

Da mesma forma:

```python
if ativo == False:
```

pode ser:

```python
if not ativo:
```

Isso deixa o código mais limpo e natural em Python.

## 24. Exercícios

Resolva os exercícios antes de pedir a solução.

### 1. Maior de idade

Peça a idade do usuário.

Mostre:

```text
Maior de idade.
```

ou:

```text
Menor de idade.
```

### 2. Par ou ímpar

Peça um número inteiro e descubra se ele é par ou ímpar.

Use:

```python
%
```

### 3. Positivo, negativo ou zero

Peça um número e mostre se ele é:

- positivo;
- negativo;
- zero.

### 4. Maior número

Peça dois números e mostre qual deles é maior.

Exemplo:

```text
Primeiro número: 10
Segundo número: 25

25 é o maior número.
```

Pense também no caso em que os dois são iguais.

### 5. Aprovação

Peça uma nota de `0` a `10`.

Regras:

```text
nota >= 7      -> aprovado
nota >= 5      -> recuperação
nota < 5       -> reprovado
```

Também trate notas inválidas.

## 25. Desafio — Sistema de acesso

Crie um programa que peça:

- idade;
- se a pessoa possui ingresso.

A pessoa só pode entrar se tiver 18 anos ou mais **e** possuir ingresso.

Exemplo:

```text
Idade: 22
Possui ingresso? sim

Entrada liberada.
```

Outro exemplo:

```text
Idade: 16
Possui ingresso? sim

Entrada negada.
```

## 26. Desafio avançado — Calculadora inteligente

Peça:

- primeiro número;
- segundo número;
- operação.

O usuário poderá escolher:

```text
+
-
*
/
```

Exemplo:

```text
Primeiro número: 10
Segundo número: 2
Operação: *

Resultado: 20
```

Use `if`, `elif` e `else`.

Lembre-se de tratar um problema importante:

```python
10 / 0
```

Divisão por zero não é permitida.

Seu programa deve impedir isso.

## Meta antes da Lição 3

Antes de avançar, você deve entender muito bem:

```python
==
!=
>
<
>=
<=
```

e conseguir usar:

```python
if
elif
else
```

Além de:

```python
and
or
not
```

Você também deve conseguir explicar por que este código funciona:

```python
idade = int(input("Idade: "))
tem_ingresso = input("Tem ingresso? ")

if idade >= 18 and tem_ingresso == "sim":
    print("Entrada permitida.")
else:
    print("Entrada negada.")
```

E, mais importante, deve entender a ideia central desta lição:

> **Programas podem escolher caminhos diferentes dependendo dos dados e das condições.**

Quando esses fundamentos estiverem sólidos, a próxima etapa será a **Lição 3**, com **loops e repetição**, usando `while`, `for` e `range()` para fazer o computador repetir tarefas sem duplicar código.
