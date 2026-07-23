---
title: "O que é AWS? Entenda como funciona a nuvem da Amazon"
seo_title: "O que é AWS? Como a nuvem da Amazon funciona"
slug: "o-que-e-aws"
meta_description: "Descubra o que é AWS, como funciona a nuvem da Amazon e entenda segurança compartilhada, escalabilidade, elasticidade, serviços e custos."
focus_keyphrase: "o que é AWS"
related_keywords:
  - "Amazon Web Services"
  - "computação em nuvem"
  - "serviços da AWS"
  - "segurança na AWS"
  - "escalabilidade na nuvem"
  - "AWS Free Tier"
category: "Computação em Nuvem"
tags:
  - "AWS"
  - "Amazon Web Services"
  - "Cloud Computing"
  - "Computação em Nuvem"
  - "Infraestrutura"
  - "DevOps"
excerpt: "Uma introdução completa à AWS, seus principais serviços, modelo de segurança, escalabilidade, elasticidade, alta disponibilidade e formas de cobrança."
---

# O que é AWS? Entenda como funciona a nuvem da Amazon

Se você está começando a estudar computação em nuvem, uma das primeiras perguntas provavelmente será: **o que é AWS?** A sigla significa **Amazon Web Services**, a plataforma de serviços de nuvem da Amazon. Por meio dela, pessoas e empresas podem usar servidores, armazenamento, bancos de dados, redes, ferramentas de segurança e muitos outros recursos pela internet.

Na infraestrutura tradicional, uma empresa compra servidores, prepara um espaço físico, instala equipamentos, configura redes e assume sua manutenção. Na AWS, boa parte desses recursos pode ser contratada conforme a necessidade. Em vez de adquirir toda a estrutura antecipadamente, o cliente provisiona serviços pela internet e paga de acordo com o modelo de cobrança escolhido.

Isso não significa apenas colocar arquivos em um servidor remoto. A computação em nuvem muda a maneira como os sistemas são planejados, disponibilizados, protegidos, ampliados e cobrados. Conceitos como responsabilidade compartilhada, escalabilidade, elasticidade e alta disponibilidade tornam-se essenciais.

Este artigo apresenta esses fundamentos e atualiza alguns pontos importantes da aula **“O que é AWS”**, da Cloud Treinamentos.

## O que é AWS em termos práticos?

A AWS é uma plataforma que oferece recursos de tecnologia da informação como serviços. Um desenvolvedor pode criar uma máquina virtual em poucos minutos, armazenar arquivos sem comprar discos físicos, disponibilizar um banco de dados gerenciado ou executar uma função sem administrar um servidor.

Pense em uma aplicação de vendas. Para mantê-la disponível, a empresa pode precisar de:

- servidores para executar o sistema;
- banco de dados para registrar clientes e pedidos;
- armazenamento para imagens e documentos;
- rede para conectar os componentes;
- controles de identidade e acesso;
- monitoramento e registros de execução;
- cópias de segurança;
- mecanismos para suportar aumentos de tráfego.

Todos esses recursos podem ser encontrados na AWS. Eles podem ser combinados para construir desde um pequeno ambiente de testes até plataformas distribuídas que atendem milhões de usuários.

A principal mudança é que a infraestrutura passa a ser tratada de maneira programável. Recursos podem ser criados pelo Console de Gerenciamento, pela linha de comando, por APIs ou por ferramentas de infraestrutura como código.

## Principais serviços da AWS

A quantidade de serviços disponíveis pode assustar quem está começando. Entretanto, não é necessário conhecer todo o catálogo para compreender a plataforma. Alguns serviços representam bem as categorias fundamentais:

| Serviço | Função principal | Exemplo de utilização |
|---|---|---|
| **Amazon EC2** | Máquinas virtuais, chamadas de instâncias | Executar uma aplicação Java ou um servidor web |
| **Amazon S3** | Armazenamento de objetos | Guardar imagens, documentos, backups e arquivos estáticos |
| **Amazon RDS** | Bancos de dados relacionais gerenciados | Utilizar PostgreSQL, MySQL, MariaDB, Oracle ou SQL Server |
| **AWS Lambda** | Execução de código sem administrar servidores | Processar eventos, arquivos e integrações |
| **Amazon VPC** | Rede virtual isolada | Organizar sub-redes, rotas e conexões entre recursos |
| **AWS IAM** | Identidades e permissões | Definir quem pode acessar cada serviço |
| **Amazon CloudWatch** | Métricas, logs e alarmes | Monitorar aplicações e identificar falhas |
| **Elastic Load Balancing** | Distribuição de tráfego | Direcionar requisições para diferentes instâncias |

Esses nomes aparecem com frequência porque representam necessidades comuns: computação, armazenamento, banco de dados, rede, segurança e observabilidade.

## IaaS, PaaS e SaaS

A aula apresenta três modelos importantes de computação em nuvem. Eles ajudam a compreender quanto da infraestrutura fica sob controle do cliente e quanto é administrado pelo provedor.

### IaaS: infraestrutura como serviço

Na **Infrastructure as a Service**, o provedor entrega componentes fundamentais, como processamento, rede e armazenamento. O Amazon EC2 é um exemplo próximo desse modelo.

Ao criar uma instância EC2, o cliente continua responsável por tarefas como:

- configuração do sistema operacional;
- instalação da aplicação;
- atualização e aplicação de correções;
- configuração das regras de acesso;
- proteção dos dados.

### PaaS: plataforma como serviço

Na **Platform as a Service**, uma parte maior da administração fica a cargo do provedor. O desenvolvedor concentra seus esforços na aplicação, enquanto a plataforma automatiza tarefas de infraestrutura e implantação.

Serviços gerenciados da AWS podem oferecer características desse modelo. Quanto maior a abstração, menor costuma ser o trabalho direto com servidores e sistemas operacionais.

### SaaS: software como serviço

Na **Software as a Service**, o cliente utiliza uma aplicação pronta pela internet. Ele normalmente não administra servidores, sistema operacional ou código da plataforma. Ainda assim, continua responsável por aspectos como contas de usuário, permissões, dados inseridos e configurações disponíveis.

Os limites entre os modelos nem sempre são absolutos. O ponto central é entender que a divisão de responsabilidades muda conforme o serviço utilizado.

## Segurança da nuvem e segurança na nuvem

Um dos ensinamentos mais importantes da aula é que colocar uma aplicação na AWS não a torna automaticamente segura. A AWS adota o **modelo de responsabilidade compartilhada**.

Esse modelo separa a segurança em duas partes:

| AWS: segurança **da** nuvem | Cliente: segurança **na** nuvem |
|---|---|
| Instalações e datacenters | Usuários, funções e permissões |
| Hardware físico | Dados e sua classificação |
| Infraestrutura global de rede | Configuração de redes e grupos de segurança |
| Camada de virtualização | Atualizações do sistema operacional no EC2 |
| Disponibilidade da infraestrutura dos serviços | Vulnerabilidades e configurações da aplicação |

Em uma instância EC2, por exemplo, a AWS protege o datacenter, os equipamentos e a virtualização. O cliente administra o sistema operacional convidado, os programas instalados, as correções de segurança e o firewall chamado **security group**.

Em serviços mais gerenciados, como Amazon S3 e DynamoDB, a AWS também administra o sistema operacional e a plataforma subjacente. Mesmo assim, o cliente deve configurar corretamente as permissões, definir quem acessa os dados e escolher os controles de criptografia adequados.

Uma falha de configuração pode expor informações mesmo que toda a infraestrutura física esteja protegida. Por isso, ferramentas de segurança só produzem resultado quando são configuradas e monitoradas corretamente.

## Escalabilidade vertical e horizontal

**Escalabilidade** é a capacidade de aumentar os recursos disponíveis para atender a uma demanda maior. Ela pode ocorrer de duas maneiras.

### Escala vertical

Na escala vertical, aumenta-se a capacidade de uma máquina existente. Uma instância pode receber mais memória, processamento ou desempenho de armazenamento.

Um exemplo é substituir uma instância com 2 GB de memória por outra com 8 GB. A máquina torna-se mais poderosa, mas continua sendo uma única unidade de processamento.

### Escala horizontal

Na escala horizontal, novas instâncias são adicionadas ao ambiente. Em vez de depender de um servidor cada vez maior, a aplicação distribui o trabalho entre vários servidores.

Essa abordagem pode combinar:

- múltiplas instâncias;
- balanceamento de carga;
- políticas de Auto Scaling;
- distribuição entre Zonas de Disponibilidade;
- aplicação preparada para operar de forma distribuída.

A nuvem não elimina a escala vertical. A AWS permite usar as duas formas. Contudo, a escala horizontal costuma ser essencial em aplicações que precisam crescer, distribuir tráfego e resistir à falha de uma instância.

## Qual é a diferença entre escalabilidade e elasticidade?

Embora sejam relacionados, os conceitos não significam exatamente a mesma coisa.

**Escalabilidade** é a capacidade de suportar crescimento. **Elasticidade** é a capacidade de aumentar e reduzir recursos de acordo com a variação da demanda.

Imagine uma loja virtual:

- em dias comuns, duas instâncias atendem os clientes;
- durante uma promoção, o tráfego aumenta e seis instâncias entram em operação;
- após a promoção, as quatro instâncias adicionais são removidas.

O ambiente foi escalado para suportar o pico e depois reduzido. Essa adaptação caracteriza a elasticidade. Quando automatizada, ela ajuda a equilibrar desempenho e custo.

Entretanto, a redução não acontece sozinha em todo serviço. É necessário escolher uma arquitetura compatível e configurar políticas, métricas e limites apropriados.

## Alta disponibilidade na AWS

Alta disponibilidade é a capacidade de manter um serviço funcionando mesmo quando parte da infraestrutura apresenta falha.

A infraestrutura global da AWS é organizada em **Regiões** e **Zonas de Disponibilidade**. Uma Região representa uma área geográfica. Dentro dela, existem Zonas de Disponibilidade fisicamente separadas e conectadas por redes de baixa latência.

Uma aplicação pode distribuir seus componentes entre mais de uma zona. Se uma instância falhar, outra pode continuar atendendo. Se uma zona apresentar problema, uma arquitetura bem projetada pode utilizar recursos de outra zona.

É importante não confundir os conceitos:

- adicionar instâncias melhora a capacidade;
- distribuir instâncias reduz a dependência de uma única máquina;
- utilizar múltiplas zonas ajuda a enfrentar falhas localizadas;
- configurar backups, monitoramento e recuperação completa a estratégia.

Portanto, escalar horizontalmente não garante alta disponibilidade por si só. A aplicação, o banco de dados, a rede e os mecanismos de recuperação precisam ser planejados para essa finalidade.

## Como funcionam os custos da AWS?

Um dos princípios da computação em nuvem é o pagamento conforme o uso, conhecido como **pay as you go**. Em vários serviços, não existe a necessidade de comprar equipamentos antecipadamente ou assumir um contrato de capacidade física fixa.

Entretanto, “usou, pagou; não usou, não pagou” é uma simplificação. Cada serviço possui sua própria unidade de cobrança. Os custos podem considerar:

- tempo de processamento;
- quantidade de armazenamento;
- número de requisições;
- transferência de dados;
- endereços IPv4 públicos;
- operações de leitura e gravação;
- capacidade provisionada;
- licenças e recursos adicionais.

Ao interromper uma instância EC2, a cobrança do processamento da instância normalmente é interrompida. Porém, volumes EBS, snapshots, endereços IP e outros recursos associados podem continuar gerando custos.

A AWS também oferece diferentes opções comerciais, como:

- preços sob demanda;
- Savings Plans;
- instâncias reservadas em determinados serviços;
- instâncias Spot;
- faixas de preço relacionadas ao volume em serviços específicos.

Uma geração mais recente de instâncias frequentemente oferece melhor relação entre preço e desempenho, mas isso não significa que todo modelo novo será sempre mais barato em qualquer configuração. A comparação deve considerar preço, arquitetura do processador, memória, desempenho e compatibilidade da aplicação.

## Como funciona o AWS Free Tier atualmente?

O modelo apresentado em treinamentos antigos — com vários serviços gratuitos durante os primeiros 12 meses — não deve ser tratado como regra para todas as contas novas.

De acordo com as condições disponíveis em julho de 2026, novos clientes podem receber até **US$ 200 em créditos**. O **plano gratuito** permite explorar serviços selecionados por até seis meses ou até o esgotamento dos créditos. Existem também mais de 30 serviços com ofertas sempre gratuitas, respeitando limites mensais.

O plano pago libera o catálogo completo e pode gerar cobranças quando o uso ultrapassa os créditos ou os limites gratuitos. Os créditos possuem regras próprias de validade, e contas anteriores podem permanecer em condições diferentes.

Antes de criar recursos, consulte a página oficial do [AWS Free Tier](https://aws.amazon.com/pt/free/) e as [perguntas frequentes do nível gratuito](https://aws.amazon.com/pt/free/free-tier-faqs/). As ofertas, os serviços elegíveis e os limites podem mudar.

## Exemplo de arquitetura para uma aplicação Java

Para um desenvolvedor Java, a AWS pode hospedar uma aplicação Spring Boot ou Jakarta EE de diferentes formas. Uma arquitetura introdutória poderia utilizar:

1. **Elastic Load Balancing** para receber e distribuir as requisições;
2. **Amazon EC2**, Amazon ECS ou outro serviço de computação para executar a aplicação;
3. **Amazon RDS** para disponibilizar um banco de dados relacional;
4. **Amazon S3** para armazenar documentos e imagens;
5. **AWS Secrets Manager** para proteger segredos e credenciais;
6. **Amazon CloudWatch** para centralizar métricas, logs e alarmes;
7. **AWS IAM** para conceder somente as permissões necessárias;
8. **Amazon VPC** para organizar os componentes em redes públicas e privadas.

Se o número de acessos aumentar, novas instâncias ou tarefas podem ser adicionadas. O balanceador distribui o tráfego, enquanto o monitoramento oferece dados para tomar decisões automáticas ou manuais.

Esse exemplo demonstra que AWS não é um único produto. Ela funciona como um conjunto de peças que podem formar arquiteturas diferentes conforme o objetivo, o orçamento, os requisitos de segurança e o conhecimento da equipe.

## Benefícios da AWS

Entre os benefícios mais comuns estão:

- provisionamento rápido de recursos;
- possibilidade de começar com uma estrutura pequena;
- acesso global a Regiões e Zonas de Disponibilidade;
- automação da infraestrutura;
- variedade de serviços gerenciados;
- escalabilidade e elasticidade;
- modelos de cobrança adaptados a diferentes cargas;
- integração entre segurança, monitoramento, redes e aplicações.

Esses benefícios não surgem automaticamente. Uma arquitetura mal dimensionada pode ficar cara, insegura ou difícil de administrar. A nuvem amplia as possibilidades, mas também exige governança e conhecimento técnico.

## Cuidados ao começar na AWS

Para estudar com segurança e reduzir o risco de cobranças inesperadas:

1. proteja o usuário-raiz com autenticação multifator;
2. não utilize o usuário-raiz nas atividades diárias;
3. conceda somente as permissões necessárias;
4. configure um orçamento e alertas no AWS Budgets;
5. acompanhe o painel de faturamento e custos;
6. identifique os recursos que continuam cobrando quando estão ociosos;
7. exclua os laboratórios após concluir os exercícios;
8. não publique chaves de acesso em repositórios;
9. documente a Região em que cada recurso foi criado;
10. leia a página de preços antes de ativar um serviço.

O orçamento ajuda a acompanhar o consumo, mas não deve ser confundido com um bloqueio automático de todos os gastos. Alertas servem para avisar que um limite foi atingido ou está próximo.

## Perguntas frequentes sobre AWS

### AWS e Amazon são a mesma coisa?

A AWS pertence à Amazon, mas representa sua área de serviços de computação em nuvem. A loja virtual da Amazon e a plataforma AWS são operações diferentes dentro do mesmo grupo empresarial.

### Preciso comprar um servidor para utilizar AWS?

Não. Você pode contratar capacidade computacional virtual e diversos serviços gerenciados. Em alguns casos, nem mesmo precisa administrar servidores, como ocorre com o AWS Lambda.

### Uma instância é uma máquina virtual?

No contexto do Amazon EC2, uma instância pode ser entendida como um servidor virtual criado a partir de uma imagem e de um tipo de capacidade escolhido.

### AWS é gratuita?

A AWS é uma plataforma comercial. Existem créditos, testes e ofertas gratuitas com limites específicos, mas a utilização fora desses limites pode gerar cobrança. As condições devem ser verificadas na página oficial do Free Tier.

### AWS é segura?

A AWS protege a infraestrutura que sustenta seus serviços e oferece diversas ferramentas de segurança. O cliente continua responsável por proteger identidades, dados, aplicações e configurações, de acordo com o serviço utilizado.

### É possível hospedar aplicações Java na AWS?

Sim. Aplicações Java podem ser executadas em máquinas virtuais, contêineres, serviços gerenciados e funções, dependendo das características do projeto.

## Referências e materiais complementares

Para confirmar os conceitos e continuar o estudo:

- consulte a apresentação oficial sobre [o que é AWS](https://aws.amazon.com/pt/what-is-aws/);
- estude o [modelo de responsabilidade compartilhada](https://aws.amazon.com/pt/compliance/shared-responsibility-model/);
- confira as condições atuais do [AWS Free Tier](https://aws.amazon.com/pt/free/);
- veja as [práticas recomendadas para o usuário-raiz](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/root-user-best-practices.html);
- aprenda a [controlar custos com AWS Budgets](https://docs.aws.amazon.com/pt_br/hands-on/latest/control-your-costs-free-tier-budgets/control-your-costs-free-tier-budgets.html).

## Conclusão: por que aprender AWS?

Compreender **o que é AWS** é o primeiro passo para trabalhar com uma das principais plataformas de computação em nuvem. A AWS permite criar servidores, armazenar arquivos, disponibilizar bancos de dados, configurar redes, monitorar aplicações e automatizar infraestruturas sem comprar todo o hardware antecipadamente.

O aprendizado, entretanto, não deve se limitar aos nomes dos serviços. É necessário compreender como as responsabilidades são divididas, como a aplicação escala, como a elasticidade afeta o consumo, como construir alta disponibilidade e como monitorar custos.

O principal resumo da aula pode ser organizado em quatro ideias:

- a AWS oferece recursos tecnológicos como serviços;
- segurança é uma responsabilidade compartilhada;
- escalabilidade e elasticidade ajudam a adaptar a infraestrutura;
- o custo depende dos recursos, do modelo de contratação e do consumo.

Quando esses fundamentos estão claros, os serviços deixam de parecer uma lista de siglas e passam a funcionar como componentes de uma arquitetura. Esse é o ponto de partida para estudar EC2, S3, VPC, IAM, RDS, Lambda e os demais recursos da plataforma com mais segurança e propósito.
