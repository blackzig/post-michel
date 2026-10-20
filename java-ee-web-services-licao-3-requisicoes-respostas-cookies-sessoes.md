---
title: "Java EE e Web Services — Lição 3: Requisições, Respostas, Cookies e Sessões HTTP"
seo_title: "Java EE Lição 3: HttpSession, Cookies e Cabeçalhos HTTP"
slug: "java-ee-web-services-licao-3-requisicoes-respostas-cookies-sessoes"
meta_description: "Aprenda cookies, sessões HTTP e HttpSession no Jakarta EE com GlassFish 7. Crie um perfil com login demonstrativo, logout, testes e simulado."
focus_keyphrase: "java ee cookies sessoes http"
related_keywords:
  - "Jakarta EE 10"
  - "Java EE Web Services"
  - "HttpSession"
  - "cookies Java servlet"
  - "HttpServletRequest"
  - "HttpServletResponse"
  - "GlassFish 7"
  - "JSESSIONID"
  - "cabeçalhos HTTP"
  - "certificação Java EE"
category: "Java"
tags:
  - "Java EE"
  - "Jakarta EE"
  - "Web Services"
  - "Servlets"
  - "HTTP"
  - "Cookies"
  - "HttpSession"
  - "GlassFish 7"
  - "Segurança Web"
  - "Certificação Java"
excerpt: "Terceira lição de Java EE e Web Services: entenda cabeçalhos HTTP, cookies, sessões, escopos, expiração e logout em um laboratório Jakarta EE 10."
---

# ☕ Java EE e Web Services — Lição 3: Requisições, Respostas, Cookies e Sessões HTTP

Na **Lição 2**, desenvolvemos o `SaudacaoServlet` e aprendemos a tratar requisições `GET` e `POST`. Além disso, utilizamos `HttpServletRequest`, `HttpServletResponse`, validações e códigos de status HTTP.

Agora avançaremos para uma pergunta importante: **como um site reconhece que duas requisições pertencem ao mesmo visitante?**

Por exemplo, imagine que um usuário informe seu apelido em um formulário. Em seguida, ele acessa outra página e espera continuar recebendo uma saudação personalizada. Como podemos conservar essa informação?

Nesta terceira lição, vamos construir um laboratório com **cabeçalhos, cookies, sessões HTTP e logout**. Ao mesmo tempo, entenderemos conceitos frequentemente cobrados em exames relacionados a Java EE e Jakarta Servlet.

> **Ambiente da trilha:** Eclipse IDE, projeto `OlaJavaEE`, Java 17, GlassFish 7, Jakarta EE 10 e **Jakarta Servlet 6.0**. Usaremos `jakarta.servlet.*`. Se você escolher uma certificação histórica de Java EE 8, precisaremos adaptar os nomes dos pacotes e conferir o programa oficial do exame.

## 1. O que vamos dominar hoje

Ao concluir a aula, você deverá conseguir:

- explicar por que HTTP é considerado um protocolo *stateless*;
- ler cabeçalhos de requisições com `getHeader()`;
- definir cabeçalhos de respostas com `setHeader()`;
- diferenciar cookies de sessões;
- criar, ler e expirar cookies;
- explicar como o contêiner usa o cookie `JSESSIONID`;
- comparar `getSession()` e `getSession(false)`;
- guardar informações com `session.setAttribute()`;
- controlar a expiração com `setMaxInactiveInterval()`;
- encerrar uma sessão com `invalidate()`;
- diferenciar os escopos request, session e application;
- compreender `forward`, `redirect` e o padrão POST/Redirect/GET;
- testar cookies e sessões usando navegador e `curl`;
- reconhecer riscos relacionados a cookies, sessões e autenticação;
- responder a dez questões de revisão.

## 2. Por que o protocolo HTTP não lembra do visitante?

O HTTP é, por natureza, **sem estado entre requisições**: uma requisição isolada não carrega automaticamente todo o histórico das anteriores.

Imagine:

~~~text
1. Navegador → GET /perfil
2. Servidor  → Responde: "Informe seu apelido"

3. Navegador → POST /perfil (apelido=Ana)
4. Servidor  → Salva uma informação na sessão

5. Navegador → GET /perfil
6. Servidor  → Responde: "Olá, Ana!"
~~~

Na quinta etapa, como o servidor sabe que a requisição pertence à mesma pessoa que informou o apelido na terceira?

Para resolver esse problema, as aplicações podem empregar mecanismos de **gerenciamento de sessão**. Um dos mais utilizados combina um identificador armazenado em cookie no navegador com informações da sessão mantidas pelo servidor.

Em outras palavras, HTTP continua sem estado por si só; **a aplicação e o contêiner acrescentam um mecanismo para relacionar requisições**.

## 3. Requisições HTTP também possuem cabeçalhos

Um cliente pode enviar uma requisição parecida com:

~~~http
GET /OlaJavaEE/perfil HTTP/1.1
Host: localhost:8080
Accept: text/plain
User-Agent: NavegadorExemplo
Cookie: tema=escuro; JSESSIONID=identificador-ilustrativo
~~~

Observe que `Cookie` é um **cabeçalho de requisição**. O cliente envia nele cookies pertinentes àquele endereço, respeitando regras de domínio, caminho, validade e segurança.

Já `User-Agent` costuma indicar informações sobre o cliente. Entretanto, esses valores são controlados pelo cliente e não devem ser considerados prova de identidade.

No Servlet, podemos consultar um cabeçalho assim:

~~~java
String agente = request.getHeader("User-Agent");
String aceitacao = request.getHeader("Accept");
~~~

Tenha cuidado: o retorno pode ser `null` quando o cabeçalho está ausente.

### Como enviar um cabeçalho na resposta?

Também podemos definir cabeçalhos antes que a resposta seja comprometida:

~~~java
response.setHeader("Cache-Control", "no-store");
response.setContentType("text/plain;charset=UTF-8");
~~~

Nesse caso, `Cache-Control: no-store` instrui caches a não armazenar a resposta. Essa configuração é especialmente útil para dados personalizados que não devem permanecer em cache.

## 4. O que é um cookie?

Um **cookie** é um pequeno conjunto de dados identificado por nome e valor. O servidor pode solicitar seu armazenamento por meio do cabeçalho de resposta `Set-Cookie`.

Por exemplo:

~~~http
HTTP/1.1 200 OK
Set-Cookie: tema=escuro; Path=/OlaJavaEE; Max-Age=604800; HttpOnly; SameSite=Lax
~~~

Em requisições posteriores que satisfaçam as regras do cookie, o navegador poderá enviar:

~~~http
Cookie: tema=escuro
~~~

Assim, conseguimos armazenar uma preferência como o tema escolhido pelo usuário.

Porém, **cookies não devem conter senhas, segredos nem informações que você suponha estar protegidas apenas por estarem no navegador**. O cliente pode visualizar ou modificar muitos valores de cookies; mesmo um cookie com `HttpOnly` não se transforma em armazenamento secreto confiável.

### Criando um cookie

~~~java
Cookie cookie = new Cookie("tema", "escuro");
cookie.setPath("/OlaJavaEE");
cookie.setMaxAge(7 * 24 * 60 * 60);
cookie.setHttpOnly(true);
cookie.setAttribute("SameSite", "Lax");

// Em produção com HTTPS, ative Secure.
cookie.setSecure(request.isSecure());

response.addCookie(cookie);
~~~

Nesse exemplo, `Max-Age` representa **segundos**, e `7 * 24 * 60 * 60` corresponde a sete dias.

Usamos `request.isSecure()` somente para permitir que o exemplo funcione também no laboratório local em HTTP. Em produção, **exija HTTPS e configure cookies sensíveis com `Secure`**, levando em conta a arquitetura do proxy e do contêiner.

### Lendo cookies

~~~java
Cookie[] cookies = request.getCookies();

if (cookies != null) {
    for (Cookie cookie : cookies) {
        if ("tema".equals(cookie.getName())) {
            String tema = cookie.getValue();
            System.out.println("Tema recebido: " + tema);
        }
    }
}
~~~

Lembre-se: `request.getCookies()` pode retornar `null`. Portanto, verifique o valor antes de percorrer o array.

### Removendo um cookie

Para solicitar sua exclusão ao navegador, envie um cookie com o mesmo nome, domínio e caminho apropriados, utilizando validade zero:

~~~java
Cookie remover = new Cookie("tema", "");
remover.setPath("/OlaJavaEE");
remover.setMaxAge(0);
response.addCookie(remover);
~~~

A configuração do caminho deve corresponder àquela utilizada na criação; caso contrário, você poderá criar um segundo cookie em vez de remover o primeiro.

## 5. O que é uma sessão HTTP?

Uma **sessão** representa o relacionamento lógico entre uma sequência de requisições de um mesmo cliente e uma aplicação.

Em Jakarta Servlet, utilizamos a interface:

~~~java
jakarta.servlet.http.HttpSession
~~~

O contêiner pode manter informações associadas à sessão, como:

~~~text
Sessão do visitante:
  apelido: Ana
  tema de preferência: escuro
  última atividade: gerenciada pelo contêiner
~~~

Ao contrário do cookie de preferência, **os atributos de `HttpSession` normalmente permanecem no lado do servidor**, conforme a estratégia do contêiner.

O navegador costuma receber apenas um identificador que permite ao servidor localizar a sessão correspondente.

## 6. Como funciona o JSESSIONID?

O nome `JSESSIONID` é comumente utilizado como cookie de acompanhamento da sessão em servidores Jakarta Servlet.

Um fluxo simplificado é:

~~~text
NAVEGADOR                            GLASSFISH

POST /perfil ---------------------> cria uma sessão
                                  gera identificador
         <------------------------ Set-Cookie: JSESSIONID=...

GET /perfil
Cookie: JSESSIONID=... ----------> localiza a sessão
         <------------------------ dados associados à sessão
~~~

**Muito importante:** o `JSESSIONID` não guarda necessariamente o apelido, a senha ou todos os dados do usuário. Ele serve para identificar uma sessão que o contêiner administra.

Além disso, o nome e os mecanismos de rastreamento podem receber configurações específicas. Nunca crie um cookie `JSESSIONID` manualmente para substituir o gerenciamento de sessão do GlassFish.

## 7. getSession() e getSession(false)

Esta diferença merece atenção especial:

~~~java
HttpSession sessao = request.getSession();
~~~

O método `getSession()` retorna uma sessão existente, se houver uma válida, ou **cria outra**.

Já:

~~~java
HttpSession sessao = request.getSession(false);
~~~

retorna uma sessão existente **sem criar uma nova**. Quando não existe sessão válida, devolve `null`.

Portanto, antes de apenas consultar informações de um visitante, normalmente faz sentido utilizar `getSession(false)`.

Quando precisamos iniciar explicitamente uma sessão, podemos chamar:

~~~java
HttpSession sessao = request.getSession(true);
~~~

O argumento `true` autoriza a criação de uma nova sessão caso seja necessário.

| Chamada | Pode criar sessão? | Pode retornar null? |
|---|---|---|
| `getSession()` | Sim | Não, em operação normal |
| `getSession(true)` | Sim | Não, em operação normal |
| `getSession(false)` | Não | Sim |

## 8. Guardando e lendo informações na sessão

Após obter uma sessão, podemos associar dados a nomes:

~~~java
HttpSession sessao = request.getSession(true);
sessao.setAttribute("apelido", "Ana");
~~~

Depois, em outra requisição associada à mesma sessão:

~~~java
HttpSession sessao = request.getSession(false);

if (sessao != null) {
    String apelido = (String) sessao.getAttribute("apelido");

    if (apelido != null) {
        System.out.println("Olá, " + apelido + "!");
    }
}
~~~

Observe dois detalhes:

1. `getAttribute()` retorna um `Object`; por isso, realizamos uma conversão de tipo.
2. Mesmo com uma sessão válida, o atributo pode ser `null` caso ainda não tenha sido configurado ou tenha sido removido.

## 9. Expiração e encerramento da sessão

Uma sessão não precisa durar indefinidamente.

Podemos definir um período máximo de inatividade:

~~~java
sessao.setMaxInactiveInterval(10 * 60);
~~~

Essa configuração estabelece **600 segundos**, ou dez minutos, entre acessos relevantes à sessão.

Atenção: `setMaxInactiveInterval()` trabalha em **segundos**, enquanto certas configurações de timeout no descritor ou em `ServletContext` utilizam **minutos**.

Para remover apenas um atributo:

~~~java
sessao.removeAttribute("apelido");
~~~

Para invalidar toda a sessão:

~~~java
sessao.invalidate();
~~~

Depois de `invalidate()`, aquela sessão deixa de ser válida. Não continue utilizando seus atributos como se ainda estivesse ativa.

**Logout real** exige também outras medidas de autenticação e segurança, conforme o projeto. Nosso laboratório terá apenas um "entrar" demonstrativo com apelido; **não implementa autenticação de usuário nem proteção de acesso**.

## 10. Laboratório: um perfil com sessão e preferência de tema

Vamos ampliar nosso projeto `OlaJavaEE` criando:

- uma página HTML para informar apelido e tema;
- um servlet `PerfilServlet`;
- uma sessão que guarda o apelido;
- um cookie que guarda a preferência de tema;
- uma ação demonstrativa para encerrar a sessão.

O resultado permitirá observar, na prática, que **cookies e sessões têm responsabilidades diferentes**.

### Estrutura recomendada

~~~text
OlaJavaEE/
└── src/
    └── main/
        ├── java/
        │   └── br/com/exemplo/javaee/web/
        │       └── PerfilServlet.java
        └── webapp/
            ├── perfil.html
            └── WEB-INF/
                └── web.xml
~~~

Não precisamos remover o `OlaServlet` nem o `SaudacaoServlet` das aulas anteriores.

## 11. Código completo do PerfilServlet

Crie a classe abaixo no pacote `br.com.exemplo.javaee.web`.

~~~java
package br.com.exemplo.javaee.web;

import java.io.IOException;
import java.io.PrintWriter;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.Cookie;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

@WebServlet("/perfil")
public class PerfilServlet extends HttpServlet {

    private static final long serialVersionUID = 1L;

    @Override
    protected void doGet(
            HttpServletRequest request,
            HttpServletResponse response)
            throws ServletException, IOException {

        response.setHeader("Cache-Control", "no-store");
        response.setContentType("text/plain;charset=UTF-8");

        HttpSession sessao = request.getSession(false);
        String apelido = null;

        if (sessao != null) {
            apelido = (String) sessao.getAttribute("apelido");
        }

        String tema = "claro";
        Cookie[] cookies = request.getCookies();

        if (cookies != null) {
            for (Cookie cookie : cookies) {
                if ("tema".equals(cookie.getName())
                        && "escuro".equals(cookie.getValue())) {
                    tema = "escuro";
                }
            }
        }

        PrintWriter saida = response.getWriter();

        if (apelido == null) {
            saida.println("Nenhum apelido registrado na sessão.");
            saida.println("Abra perfil.html para começar.");
        } else {
            saida.println("Olá, " + apelido + "!");
        }

        saida.println("Tema salvo no cookie: " + tema);
    }

    @Override
    protected void doPost(
            HttpServletRequest request,
            HttpServletResponse response)
            throws ServletException, IOException {

        request.setCharacterEncoding("UTF-8");
        String acao = request.getParameter("acao");

        if ("sair".equals(acao)) {
            HttpSession sessao = request.getSession(false);

            if (sessao != null) {
                sessao.invalidate();
            }

            response.sendRedirect(
                    request.getContextPath() + "/perfil");
            return;
        }

        if (!"entrar".equals(acao)) {
            response.sendError(
                    HttpServletResponse.SC_BAD_REQUEST,
                    "Ação inválida.");
            return;
        }

        String apelido = request.getParameter("apelido");
        String tema = request.getParameter("tema");

        if (apelido == null || apelido.isBlank()
                || apelido.strip().length() > 60) {
            response.sendError(
                    HttpServletResponse.SC_BAD_REQUEST,
                    "Informe um apelido com até 60 caracteres.");
            return;
        }

        if (!"claro".equals(tema) && !"escuro".equals(tema)) {
            response.sendError(
                    HttpServletResponse.SC_BAD_REQUEST,
                    "Tema inválido.");
            return;
        }

        HttpSession sessao = request.getSession(true);
        sessao.setAttribute("apelido", apelido.strip());
        sessao.setMaxInactiveInterval(10 * 60);

        Cookie preferencia = new Cookie("tema", tema);
        String contexto = request.getContextPath();

        preferencia.setPath(
                contexto.isEmpty() ? "/" : contexto);
        preferencia.setMaxAge(7 * 24 * 60 * 60);
        preferencia.setHttpOnly(true);
        preferencia.setSecure(request.isSecure());
        preferencia.setAttribute("SameSite", "Lax");

        response.addCookie(preferencia);

        response.sendRedirect(contexto + "/perfil");
    }
}
~~~

### Por que o exemplo funciona?

**No GET**, chamamos `getSession(false)` para consultar uma sessão sem criar outra desnecessariamente. Em seguida, lemos o atributo `apelido` e procuramos o cookie `tema`.

**No POST**, analisamos a ação solicitada. Se for `entrar`, validamos o apelido e o tema, criamos ou recuperamos a sessão e gravamos os dados.

Depois disso, usamos `response.addCookie()` para solicitar ao navegador que armazene a preferência.

Finalmente, realizamos um redirecionamento para a URL `/perfil`. Assim, o navegador apresenta o resultado por GET, evitando que uma atualização simples da página precise reenviar o formulário.

Por outro lado, quando a ação é `sair`, invalidamos a sessão existente e redirecionamos para o perfil. **O cookie de preferência continua existindo**, pois seu propósito é diferente do encerramento da sessão.

> **Nota de segurança:** o nome `entrar` é apenas uma ação do laboratório. Informar qualquer apelido **não é fazer login autenticado**. Além disso, o formulário não possui token CSRF; portanto, esse exemplo não deve ser utilizado como fluxo de autenticação ou de alteração sensível em produção.

## 12. Criando o formulário HTML

Crie o arquivo `src/main/webapp/perfil.html`:

~~~html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Laboratório de cookies e sessões</title>
</head>
<body>
    <h1>Perfil do visitante</h1>

    <form action="perfil" method="post" accept-charset="UTF-8">
        <input type="hidden" name="acao" value="entrar">

        <label for="apelido">Apelido:</label>
        <input id="apelido" name="apelido"
               type="text" maxlength="60" required>

        <label for="tema">Tema:</label>
        <select id="tema" name="tema">
            <option value="claro">Claro</option>
            <option value="escuro">Escuro</option>
        </select>

        <button type="submit">Salvar perfil</button>
    </form>

    <hr>

    <form action="perfil" method="post">
        <input type="hidden" name="acao" value="sair">
        <button type="submit">Encerrar sessão</button>
    </form>

    <p><a href="perfil">Ver perfil atual</a></p>
</body>
</html>
~~~

Repare no campo oculto `acao`. Nosso servlet utiliza esse parâmetro para identificar qual das duas operações foi solicitada.

Além disso, o campo `tema` permite escolher entre dois valores conhecidos. Mesmo assim, repetimos a validação no Java, porque **não podemos confiar apenas nas restrições do HTML**.

## 13. Publicando no GlassFish 7

No Eclipse:

1. Confirme que o projeto continua configurado para o **GlassFish 7**.
2. Verifique se `PerfilServlet.java` está dentro do pacote correto.
3. Confira se `perfil.html` está em `src/main/webapp`.
4. Publique novamente o projeto usando as opções de atualização do servidor.
5. Abra o endereço:

~~~text
http://localhost:8080/OlaJavaEE/perfil.html
~~~

Se o projeto não tiver esse caminho de contexto, substitua `OlaJavaEE` pelo contexto definido no GlassFish.

### Atenção ao web.xml herdado da primeira lição

Na primeira prática, seu `web.xml` continha um descritor antigo com `version="2.4"`.

Para uma aplicação Jakarta EE 10, prefira um descritor Servlet 6.0, como estudamos na Lição 2, **preservando os mapeamentos XML antigos que ainda utiliza**.

A anotação `@WebServlet("/perfil")` deve ser processada quando a descoberta de anotações de implantação estiver habilitada. Evite `metadata-complete="true"` se depender exclusivamente dessa anotação.

Se o navegador retornar HTTP 404, verifique a URL, o contexto publicado, o mapeamento do servlet, o `web.xml`, a publicação das classes e o console do GlassFish antes de modificar o código indiscriminadamente.

## 14. Teste 1: acessando o perfil sem sessão

Primeiramente, abra uma janela privativa nova ou limpe os dados do site de teste.

Depois, acesse:

~~~text
http://localhost:8080/OlaJavaEE/perfil
~~~

Resultado esperado:

~~~text
Nenhum apelido registrado na sessão.
Abra perfil.html para começar.
Tema salvo no cookie: claro
~~~

Nesse momento, o servlet não criou uma sessão apenas para consultar o perfil.

Isso acontece porque utilizamos:

~~~java
request.getSession(false);
~~~

Por isso, essa chamada funciona bem em operações que somente consultam uma sessão já existente.

## 15. Teste 2: criando uma sessão e um cookie

Agora abra:

~~~text
http://localhost:8080/OlaJavaEE/perfil.html
~~~

Preencha:

~~~text
Apelido: Ana
Tema: Escuro
~~~

Clique em **Salvar perfil**.

O navegador deverá seguir o redirecionamento para `/perfil` e exibir:

~~~text
Olá, Ana!
Tema salvo no cookie: escuro
~~~

Portanto, conseguimos manter o apelido na sessão e a preferência no cookie.

Atualize a página. Se o navegador continuar enviando o identificador da sessão, o servidor poderá recuperar novamente o apelido.

## 16. Teste 3: encerrando a sessão

Volte para:

~~~text
http://localhost:8080/OlaJavaEE/perfil.html
~~~

Depois, clique em **Encerrar sessão**.

O servlet executará:

~~~java
sessao.invalidate();
~~~

Ao retornar à página do perfil, o resultado esperado será:

~~~text
Nenhum apelido registrado na sessão.
Abra perfil.html para começar.
Tema salvo no cookie: escuro
~~~

Esse resultado revela uma diferença essencial: **invalidar a sessão não implica apagar automaticamente todos os cookies de preferência da aplicação**.

Se você quiser apagar a preferência, deverá implementar também a remoção do cookie correspondente.

## 17. Teste 4: inspecionando cookies no navegador

Abra as ferramentas de desenvolvedor do navegador, normalmente com `F12`.

Em navegadores baseados em Chromium:

1. Abra a guia **Application** ou **Aplicativo**.
2. Procure **Storage → Cookies**.
3. Selecione o endereço do seu laboratório.
4. Observe os cookies presentes e seus atributos.

Você poderá encontrar um cookie de sessão, frequentemente chamado `JSESSIONID`, e outro chamado `tema`.

Observe os campos de nome, valor, caminho, validade, `HttpOnly`, `Secure` e `SameSite`.

**Não publique identificadores reais de sessão em imagens, logs ou repositórios.** Eles podem permitir acesso não autorizado a sessões ativas se forem expostos.

## 18. Teste 5: usando curl e cookies persistidos

O navegador administra cookies automaticamente. Entretanto, podemos controlar esse comportamento em testes com `curl`.

No Windows, utilize `curl.exe` no PowerShell.

### Criando a sessão

~~~powershell
curl.exe -i -c cookies.txt -d "acao=entrar&apelido=Ana&tema=escuro" "http://localhost:8080/OlaJavaEE/perfil"
~~~

A opção `-c cookies.txt` solicita que o `curl` guarde os cookies recebidos em um arquivo.

### Reutilizando a sessão

~~~powershell
curl.exe -i -b cookies.txt "http://localhost:8080/OlaJavaEE/perfil"
~~~

Dessa vez, `-b cookies.txt` instrui o programa a enviar os cookies persistidos.

### Sem reutilizar os cookies

~~~powershell
curl.exe -i "http://localhost:8080/OlaJavaEE/perfil"
~~~

Compare as respostas. Sem o identificador de sessão anterior, o contêiner normalmente não encontrará o apelido registrado.

### Encerrando a sessão

~~~powershell
curl.exe -i -b cookies.txt -d "acao=sair" "http://localhost:8080/OlaJavaEE/perfil"
~~~

Depois, execute novamente o GET utilizando o arquivo de cookies. A sessão antiga já não deverá conter o apelido.

> **Importante:** `cookies.txt` é apenas um arquivo local do experimento. Como pode conter identificadores de sessão, não o envie para o GitHub; apague-o quando terminar os testes.

## 19. Qual a diferença entre request, session e application?

Em Java Enterprise, diferentes escopos controlam por quanto tempo e para quem uma informação fica acessível.

| Escopo | API mais comum | Duração típica | Exemplo |
|---|---|---|---|
| Request | `ServletRequest` | Uma requisição e seus despachos associados | Resultado temporário de validação |
| Session | `HttpSession` | Conjunto de requisições associadas à mesma sessão | Apelido do visitante |
| Application | `ServletContext` | Vida da aplicação no contêiner | Configuração compartilhada |

### Request

~~~java
request.setAttribute("mensagem", "Processamento concluído.");
String mensagem = (String) request.getAttribute("mensagem");
~~~

Um atributo de request serve para compartilhar dados entre componentes envolvidos no processamento de uma mesma requisição, inclusive durante um `forward`.

### Session

~~~java
request.getSession(true).setAttribute("apelido", "Ana");
~~~

O atributo estará associado à sessão identificada pelo contêiner.

### Application

~~~java
getServletContext().setAttribute("modo", "laboratorio");
~~~

Nesse caso, o atributo pertence ao contexto da aplicação e pode ser acessado por componentes desse contexto.

**Cuidado com concorrência:** os objetos armazenados em sessões e no contexto da aplicação podem ser acessados por diferentes threads. A aplicação precisa proteger alterações concorrentes quando necessário.

> **Ponto de prova:** não confunda `request.getAttribute()` com `request.getParameter()`. O primeiro recupera um atributo colocado no escopo request. O segundo recupera parâmetros enviados pelo cliente.

## 20. Forward, redirect e POST/Redirect/GET

Agora vamos diferenciar duas formas de encaminhar o processamento.

### Forward

~~~java
request.getRequestDispatcher("/resultado")
       .forward(request, response);
~~~

O `forward` transfere o processamento para outro recurso no lado do servidor. Em geral, **o navegador não recebe uma nova URL para acessar** e os atributos de request continuam disponíveis no despacho.

O caminho `/resultado` é ilustrativo: para funcionar, deve existir um recurso correspondente na aplicação.

### Redirect

~~~java
response.sendRedirect(
        request.getContextPath() + "/perfil");
~~~

O `sendRedirect()` envia uma resposta de redirecionamento ao cliente. O navegador realiza uma **nova requisição** para a URL indicada.

Por padrão, essa chamada de Servlet produz um redirecionamento temporário, tipicamente HTTP **302**.

No laboratório, utilizamos POST seguido de redirect e depois GET. Esse padrão é conhecido como **Post/Redirect/Get**, ou PRG.

Com ele, após o envio do formulário, o navegador normalmente termina em uma página GET. Assim, uma simples atualização da página não reenvia o formulário POST original.

> **Observação:** quando você precisa indicar explicitamente o redirecionamento para GET após um POST em uma API, o código HTTP **303 See Other** expressa essa intenção de forma mais clara. O `sendRedirect()` padrão utiliza a semântica de 302; navegadores normalmente convertem o POST em GET ao segui-lo.

## 21. Segurança: o que você precisa saber antes de usar sessões em produção

Nosso projeto é didático. Entretanto, estes cuidados são essenciais no mundo real.

### Secure, HttpOnly e SameSite

**Secure** limita o envio do cookie a conexões seguras, com exceções e comportamentos específicos de navegadores para ambientes locais. Em produção, utilize HTTPS.

**HttpOnly** restringe o acesso ao cookie por scripts do navegador, reduzindo determinadas oportunidades de roubo de cookie por JavaScript malicioso. Não impede que o navegador envie automaticamente o cookie.

**SameSite** ajuda a controlar situações nas quais o navegador envia cookies em requisições entre sites. Não substitui, isoladamente, uma estratégia completa de proteção contra CSRF.

A API `Cookie` de Jakarta Servlet 6.0 oferece atributos genéricos, permitindo configurações como:

~~~java
cookie.setAttribute("SameSite", "Lax");
~~~

### Fixação de sessão

Ao implementar autenticação verdadeira, utilize os mecanismos de segurança do contêiner e **renove o identificador de sessão na transição de autenticação**, quando apropriado. A API disponibiliza:

~~~java
request.changeSessionId();
~~~

Essa chamada exige uma sessão associada à requisição e permite trocar o identificador sem necessariamente perder seus atributos.

### Outros cuidados

- Nunca considere o apelido do laboratório uma identidade autenticada.
- Nunca use somente um cookie editável como fonte de autorização.
- Configure expiração e encerramento de sessões.
- Proteja operações que alteram estado contra CSRF.
- Evite escrever identificadores de sessão em logs.
- Considere que um navegador pode bloquear ou excluir cookies.
- Não exponha identificadores de sessão em URLs públicas, histórico, logs ou links compartilháveis.
- Evite armazenar grandes quantidades de dados ou objetos inadequados na sessão.
- Em aplicações distribuídas, planeje persistência, replicação e serialização conforme a infraestrutura.

## 22. O que acontece se o navegador bloquear cookies?

Se o navegador não devolver um identificador de sessão válido, o contêiner pode não conseguir relacionar a requisição atual à sessão anterior.

Jakarta Servlet também descreve mecanismos alternativos de acompanhamento, como reescrita de URL. Porém, colocar identificadores de sessão em URLs implica riscos importantes de exposição.

Por isso, em sistemas com autenticação real, devemos planejar cuidadosamente os mecanismos permitidos e as políticas de segurança em vez de recorrer automaticamente à reescrita de URL.

## 23. Simulado de certificação — 10 questões

Responda antes de consultar o gabarito.

### Questão 1

Qual método consulta uma sessão sem criá-la quando ela não existe?

A) `request.getSession(true)`  
B) `request.getSession(false)`  
C) `response.getSession()`  
D) `session.create()`

### Questão 2

Qual método associa o apelido a uma sessão?

A) `response.setAttribute("apelido", valor)`  
B) `request.getParameter("apelido")`  
C) `session.setAttribute("apelido", valor)`  
D) `cookie.setSession(valor)`

### Questão 3

Em `Cookie.setMaxAge(int)`, o intervalo positivo é expresso em:

A) milissegundos  
B) segundos  
C) minutos  
D) horas

### Questão 4

O que `HttpSession.invalidate()` faz?

A) Renova automaticamente o identificador da sessão.  
B) Apaga todos os cookies do navegador.  
C) Invalida a sessão.  
D) Remove apenas o último atributo criado.

### Questão 5

Qual afirmação descreve melhor o cookie `JSESSIONID`?

A) Contém necessariamente todos os dados do usuário.  
B) Normalmente participa da identificação da sessão gerenciada pelo contêiner.  
C) Substitui qualquer mecanismo de autenticação.  
D) É obrigatoriamente configurado manualmente em cada servlet.

### Questão 6

Qual afirmação sobre `getAttribute()` e `getParameter()` está correta?

A) Ambos leem somente cabeçalhos HTTP.  
B) `getAttribute()` lê dados de escopo; `getParameter()` lê parâmetros da requisição.  
C) `getParameter()` cria uma nova sessão.  
D) Não existe diferença entre eles.

### Questão 7

O que ocorre tipicamente em um `forward`?

A) O navegador sempre realiza uma nova requisição externa.  
B) O processamento é transferido internamente para outro recurso.  
C) A sessão é invalidada.  
D) O endereço do navegador muda obrigatoriamente.

### Questão 8

Qual chamada define dez minutos como intervalo máximo de inatividade da sessão?

A) `session.setMaxInactiveInterval(10)`  
B) `session.setMaxInactiveInterval(10 * 60)`  
C) `session.setMaxInactiveInterval(10 * 60 * 1000)`  
D) `cookie.setMaxAge(10)`

### Questão 9

Qual afirmação sobre `HttpOnly` é correta?

A) Criptografa todos os dados do cookie.  
B) Impede todo tipo de ataque CSRF.  
C) Ajuda a restringir o acesso ao cookie por scripts do navegador.  
D) Transforma o cookie em atributo exclusivo da sessão Java.

### Questão 10

Se uma sessão contém um atributo apelido e o usuário encerra a sessão pelo nosso formulário, o que ocorre com o cookie de preferência tema?

A) É sempre removido automaticamente junto com a sessão.  
B) Continua existindo conforme seus atributos e regras de validade, pois nosso código não o remove.  
C) Passa a conter o apelido.  
D) É convertido em um parâmetro da URL.

### Gabarito comentado

| Questão | Resposta | Justificativa |
|---|---|---|
| 1 | **B** | `getSession(false)` não cria uma sessão ausente. |
| 2 | **C** | `setAttribute()` associa um objeto à sessão. |
| 3 | **B** | `setMaxAge()` utiliza segundos. |
| 4 | **C** | `invalidate()` encerra a validade da sessão. |
| 5 | **B** | `JSESSIONID` costuma transportar um identificador de sessão. |
| 6 | **B** | Atributos de escopo e parâmetros são conceitos diferentes. |
| 7 | **B** | `forward` ocorre dentro do processamento do servidor. |
| 8 | **B** | `setMaxInactiveInterval()` usa segundos: 600 = 10 minutos. |
| 9 | **C** | `HttpOnly` restringe o acesso por scripts compatíveis. |
| 10 | **B** | A invalidação da sessão não exclui automaticamente o cookie `tema`. |

### Como avaliar o resultado?

- **9 ou 10 acertos:** excelente domínio dos conceitos introdutórios.
- **7 ou 8 acertos:** revise as questões erradas e refaça o laboratório.
- **Até 6 acertos:** releia as seções de sessões, cookies e escopos antes de avançar.

Esse é um critério pedagógico desta trilha; não é uma pontuação oficial de qualquer exame de certificação.

## 24. Exercícios e desafios práticos

### Exercício 1 — Consultar cabeçalhos

No `doGet()` do `PerfilServlet`, consulte o cabeçalho `Accept` usando `request.getHeader("Accept")`. Trate o retorno `null` e exiba uma representação textual segura no laboratório.

### Exercício 2 — Remover o tema

Crie uma nova ação POST chamada `limpar-tema` que envie um cookie `tema` com `Max-Age=0` e o mesmo `Path` utilizado ao gravá-lo.

### Exercício 3 — Entender múltiplas janelas

Abra duas abas do mesmo navegador e acesse o perfil nas duas. Observe se elas compartilham a sessão. Depois, compare com uma janela privativa isolada.

### Exercício 4 — Investigar expiração

Modifique temporariamente:

~~~java
sessao.setMaxInactiveInterval(30);
~~~

Aguarde mais de trinta segundos **sem enviar requisições que acessem aquela sessão**. Depois, tente consultar o perfil e observe o comportamento.

### Desafio extra — Histórico de preferências

Armazene na sessão um histórico simples de mudanças do tema, limitado a poucas entradas. Mostre-o em texto simples. Depois, explique por que esse histórico desaparece quando a sessão é invalidada, mas o cookie de preferência pode continuar armazenado no navegador.

## 25. Checklist de conclusão

- [ ] Consigo explicar HTTP sem estado.
- [ ] Sei distinguir cabeçalhos de requisição e de resposta.
- [ ] Criei e li um cookie.
- [ ] Sei por que `request.getCookies()` pode ser `null`.
- [ ] Entendi a diferença entre cookie e sessão.
- [ ] Comparei `getSession(true)` e `getSession(false)`.
- [ ] Usei `setAttribute()` e `getAttribute()`.
- [ ] Configurei `setMaxInactiveInterval()` em segundos.
- [ ] Invalidei uma sessão com `invalidate()`.
- [ ] Diferenciei request, session e application.
- [ ] Entendi `forward`, `redirect` e PRG.
- [ ] Testei cookies pelo navegador e pelo `curl`.
- [ ] Identifiquei riscos de segurança relacionados a sessões.
- [ ] Respondi às dez questões do simulado.

## 26. O que vem na Lição 4?

Na próxima lição, vamos aprofundar o funcionamento do pipeline de requisições com **Servlet Filters e Listeners**.

Estudaremos como interceptar requisições, registrar eventos, centralizar tarefas transversais e compreender o ciclo de vida dos componentes web. Essa base também será útil quando começarmos a desenvolver nossas APIs REST com Jakarta REST.

---

## Referências oficiais e leituras recomendadas

1. [Jakarta Servlet 6.0 — Especificação](https://jakarta.ee/specifications/servlet/6.0/)
2. [Jakarta Servlet 6.0 — Documento completo](https://jakarta.ee/specifications/servlet/6.0/jakarta-servlet-spec-6.0.html)
3. [Jakarta Servlet 6.0 — Javadoc](https://jakarta.ee/specifications/servlet/6.0/apidocs/)
4. [Jakarta EE Tutorial — Aplicações web](https://jakarta.ee/learn/docs/jakartaee-tutorial/current/web/webapp/webapp.html)
5. [OWASP — Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)
6. [Eclipse GlassFish 7 — Documentação e versões](https://glassfish.org/download_gf7.html)

**Metadescrição:** Aprenda cookies, sessões HTTP e HttpSession no Jakarta EE com GlassFish 7. Crie um perfil com login demonstrativo, logout, testes e simulado.

**Tags:** Java EE, Jakarta EE, Servlets, HttpSession, Cookies, JSESSIONID, GlassFish 7, HTTP, sessão Java, segurança web, request, response, certificação Java.
