---
title: "Git e GitHub — Lição 2: do Clone ao Pull Request"
seo_title: "Git e GitHub na Prática: Clone, Branch, Push e Pull Request"
slug: "git-github-licao-2-clone-pull-request"
meta_description: "Aprenda Git e GitHub na prática: clone, status, remote, branch, add, commit, push, Pull Request, Code Review e merge em um fluxo profissional."
focus_keyphrase: "Git e GitHub na prática"
related_keywords:
  - "git clone"
  - "git status"
  - "git branch"
  - "git switch"
  - "git add"
  - "git commit"
  - "git push"
  - "git pull"
  - "pull request GitHub"
  - "code review GitHub"
  - "merge Git"
  - "Git para iniciantes"
  - "GitHub para iniciantes"
category: "Git e GitHub"
tags:
  - "Git"
  - "GitHub"
  - "Controle de Versão"
  - "Programação"
  - "Iniciantes"
  - "Branch"
  - "Commit"
  - "Pull Request"
  - "Code Review"
  - "Merge"
excerpt: "Segunda lição de Git e GitHub: aprenda o fluxo real de desenvolvimento, do clone do repositório até o Pull Request, revisão de código e merge."
---

# 🔀 Git e GitHub — Lição 2: do Clone ao Pull Request

Na lição anterior, aprendemos os principais conceitos do Git:

- repositório;
- commit;
- staging area;
- HEAD;
- branch;
- merge;
- conflito de merge.

Agora vamos começar a trabalhar como um desenvolvedor realmente trabalha em um projeto hospedado no GitHub.

O objetivo desta aula é aprender este fluxo:

```text
GitHub
   ↓
git clone
   ↓
Criar branch
   ↓
Modificar arquivos
   ↓
git add
   ↓
git commit
   ↓
git push
   ↓
Pull Request
   ↓
Code Review
   ↓
Merge
```

Esse é um dos fluxos mais utilizados no desenvolvimento profissional.

## 1. O que vamos dominar hoje

Nesta lição, você aprenderá:

- a diferença entre repositório local e remoto;
- como clonar um projeto do GitHub;
- como verificar o estado do repositório;
- o que é o remote `origin`;
- como atualizar a branch principal;
- como criar branches de trabalho;
- como analisar alterações antes do commit;
- como usar o Staging Area;
- como criar commits claros;
- como enviar uma branch para o GitHub;
- como criar um Pull Request;
- como funciona um Code Review;
- como fazer o merge;
- como limpar branches depois da integração;
- como executar o fluxo completo de trabalho.

A meta é sair desta aula entendendo o caminho completo entre **seu computador, o Git e o GitHub**.

## 2. Repositório local e repositório remoto

Antes de continuar, precisamos entender uma diferença muito importante.

### Repositório local

É o repositório que está no seu computador.

Exemplo:

```text
C:\projetos\meu-sistema
```

É nele que você:

- modifica arquivos;
- cria commits;
- cria branches;
- visualiza o histórico;
- testa suas alterações.

### Repositório remoto

É uma cópia do repositório armazenada em algum servidor.

No nosso caso:

```text
GitHub
```

Um endereço poderia ser:

```text
https://github.com/usuario/meu-projeto
```

Portanto:

```text
SEU COMPUTADOR                      GITHUB

Repositório local  ←──────────→  Repositório remoto
```

Git e GitHub não são a mesma coisa.

**Git** controla as versões.

**GitHub** hospeda repositórios Git e oferece ferramentas para colaboração.

## 3. O que é `git clone`?

Imagine que encontramos um projeto no GitHub e queremos trabalhar nele.

Precisamos trazer esse projeto para nosso computador.

Usamos:

```bash
git clone URL_DO_REPOSITORIO
```

Exemplo:

```bash
git clone https://github.com/usuario/meu-projeto.git
```

O Git irá:

1. baixar os arquivos;
2. baixar o histórico de commits;
3. baixar as referências do repositório;
4. configurar automaticamente o repositório remoto.

Depois:

```bash
cd meu-projeto
```

Agora estamos dentro do projeto.

## 4. Verificando o estado do projeto

Um dos comandos que você mais utilizará no Git é:

```bash
git status
```

Ele responde perguntas como:

- Qual branch estou usando?
- Existem arquivos modificados?
- Existem arquivos novos?
- Existem alterações no Staging Area?
- Existem commits ainda não enviados?

Por exemplo:

```text
On branch main

Changes not staged for commit:
    modified: index.html
```

Isso significa:

> O arquivo `index.html` foi modificado, mas ainda não foi colocado no Stage.

Use `git status` constantemente.

Uma excelente rotina é executar:

```bash
git status
```

antes e depois de operações importantes.

## 5. Conhecendo o remote `origin`

Quando fazemos um `git clone`, o Git normalmente cria automaticamente um remote chamado:

```text
origin
```

Podemos verificar:

```bash
git remote
```

Resultado:

```text
origin
```

Para descobrir para onde ele aponta:

```bash
git remote -v
```

Resultado parecido com:

```text
origin  https://github.com/usuario/meu-projeto.git (fetch)
origin  https://github.com/usuario/meu-projeto.git (push)
```

Pense no `origin` como um apelido para o endereço do repositório remoto.

Em vez de escrever:

```text
https://github.com/usuario/meu-projeto.git
```

constantemente, usamos:

```text
origin
```

## 6. Antes de trabalhar, atualize seu projeto

Imagine que você trabalhou ontem.

Enquanto estava fora, outro desenvolvedor modificou o projeto.

Se começar a trabalhar usando sua versão antiga, poderá criar conflitos desnecessários.

Por isso, antes de começar, normalmente voltamos para a branch principal e a atualizamos:

```bash
git switch main
git pull origin main
```

Em instalações ou projetos que ainda usam o comando clássico, também podemos encontrar:

```bash
git checkout main
git pull origin main
```

O `git pull` busca alterações do repositório remoto e as integra à branch local conforme a configuração do Git.

Visualmente:

```text
GitHub
  ↓
Buscar alterações
  ↓
Integrar na branch local
```

## 7. Evite desenvolver tudo diretamente na `main`

Em projetos profissionais, normalmente evitamos desenvolver uma funcionalidade diretamente na branch principal.

Imagine que precisamos criar uma tela de login.

Primeiro atualizamos a `main`:

```bash
git switch main
git pull origin main
```

Depois criamos nossa branch:

```bash
git switch -c feature/login
```

Esse comando faz duas coisas:

```text
Cria a branch feature/login
+
Muda para ela
```

Equivale a:

```bash
git branch feature/login
git switch feature/login
```

Agora:

```bash
git branch
```

poderia mostrar:

```text
* feature/login
  main
```

O `*` indica a branch atual.

## 8. Convenções para nomes de branches

Use nomes que indiquem claramente o objetivo.

### Novas funcionalidades

```text
feature/login
feature/cadastro-cliente
feature/pagamento-pix
```

### Correções

```text
fix/login
fix/calculo-imposto
fix/erro-cadastro
```

### Documentação

```text
docs/readme
docs/instalacao
```

### Refatorações

```text
refactor/autenticacao
refactor/servico-email
```

Assim, somente vendo:

```text
feature/pagamento-pix
```

já sabemos o que está sendo desenvolvido.

Não existe uma única convenção obrigatória para todos os projetos. O mais importante é que a equipe escolha um padrão e o utilize de forma consistente.

## 9. Fazendo alterações

Agora podemos modificar o projeto normalmente.

Imagine que criamos:

```text
LoginController.java
```

E modificamos:

```text
UsuarioService.java
```

Verificamos:

```bash
git status
```

O Git poderá informar:

```text
Untracked files:
    LoginController.java

Changes not staged for commit:
    UsuarioService.java
```

Temos duas situações.

`LoginController.java` é novo.

`UsuarioService.java` já existia e foi alterado.

## 10. Analisando exatamente o que mudou

Antes de adicionar tudo ao Stage, podemos analisar as modificações.

Use:

```bash
git diff
```

O Git mostrará algo semelhante a:

```diff
- return false;
+ return usuario != null;
```

Linhas removidas aparecem com:

```text
-
```

Linhas adicionadas:

```text
+
```

Isso é extremamente importante.

Antes de criar um commit, acostume-se a perguntar:

> O que exatamente estou colocando neste commit?

## 11. Adicionando alterações ao Stage

Podemos adicionar um arquivo específico:

```bash
git add LoginController.java
```

Outro:

```bash
git add UsuarioService.java
```

Ou todas as alterações do diretório atual:

```bash
git add .
```

Depois:

```bash
git status
```

Agora aparecerão como:

```text
Changes to be committed
```

Significa:

> Essas alterações estão preparadas para entrar no próximo commit.

### Verificando o que já está no Stage

Existe uma diferença importante entre:

```bash
git diff
```

e:

```bash
git diff --staged
```

O primeiro mostra alterações ainda não adicionadas ao Stage.

O segundo mostra o que está preparado para o próximo commit.

Antes de commitar, vale a pena executar:

```bash
git diff --staged
```

## 12. Criando um bom commit

Agora fazemos:

```bash
git commit -m "Adiciona autenticação de usuários"
```

Evite mensagens como:

```text
alteração
teste
coisas
ajustes
```

Prefira:

```text
Adiciona autenticação de usuários
Corrige validação de e-mail
Remove código duplicado do serviço de login
```

Um bom histórico poderia parecer:

```text
a82fd91 Adiciona autenticação de usuários
8dc221a Corrige validação de senha
751c891 Adiciona testes do serviço de login
52a912c Cria estrutura inicial do projeto
```

Isso transforma o histórico em documentação.

## 13. Verificando o histórico

Para visualizar os commits:

```bash
git log
```

Você verá informações como:

```text
commit a82fd91...
Author: Michel
Date: ...

Adiciona autenticação de usuários
```

Uma forma mais compacta:

```bash
git log --oneline
```

Resultado:

```text
a82fd91 Adiciona autenticação de usuários
8dc221a Corrige validação de senha
751c891 Adiciona testes do serviço de login
```

Outra visualização muito útil:

```bash
git log --oneline --graph --all
```

Exemplo:

```text
* a82fd91 Adiciona autenticação
* 8dc221a Corrige senha
| * 77f3911 Adiciona relatório
|/
* 751c891 Estrutura inicial
```

Aqui começamos a enxergar visualmente as branches.

## 14. O commit ainda não está no GitHub

Esse é um erro comum entre iniciantes.

Quando fazemos:

```bash
git commit
```

o commit foi criado no repositório local.

Temos:

```text
Computador

A --- B --- C
          ↑
        novo commit
```

Mas o GitHub talvez ainda esteja:

```text
GitHub

A --- B
```

Precisamos enviar o commit.

## 15. O que é `git push`?

Usamos:

```bash
git push
```

Na primeira vez que enviamos uma nova branch:

```bash
git push -u origin feature/login
```

Isso significa aproximadamente:

```text
Envie minha branch feature/login
para o remote origin.
```

Depois disso, normalmente podemos simplesmente executar:

```bash
git push
```

Agora:

```text
COMPUTADOR                         GITHUB

feature/login                     feature/login

A---B---C        ───────→         A---B---C
```

Nossa branch existe também no GitHub.

## 16. O que significa `-u`?

No comando:

```bash
git push -u origin feature/login
```

o:

```text
-u
```

é uma forma curta de:

```text
--set-upstream
```

Ele configura uma relação entre a branch local e a branch remota.

Depois o Git passa a saber que:

```text
feature/login local
        ↓
origin/feature/login
```

estão relacionadas.

Então futuramente podemos usar apenas:

```bash
git push
```

e, quando apropriado:

```bash
git pull
```

## 17. Agora entra o Pull Request

Depois que terminamos nossa funcionalidade, podemos ter algo assim:

```text
main
 |
 A---B---C
      \
       D---E---F
             feature/login
```

Não queremos simplesmente integrar tudo à `main` sem revisão.

No GitHub criamos um:

# Pull Request

Também chamado de:

```text
PR
```

Um Pull Request significa aproximadamente:

> Eu fiz estas alterações nesta branch. Gostaria que elas fossem analisadas e integradas à branch de destino.

O Pull Request é uma funcionalidade do GitHub e de plataformas semelhantes. Ele não é um comando do Git.

## 18. Anatomia de um Pull Request

Normalmente um PR possui:

### Título

Exemplo:

```text
Adiciona autenticação de usuários
```

### Descrição

Explique:

- o que foi feito;
- por que foi feito;
- como testar;
- possíveis impactos.

Exemplo:

```text
Implementa autenticação de usuários utilizando
e-mail e senha.

Alterações:

- adiciona LoginController;
- adiciona validação de credenciais;
- adiciona testes automatizados;
- trata usuário inexistente.

Como testar:

1. Execute a aplicação.
2. Acesse /login.
3. Informe usuário e senha válidos.
```

Uma boa descrição reduz dúvidas durante a revisão.

## 19. Code Review

Uma das funcionalidades mais importantes do GitHub é a revisão de código.

Outro desenvolvedor pode analisar seu Pull Request.

Ele pode:

- fazer comentários;
- sugerir melhorias;
- identificar bugs;
- solicitar alterações;
- aprovar o código.

Por exemplo:

```java
if (usuario != null) {
```

O revisor pode comentar:

```text
Podemos mover esta validação para um método específico?
```

Você modifica o código.

Depois:

```bash
git add .
git commit -m "Refatora validação do usuário"
git push
```

O Pull Request é atualizado automaticamente porque os novos commits foram enviados para a mesma branch.

Você não precisa criar outro PR.

## 20. Merge do Pull Request

Depois que tudo estiver:

```text
✓ revisado
✓ testado
✓ aprovado
```

podemos fazer o merge.

O GitHub normalmente pode oferecer diferentes estratégias, dependendo da configuração do repositório.

### Create a merge commit

Preserva a existência da branch no histórico e cria um commit de merge.

Representação simplificada:

```text
A---B-------F
     \     /
      C---D
```

### Squash and merge

Junta vários commits da branch em um único commit antes da integração.

Antes:

```text
C Corrige botão
D Ajusta CSS
E Corrige teste
F Finaliza login
```

Depois:

```text
G Adiciona funcionalidade de login
```

Essa estratégia pode deixar o histórico da branch principal mais compacto.

### Rebase and merge

Reposiciona os commits da branch sobre a branch de destino, produzindo um histórico linear.

O `rebase` é poderoso e merece uma aula própria.

## 21. Depois do merge

Depois que `feature/login` foi integrada à `main`, voltamos para nossa máquina.

Primeiro:

```bash
git switch main
```

Depois:

```bash
git pull origin main
```

Agora nossa `main` local também possui a funcionalidade.

Podemos excluir a branch local:

```bash
git branch -d feature/login
```

E, quando a branch remota ainda existir e não for mais necessária:

```bash
git push origin --delete feature/login
```

Não é obrigatório manter branches de funcionalidades concluídas para sempre.

## 22. Fluxo profissional completo

Agora já podemos visualizar todo o processo.

### Atualize a branch principal

```bash
git switch main
git pull origin main
```

### Crie uma branch

```bash
git switch -c feature/login
```

### Desenvolva e confira

```bash
git status
git diff
```

### Prepare as alterações

```bash
git add .
```

### Revise o Stage

```bash
git diff --staged
```

### Crie o commit

```bash
git commit -m "Adiciona autenticação de usuários"
```

### Envie a branch

```bash
git push -u origin feature/login
```

### No GitHub

```text
Criar Pull Request
        ↓
Code Review
        ↓
Correções
        ↓
Aprovação
        ↓
Merge
```

### Depois do merge

```bash
git switch main
git pull origin main
git branch -d feature/login
```

Esse já é um fluxo muito próximo do utilizado em equipes profissionais.

## 23. Fluxo visual resumido

```text
               GITHUB
                  │
                  │ git clone
                  ↓
                main
                  │
                  │ git switch -c
                  ↓
           feature/login
                  │
                  │ editar código
                  ↓
             git status
                  │
                  ↓
              git diff
                  │
                  ↓
              git add .
                  │
                  ↓
        git diff --staged
                  │
                  ↓
             git commit
                  │
                  ↓
              git push
                  │
                  ↓
              GitHub
                  │
                  ↓
           Pull Request
                  │
                  ↓
            Code Review
                  │
                  ↓
               Merge
                  │
                  ↓
                main
```

## 24. Comandos que você deve dominar até aqui

| Comando | Função |
|---|---|
| `git clone` | baixa um repositório |
| `git status` | mostra o estado atual |
| `git diff` | mostra alterações ainda não preparadas |
| `git diff --staged` | mostra alterações preparadas para o commit |
| `git branch` | lista ou gerencia branches |
| `git switch` | troca de branch |
| `git switch -c` | cria uma branch e entra nela |
| `git add` | adiciona alterações ao Stage |
| `git commit` | cria um commit |
| `git log` | mostra o histórico |
| `git remote -v` | mostra os repositórios remotos |
| `git pull` | busca e integra alterações remotas |
| `git push` | envia commits e referências ao remoto |
| `git merge` | integra históricos de branches |

## 25. Exercício prático

Agora vamos praticar.

Crie um repositório no GitHub chamado:

```text
git-laboratorio
```

Clone:

```bash
git clone URL_DO_REPOSITORIO
```

Entre na pasta:

```bash
cd git-laboratorio
```

Crie:

```text
README.md
```

Adicione:

```markdown
# Meu laboratório Git

Projeto criado para estudar Git e GitHub.
```

Depois:

```bash
git add README.md
git commit -m "Adiciona README inicial"
git push
```

Agora crie uma branch:

```bash
git switch -c feature/sobre
```

Crie:

```text
sobre.md
```

Com:

```markdown
# Sobre

Este projeto é utilizado para praticar Git e GitHub.
```

Depois:

```bash
git add .
git commit -m "Adiciona página sobre"
git push -u origin feature/sobre
```

Entre no GitHub.

Crie um Pull Request:

```text
feature/sobre → main
```

Analise as alterações.

Faça o merge.

Depois volte ao terminal:

```bash
git switch main
git pull origin main
git branch -d feature/sobre
```

Se conseguiu fazer isso sozinho, você já domina o fluxo fundamental de Git e GitHub.

## 26. Desafio extra

Depois de concluir o exercício principal:

1. crie uma branch chamada `feature/contato`;
2. adicione um arquivo `contato.md`;
3. faça dois commits separados;
4. envie a branch ao GitHub;
5. abra um Pull Request;
6. altere novamente o arquivo local;
7. faça outro commit;
8. execute `git push`;
9. observe o mesmo Pull Request sendo atualizado;
10. faça o merge.

Esse exercício é importante para perceber que um Pull Request acompanha a evolução da branch.

## 27. Erros comuns de quem está começando

### Fazer tudo diretamente na `main`

Pode funcionar em projetos pessoais pequenos, mas dificulta revisão e colaboração.

### Criar um único commit gigantesco

Commits menores e coerentes são mais fáceis de revisar e entender.

### Usar `git add .` sem olhar o que mudou

Antes:

```bash
git status
git diff
```

Depois:

```bash
git add .
git diff --staged
```

### Achar que `commit` já enviou o código ao GitHub

Não enviou.

```text
git commit = grava no repositório local
git push   = envia ao remoto
```

### Confundir Git com GitHub

Git é o sistema de controle de versão.

GitHub é uma plataforma que hospeda e organiza repositórios Git.

## 28. Checklist antes de abrir um Pull Request

Antes de abrir o PR, pergunte:

- estou na branch correta?
- meu código compila?
- os testes passam?
- removi arquivos temporários?
- não coloquei senhas ou chaves no repositório?
- revisei `git status`?
- revisei meu diff?
- os commits possuem mensagens claras?
- a branch está atualizada o suficiente para ser integrada?

Esse hábito reduz problemas durante o Code Review.

## 29. Próximas etapas para masterizar Git e GitHub

Agora entramos em assuntos progressivamente mais poderosos.

As próximas lições serão:

1. **Git Fetch, Pull e Push profundamente**
2. **Git Restore, Reset e Revert — como desfazer alterações sem destruir seu projeto**
3. **Git Stash — guardar trabalho temporariamente**
4. **Merge avançado e resolução de conflitos reais**
5. **Git Rebase — reorganizando o histórico**
6. **Interactive Rebase — alterando, juntando e reorganizando commits**
7. **Cherry-pick — trazendo commits específicos**
8. **Tags e Releases no GitHub**
9. **GitHub Issues e organização de projetos**
10. **Pull Requests e Code Review profissional**
11. **GitHub Actions e CI/CD**
12. **Proteção da branch main**
13. **CODEOWNERS e revisão obrigatória**
14. **GitHub Projects**
15. **GitHub Releases e versionamento semântico**
16. **GitHub CLI**
17. **SSH e autenticação segura**
18. **Git Bisect — encontrando o commit que criou um bug**
19. **Reflog — recuperando commits que pareciam perdidos**
20. **Git Hooks e automação**
21. **Estratégias de branches: Git Flow, GitHub Flow e Trunk-Based Development**
22. **Como contribuir em projetos Open Source**
23. **Forks e Pull Requests em projetos de terceiros**
24. **GitHub Actions completo**
25. **Segurança: Secrets, Dependabot e Code Scanning**

Quando dominarmos essa trilha, você não estará apenas sabendo usar `git add`, `commit` e `push`.

Você terá domínio suficiente para utilizar **Git e GitHub como ferramentas profissionais de desenvolvimento, colaboração, automação, versionamento e entrega de software**.

## 30. Resumo da lição

Nesta aula, percorremos o fluxo completo:

```text
clone
  ↓
atualizar main
  ↓
criar branch
  ↓
editar
  ↓
status / diff
  ↓
add
  ↓
commit
  ↓
push
  ↓
Pull Request
  ↓
Code Review
  ↓
merge
  ↓
atualizar main local
```

A partir daqui, não vamos apenas decorar comandos.

Nas próximas lições, vamos entender **o que o Git realmente faz por trás de cada operação**, começando pela diferença entre **fetch, pull e push**.
