---
title: "JTE com Spring Boot — Parte 2: layouts, componentes e produção"
seo_title: "JTE com Spring Boot: layouts, componentes e produção"
slug: "jte-spring-boot-parte-2-layouts-componentes-producao"
meta_description: "Aprenda JTE com Spring Boot na prática: modelos tipados, loops, condicionais, layouts, componentes, escaping e configuração para produção."
focus_keyphrase: "JTE com Spring Boot"
related_keywords:
  - "JTE Spring Boot"
  - "Java Template Engine"
  - "templates Spring Boot"
  - "server-side rendering Java"
  - "JTE layouts"
  - "JTE componentes"
  - "Spring MVC JTE"
  - "JTE produção"
category: "Java"
tags:
  - "Java"
  - "Spring Boot"
  - "JTE"
  - "Java Template Engine"
  - "Spring MVC"
  - "Server-Side Rendering"
  - "Templates"
  - "Back-End"
excerpt: "Segunda parte do guia de JTE com Spring Boot: avance do primeiro template para páginas tipadas, componentes reutilizáveis, layouts e uma configuração adequada para produção."
---

# JTE com Spring Boot — Parte 2: layouts, componentes e produção

Na primeira parte deste conteúdo, vimos como colocar o **JTE com Spring Boot** para funcionar e renderizar uma página simples. Agora vamos avançar para o ponto em que um motor de templates realmente começa a mostrar seu valor em uma aplicação: páginas com modelos tipados, listas, condicionais, componentes reutilizáveis e layouts.

O objetivo desta continuação é sair do tradicional "Hello World" e montar uma pequena aplicação server-side com uma estrutura que poderia evoluir para um sistema real.

Nesta parte, você aprenderá:

- como usar parâmetros tipados em templates JTE;
- como trabalhar com registros Java como View Models;
- como criar condicionais com `@if`;
- como percorrer coleções com `@for`;
- como criar componentes reutilizáveis;
- como criar um layout compartilhado;
- como passar blocos de conteúdo entre templates;
- como o escaping HTML ajuda na segurança;
- como organizar o projeto;
- como configurar desenvolvimento e produção;
- o que muda entre Spring Boot 3 e Spring Boot 4.

> Os exemplos deste artigo usam a linha atual do JTE 3.2.x. Em setembro de 2026, a documentação oficial apresenta a versão 3.2.4 nos starters para Spring Boot 3 e 4.

## 1. Uma atualização importante sobre o setup

Antes de continuar, vale alinhar a configuração com a documentação atual do projeto.

No **Spring Boot 4**, o starter utilizado é:

```xml
<dependency>
    <groupId>gg.jte</groupId>
    <artifactId>jte-spring-boot-starter-4</artifactId>
    <version>3.2.4</version>
</dependency>
```

Já no **Spring Boot 3**, a documentação atual apresenta:

```xml
<dependency>
    <groupId>gg.jte</groupId>
    <artifactId>jte-spring-boot-starter-3</artifactId>
    <version>3.2.4</version>
</dependency>

<dependency>
    <groupId>gg.jte</groupId>
    <artifactId>jte</artifactId>
    <version>3.2.4</version>
</dependency>
```

No Spring Boot 4, o JTE também aparece no ecossistema do **Spring Initializr** como uma opção de template engine.

Por padrão, os templates ficam em:

```text
src/main/jte
```

Essa é uma diferença importante em relação a engines que tradicionalmente usam:

```text
src/main/resources/templates
```

A configuração padrão pode ser alterada, mas neste artigo vamos seguir a convenção oficial.

## 2. Estrutura do projeto

Vamos montar um pequeno catálogo de cursos.

A estrutura será semelhante a esta:

```text
src/
└── main/
    ├── java/
    │   └── com/
    │       └── exemplo/
    │           └── cursos/
    │               ├── CursosApplication.java
    │               ├── controller/
    │               │   └── CursoController.java
    │               └── viewmodel/
    │                   ├── CursoView.java
    │                   └── CursosPage.java
    ├── jte/
    │   ├── cursos.jte
    │   └── layout/
    │       ├── page.jte
    │       └── cursoCard.jte
    └── resources/
        └── application.properties
```

A ideia é separar:

- código Java;
- modelos usados pela interface;
- templates de página;
- componentes;
- layout compartilhado.

Esse tipo de organização ajuda quando a quantidade de páginas aumenta.

## 3. Criando um View Model tipado

Uma das características mais interessantes do JTE é trabalhar diretamente com tipos Java.

Vamos representar um curso com um `record`:

```java
package com.exemplo.cursos.viewmodel;

public record CursoView(
        Long id,
        String nome,
        String categoria,
        double preco,
        boolean destaque
) {
}
```

Agora criamos o modelo da página:

```java
package com.exemplo.cursos.viewmodel;

import java.util.List;

public record CursosPage(
        String titulo,
        String usuario,
        List<CursoView> cursos
) {
}
```

Por que criar um modelo específico para a view?

Porque o template não precisa receber diretamente uma entidade JPA cheia de campos, relacionamentos e detalhes internos.

Podemos expor apenas o que a interface realmente precisa.

Isso reduz acoplamento e evita situações como:

```text
Entidade do banco
        ↓
carregamento lazy
        ↓
template acessa relacionamento
        ↓
consulta inesperada
```

Um View Model deixa o contrato da página explícito.

## 4. Criando o controller

Agora montamos um controller Spring MVC.

```java
package com.exemplo.cursos.controller;

import com.exemplo.cursos.viewmodel.CursoView;
import com.exemplo.cursos.viewmodel.CursosPage;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

import java.util.List;

@Controller
public class CursoController {

    @GetMapping("/cursos")
    public String cursos(Model model) {

        List<CursoView> cursos = List.of(
                new CursoView(
                        1L,
                        "Spring Boot do Zero",
                        "Java",
                        149.90,
                        true
                ),
                new CursoView(
                        2L,
                        "APIs REST com Java",
                        "Back-End",
                        129.90,
                        false
                ),
                new CursoView(
                        3L,
                        "Docker para Desenvolvedores",
                        "DevOps",
                        99.90,
                        true
                )
        );

        CursosPage page = new CursosPage(
                "Catálogo de Cursos",
                "Michel",
                cursos
        );

        model.addAttribute("page", page);

        return "cursos";
    }
}
```

Observe o retorno:

```java
return "cursos";
```

Com o starter configurado, não precisamos retornar:

```java
return "cursos.jte";
```

O `ViewResolver` do JTE localiza o template correspondente.

## 5. Declarando parâmetros no template

Crie:

```text
src/main/jte/cursos.jte
```

O início do arquivo pode ficar assim:

```html
@import com.exemplo.cursos.viewmodel.CursosPage

@param CursosPage page

<h1>${page.titulo()}</h1>

<p>Olá, ${page.usuario()}!</p>
```

O ponto mais importante está aqui:

```html
@param CursosPage page
```

O template declara explicitamente qual tipo espera receber.

Isso significa que o JTE consegue validar expressões durante a compilação.

Se escrevermos por engano:

```html
${page.nomeInexistente()}
```

o erro pode ser detectado antes de chegar ao usuário final.

Essa abordagem é diferente de engines muito dependentes de expressões dinâmicas avaliadas apenas em tempo de execução.

## 6. Trabalhando com condicionais

Vamos mostrar uma mensagem diferente quando houver cursos cadastrados.

```html
@if(page.cursos().isEmpty())
    <p>Nenhum curso disponível no momento.</p>
@else
    <p>
        Encontramos ${page.cursos().size()} cursos.
    </p>
@endif
```

A estrutura lembra Java:

```text
@if(...)
@else
@endif
```

Também existe:

```text
@elseif
```

Exemplo:

```html
@if(page.cursos().size() >= 10)
    <p>Catálogo grande.</p>
@elseif(page.cursos().size() >= 5)
    <p>Catálogo médio.</p>
@else
    <p>Catálogo compacto.</p>
@endif
```

A sintaxe permanece próxima da linguagem Java, o que reduz a necessidade de aprender uma linguagem de expressão completamente diferente.

## 7. Percorrendo listas com @for

Agora vamos mostrar todos os cursos.

```html
@for(var curso : page.cursos())
    <article>
        <h2>${curso.nome()}</h2>
        <p>Categoria: ${curso.categoria()}</p>
        <p>Preço: R$ ${curso.preco()}</p>
    </article>
@endfor
```

Também podemos combinar loop e condição:

```html
@for(var curso : page.cursos())
    <article>
        <h2>${curso.nome()}</h2>

        @if(curso.destaque())
            <strong>Curso em destaque</strong>
        @endif

        <p>${curso.categoria()}</p>
    </article>
@endfor
```

Desde o JTE 3, o `@for` também pode usar um `@else` para o estado vazio:

```html
@for(var curso : page.cursos())
    <p>${curso.nome()}</p>
@else
    <p>Nenhum curso encontrado.</p>
@endfor
```

Isso elimina um `@if` separado em muitos casos.

## 8. Criando um componente reutilizável

Quando repetimos o mesmo HTML em várias páginas, podemos criar outro template e chamá-lo como componente.

Crie:

```text
src/main/jte/layout/cursoCard.jte
```

Conteúdo:

```html
@import com.exemplo.cursos.viewmodel.CursoView

@param CursoView curso

<article class="curso-card">
    <h2>${curso.nome()}</h2>

    <p>
        Categoria:
        <strong>${curso.categoria()}</strong>
    </p>

    <p>
        R$ ${String.format("%.2f", curso.preco())}
    </p>

    @if(curso.destaque())
        <span class="badge">Destaque</span>
    @endif
</article>
```

No template principal:

```html
@for(var curso : page.cursos())
    @template.layout.cursoCard(curso = curso)
@endfor
```

O caminho:

```text
layout/cursoCard.jte
```

vira a chamada:

```text
@template.layout.cursoCard(...)
```

Isso permite criar uma biblioteca de componentes server-side.

Por exemplo:

```text
layout/
├── button.jte
├── alert.jte
├── navbar.jte
├── pagination.jte
├── cursoCard.jte
└── page.jte
```

## 9. Parâmetros nomeados

O JTE permite passar parâmetros pelo nome.

Exemplo:

```html
@template.layout.cursoCard(
    curso = curso
)
```

Isso melhora a leitura quando um componente recebe vários argumentos.

Imagine:

```html
@template.layout.alert(
    titulo = "Atenção",
    mensagem = "O curso está indisponível.",
    tipo = "warning"
)
```

A chamada fica mais clara do que depender apenas da posição de cada parâmetro.

## 10. Parâmetros com valor padrão

Também é possível declarar valores padrão.

Exemplo de componente:

```html
@param String texto
@param String tipo = "primary"

<button class="btn btn-${tipo}">
    ${texto}
</button>
```

Agora podemos chamar:

```html
@template.layout.button(
    texto = "Comprar"
)
```

Ou sobrescrever:

```html
@template.layout.button(
    texto = "Excluir",
    tipo = "danger"
)
```

Esse recurso ajuda a criar componentes reutilizáveis sem multiplicar versões quase iguais do mesmo template.

## 11. Criando um layout compartilhado

Uma aplicação real costuma repetir:

- `<!DOCTYPE html>`;
- `<html>`;
- `<head>`;
- menu;
- rodapé;
- arquivos CSS;
- metadados.

Não queremos copiar essa estrutura em todas as páginas.

Para isso, podemos usar `gg.jte.Content`.

Crie:

```text
src/main/jte/layout/page.jte
```

Conteúdo:

```html
@import gg.jte.Content

@param String title
@param Content content

<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta
        name="viewport"
        content="width=device-width, initial-scale=1.0"
    >
    <title>${title}</title>
</head>

<body>

<header>
    <nav>
        <a href="/">Início</a>
        <a href="/cursos">Cursos</a>
    </nav>
</header>

<main>
    ${content}
</main>

<footer>
    <p>Projeto de exemplo com Spring Boot e JTE.</p>
</footer>

</body>
</html>
```

Agora o template `cursos.jte` pode utilizar esse layout:

```html
@import com.exemplo.cursos.viewmodel.CursosPage

@param CursosPage page

@template.layout.page(
    title = page.titulo(),
    content = @`
        <h1>${page.titulo()}</h1>

        <p>Bem-vindo, ${page.usuario()}.</p>

        <section class="cursos">
            @for(var curso : page.cursos())
                @template.layout.cursoCard(
                    curso = curso
                )
            @else
                <p>Nenhum curso encontrado.</p>
            @endfor
        </section>
    `
)
```

O trecho:

```text
@`
...
`
```

representa um bloco de conteúdo que será passado para o layout.

Com isso, cada página define apenas seu conteúdo específico.

## 12. Por que layouts e componentes importam

Sem composição, uma aplicação cresce assim:

```text
pagina1.jte -> HTML completo
pagina2.jte -> HTML completo copiado
pagina3.jte -> HTML completo copiado
pagina4.jte -> HTML completo copiado
```

Depois de alguns meses, alterar o menu significa modificar vários arquivos.

Com layouts:

```text
layout/page.jte
       ↑
       ├── cursos.jte
       ├── usuarios.jte
       ├── pedidos.jte
       └── dashboard.jte
```

Agora uma mudança no cabeçalho pode ser feita em um único lugar.

O mesmo princípio vale para componentes menores.

## 13. Escaping HTML e segurança

Um motor de templates precisa tratar cuidadosamente conteúdo vindo do usuário.

Imagine:

```java
String nome = "<script>alert('xss')</script>";
```

Se esse valor for inserido diretamente em HTML sem escaping, a página pode ficar vulnerável a **Cross-Site Scripting (XSS)**.

O JTE foi projetado com escaping HTML sensível ao contexto.

Quando utilizamos:

```html
${page.usuario()}
```

o valor não deve ser interpretado simplesmente como HTML arbitrário.

Isso é uma vantagem importante.

Contudo, escaping de template não substitui uma estratégia completa de segurança.

Continue aplicando:

- validação de entrada;
- Spring Security;
- proteção CSRF quando aplicável;
- Content Security Policy;
- cookies seguros;
- controle de autorização;
- tratamento adequado de URLs;
- sanitização quando HTML fornecido pelo usuário realmente precisa ser permitido.

Segurança deve existir em várias camadas.

## 14. Evite colocar regra de negócio no template

Como o JTE permite expressões Java, existe uma tentação:

> Se posso escrever Java no template, posso colocar qualquer lógica aqui.

Tecnicamente, muita coisa é possível.

Arquiteturalmente, isso não significa que seja uma boa ideia.

Evite:

```text
template
  ↓
calcula desconto
  ↓
consulta permissão
  ↓
decide regra fiscal
  ↓
transforma entidade
```

Prefira:

```text
Service
   ↓
regra de negócio

Controller
   ↓
monta View Model

JTE
   ↓
renderiza a apresentação
```

O template pode decidir:

- mostrar ou esconder um botão;
- aplicar uma classe CSS;
- percorrer uma lista;
- mostrar estado vazio;
- exibir um texto alternativo.

Mas a regra central do negócio deve continuar no Java apropriado.

## 15. Um controller mais próximo de uma aplicação real

Vamos separar a obtenção dos cursos.

```java
@Service
public class CursoService {

    public List<CursoView> listar() {
        return List.of(
                new CursoView(
                        1L,
                        "Spring Boot do Zero",
                        "Java",
                        149.90,
                        true
                ),
                new CursoView(
                        2L,
                        "APIs REST com Java",
                        "Back-End",
                        129.90,
                        false
                )
        );
    }
}
```

Controller:

```java
@Controller
public class CursoController {

    private final CursoService cursoService;

    public CursoController(CursoService cursoService) {
        this.cursoService = cursoService;
    }

    @GetMapping("/cursos")
    public String cursos(Model model) {

        CursosPage page = new CursosPage(
                "Catálogo de Cursos",
                "Michel",
                cursoService.listar()
        );

        model.addAttribute("page", page);

        return "cursos";
    }
}
```

A responsabilidade fica mais clara:

```text
CursoService
    ↓
dados e regras

CursoController
    ↓
montagem da página

cursos.jte
    ↓
HTML
```

## 16. Desenvolvimento com hot reload

Durante o desenvolvimento, a documentação do JTE permite habilitar:

```properties
gg.jte.developmentMode=true
```

Nesse modo, o JTE acompanha alterações nos templates e pode recompilá-los.

É importante lembrar:

> O modo de desenvolvimento requer um JDK.

Isso ocorre porque os templates precisam ser compilados durante o desenvolvimento.

Uma configuração local pode ficar assim:

```properties
gg.jte.developmentMode=true
gg.jte.templateLocation=src/main/jte
gg.jte.templateSuffix=.jte
```

## 17. Produção deve usar templates pré-compilados

Para produção, a recomendação é diferente.

Os templates podem ser pré-compilados durante o build.

A propriedade utilizada pelo starter é:

```properties
gg.jte.usePrecompiledTemplates=true
```

Nesse cenário:

```text
build
  ↓
templates JTE compilados
  ↓
artefato da aplicação
  ↓
produção carrega templates prontos
```

Isso traz vantagens:

- reduz trabalho durante a inicialização;
- evita compilar templates em produção;
- permite executar com JRE sem depender da compilação dinâmica;
- antecipa erros de template para o pipeline de build.

O JTE possui plugins oficiais para Maven e Gradle destinados a essa etapa.

A própria documentação recomenda manter a versão do plugin alinhada com a versão da dependência JTE.

Por exemplo:

```xml
<properties>
    <jte.version>3.2.4</jte.version>
</properties>
```

E reutilizar:

```xml
<version>${jte.version}</version>
```

tanto nas dependências quanto no plugin.

## 18. Não ative desenvolvimento e pré-compilado ao mesmo tempo

A configuração deve representar claramente o ambiente.

Desenvolvimento:

```properties
gg.jte.developmentMode=true
gg.jte.usePrecompiledTemplates=false
```

Produção:

```properties
gg.jte.developmentMode=false
gg.jte.usePrecompiledTemplates=true
```

Não faz sentido pedir simultaneamente:

- recompilação dinâmica;
- somente templates pré-compilados.

Uma boa estratégia é utilizar profiles.

### application-dev.properties

```properties
gg.jte.developmentMode=true
gg.jte.usePrecompiledTemplates=false
```

### application-prod.properties

```properties
gg.jte.developmentMode=false
gg.jte.usePrecompiledTemplates=true
```

Assim o comportamento muda junto com o ambiente.

## 19. Spring Boot 3 ou Spring Boot 4?

Hoje o JTE possui starters separados.

| Versão do Spring Boot | Starter |
|---|---|
| Spring Boot 3.x | `jte-spring-boot-starter-3` |
| Spring Boot 4.x | `jte-spring-boot-starter-4` |

No Boot 4, o starter atual também oferece uma variante `-core` para cenários em que você quer a configuração do `TemplateEngine`, mas não precisa registrar o `ViewResolver` MVC.

Isso pode ser útil quando a aplicação usa o motor de templates para gerar:

- e-mails HTML;
- arquivos;
- relatórios;
- fragmentos;
- conteúdo fora do fluxo tradicional de views MVC.

## 20. JTE não serve apenas para páginas web

Embora o exemplo seja uma aplicação MVC, um template engine também pode ser útil para produzir outros conteúdos.

Exemplos:

```text
Template JTE
   ↓
HTML de e-mail

Template JTE
   ↓
documento HTML

Template JTE
   ↓
fragmento para HTMX

Template JTE
   ↓
página SSR

Template JTE
   ↓
conteúdo textual estruturado
```

Quando usamos apenas o `TemplateEngine`, não precisamos necessariamente retornar uma view por um controller.

## 21. JTE versus Thymeleaf

Uma dúvida natural é:

> O JTE substitui o Thymeleaf?

A melhor resposta é: depende do projeto.

### JTE

O JTE tende a agradar equipes que valorizam:

- templates tipados;
- sintaxe próxima de Java/Kotlin;
- compilação;
- alta performance;
- componentes simples;
- feedback de erro mais cedo;
- menor dependência de uma linguagem de expressão própria.

### Thymeleaf

O Thymeleaf possui um ecossistema muito maduro e uma filosofia de templates HTML naturais.

Ele pode ser interessante quando:

- a equipe já domina Thymeleaf;
- o projeto possui muitos templates existentes;
- integrações específicas do ecossistema são importantes;
- designers precisam trabalhar diretamente com HTML que continua visualmente próximo de um documento normal.

Não existe obrigação de migrar uma aplicação saudável apenas porque surgiu outra opção.

A decisão deve considerar:

- manutenção;
- experiência da equipe;
- desempenho;
- quantidade de páginas;
- necessidade de tipagem;
- integração com o projeto atual.

## 22. Um ponto forte: refatoração orientada por tipos

Considere:

```java
public record CursoView(
        String nome,
        double preco
) {
}
```

E o template:

```html
<h2>${curso.nome()}</h2>
```

Se o método mudar para:

```java
titulo()
```

o template precisa ser atualizado.

Como existe um contrato tipado, ferramentas de desenvolvimento e a compilação conseguem ajudar a encontrar o problema.

Em aplicações grandes, isso é relevante porque páginas deixam de ser apenas arquivos de texto interpretados sem relação explícita com o modelo Java.

## 23. Plugins da IDE

O projeto JTE disponibiliza suporte para IntelliJ IDEA.

Esse tipo de integração melhora a experiência com:

- autocomplete;
- navegação;
- refatoração;
- destaque de sintaxe;
- reconhecimento de parâmetros;
- código Java dentro dos templates.

Quanto mais o projeto depende de templates tipados, mais valioso se torna ter uma IDE que entenda essa relação.

## 24. Testando o controller

Podemos testar se o controller retorna a view esperada e cria o modelo.

Exemplo conceitual com Spring MVC Test:

```java
@WebMvcTest(CursoController.class)
class CursoControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private CursoService cursoService;

    @Test
    void deveExibirPaginaDeCursos() throws Exception {

        when(cursoService.listar())
                .thenReturn(List.of());

        mockMvc.perform(get("/cursos"))
                .andExpect(status().isOk())
                .andExpect(view().name("cursos"))
                .andExpect(model().attributeExists("page"));
    }
}
```

Podemos também adicionar um teste de integração que valide trechos do HTML renderizado.

Isso ajuda a detectar:

- template inexistente;
- parâmetros incompatíveis;
- layout quebrado;
- alteração inesperada na página.

## 25. Uma estrutura recomendada para projetos maiores

Conforme o sistema cresce, uma estrutura possível é:

```text
src/main/jte/
├── layout/
│   ├── page.jte
│   ├── navbar.jte
│   └── footer.jte
│
├── components/
│   ├── alert.jte
│   ├── button.jte
│   ├── pagination.jte
│   └── modal.jte
│
├── cursos/
│   ├── index.jte
│   ├── detalhes.jte
│   └── form.jte
│
├── usuarios/
│   ├── index.jte
│   └── perfil.jte
│
└── dashboard/
    └── index.jte
```

O controller pode retornar:

```java
return "cursos/index";
```

Isso mantém as páginas agrupadas por domínio.

## 26. Erros comuns ao começar com JTE

### Colocar templates no diretório errado

Na configuração padrão atual:

```text
src/main/jte
```

e não necessariamente:

```text
src/main/resources/templates
```

### Retornar o nome com extensão sem necessidade

Prefira:

```java
return "cursos";
```

em vez de acoplar o controller à extensão:

```java
return "cursos.jte";
```

### Esquecer o @param

Se o template depende de um objeto, declare explicitamente:

```html
@param CursosPage page
```

### Passar atributo com nome diferente

Controller:

```java
model.addAttribute("page", page);
```

Template:

```html
@param CursosPage page
```

Os nomes precisam representar o mesmo contrato.

### Misturar regra de negócio e apresentação

Não transforme o template em uma segunda camada de serviço.

### Usar developmentMode em produção

Produção deve preferir templates pré-compilados.

### Não alinhar versões

Quando usar plugin Maven ou Gradle do JTE, mantenha a versão alinhada com a biblioteca.

## 27. Mini projeto

Como exercício, transforme nosso catálogo em uma pequena aplicação.

Implemente:

1. página de listagem;
2. página de detalhes;
3. componente de card;
4. componente de botão;
5. layout principal;
6. estado vazio;
7. destaque visual para cursos promocionais;
8. View Model próprio para cada página;
9. service separado do controller;
10. profile de desenvolvimento e produção.

Depois crie:

```text
GET /cursos
GET /cursos/{id}
```

Na página de detalhes, mostre:

- nome;
- categoria;
- descrição;
- preço;
- status;
- botão de matrícula.

## 28. Desafio: adicionar paginação

Crie:

```java
public record CursosPage(
        String titulo,
        List<CursoView> cursos,
        int paginaAtual,
        int totalPaginas
) {
}
```

Depois, no JTE:

```html
<nav>
    @for(int pagina = 1; pagina <= page.totalPaginas(); pagina++)
        <a href="/cursos?page=${pagina}">
            ${pagina}
        </a>
    @endfor
</nav>
```

Em seguida, transforme essa navegação em:

```text
components/pagination.jte
```

O desafio ensina três conceitos ao mesmo tempo:

- loops;
- parâmetros;
- componentes.

## 29. Desafio avançado: HTMX

JTE combina naturalmente com aplicações server-side e também pode ser usado em arquiteturas HTML-over-the-wire.

Uma evolução interessante é adicionar **HTMX**.

Nesse modelo:

```text
Browser
   ↓
requisição HTTP
   ↓
Spring MVC
   ↓
JTE
   ↓
fragmento HTML
   ↓
HTMX substitui parte da página
```

Isso permite construir interfaces interativas sem transformar obrigatoriamente toda a aplicação em uma SPA.

Um próximo artigo pode explorar justamente:

**Spring Boot + JTE + HTMX**.

## 30. Quando JTE faz sentido

JTE merece consideração quando você deseja:

- renderização server-side;
- páginas HTML rápidas;
- contratos tipados entre Java e template;
- poucas abstrações entre template e linguagem;
- componentes simples;
- compilação antecipada;
- boa integração com Spring Boot.

Ele pode ser especialmente interessante em:

- sistemas administrativos;
- dashboards;
- SaaS server-side;
- portais;
- intranets;
- CRUDs;
- aplicações que usam HTMX;
- páginas que não precisam de uma SPA completa.

## 31. Quando talvez não seja a melhor escolha

Considere outra arquitetura quando:

- a aplicação é essencialmente uma SPA rica;
- o front-end possui uma equipe separada totalmente baseada em React, Angular ou Vue;
- o projeto já possui grande investimento em outro template engine;
- a equipe depende de integrações específicas inexistentes no JTE;
- a interface exige grande quantidade de estado no cliente.

Tecnologia deve resolver o problema do projeto, não apenas seguir novidade.

## 32. O que aprendemos

Nesta segunda parte, evoluímos de um template simples para uma estrutura mais próxima de uma aplicação real.

Aprendemos:

- `@param` para modelos tipados;
- `@if`, `@elseif` e `@else`;
- `@for` e estado vazio;
- chamadas com `@template`;
- parâmetros nomeados;
- parâmetros com valores padrão;
- `gg.jte.Content`;
- layouts compartilhados;
- View Models;
- escaping HTML;
- separação entre regra de negócio e apresentação;
- development mode;
- templates pré-compilados;
- diferenças entre os starters do Spring Boot 3 e 4.

O ponto principal é perceber que **JTE não é apenas uma forma de colocar valores dentro de HTML**.

Ele permite tratar templates como parte tipada da aplicação Java.

## 33. Próxima parte

Na próxima etapa, podemos avançar para uma aplicação mais interativa com:

**Spring Boot + JTE + HTMX: criando páginas dinâmicas sem SPA**.

Podemos implementar:

- busca sem recarregar a página inteira;
- filtros;
- paginação;
- formulários;
- validação;
- fragments JTE;
- loading states;
- modais;
- atualização parcial do HTML.

Essa combinação mantém grande parte da lógica no servidor e usa JavaScript apenas quando realmente necessário.

## Perguntas para revisão

1. Onde os templates JTE ficam por padrão?
2. Para que serve `@param`?
3. Qual vantagem existe em usar View Models?
4. Como funciona `@for`?
5. Como criar um componente reutilizável?
6. Para que serve `gg.jte.Content`?
7. Qual a diferença entre development mode e templates pré-compilados?
8. Por que não devemos colocar regra de negócio no template?
9. Qual starter usar com Spring Boot 3?
10. Qual starter usar com Spring Boot 4?
11. Como layouts reduzem duplicação?
12. Por que a tipagem ajuda em refatorações?

## Resumo da lição

O **JTE com Spring Boot** oferece uma abordagem moderna para renderização server-side em Java. Templates tipados, componentes, layouts e pré-compilação permitem construir aplicações HTML organizadas sem abandonar o ecossistema Spring.

Depois do Quick Start, o próximo passo é estruturar o código para que controllers forneçam View Models claros e templates cuidem apenas da apresentação.

Essa separação cria uma base simples para evoluir o sistema para páginas maiores, componentes reutilizáveis e interfaces dinâmicas.

## Referências e materiais complementares

- [Documentação oficial do JTE](https://jte.gg/)
- [Sintaxe dos templates JTE](https://jte.gg/syntax/)
- [JTE Spring Boot Starter 4](https://jte.gg/spring-boot-starter-4/)
- [JTE Spring Boot Starter 3](https://jte.gg/spring-boot-starter-3/)
- [Pré-compilação de templates JTE](https://jte.gg/pre-compiling/)
- [Repositório oficial do JTE no GitHub](https://github.com/casid/jte)
- [Spring Initializr](https://start.spring.io/)
