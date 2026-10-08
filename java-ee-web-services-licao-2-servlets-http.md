---
title: "Java EE e Web Services — Lição 2: Servlets e Protocolo HTTP"
seo_title: "Java EE Lição 2: Servlets, HTTP, GET e POST na prática"
slug: "java-ee-web-services-licao-2-servlets-http"
meta_description: "Aprenda Servlets e HTTP no Jakarta EE com GlassFish 7. Entenda GET, POST, request, response, web.xml, exemplos práticos e questões de certificação."
focus_keyphrase: "java ee servlets http"
related_keywords:
  - "Java EE Web Services"
  - "Jakarta EE 10"
  - "Jakarta Servlet 6.0"
  - "GlassFish 7"
  - "doGet e doPost"
  - "HttpServletRequest"
  - "HttpServletResponse"
  - "protocolo HTTP"
  - "web.xml"
  - "certificação Java EE"
category: "Java"
tags:
  - "Java EE"
  - "Jakarta EE"
  - "Web Services"
  - "Servlets"
  - "HTTP"
  - "GET"
  - "POST"
  - "GlassFish"
  - "Certificação Java"
  - "Programação Web"
excerpt: "Segunda lição de Java EE e Web Services: domine o ciclo de requisições HTTP, os métodos GET e POST e construa um servlet completo no GlassFish 7."
---

# ☕ Java EE e Web Services — Lição 2: Servlets e Protocolo HTTP

Na lição anterior, configuramos o Eclipse, adicionamos o projeto `OlaJavaEE` ao GlassFish 7 e conseguimos acessar:

~~~text
http://localhost:8080/OlaJavaEE/ola
~~~

O navegador exibiu:

~~~text
Olá, Java EE!
~~~

Agora vamos entender **o que acontece entre o navegador, o servidor GlassFish e o servlet**. Além disso, construiremos uma página HTML que envia informações para um servlet por meio de uma requisição HTTP.

Nosso objetivo não é apenas fazer o código funcionar. Queremos dominar os conceitos que aparecem em avaliações de Java Enterprise e que são essenciais para desenvolver Web Services.

> **Atenção à versão:** nosso laboratório usa **GlassFish 7, Jakarta EE 10 e Jakarta Servlet 6.0**, com imports `jakarta.servlet.*`. Uma certificação específica de **Java EE 8** pode cobrar as APIs históricas `javax.servlet.*`. Os fundamentos de HTTP e servlets permanecem relevantes, mas os nomes de pacotes e algumas regras mudam conforme a versão.

## 1. O que vamos dominar hoje

Nesta lição, você aprenderá:

- o que é o protocolo HTTP;
- a diferença entre cliente, servidor e contêiner web;
- como o GlassFish recebe uma requisição;
- o ciclo de vida de um servlet;
- as diferenças entre `GET` e `POST`;
- o papel de `HttpServletRequest` e `HttpServletResponse`;
- como usar `doGet()` e `doPost()`;
- como ler parâmetros com `getParameter()`;
- como definir tipo e codificação da resposta;
- como utilizar códigos de status HTTP;
- como funciona o mapeamento `@WebServlet`;
- quando usar `web.xml`;
- como testar uma aplicação no navegador e no terminal;
- quais armadilhas merecem atenção em uma prova de certificação.

## 2. Entendendo a comunicação cliente-servidor

Imagine que você digite no navegador:

~~~text
http://localhost:8080/OlaJavaEE/ola
~~~

Primeiramente, o navegador atua como **cliente** e envia uma requisição HTTP. Em seguida, o GlassFish recebe a requisição e identifica a aplicação responsável. Depois disso, o contêiner web localiza o servlet associado ao endereço solicitado.

Finalmente, o servlet prepara a resposta e o servidor a envia de volta ao navegador.

~~~text
NAVEGADOR (cliente)
         |
         | Requisição HTTP
         v
GLASSFISH (servidor / contêiner)
         |
         | Identifica a aplicação e o mapeamento
         v
SERVLET (código Java)
         |
         | Cria a resposta HTTP
         v
NAVEGADOR (exibe o resultado)
~~~

Portanto, um servlet não funciona como um programa Java comum que espera instruções de `main()`. Em vez disso, o servidor controla sua criação e o chama durante o atendimento das requisições.

## 3. O que é HTTP?

**HTTP** significa *Hypertext Transfer Protocol*. Esse protocolo define como clientes e servidores trocam requisições e respostas na web.

Uma requisição simples pode ter a seguinte aparência:

~~~http
GET /OlaJavaEE/ola HTTP/1.1
Host: localhost:8080
Accept: text/html
~~~

Observe três elementos importantes:

1. `GET` informa o método HTTP.
2. `/OlaJavaEE/ola` identifica o recurso solicitado.
3. `HTTP/1.1` indica a versão do protocolo nessa representação.

O servidor pode responder assim:

~~~http
HTTP/1.1 200 OK
Content-Type: text/plain;charset=UTF-8

Olá, Java EE!
~~~

Nesse exemplo, `200` indica sucesso. Além disso, `Content-Type` informa ao cliente como interpretar o corpo da resposta.

> **Ponto de prova:** HTTP é um protocolo de requisição e resposta. Ele não exige que a resposta contenha HTML; também pode transportar texto, JSON, XML, imagens e muitos outros tipos de conteúdo.

## 4. O que é um servlet?

Um **servlet** é um componente Java executado dentro de um contêiner web para processar requisições e produzir respostas.

No nosso ambiente, o GlassFish exerce esse papel de contêiner. A classe `HttpServlet` já oferece o comportamento necessário para tratar métodos HTTP conhecidos.

Por exemplo:

~~~java
public class OlaServlet extends HttpServlet {
    // Métodos de tratamento das requisições.
}
~~~

Entretanto, herdar de `HttpServlet` não basta para expor uma URL. Também precisamos fornecer um **mapeamento**, normalmente por anotação ou pelo descritor `web.xml`.

## 5. O ciclo de vida de um servlet

O contêiner gerencia o ciclo de vida do servlet. Em uma situação típica:

1. O contêiner carrega e instancia a classe do servlet.
2. Em seguida, chama `init()` para inicialização.
3. A cada requisição, chama `service()`.
4. Em `HttpServlet`, o método `service()` direciona a requisição ao manipulador apropriado, como `doGet()` ou `doPost()`.
5. Por fim, quando retira o servlet de serviço, o contêiner chama `destroy()`.

~~~text
Instanciação
     |
     v
   init()
     |
     v
 service() ----> doGet() / doPost() / ...
     |
     | outras requisições
     v
 destroy()
~~~

Em geral, **não devemos chamar `init()` ou `service()` manualmente para simular uma requisição HTTP**. O contêiner assume essa responsabilidade.

Além disso, vários usuários podem acessar o mesmo servlet simultaneamente. Por isso, evite guardar dados específicos de uma requisição em campos mutáveis da instância. Prefira variáveis locais nos métodos de atendimento.

## 6. GET e POST: qual é a diferença?

Os dois métodos permitem enviar informações para o servidor, mas possuem propósitos diferentes.

| Característica | GET | POST |
|---|---|---|
| Propósito típico | Consultar um recurso | Enviar dados para processamento |
| Dados em formulário HTML | Geralmente na URL, como query string | Geralmente no corpo da requisição |
| Exemplo | `/saudacao?nome=Ana` | Formulário enviado para `/saudacao` |
| Semântica | Método seguro e idempotente | Não é, em geral, seguro ou idempotente |
| Método do `HttpServlet` | `doGet()` | `doPost()` |

**Seguro** significa que o método não deve ser usado para solicitar mudança de estado da aplicação. **Idempotente** significa que repetir a mesma operação tem o mesmo efeito pretendido que executá-la uma única vez.

Atenção: **POST não oferece criptografia por si só**. Tanto GET quanto POST devem usar HTTPS quando houver dados sensíveis.

## 7. Os objetos request e response

Os métodos `doGet()` e `doPost()` recebem dois objetos centrais:

~~~java
HttpServletRequest request
HttpServletResponse response
~~~

O primeiro representa a **requisição** que chegou. O segundo permite construir a **resposta** que será enviada ao cliente.

| Objeto | Métodos úteis | Para que servem |
|---|---|---|
| `request` | `getParameter("nome")` | Lê um parâmetro |
| `request` | `getMethod()` | Obtém o método HTTP |
| `request` | `getContextPath()` | Obtém o contexto da aplicação |
| `request` | `getHeader("Accept")` | Consulta um cabeçalho |
| `response` | `setContentType(...)` | Define o formato e a codificação da resposta |
| `response` | `getWriter()` | Escreve conteúdo textual |
| `response` | `setStatus(...)` | Define o status HTTP |
| `response` | `sendError(...)` | Envia uma resposta de erro |

Portanto, lembre-se desta distinção: **request lê a entrada; response constrói a saída**.

## 8. Preparando o projeto no Eclipse

Vamos reutilizar o projeto `OlaJavaEE` que já funciona no GlassFish 7.

Crie o pacote:

~~~text
br.com.exemplo.javaee.web
~~~

Depois disso, crie a classe `SaudacaoServlet` dentro desse pacote.

A estrutura ficará semelhante a:

~~~text
OlaJavaEE/
└── src/
    └── main/
        ├── java/
        │   └── br/com/exemplo/javaee/web/
        │       └── SaudacaoServlet.java
        └── webapp/
            ├── formulario.html
            └── WEB-INF/
                └── web.xml
~~~

Observe que **`src/main/java` guarda o código Java**, enquanto **`src/main/webapp` guarda os arquivos disponibilizados na aplicação web**. As classes compiladas devem chegar a `WEB-INF/classes` no artefato publicado.

Não coloque a instalação do GlassFish dentro da pasta do projeto.

## 9. Criando um servlet com GET e POST

Abra `SaudacaoServlet.java` e escreva:

~~~java
package br.com.exemplo.javaee.web;

import java.io.IOException;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

@WebServlet("/saudacao")
public class SaudacaoServlet extends HttpServlet {

    private static final long serialVersionUID = 1L;

    @Override
    protected void doGet(
            HttpServletRequest request,
            HttpServletResponse response)
            throws ServletException, IOException {

        responderSaudacao(request, response);
    }

    @Override
    protected void doPost(
            HttpServletRequest request,
            HttpServletResponse response)
            throws ServletException, IOException {

        // Defina antes de ler parâmetros do corpo do formulário.
        request.setCharacterEncoding("UTF-8");

        responderSaudacao(request, response);
    }

    private void responderSaudacao(
            HttpServletRequest request,
            HttpServletResponse response)
            throws IOException {

        String nome = request.getParameter("nome");

        if (nome == null || nome.isBlank()) {
            response.sendError(
                    HttpServletResponse.SC_BAD_REQUEST,
                    "Informe o nome.");
            return;
        }

        nome = nome.strip();

        if (nome.length() > 80) {
            response.sendError(
                    HttpServletResponse.SC_BAD_REQUEST,
                    "O nome deve ter no máximo 80 caracteres.");
            return;
        }

        response.setContentType("text/plain;charset=UTF-8");
        response.getWriter().println("Olá, " + nome + "!");
    }
}
~~~

Nesta experiência, GET e POST produzem a mesma saudação. Isso é intencional: queremos comparar o transporte dos parâmetros sem alterar dados persistidos. Em aplicações reais, o método deve refletir a finalidade da operação.

### Entendendo cada parte

**`@WebServlet("/saudacao")`**: informa ao contêiner a rota que o servlet atende.

**`@Override`**: indica que `doGet()` e `doPost()` sobrescrevem métodos herdados de `HttpServlet`.

**`request.getParameter("nome")`**: lê o parâmetro `nome` da requisição. Para formulários com `application/x-www-form-urlencoded`, o contêiner também fornece acesso aos campos do corpo no POST.

**`request.setCharacterEncoding("UTF-8")`**: define a decodificação do corpo antes de consultar os parâmetros. O tratamento da codificação da URL é uma questão distinta, administrada pelo contêiner.

**`response.sendError(400, ...)`**: comunica ao cliente que a entrada não atende à regra. A instrução `return` impede que o método continue tentando escrever uma resposta de sucesso.

**`response.setContentType("text/plain;charset=UTF-8")`**: identifica texto simples e evita que o navegador interprete a saudação como HTML. Isso também reduz o risco de executar conteúdo HTML fornecido pelo usuário nesse exemplo.

**`throws IOException`**: preserva a propagação de possíveis falhas de escrita. Não precisamos capturar a exceção apenas para chamar `printStackTrace()` e escondê-la do contêiner.

## 10. Testando uma requisição GET

Primeiramente, publique o projeto atualizado no GlassFish.

Depois, acesse:

~~~text
http://localhost:8080/OlaJavaEE/saudacao?nome=Ana
~~~

Resultado esperado:

~~~text
Olá, Ana!
~~~

Agora experimente:

~~~text
http://localhost:8080/OlaJavaEE/saudacao?nome=Carlos
~~~

Resultado esperado:

~~~text
Olá, Carlos!
~~~

Perceba que `?nome=Ana` representa um **parâmetro da query string**. O método `getParameter("nome")` permite recuperar esse valor.

Se você abrir apenas `/saudacao`, sem informar o parâmetro, nossa validação enviará o status HTTP **400 — Bad Request**.

## 11. Criando um formulário HTML para POST

Crie o arquivo:

~~~text
src/main/webapp/formulario.html
~~~

Adicione o conteúdo:

~~~html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Formulário de saudação</title>
</head>
<body>
    <h1>Envie seu nome</h1>

    <form action="saudacao" method="post" accept-charset="UTF-8">
        <label for="nome">Nome:</label>
        <input
            type="text"
            id="nome"
            name="nome"
            maxlength="80"
            required>

        <button type="submit">Enviar</button>
    </form>
</body>
</html>
~~~

Agora abra:

~~~text
http://localhost:8080/OlaJavaEE/formulario.html
~~~

Digite, por exemplo, `Maria` e clique em **Enviar**.

O navegador enviará uma requisição POST para `/OlaJavaEE/saudacao`. Em seguida, o contêiner chamará `doPost()` e o servlet retornará:

~~~text
Olá, Maria!
~~~

O atributo `name="nome"` no campo HTML é importante: é ele que corresponde à chamada `request.getParameter("nome")`.

> **Atenção:** a validação `required` no HTML melhora a experiência do usuário, mas não substitui a validação do lado do servidor. Um cliente HTTP pode enviar requisições sem usar o formulário.

## 12. Entendendo a URL da aplicação

Observe este endereço:

~~~text
http://localhost:8080/OlaJavaEE/saudacao
~~~

Ele contém:

~~~text
http://             -> protocolo
localhost           -> host
8080                -> porta
/OlaJavaEE          -> contexto da aplicação
/saudacao           -> mapeamento do servlet
~~~

Consequentemente, a anotação `@WebServlet("/saudacao")` **não inclui o contexto `/OlaJavaEE`**. O servidor adiciona esse contexto conforme o nome ou a configuração do aplicativo publicado.

Se o contexto mudar, a URL também poderá mudar.

## 13. @WebServlet ou web.xml?

Na primeira lição, você configurou o servlet no arquivo `WEB-INF/web.xml`, e a rota `/ola` começou a funcionar.

Esse mecanismo continua válido. Entretanto, nas versões modernas de Servlet, também podemos utilizar anotações.

### Alternativa A — Anotação

~~~java
@WebServlet("/saudacao")
public class SaudacaoServlet extends HttpServlet {
    // ...
}
~~~

Nesse caso, o contêiner descobre o componente durante o processamento das anotações de implantação, desde que a configuração permita isso.

### Alternativa B — Descritor XML

Se você preferir registrar o mesmo servlet por XML, retire a anotação `@WebServlet` da classe e adicione ao `web.xml`:

~~~xml
<servlet>
    <servlet-name>SaudacaoServlet</servlet-name>
    <servlet-class>br.com.exemplo.javaee.web.SaudacaoServlet</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>SaudacaoServlet</servlet-name>
    <url-pattern>/saudacao</url-pattern>
</servlet-mapping>
~~~

**Escolha uma configuração clara para a mesma rota**, evitando duplicações desnecessárias.

### E o seu web.xml antigo?

O arquivo usado no primeiro exercício declara `version="2.4"` e o namespace histórico `http://java.sun.com/xml/ns/j2ee`. Como você trabalha com GlassFish 7 e Jakarta EE 10, vale modernizá-lo com cuidado.

A estrutura mínima de um descritor Jakarta Servlet 6.0 é:

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app
    xmlns="https://jakarta.ee/xml/ns/jakartaee"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee
        https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd"
    version="6.0">

    <display-name>OlaJavaEE</display-name>

    <!-- Mantenha aqui eventuais mapeamentos XML necessários. -->
</web-app>
~~~

**Não substitua o arquivo atual sem preservar o mapeamento `/ola` que já funciona.** Faça a modernização como um exercício separado e teste novamente os dois endereços.

Também confira no Eclipse se o facet **Dynamic Web Module** e o servidor estão alinhados com a especificação do projeto. GlassFish 7 corresponde a Jakarta EE 10 e Jakarta Servlet 6.0; a versão 6.1 do Servlet pertence à geração Jakarta EE 11.

> **Ponto de certificação:** `metadata-complete="true"` no descritor impede a descoberta automática de certas informações de implantação por anotações. Por padrão, sem essa configuração, as anotações suportadas são processadas de acordo com a especificação do contêiner. A simples existência de um `web.xml` não significa que as anotações estão desativadas.

## 14. Principais status HTTP

Um desenvolvedor Java Enterprise precisa reconhecer os códigos mais comuns:

| Status | Nome | Exemplo |
|---|---|---|
| 200 | OK | Requisição atendida com sucesso |
| 201 | Created | Recurso criado com sucesso |
| 204 | No Content | Sucesso sem corpo na resposta |
| 400 | Bad Request | Entrada inválida |
| 401 | Unauthorized | Autenticação necessária ou inválida |
| 403 | Forbidden | Acesso não autorizado para o cliente |
| 404 | Not Found | Recurso não encontrado |
| 405 | Method Not Allowed | Método HTTP não permitido |
| 500 | Internal Server Error | Erro interno no servidor |

Por exemplo, o status `404` que vimos ao testar o primeiro servlet significava que o endereço solicitado **não correspondia a um recurso disponível naquele momento**. Não significava, por si só, que o servidor GlassFish estava desligado.

## 15. Testes pelo terminal

Além do navegador, você pode testar os métodos HTTP usando `curl`.

**GET:**

~~~bash
curl -i "http://localhost:8080/OlaJavaEE/saudacao?nome=Ana"
~~~

**POST:**

~~~bash
curl -i -X POST --data-urlencode "nome=Maria" "http://localhost:8080/OlaJavaEE/saudacao"
~~~

**Entrada inválida:**

~~~bash
curl -i "http://localhost:8080/OlaJavaEE/saudacao"
~~~

A opção `-i` exibe também os cabeçalhos da resposta. Assim, você poderá observar códigos como `200` e `400`, em vez de enxergar apenas o texto retornado.

No PowerShell, dependendo da versão do Windows e da configuração, pode ser necessário chamar explicitamente `curl.exe` para evitar conflitos com aliases.

## 16. Boas práticas para o servlet

Antes de avançar, guarde estas recomendações:

- **Use um pacote nomeado.** Evite o *default package* para classes da aplicação.
- **Mantenha `@Override`.** Isso ajuda o compilador a identificar assinaturas incorretas.
- **Valide os parâmetros no servidor.** O HTML não é uma barreira de segurança.
- **Escolha o `Content-Type` apropriado.** Texto simples, HTML e JSON exigem tratamento diferente.
- **Evite campos mutáveis para dados da requisição.** Um servlet pode atender vários usuários concorrentes.
- **Não esconda exceções.** Preserve erros relevantes para que o contêiner e os mecanismos de observabilidade possam tratá-los.
- **Prefira HTTPS para dados sensíveis.** POST não oculta dados de intermediários sem criptografia.
- **Separe consulta de alteração de estado.** Um GET não deve ser a operação que efetiva uma compra ou exclui um cadastro.

## 17. Simulado de certificação

Responda **sem consultar o gabarito**, para avaliar seu entendimento.

### Questão 1

Qual método de `HttpServlet` normalmente trata requisições HTTP GET?

A) `init()`  
B) `doGet()`  
C) `destroy()`  
D) `getParameter()`

### Questão 2

Qual objeto representa os dados da requisição recebida pelo servlet?

A) `HttpServletResponse`  
B) `ServletConfig`  
C) `HttpServletRequest`  
D) `PrintWriter`

### Questão 3

Qual método recupera um parâmetro chamado `nome`?

A) `response.getParameter("nome")`  
B) `request.getParameter("nome")`  
C) `request.getWriter("nome")`  
D) `response.getHeader("nome")`

### Questão 4

Quando a URL `/saudacao` recebe um parâmetro inválido, qual status nosso exemplo envia?

A) 200  
B) 201  
C) 400  
D) 404

### Questão 5

Qual afirmação descreve melhor o método HTTP GET?

A) Deve realizar alterações irreversíveis no servidor.  
B) Só funciona se houver um formulário HTML.  
C) É destinado a consultas e possui semântica segura e idempotente.  
D) Criptografa automaticamente os parâmetros.

### Questão 6

O que acontece quando o navegador envia um POST a um servlet que herda de `HttpServlet`, mas não implementa `doPost()`?

A) O contêiner executa `doGet()` automaticamente.  
B) A implementação padrão normalmente informa que o método não é suportado, como por uma resposta 405 em HTTP/1.1.  
C) O contêiner chama `destroy()`.  
D) O servidor converte o POST em GET e continua.

### Questão 7

Por que é arriscado armazenar o nome do visitante em um atributo mutável da instância do servlet?

A) Porque Java proíbe atributos em servlets.  
B) Porque o nome só pode aparecer no `web.xml`.  
C) Porque requisições concorrentes podem compartilhar a mesma instância e interferir nesse atributo.  
D) Porque todas as requisições criam obrigatoriamente um servlet novo.

### Questão 8

Em uma aplicação Servlet moderna, qual configuração pode impedir o processamento automático de anotações de implantação?

A) `metadata-complete="true"`  
B) `Content-Type: text/plain`  
C) `@Override`  
D) `request.getContextPath()`

### Gabarito comentado

| Questão | Resposta | Explicação |
|---|---|---|
| 1 | **B** | `doGet()` atende requisições GET em `HttpServlet`. |
| 2 | **C** | `HttpServletRequest` representa a requisição. |
| 3 | **B** | `getParameter()` recupera um parâmetro da requisição. |
| 4 | **C** | O código usa `SC_BAD_REQUEST`, equivalente a 400. |
| 5 | **C** | GET é seguro e idempotente por definição semântica. |
| 6 | **B** | A implementação padrão de `doPost()` comunica que a operação não é suportada. |
| 7 | **C** | O contêiner pode atender requisições concorrentes na mesma instância. |
| 8 | **A** | `metadata-complete` controla a descoberta por anotações de implantação. |

## 18. Exercícios práticos

### Exercício 1 — Altere o cumprimento

Faça o servlet retornar:

~~~text
Bem-vinda, Maria! Você está estudando Jakarta EE.
~~~

Você pode escolher uma frase neutra, como `Boas-vindas`, para qualquer nome.

### Exercício 2 — Adicione outro parâmetro

Amplie o formulário com um campo chamado `cidade`. Em seguida, leia o valor com:

~~~java
request.getParameter("cidade");
~~~

Faça a aplicação retornar o nome e a cidade, validando ambos os campos.

### Exercício 3 — Compare GET e POST

Realize uma chamada GET usando o navegador e uma chamada POST usando o formulário. Depois, observe as diferenças no **Network** das ferramentas de desenvolvedor do navegador, especialmente método, URL, parâmetros e corpo.

### Exercício 4 — Simule um erro

Envie uma requisição sem o parâmetro `nome` e confirme que o servidor responde com HTTP 400.

### Desafio extra

Crie um servlet com a rota `/calculadora` que receba `a` e `b`, valide dois números inteiros e devolva a soma. Se a entrada não for um número válido, responda com HTTP 400.

## 19. Checklist de conclusão

Considere a lição praticada quando você conseguir:

- [ ] Explicar o fluxo navegador → GlassFish → servlet → navegador.
- [ ] Diferenciar GET e POST.
- [ ] Criar um servlet usando `jakarta.servlet.*`.
- [ ] Configurar uma rota com `@WebServlet`.
- [ ] Ler um parâmetro com `getParameter()`.
- [ ] Retornar uma resposta com `getWriter()`.
- [ ] Enviar um status HTTP 400 para entrada inválida.
- [ ] Criar e enviar um formulário HTML por POST.
- [ ] Explicar a diferença entre anotação e `web.xml`.
- [ ] Responder às oito questões de revisão.

## 20. O que vem na próxima lição?

Na **Lição 3**, aprofundaremos o trabalho com **requisições, respostas, cabeçalhos, cookies e sessões HTTP**.

Esses conceitos prepararão o caminho para estudar **REST com Jakarta RESTful Web Services (JAX-RS)** e, depois, **SOAP com Jakarta XML Web Services (JAX-WS)**.

---

## Referências oficiais

- [Jakarta Servlet 6.0 — página da especificação](https://jakarta.ee/specifications/servlet/6.0/)
- [Jakarta Servlet 6.0 — especificação completa](https://jakarta.ee/specifications/servlet/6.0/jakarta-servlet-spec-6.0)
- [Jakarta EE Tutorial — Web Applications](https://jakarta.ee/learn/docs/jakartaee-tutorial/current/web/webapp/webapp.html)
- [Esquemas XML de Jakarta EE](https://jakarta.ee/xml/ns/jakartaee/)
- [GlassFish 7 — projeto](https://github.com/jakarta-ee/glassfish70)

**Metadescrição:** Aprenda Servlets e HTTP no Jakarta EE com GlassFish 7. Entenda GET, POST, request, response, web.xml, exemplos práticos e questões de certificação.

**Tags:** Java EE, Jakarta EE, Web Services, Servlets, HTTP, GET, POST, GlassFish 7, HttpServletRequest, HttpServletResponse, web.xml, certificação Java.
