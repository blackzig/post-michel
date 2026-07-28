---
title: "Factory Method em Java: aprenda o padrão com exemplos práticos"
seo_title: "Factory Method em Java: guia completo com exemplos"
slug: "factory-method-java"
meta_description: "Aprenda Factory Method em Java, entenda sua estrutura, vantagens, erros comuns, testes e diferenças para Simple Factory e Abstract Factory."
focus_keyphrase: "Factory Method em Java"
related_keywords:
  - "padrão Factory Method"
  - "Design Patterns em Java"
  - "padrões de criação"
  - "fábrica de objetos Java"
  - "Simple Factory"
  - "Abstract Factory"
category: "Programação"
tags:
  - "Design Patterns"
  - "Java"
  - "Factory Method"
  - "Padrões de Criação"
  - "Orientação a Objetos"
  - "SOLID"
  - "Arquitetura de Software"
excerpt: "Aprenda a aplicar Factory Method em Java para criar objetos com baixo acoplamento, extensibilidade e testes mais simples."
featured_image_url: "https://upload.wikimedia.org/wikipedia/en/3/30/Java_programming_language_logo.svg"
featured_image_alt: "Factory Method em Java representado pelo logotipo da linguagem Java"
featured_image_title: "Factory Method em Java"
featured_image_caption: "Logotipo da linguagem Java, utilizado para representar um exemplo prático do padrão Factory Method. Java é uma marca registrada da Oracle."
---

# Factory Method em Java: aprenda o padrão com exemplos práticos

O **Factory Method em Java** resolve um problema frequente em sistemas orientados a objetos: como criar instâncias sem espalhar dependências de classes concretas por toda a aplicação?

Em um código pequeno, escrever `new EmailNotification()` diretamente pode parecer suficiente. Entretanto, o cenário muda quando a aplicação precisa escolher entre e-mail, SMS, notificação push ou outro canal. Se cada serviço conhecer todas essas implementações, o código passa a acumular condicionais, dependências e responsabilidades.

O padrão Factory Method transfere a decisão de criação para um método especializado. Dessa forma, o fluxo principal trabalha com abstrações, enquanto subclasses ou implementações específicas decidem qual objeto concreto produzir.

Nesta segunda lição da trilha de Design Patterns, você aprenderá:

- o problema que o Factory Method resolve;
- os participantes do padrão;
- uma implementação completa em Java;
- como adicionar novos produtos sem alterar o fluxo principal;
- como testar o código;
- diferenças entre Factory Method, Simple Factory e Abstract Factory;
- situações nas quais o padrão ajuda ou apenas aumenta a complexidade.

## O que é o padrão Factory Method?

O Factory Method pertence ao grupo dos **padrões de criação**. Esses padrões organizam a construção de objetos e reduzem o acoplamento entre quem solicita uma instância e quem conhece sua classe concreta.

A ideia central pode ser resumida assim:

> Uma classe define o fluxo de uma operação, mas delega a criação de um objeto para um método que subclasses podem sobrescrever.

Portanto, o padrão não significa apenas criar uma classe com o nome `Factory`. O elemento decisivo é o uso de **polimorfismo** para variar o produto criado.

Considere um serviço responsável por enviar notificações. O algoritmo geral pode seguir estas etapas:

1. validar o destinatário;
2. criar a notificação apropriada;
3. registrar o início do envio;
4. enviar a mensagem;
5. registrar o resultado.

O fluxo permanece igual para e-mail, SMS ou push. Contudo, cada canal usa uma implementação diferente. O Factory Method preserva o algoritmo comum e permite que cada criador concreto escolha seu produto.

## O problema do acoplamento direto

Observe uma implementação inicial:

```java
public class NotificationService {

    public void send(String channel, String recipient, String message) {
        if ("email".equalsIgnoreCase(channel)) {
            EmailNotification notification = new EmailNotification();
            notification.send(recipient, message);
        } else if ("sms".equalsIgnoreCase(channel)) {
            SmsNotification notification = new SmsNotification();
            notification.send(recipient, message);
        } else {
            throw new IllegalArgumentException("Canal não suportado: " + channel);
        }
    }
}
```

Esse código funciona. Porém, ele apresenta alguns problemas:

- `NotificationService` conhece todas as classes concretas;
- cada novo canal exige a alteração do método;
- a condição cresce conforme o sistema recebe novas opções;
- a classe mistura orquestração com criação de objetos;
- os testes precisam considerar várias ramificações no mesmo componente.

Além disso, a classe viola a intenção do **Princípio Aberto/Fechado**, do SOLID. O sistema deveria permitir extensão sem exigir mudanças constantes no fluxo já validado.

O **Factory Method em Java** separa essas responsabilidades.

## Participantes do Factory Method

A estrutura clássica contém quatro papéis principais.

| Participante | Responsabilidade |
|---|---|
| **Product** | Define o contrato dos objetos produzidos |
| **Concrete Product** | Implementa o produto para um caso específico |
| **Creator** | Declara o factory method e contém o fluxo comum |
| **Concrete Creator** | Sobrescreve o factory method e escolhe o produto concreto |

No exemplo de notificações, teremos:

```text
Notification                  -> Product
EmailNotification             -> Concrete Product
SmsNotification               -> Concrete Product
NotificationService           -> Creator
EmailNotificationService      -> Concrete Creator
SmsNotificationService        -> Concrete Creator
```

A classe criadora não precisa limitar seu trabalho à fabricação do objeto. Pelo contrário, ela normalmente contém uma operação de negócio que utiliza o produto criado.

## Implementação do Product

Primeiramente, definimos o contrato das notificações:

```java
public interface Notification {

    void send(String recipient, String message);
}
```

A interface representa aquilo que o fluxo principal precisa. Como resultado, o criador não precisa conhecer detalhes de SMTP, gateway de SMS ou API de push.

Agora, criamos os produtos concretos.

```java
public final class EmailNotification implements Notification {

    @Override
    public void send(String recipient, String message) {
        System.out.printf(
                "Enviando e-mail para %s: %s%n",
                recipient,
                message
        );
    }
}
```

```java
public final class SmsNotification implements Notification {

    @Override
    public void send(String recipient, String message) {
        System.out.printf(
                "Enviando SMS para %s: %s%n",
                recipient,
                message
        );
    }
}
```

As duas classes implementam o mesmo contrato. Entretanto, cada uma pode integrar um provedor diferente, aplicar validações próprias ou transformar a mensagem antes do envio.

## Implementação do Creator

Em seguida, criamos a classe abstrata que define o método fábrica:

```java
import java.util.Objects;

public abstract class NotificationService {

    protected abstract Notification createNotification();

    public final void notify(
            String recipient,
            String message
    ) {
        validate(recipient, message);

        Notification notification = createNotification();

        System.out.println("Iniciando envio...");
        notification.send(recipient, message);
        System.out.println("Envio concluído.");
    }

    private void validate(String recipient, String message) {
        Objects.requireNonNull(recipient, "Destinatário obrigatório");
        Objects.requireNonNull(message, "Mensagem obrigatória");

        if (recipient.isBlank()) {
            throw new IllegalArgumentException(
                    "Destinatário não pode ficar vazio"
            );
        }

        if (message.isBlank()) {
            throw new IllegalArgumentException(
                    "Mensagem não pode ficar vazia"
            );
        }
    }
}
```

O método `createNotification()` representa o Factory Method. A classe base declara o contrato de criação, mas não escolhe a classe concreta.

Por outro lado, o método `notify()` contém o algoritmo comum. Ele valida os dados, solicita um produto, executa o envio e registra o andamento.

Além disso, o modificador `final` impede que subclasses substituam todo o fluxo e eliminem etapas importantes. As subclasses podem variar a criação, mas não podem ignorar as validações definidas pelo criador.

Essa combinação aproxima o Factory Method do **Template Method**. O algoritmo principal forma um modelo, enquanto um ponto específico permite variação. Contudo, cada padrão possui uma intenção diferente: Factory Method concentra-se na criação; Template Method concentra-se na variação de etapas de um algoritmo.

## Implementação dos Concrete Creators

Agora, cada criador concreto escolhe um produto:

```java
public final class EmailNotificationService
        extends NotificationService {

    @Override
    protected Notification createNotification() {
        return new EmailNotification();
    }
}
```

```java
public final class SmsNotificationService
        extends NotificationService {

    @Override
    protected Notification createNotification() {
        return new SmsNotification();
    }
}
```

Por fim, usamos os serviços:

```java
public class Main {

    public static void main(String[] args) {
        NotificationService emailService =
                new EmailNotificationService();

        NotificationService smsService =
                new SmsNotificationService();

        emailService.notify(
                "aluno@example.com",
                "Sua matrícula foi confirmada."
        );

        smsService.notify(
                "+5511999999999",
                "Seu código de acesso é 4821."
        );
    }
}
```

O cliente ainda escolhe qual criador utilizar. No entanto, ele trabalha com o tipo abstrato `NotificationService`, e o fluxo principal não conhece os produtos concretos.

## Como adicionar notificação push

Uma das principais vantagens do **Factory Method em Java** aparece quando o sistema precisa crescer.

Primeiro, criamos outro produto:

```java
public final class PushNotification implements Notification {

    @Override
    public void send(String recipient, String message) {
        System.out.printf(
                "Enviando push ao dispositivo %s: %s%n",
                recipient,
                message
        );
    }
}
```

Depois, criamos o respectivo criador:

```java
public final class PushNotificationService
        extends NotificationService {

    @Override
    protected Notification createNotification() {
        return new PushNotification();
    }
}
```

Não alteramos `NotificationService`, `EmailNotificationService` ou `SmsNotificationService`. Assim, o novo comportamento entra por extensão.

Esse resultado demonstra o Princípio Aberto/Fechado de maneira prática. O código continua aberto para novos canais, mas permanece fechado para alterações desnecessárias no algoritmo central.

## Factory Method com dependências reais

Em sistemas reais, um produto normalmente depende de outras classes. Por exemplo, uma notificação por e-mail pode precisar de um cliente SMTP e de configurações.

```java
public interface EmailClient {

    void deliver(String recipient, String message);
}
```

```java
public final class SmtpEmailNotification
        implements Notification {

    private final EmailClient emailClient;

    public SmtpEmailNotification(EmailClient emailClient) {
        this.emailClient = emailClient;
    }

    @Override
    public void send(String recipient, String message) {
        emailClient.deliver(recipient, message);
    }
}
```

O criador pode receber a dependência por construtor:

```java
public final class SmtpNotificationService
        extends NotificationService {

    private final EmailClient emailClient;

    public SmtpNotificationService(EmailClient emailClient) {
        this.emailClient = emailClient;
    }

    @Override
    protected Notification createNotification() {
        return new SmtpEmailNotification(emailClient);
    }
}
```

Dessa maneira, a fábrica não precisa criar todas as dependências com `new`. Um contêiner de injeção, como Spring, CDI ou Guice, pode fornecer o `EmailClient` ao criador.

Portanto, Factory Method e injeção de dependências não competem. Em muitos projetos, eles trabalham juntos.

## Como testar o Factory Method

O padrão facilita testes quando o fluxo principal depende apenas da interface do produto.

Podemos criar um produto espião para verificar a chamada:

```java
public final class NotificationSpy
        implements Notification {

    private boolean sent;
    private String recipient;
    private String message;

    @Override
    public void send(String recipient, String message) {
        this.sent = true;
        this.recipient = recipient;
        this.message = message;
    }

    public boolean wasSent() {
        return sent;
    }

    public String recipient() {
        return recipient;
    }

    public String message() {
        return message;
    }
}
```

Em seguida, criamos um criador específico para o teste:

```java
public final class TestNotificationService
        extends NotificationService {

    private final Notification notification;

    public TestNotificationService(Notification notification) {
        this.notification = notification;
    }

    @Override
    protected Notification createNotification() {
        return notification;
    }
}
```

Com JUnit 5, o teste pode ficar assim:

```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertTrue;

import org.junit.jupiter.api.Test;

class NotificationServiceTest {

    @Test
    void shouldSendNotificationCreatedByFactoryMethod() {
        NotificationSpy spy = new NotificationSpy();
        NotificationService service =
                new TestNotificationService(spy);

        service.notify(
                "cliente@example.com",
                "Pedido aprovado"
        );

        assertTrue(spy.wasSent());
        assertEquals(
                "cliente@example.com",
                spy.recipient()
        );
        assertEquals("Pedido aprovado", spy.message());
    }
}
```

Esse teste verifica o fluxo sem enviar e-mail ou SMS de verdade. Além disso, ele não depende de bibliotecas de mock.

Também devemos testar as validações:

```java
import static org.junit.jupiter.api.Assertions.assertThrows;

import org.junit.jupiter.api.Test;

class NotificationValidationTest {

    @Test
    void shouldRejectBlankRecipient() {
        NotificationService service =
                new TestNotificationService(
                        new NotificationSpy()
                );

        assertThrows(
                IllegalArgumentException.class,
                () -> service.notify(" ", "Mensagem")
        );
    }
}
```

## Factory Method não é apenas uma fábrica estática

Muitos projetos chamam qualquer método que cria objetos de Factory Method. Entretanto, existe uma diferença importante entre o padrão GoF e uma fábrica simples.

Considere este exemplo:

```java
public final class NotificationFactory {

    private NotificationFactory() {
    }

    public static Notification create(String channel) {
        return switch (channel.toLowerCase()) {
            case "email" -> new EmailNotification();
            case "sms" -> new SmsNotification();
            case "push" -> new PushNotification();
            default -> throw new IllegalArgumentException(
                    "Canal inválido: " + channel
            );
        };
    }
}
```

Essa classe centraliza a construção e pode melhorar o código. Contudo, ela representa uma **Simple Factory**, não o Factory Method clássico.

A Simple Factory usa uma decisão central, normalmente com `if` ou `switch`. Já o Factory Method usa herança ou polimorfismo para delegar a criação.

Nenhuma opção vence em todos os cenários. Uma Simple Factory pode resolver perfeitamente um problema pequeno e com poucas variações. Por outro lado, o Factory Method oferece mais flexibilidade quando cada produto acompanha um fluxo especializado ou quando novas extensões surgem com frequência.

## Factory Method com `Supplier`

O Java também permite tratar construtores como funções por meio da interface `Supplier<T>`.

```java
import java.util.function.Supplier;

public final class ConfigurableNotificationService {

    private final Supplier<Notification> factory;

    public ConfigurableNotificationService(
            Supplier<Notification> factory
    ) {
        this.factory = factory;
    }

    public void notify(String recipient, String message) {
        Notification notification = factory.get();
        notification.send(recipient, message);
    }
}
```

O cliente pode passar referências de construtor:

```java
ConfigurableNotificationService emailService =
        new ConfigurableNotificationService(
                EmailNotification::new
        );

ConfigurableNotificationService smsService =
        new ConfigurableNotificationService(
                SmsNotification::new
        );
```

Essa abordagem usa composição em vez de herança. Portanto, ela reduz a quantidade de subclasses e permite trocar a estratégia de criação em tempo de execução.

Embora o formato não reproduza exatamente a estrutura clássica do GoF, ele preserva a intenção principal: separar o uso do objeto da decisão sobre sua construção.

Em Java moderno, vale comparar as duas soluções antes de criar muitas subclasses.

## Factory Method, Abstract Factory e Builder

Os padrões de criação podem parecer semelhantes. Entretanto, cada um resolve um problema distinto.

### Factory Method

O Factory Method delega a criação de um produto para subclasses ou implementações especializadas. Ele funciona bem quando uma operação comum precisa variar o tipo do objeto utilizado.

### Abstract Factory

A Abstract Factory cria **famílias de objetos relacionados**. Por exemplo, uma fábrica de interface gráfica pode produzir botão, campo de texto e janela para Windows; outra fábrica pode produzir os mesmos componentes para Linux.

Enquanto o Factory Method costuma tratar um ponto de criação, a Abstract Factory coordena vários produtos compatíveis.

### Builder

O Builder organiza a construção de objetos complexos em etapas. Ele ajuda quando uma classe possui muitos parâmetros, combinações opcionais ou regras de montagem.

Portanto:

- use Factory Method para variar o produto criado dentro de um fluxo;
- use Abstract Factory para produzir famílias de produtos compatíveis;
- use Builder para montar um objeto complexo passo a passo.

## Relação com os princípios SOLID

O **Factory Method em Java** pode apoiar vários princípios do SOLID.

### Princípio da Responsabilidade Única

O criador coordena o fluxo, enquanto o produto executa uma operação específica. Além disso, criadores concretos encapsulam a escolha da implementação.

### Princípio Aberto/Fechado

Novos produtos entram por meio de novas classes. Assim, o algoritmo central não precisa mudar a cada extensão.

### Princípio da Substituição de Liskov

Todo produto concreto deve respeitar o contrato definido por `Notification`. Da mesma forma, cada criador concreto deve funcionar onde o tipo abstrato `NotificationService` aparece.

### Princípio da Segregação de Interfaces

A interface `Notification` deve conter apenas operações necessárias aos clientes. Uma interface excessivamente ampla força produtos a implementar métodos que não utilizam.

### Princípio da Inversão de Dependência

O fluxo principal depende da abstração `Notification`, não de `EmailNotification` ou `SmsNotification`.

Entretanto, aplicar o padrão não garante automaticamente um bom design. As abstrações precisam representar contratos coerentes, e as subclasses precisam preservar o comportamento esperado.

## Erros comuns ao implementar Factory Method

### Criar subclasses sem variação real

Se todas as subclasses retornam o mesmo produto e não existe perspectiva de mudança, o padrão apenas adiciona arquivos e indireção.

### Manter um grande `switch` no criador abstrato

Quando o método fábrica contém todas as decisões concretas, a classe continua acoplada aos produtos. Nesse caso, você criou uma fábrica central, não uma extensão polimórfica.

### Expor o tipo concreto ao cliente

O cliente deve trabalhar com `Notification`, sempre que possível. Retornar `EmailNotification` elimina parte do benefício da abstração.

### Usar herança apenas para evitar `new`

A palavra-chave `new` não representa um problema por si mesma. O problema surge quando a decisão de criação muda, se repete ou acopla componentes que deveriam evoluir separadamente.

### Criar uma hierarquia profunda

Uma estrutura com muitos níveis de criadores pode dificultar a navegação. Prefira hierarquias rasas e considere composição com `Supplier`, funções ou injeção de dependências.

### Ignorar o ciclo de vida do produto

O factory method pode criar uma instância nova a cada chamada ou reutilizar uma instância existente. A decisão depende do estado, da segurança em concorrência e do custo de construção.

Produtos mutáveis não devem ser compartilhados sem uma análise cuidadosa. Por outro lado, produtos imutáveis ou sem estado podem permitir reutilização segura.

## Quando usar Factory Method

Considere o padrão quando:

- uma classe não consegue prever qual produto concreto utilizará;
- subclasses precisam escolher o objeto usado pelo algoritmo;
- novos produtos devem entrar sem alterações frequentes no código central;
- a criação inclui regras, dependências ou configurações relevantes;
- o código precisa substituir produtos em testes;
- frameworks precisam oferecer pontos de extensão para aplicações clientes.

Frameworks usam essa ideia com frequência. Eles fornecem um fluxo geral e permitem que a aplicação substitua um componente específico.

## Quando evitar o padrão

Evite o Factory Method quando:

- existe apenas uma implementação estável;
- a criação consiste em um `new` simples e local;
- uma função ou `Supplier` resolve o problema com menos estrutura;
- a hierarquia de classes não representa uma relação conceitual válida;
- a equipe teria dificuldade para compreender a indireção adicionada.

Design Patterns não servem como metas de quantidade. Um projeto não melhora porque contém mais padrões. Ele melhora quando suas decisões de design reduzem riscos concretos e tornam as mudanças mais seguras.

## Exercício prático

Crie um sistema de exportação de relatórios com os seguintes elementos:

1. uma interface `ReportExporter` com o método `export(String content)`;
2. produtos concretos para PDF, CSV e JSON;
3. uma classe abstrata `ReportService`;
4. um factory method chamado `createExporter()`;
5. um método final `generateReport()` que prepare o conteúdo e execute a exportação;
6. um criador concreto para cada formato;
7. testes que usem um `ReportExporterSpy`.

Depois, responda:

- qual classe conhece o produto concreto?
- qual classe contém o algoritmo comum?
- como você adicionaria XML sem alterar o criador abstrato?
- uma solução com `Supplier<ReportExporter>` ficaria mais simples?
- o produto deve gerar uma nova instância em cada chamada?

## Desafio avançado

Adapte o exemplo para receber uma configuração externa, como `EMAIL`, `SMS` ou `PUSH`, sem criar um grande `switch` dentro do fluxo de envio.

Uma alternativa consiste em registrar criadores em um mapa:

```java
Map<String, Supplier<NotificationService>> services = Map.of(
        "EMAIL", EmailNotificationService::new,
        "SMS", SmsNotificationService::new,
        "PUSH", PushNotificationService::new
);
```

Depois, selecione o criador na borda da aplicação:

```java
NotificationService service =
        services.getOrDefault(
                configuredChannel,
                () -> {
                    throw new IllegalArgumentException(
                            "Canal não configurado"
                    );
                }
        ).get();
```

Esse registro mantém a decisão de configuração fora do algoritmo de negócio. Além disso, ele permite combinar Factory Method, composição e funções de fábrica.

## Conclusão

O **Factory Method em Java** separa o fluxo de uso da decisão sobre qual objeto criar. O padrão usa abstrações e polimorfismo para reduzir o acoplamento com classes concretas.

A implementação clássica contém Product, Concrete Product, Creator e Concrete Creator. O criador define o algoritmo comum e chama um método fábrica. Em seguida, cada criador concreto escolhe o produto apropriado.

Contudo, o padrão não deve entrar automaticamente em todo ponto que contém `new`. Antes de aplicá-lo, avalie a frequência das mudanças, a quantidade de produtos, a complexidade da criação e a possibilidade de usar composição.

Quando o problema realmente exige extensibilidade, o Factory Method ajuda a manter o código aberto para novos comportamentos e protegido contra alterações repetidas no núcleo da aplicação.

## Perguntas para revisão

1. Qual problema o Factory Method resolve?
2. Qual diferença existe entre Product e Creator?
3. Por que uma fábrica estática com `switch` não representa necessariamente o padrão clássico?
4. Como o Factory Method apoia o Princípio Aberto/Fechado?
5. Quando `Supplier<T>` pode substituir uma hierarquia de criadores?
6. Qual diferença existe entre Factory Method e Abstract Factory?
7. Como testar o algoritmo do Creator sem executar uma integração real?
8. Por que o uso de `new` não constitui, sozinho, um problema de design?

## Resumo da lição

O Factory Method delega a criação de objetos a componentes especializados. Em Java, ele reduz o acoplamento, facilita extensões e melhora a testabilidade quando o sistema possui produtos variáveis.

## Próxima lição

Na próxima etapa, estudaremos o **Abstract Factory**, padrão que cria famílias de objetos relacionados e garante compatibilidade entre os produtos de cada família.

## Referências e materiais complementares

- Erich Gamma, Richard Helm, Ralph Johnson e John Vlissides. *Design Patterns: Elements of Reusable Object-Oriented Software*.
- Joshua Bloch. *Effective Java*.
- [Interfaces em Java — documentação oficial](https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html)
- [Expressões lambda em Java — documentação oficial](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)
- [Interface Supplier — Java API](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/function/Supplier.html)
- [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)
