---
title: "PHP Code Coverage — Lição 2: arquitetura, drivers e importação Clover"
seo_title: "PHP Code Coverage: arquitetura, Xdebug, PCOV e Clover na prática"
slug: "php-code-coverage-licao-2-arquitetura-drivers-clover"
meta_description: "Domine a arquitetura do PHP Code Coverage 14.4.1, entenda Filter, Driver e ReportFacade e crie um importador Clover em PHP para um futuro SaaS."
focus_keyphrase: "PHP Code Coverage"
related_keywords:
  - "php-code-coverage"
  - "cobertura de código PHP"
  - "PHPUnit code coverage"
  - "Xdebug e PCOV"
  - "CodeCoverage start stop"
  - "Granularity PHP"
  - "Clover XML PHP"
  - "relatório JSONL PHP"
  - "dashboard de cobertura de testes"
  - "arquitetura de software PHP"
category: "Programação"
tags:
  - "PHP"
  - "PHPUnit"
  - "Code Coverage"
  - "Testes Automatizados"
  - "Xdebug"
  - "PCOV"
  - "Clover"
  - "XML"
  - "Qualidade de Código"
  - "SaaS"
  - "Composer"
excerpt: "Na lição 2 da trilha PHP Code Coverage, estudamos a arquitetura da versão 14.4.1 e construímos um primeiro importador Clover para o futuro CoverageRadar PHP."
---

# 🧪 PHP Code Coverage — Lição 2: arquitetura, drivers e nosso primeiro importador Clover

Quando executamos testes com o PHPUnit e abrimos um relatório de cobertura, normalmente vemos porcentagens, arquivos coloridos e linhas que passaram ou não pelos testes. Entretanto, de onde vêm essas informações? Quem decide quais arquivos entram no relatório? Como um resultado bruto se transforma em HTML, Clover ou JSONL?

Nesta segunda lição da trilha de **PHP Code Coverage**, investigaremos a arquitetura do projeto [sebastianbergmann/php-code-coverage](https://github.com/sebastianbergmann/php-code-coverage) e começaremos um produto próprio: o **CoverageRadar PHP**, um analisador de relatórios com potencial para evoluir até um serviço web.

A proposta é dupla. Primeiro, **dominar o projeto de referência e suas decisões de engenharia**. Depois, **usar o conhecimento para criar uma aplicação independente, com código e identidade próprios**. Não precisamos escrever outro Xdebug nem copiar as classes do projeto original para oferecer algo útil.

> **Versão verificada em 15/10/2026:** a última release estável consultada foi **14.4.1, de 03/10/2026**. Exemplos de API desta lição foram alinhados ao código da tag [14.4.1](https://github.com/sebastianbergmann/php-code-coverage/releases/tag/14.4.1). A branch principal pode evoluir e apresentar APIs de versões futuras; por isso, sempre devemos indicar qual versão estamos estudando.

## 1. O que vamos dominar hoje

Ao concluir a aula, você deverá conseguir:

1. descrever a diferença entre **coleta**, **processamento** e **relatório** de cobertura;
2. explicar as responsabilidades de **Filter**, **Driver**, **CodeCoverage**, **Data**, **StaticAnalysis**, **Node** e **Report**;
3. compreender como o **Selector** escolhe PCOV ou Xdebug;
4. identificar os níveis **Line**, **LineAndBranch** e **LineBranchAndPath**;
5. usar a API atual para iniciar e encerrar uma coleta;
6. gerar relatórios de texto, HTML, Clover e JSONL;
7. compreender por que arquivos não executados precisam entrar no denominador;
8. ler a cobertura geral de um arquivo **Clover XML** usando nosso próprio PHP;
9. listar arquivos com menor cobertura e obter um JSON simples;
10. separar o código do futuro SaaS da execução de testes de terceiros.

Na primeira lição, aprendemos que **cobertura mede execução, não correção**. Um teste pode percorrer uma linha sem verificar o resultado esperado. Nesta aula, veremos como a própria arquitetura da ferramenta reflete essa diferença.

## 2. Antes do relatório: o fluxo completo

O fluxo de cobertura pode ser representado assim:

~~~text
Projeto PHP + testes
         |
         v
PHPUnit / executor de testes
         |
         v
Extensão nativa: PCOV ou Xdebug
         |
         v
Driver -> coleta bruta
         |
         v
Filter + StaticAnalysis
         |
         v
CodeCoverage + Data
         |
         v
Node (árvore de projeto, arquivos e unidades)
         |
         v
Report\Facade
         |
         +--> HTML
         +--> Texto
         +--> Clover / Cobertura / OpenClover
         +--> XML
         +--> JSONL
~~~

Essa é uma **visão didática**. Internamente, as etapas não são uma fila isolada de classes: **CodeCoverage** coordena o driver, filtros, processamento e construção dos dados consumidos pelos relatórios.

A distinção central é:

| Etapa | Pergunta que responde | Exemplo |
|---|---|---|
| Coleta | O que foi executado? | Xdebug informa linhas e caminhos percorridos |
| Filtragem | Qual código pertence à aplicação? | Incluir apenas arquivos de src |
| Processamento | Como representar os dados de forma útil? | Associar linhas executadas aos testes |
| Análise estática | Quais linhas são executáveis? | Interpretar o código PHP |
| Agregação | Como somar por arquivo e projeto? | Total de linhas cobertas |
| Relatório | Como apresentar ou exportar? | HTML, Clover, JSONL |

Uma boa arquitetura mantém essas responsabilidades separadas. Isso facilita testes, evolução das APIs e inclusão de novos formatos.

## 3. Conhecendo os diretórios do repositório

A estrutura de código-fonte da biblioteca inclui estas áreas:

~~~text
src/
├── CodeCoverage.php
├── Filter.php
├── FilterProcessor.php
├── Data/
├── Driver/
├── Node/
├── Report/
├── Serialization/
├── StaticAnalysis/
├── Test/
└── Util/
~~~

Veja como estudar cada uma:

| Módulo | Responsabilidade predominante |
|---|---|
| CodeCoverage | Orquestrar captura, anexação, combinação e resultados |
| Filter | Definir arquivos elegíveis para a cobertura |
| FilterProcessor | Refinar os dados coletados conforme regras de inclusão |
| Driver | Comunicar-se com o mecanismo de cobertura |
| Data | Armazenar informações brutas e processadas |
| StaticAnalysis | Examinar o código-fonte para enriquecer os dados |
| Node | Organizar os dados em uma árvore de diretórios e arquivos |
| Report | Exportar resultados em formatos consumíveis |
| Serialization | Persistir e recuperar dados em formato próprio |
| Test | Modelar informações relacionadas a testes e seus alvos |
| Util | Compartilhar funcionalidades auxiliares |

**Recomendação de leitura:** abra primeiro a [classe CodeCoverage](https://github.com/sebastianbergmann/php-code-coverage/blob/14.4.1/src/CodeCoverage.php), mas não tente memorizar todas as linhas. Procure suas dependências e os métodos públicos. Depois navegue para cada componente utilizado.

## 4. Filter: estabelecer os limites da análise

Suponha que um projeto contenha código próprio, dependências instaladas pelo Composer e testes. Se incluirmos todos os arquivos no relatório, a porcentagem poderá perder significado.

É o **Filter** que delimita os arquivos elegíveis.

~~~php
use SebastianBergmann\CodeCoverage\Filter;

$filter = new Filter();

$filter->includeFiles([
    __DIR__ . '/src/Calculator.php',
]);
~~~

Na versão 14.4.1, a classe **Filter** oferece métodos como **includeFile()**, **includeFiles()**, **files()**, **isEmpty()** e **isExcluded()**. O caminho incluído é normalizado com **realpath()**; caminhos inexistentes não são adicionados.

Isso produz um detalhe importante: **incluir arquivos não é o mesmo que executar arquivos**. Um arquivo elegível pode aparecer com zero linhas cobertas se nunca for exercitado.

### Erro comum: cobertura artificialmente alta

Imagine:

- **Calculator.php:** 50 linhas executáveis, 50 cobertas.
- **PaymentService.php:** 50 linhas executáveis, nenhuma coberta.

Se filtrarmos apenas a calculadora, o projeto parecerá ter 100% de cobertura. Se considerarmos os dois arquivos elegíveis, a cobertura global será **50%**.

Portanto, antes de comemorar um número alto, pergunte: **qual é o denominador?**

## 5. Driver: coletar dados de execução

A biblioteca não monitora o interpretador PHP sozinha. Para isso, utiliza uma extensão compatível.

**PCOV** é voltado à cobertura de linhas. **Xdebug**, quando configurado para coverage, fornece granularidade mais avançada, incluindo branches e paths.

Na tag 14.4.1, a classe [Driver\Selector](https://github.com/sebastianbergmann/php-code-coverage/blob/14.4.1/src/Driver/Selector.php) contém uma API importante:

~~~php
use SebastianBergmann\CodeCoverage\Driver\Granularity;
use SebastianBergmann\CodeCoverage\Driver\Selector;

$driver = (new Selector())->select(
    $filter,
    Granularity::Line
);
~~~

O comportamento principal é:

1. Para **Granularity::Line**, o seletor prefere PCOV quando disponível.
2. Caso contrário, tenta Xdebug.
3. Para granularidade de branches/paths, utiliza Xdebug quando disponível e compatível.
4. Se não houver mecanismo suportado, lança uma exceção.

### Granularity: as três opções

~~~php
Granularity::Line
Granularity::LineAndBranch
Granularity::LineBranchAndPath
~~~

- **Line:** quais linhas executáveis foram percorridas?
- **LineAndBranch:** quais linhas e desvios foram exercitados?
- **LineBranchAndPath:** quais linhas, desvios e caminhos foram percorridos?

No código atual, os antigos métodos **forLineCoverage()** e **forLineAndPathCoverage()** ainda aparecem, mas estão marcados como **deprecated**. Para nosso estudo, adotaremos **select()** e **Granularity**.

> **Atenção:** habilitar a granularidade no PHP não garante que um teste seja bom. Também não significa que todos os projetos terão dados de branch se o driver utilizado não suportar essa modalidade.

## 6. CodeCoverage: o coordenador do sistema

A classe principal recebe dois objetos: o driver e o filtro.

~~~php
use SebastianBergmann\CodeCoverage\CodeCoverage;

$coverage = new CodeCoverage($driver, $filter);
~~~

O ciclo mais simples é:

~~~php
$coverage->start('exemplo-de-execucao');

// Código que queremos observar.
$calculator = new \App\Calculator();
$calculator->add(2, 3);

$coverage->stop();
~~~

Nesse fluxo:

1. **start()** registra um identificador para a coleta e aciona o driver.
2. O código de produção executa normalmente.
3. **stop()** encerra a captura no driver.
4. Os dados recebidos passam pelo processamento e são associados ao identificador da execução.
5. O objeto fica disponível para relatórios e agregações.

Esse exemplo **não substitui um teste PHPUnit**: executa código sob medição, mas não verifica automaticamente os resultados. Podemos usar PHPUnit para unir execução e asserções.

### Uma sutileza de engenharia

O método **merge()** combina informações de duas instâncias de cobertura. Na release **14.4.1**, uma correção evitou copiar repetidamente todos os testes já acumulados a cada combinação. Isso reduziu um comportamento que podia crescer quadraticamente ao mesclar os resultados de muitos processos.

A lição para nosso produto é clara: **um algoritmo aparentemente simples pode ficar caro à medida que a quantidade de builds e testes aumenta**.

## 7. Data e StaticAnalysis: dados brutos não bastam

A extensão de cobertura informa dados de execução, mas um relatório útil precisa ir além.

**Data** organiza os dados capturados. **StaticAnalysis** ajuda a entender características dos arquivos PHP e das unidades de código, como linhas executáveis e estrutura de funções ou métodos.

Considere:

~~~php
public function classifyNumber(int $number): string
{
    if ($number > 0) {
        return 'positive';
    }

    return 'zero';
}
~~~

O relatório precisa distinguir entre:

- linha de código real e linha em branco;
- linha executável e estrutura meramente sintática;
- linha executada e linha ainda não percorrida;
- método parcialmente coberto e método completamente coberto.

Nem toda linha física deve entrar no cálculo. Além disso, se um arquivo não puder ser analisado estaticamente, o projeto mantém mecanismos para trabalhar com os dados brutos disponíveis, mas perde parte do enriquecimento possível.

Por esse motivo, **quantidade de linhas do arquivo não é igual ao total de linhas executáveis**.

## 8. Node e Report\Facade: organizar e apresentar

Depois da coleta e do processamento, a biblioteca constrói uma representação hierárquica do projeto. As estruturas em **Node** permitem agregar estatísticas de arquivos e diretórios.

A classe [Report\Facade](https://github.com/sebastianbergmann/php-code-coverage/blob/14.4.1/src/Report/Facade.php) reúne a geração de relatórios. Sua instanciação acontece por meio de um método de fábrica:

~~~php
use SebastianBergmann\CodeCoverage\Report\Facade as ReportFacade;

$report = ReportFacade::fromObject($coverage);
~~~

Entre as saídas disponíveis estão:

~~~php
$report->renderText(null);
$report->renderHtml('build/html');
$report->renderClover('build/clover.xml');
$report->renderCobertura('build/cobertura.xml');
$report->renderJsonl('build/jsonl');
~~~

Os caminhos mostrados são **exemplos**. Precisamos garantir que o diretório de destino exista e seja gravável, de acordo com o formato.

### Por que Clover é uma boa primeira integração?

O formato **Clover XML** possui métricas de projeto e de arquivos. Ele facilita nosso primeiro experimento: importar um arquivo gerado por outra ferramenta e calcular uma cobertura percentual sem precisar executar o projeto que o produziu.

### E a novidade JSONL?

A release **14.4.0** adicionou a saída **renderJsonl()**, que grava em um diretório:

- **meta.json:** informações gerais, versão do esquema e totais;
- **coverage.jsonl:** um registro por arquivo com dados de cobertura;
- **tests.jsonl:** linhas associadas aos testes que as executaram.

Essa saída poderá servir para análises mais sofisticadas no CoverageRadar PHP. Não a confundiremos com Clover: são **formatos diferentes**, com dados e esquemas distintos.

## 9. Laboratório A — Usando a biblioteca diretamente

Retomaremos o projeto **coverage-lab** da Aula 1, que contém uma classe **App\Calculator** em **src/Calculator.php**. Ela possui métodos de soma, divisão e classificação de números.

### 9.1. Diagnostique o ambiente

No PowerShell:

~~~powershell
php -v
php -m
composer --version
~~~

Para esta versão da biblioteca, o ambiente precisa cumprir os requisitos do pacote, incluindo **PHP 8.4 ou superior** e as extensões obrigatórias de XML e strings. Para obter dados de cobertura, precisamos também de **PCOV** ou **Xdebug** devidamente habilitado no PHP CLI.

Confira:

~~~powershell
php -m | Select-String -Pattern "xdebug|pcov"
~~~

Se estiver usando Xdebug, ative o modo de cobertura na sessão:

~~~powershell
$env:XDEBUG_MODE = "coverage"
~~~

> **Se não aparecer PCOV nem Xdebug:** não há driver disponível para coletar cobertura. Isso não impede a realização do **Laboratório B**, que apenas importa um XML e não executa a suíte de testes.

### 9.2. Instale a dependência de desenvolvimento

Na pasta do projeto:

~~~powershell
composer require --dev "phpunit/php-code-coverage:^14.4"
~~~

A resolução do Composer dependerá das outras dependências e do arquivo **composer.lock**. Registre a versão instalada:

~~~powershell
composer show phpunit/php-code-coverage
~~~

Não substitua bibliotecas instaladas nem ignore conflitos de dependência às cegas. Se o projeto existente ainda usar uma versão incompatível de PHPUnit, crie um laboratório separado antes de atualizar.

### 9.3. Crie um script de coleta

Salve o arquivo **examples/coverage-direct.php**:

~~~php
<?php

declare(strict_types=1);

require dirname(__DIR__) . '/vendor/autoload.php';

use App\Calculator;
use SebastianBergmann\CodeCoverage\CodeCoverage;
use SebastianBergmann\CodeCoverage\Driver\Granularity;
use SebastianBergmann\CodeCoverage\Driver\Selector;
use SebastianBergmann\CodeCoverage\Filter;
use SebastianBergmann\CodeCoverage\Report\Facade as ReportFacade;

$root = dirname(__DIR__);
$source = $root . '/src/Calculator.php';
$output = $root . '/build';

if (!is_file($source)) {
    throw new RuntimeException('Calculator.php não encontrado.');
}

if (!is_dir($output) && !mkdir($output, 0775, true) && !is_dir($output)) {
    throw new RuntimeException('Não foi possível criar build.');
}

$filter = new Filter();
$filter->includeFiles([$source]);

$driver = (new Selector())->select($filter, Granularity::Line);
$coverage = new CodeCoverage($driver, $filter);

$coverage->start('laboratorio-manual');

$calculator = new Calculator();
$calculator->add(2, 3);
$calculator->divide(10, 5);
$calculator->classifyNumber(9);

$coverage->stop();

$report = ReportFacade::fromObject($coverage);

echo $report->renderText(null);
$report->renderClover($output . '/clover.xml');
$report->renderHtml($output . '/html');
$report->renderJsonl($output . '/jsonl');

echo PHP_EOL . 'Relatórios gerados em: ' . $output . PHP_EOL;
~~~

> **O que esse código demonstra:** utilizamos a API real da biblioteca na versão estudada. Executamos três operações manualmente para observar a coleta, mas ainda precisamos de testes com asserções para validar os comportamentos.

Execute no PowerShell:

~~~powershell
php .\examples\coverage-direct.php
~~~

Ao final, procure:

~~~text
coverage-lab/
├── build/
│   ├── clover.xml
│   ├── html/
│   │   └── index.html
│   └── jsonl/
│       ├── meta.json
│       ├── coverage.jsonl
│       └── tests.jsonl
└── examples/
    └── coverage-direct.php
~~~

A execução deve gerar as saídas descritas **quando a instalação e o driver estiverem corretos**. Não afirmamos uma porcentagem antecipadamente, pois ela depende dos caminhos percorridos, do filtro e da versão exata do código.

### 9.4. Compare com o PHPUnit

Se o laboratório da Aula 1 já estiver configurado com PHPUnit, faça a coleta pela suíte:

~~~powershell
.\vendor\bin\phpunit --coverage-clover build\clover.xml
~~~

Compare o XML do teste automatizado com o XML do nosso script manual. Os resultados podem diferir: no segundo caso, somente as três chamadas do exemplo são exercitadas.

## 10. Laboratório B — Começando o CoverageRadar PHP

Agora começa a parte do **nosso produto**. Em vez de repetir a coleta do php-code-coverage, criaremos um pequeno programa capaz de **importar Clover XML**, extrair a cobertura do projeto e produzir um resumo JSON.

Esta decisão é importante para a monetização futura: um dashboard pode receber relatórios gerados no computador ou no pipeline de CI de cada equipe, sem executar o código enviado pelo cliente.

### 10.1. Primeiro contrato de entrada e saída

**Entrada:** caminho para um XML Clover válido.

**Saída:** JSON com:

- formato de origem;
- total de linhas executáveis;
- total de linhas executadas;
- porcentagem geral;
- relação de arquivos e suas porcentagens.

O objetivo é manter um primeiro contrato simples e previsível.

~~~text
clover.xml
    |
    v
CoverageRadar PHP: Clover Reader
    |
    +--> valida XML e métricas
    +--> extrai totais do projeto
    +--> extrai métricas por arquivo
    +--> ordena arquivos por menor cobertura
    |
    v
Resumo JSON
~~~

### 10.2. Entendendo as métricas Clover

Um trecho **simplificado e ilustrativo** pode ter este formato:

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<coverage generated="0">
  <project name="Exemplo" timestamp="0">
    <file name="src/Calculator.php">
      <metrics statements="10" coveredstatements="7"/>
    </file>
    <metrics statements="10" coveredstatements="7"/>
  </project>
</coverage>
~~~

No formato gerado pela versão estudada, **statements** representa linhas executáveis e **coveredstatements** representa as executadas. A fórmula do nosso cálculo é:

**Cobertura (%) = 100 × linhas executadas / linhas executáveis.**

Para o exemplo: **7 / 10 = 70%**.

Cuidado: o arquivo real pode conter **package**, **class**, **line** e outros nós **metrics**. Não devemos pegar o primeiro elemento metrics do documento por acaso. Precisamos selecionar precisamente **/coverage/project/metrics** para a cobertura global.

### 10.3. Crie o nosso primeiro leitor

Salve como **coverage-radar/ler-clover.php**, na raiz do laboratório:

~~~php
<?php

declare(strict_types=1);

$path = $argv[1] ?? '';

if ($path === '' || !is_file($path) || !is_readable($path)) {
    fwrite(STDERR, "Uso: php coverage-radar/ler-clover.php build/clover.xml\n");
    exit(2);
}

$size = filesize($path);

if ($size === false || $size > 5 * 1024 * 1024) {
    fwrite(STDERR, "XML ausente ou maior que o limite de 5 MiB.\n");
    exit(2);
}

$content = file_get_contents($path);

if ($content === false || preg_match('/<!\s*(DOCTYPE|ENTITY)\b/i', $content)) {
    fwrite(STDERR, "XML não permitido.\n");
    exit(2);
}

$previous = libxml_use_internal_errors(true);

try {
    $document = new DOMDocument();

    if (!$document->loadXML($content, LIBXML_NONET)) {
        throw new RuntimeException('XML malformado.');
    }
} catch (Throwable $error) {
    fwrite(STDERR, $error->getMessage() . PHP_EOL);
    exit(2);
} finally {
    libxml_clear_errors();
    libxml_use_internal_errors($previous);
}

$xpath = new DOMXPath($document);
$projectMetrics = $xpath->query('/coverage/project/metrics');

if ($projectMetrics === false || $projectMetrics->length !== 1) {
    fwrite(STDERR, "Clover sem métricas do projeto.\n");
    exit(2);
}

$metrics = $projectMetrics->item(0);

if (!$metrics instanceof DOMElement) {
    exit(2);
}

$readCounts = static function (DOMElement $element): array {
    foreach (['statements', 'coveredstatements'] as $name) {
        if (!preg_match('/^\d+$/', $element->getAttribute($name))) {
            throw new RuntimeException("Métrica inválida: $name");
        }
    }

    $total = (int) $element->getAttribute('statements');
    $covered = (int) $element->getAttribute('coveredstatements');

    if ($covered > $total) {
        throw new RuntimeException('Linhas cobertas excedem o total.');
    }

    return [$total, $covered];
};

$percentage = static function (int $total, int $covered): ?float {
    return $total === 0 ? null : round(100 * $covered / $total, 2);
};

try {
    [$total, $covered] = $readCounts($metrics);

    $files = [];
    $fileNodes = $xpath->query(
        '/coverage/project/file | /coverage/project/package/file'
    );

    if ($fileNodes === false) {
        throw new RuntimeException('Não foi possível consultar arquivos.');
    }

    foreach ($fileNodes as $fileNode) {
        if (!$fileNode instanceof DOMElement) {
            continue;
        }

        // Busca apenas metrics filho direto do file.
        $fileMetrics = $xpath->query('./metrics', $fileNode);

        if ($fileMetrics === false || $fileMetrics->length !== 1) {
            throw new RuntimeException('Arquivo sem métricas válidas.');
        }

        $fileMetric = $fileMetrics->item(0);

        if (!$fileMetric instanceof DOMElement) {
            throw new RuntimeException('Métricas inválidas.');
        }

        [$fileTotal, $fileCovered] = $readCounts($fileMetric);

        $files[] = [
            'file' => $fileNode->getAttribute('name'),
            'executableLines' => $fileTotal,
            'executedLines' => $fileCovered,
            'coveragePercent' => $percentage($fileTotal, $fileCovered),
        ];
    }

    usort($files, static function (array $a, array $b): int {
        return ($a['coveragePercent'] ?? -1)
            <=> ($b['coveragePercent'] ?? -1);
    });

    $result = [
        'format' => 'clover',
        'summary' => [
            'executableLines' => $total,
            'executedLines' => $covered,
            'coveragePercent' => $percentage($total, $covered),
        ],
        'files' => $files,
    ];

    echo json_encode(
        $result,
        JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE
        | JSON_UNESCAPED_SLASHES | JSON_THROW_ON_ERROR
    ), PHP_EOL;
} catch (Throwable $error) {
    fwrite(STDERR, $error->getMessage() . PHP_EOL);
    exit(2);
}
~~~

Execute:

~~~powershell
php .\coverage-radar\ler-clover.php .\build\clover.xml
~~~

**Saída ilustrativa**, não resultado medido nesta lição:

~~~json
{
  "format": "clover",
  "summary": {
    "executableLines": 10,
    "executedLines": 7,
    "coveragePercent": 70
  },
  "files": [
    {
      "file": "src/Calculator.php",
      "executableLines": 10,
      "executedLines": 7,
      "coveragePercent": 70
    }
  ]
}
~~~

### O que acabamos de construir?

Criamos um **protótipo CLI independente** capaz de ler métricas reais de um relatório Clover e transformá-las em dados estruturados. Esse é o primeiro passo concreto em direção ao CoverageRadar PHP.

**Limitações intencionais deste protótipo:**

- não importa dados de branches, métodos, hit counts ou complexidade;
- não verifica todas as regras do esquema Clover;
- limita o XML local a 5 MiB apenas para manter o exercício controlado;
- usa o nome de arquivo que consta no relatório, que pode ser um caminho absoluto;
- ainda não possui testes próprios, interface web, banco de dados nem autenticação;
- **não deve ser exposto como serviço de upload público**.

No código de produção, precisaremos limitar recursos, endurecer validações, evitar exposição de caminhos privados e isolar a ingestão de dados. O leitor **nunca deve executar arquivos PHP recebidos de terceiros**.

## 11. Por que separar nosso sistema do coletor?

Poderíamos instalar PCOV ou Xdebug no servidor SaaS, receber repositórios e executar testes remotamente? Tecnicamente, há cenários em que isso é possível. Porém, para o MVP, essa escolha criaria riscos de segurança e custos operacionais muito maiores.

Vamos separar:

~~~text
MÁQUINA DO DESENVOLVEDOR / CI
  - executa PHPUnit
  - usa Xdebug ou PCOV
  - gera Clover ou JSONL
               |
               | envia somente o relatório
               v
COVERAGERADAR PHP
  - autentica o projeto
  - valida o arquivo
  - importa métricas
  - armazena histórico
  - compara builds
  - exibe dashboards
  - dispara alertas
~~~

Essa divisão permite que a plataforma se concentre em **importação, análise histórica e apresentação**, reduzindo a necessidade de executar código não confiável no servidor.

## 12. Nossa arquitetura futura

Começaremos simples, mas já podemos imaginar responsabilidades de uma aplicação evolutiva:

~~~text
coverage-radar/
├── src/
│   ├── Domain/
│   │   ├── CoverageSummary.php
│   │   └── FileCoverage.php
│   ├── Importer/
│   │   ├── CloverImporter.php
│   │   └── JsonlImporter.php
│   ├── Analyzer/
│   │   ├── CoverageTrendAnalyzer.php
│   │   └── RiskAnalyzer.php
│   └── Report/
│       └── JsonSummaryReport.php
├── tests/
│   └── ...
├── public/
│   └── index.php
└── composer.json
~~~

Essa estrutura é um **projeto proposto**, ainda não um conjunto de arquivos publicados. No exercício de hoje, implementamos apenas o pequeno **ler-clover.php** dentro do laboratório.

Na próxima iteração, vamos extrair classes, introduzir testes de unidade para o importador e criar dados de exemplo. Depois poderemos adicionar armazenamento de projetos e builds.

## 13. Qualidade de dados: armadilhas que o produto precisará tratar

Uma plataforma de cobertura pode gerar relatórios bonitos e ainda assim apresentar métricas incorretas. Antes da monetização, alguns cuidados serão obrigatórios:

**Arquivos ignorados:** um relatório que exclui metade do código não oferece um retrato completo do projeto. Precisaremos deixar claro qual foi o escopo informado pela ferramenta coletora.

**Zero linhas executáveis:** dividir por zero seria incorreto. No nosso leitor, retornamos **null** quando não existe denominador; isso significa **não aplicável**, e não cobertura zero.

**Relatórios duplicados:** importar duas vezes o mesmo build não deveria criar duas versões históricas.

**Caminhos absolutos:** arquivos Clover podem conter caminhos locais ou nomes que revelam detalhes internos. Em um SaaS, devemos normalizar, restringir acesso e evitar exibi-los publicamente.

**Dados incompatíveis:** relatórios gerados por versões e ferramentas diferentes podem exigir adaptadores específicos.

**Métricas de linha versus ramo:** um arquivo pode ter todas as linhas executadas e, mesmo assim, conter decisões não testadas. Nunca misture duas métricas diferentes como se fossem a mesma porcentagem.

## 14. Exercícios práticos

### Exercício 1 — Mude o filtro

Inclua outro arquivo PHP de produção no filtro do Laboratório A. Não execute seu código. Verifique como a cobertura global muda ao incluir arquivos elegíveis que ficaram sem execução.

### Exercício 2 — Teste branches

Se tiver Xdebug habilitado, troque o seletor para:

~~~php
$driver = (new Selector())->select(
    $filter,
    Granularity::LineAndBranch
);
~~~

Observe se o relatório passa a exibir informações adicionais. Não use essa configuração apenas com PCOV.

### Exercício 3 — Compare formatos

Gere **Clover** e **JSONL** com os mesmos dados. Confira quais informações aparecem nos arquivos e quais existem apenas em um dos formatos.

### Exercício 4 — Valide XML inválido

Entregue ao nosso importador um arquivo de texto que não seja XML. Depois forneça um Clover sem **/coverage/project/metrics**. O programa deve recusar as duas entradas.

### Exercício 5 — Ordenação e métricas

Crie um Clover de teste com três arquivos: 25%, 80% e 60% de cobertura. Verifique se o JSON retorna os arquivos na ordem esperada. Compare também os totais globais.

### Desafio extra — Primeiro teste automatizado do produto

Separe a leitura do XML em uma classe **CloverImporter** e teste os cenários:

- projeto com linhas cobertas;
- projeto sem linhas executáveis;
- arquivo com zero execução;
- métrica ausente;
- quantidade coberta superior ao total;
- XML malformado;
- presença de DTD;
- arquivo muito grande.

Essa transformação será uma das primeiras etapas necessárias para tornar o CoverageRadar PHP confiável.

## 15. Questões de revisão

**1. Quem coleta diretamente as informações de execução do PHP?**

A) A página HTML do relatório  
B) Uma extensão como Xdebug ou PCOV  
C) O banco de dados MySQL  
D) O navegador  

**2. Para que serve o Filter?**

A) Alterar resultados dos testes  
B) Escolher arquivos elegíveis para cobertura  
C) Salvar senhas  
D) Converter PHP em JavaScript  

**3. Qual API devemos preferir na versão 14.4.1 para selecionar granularidade?**

A) select() com Granularity  
B) Apenas forLineCoverage(), sem alternativas  
C) selectFormat()  
D) buildDriverFromHtml()  

**4. Qual é a principal função de Report\Facade?**

A) Executar testes no navegador  
B) Gerar relatórios a partir dos dados processados  
C) Substituir o Xdebug  
D) Gerenciar usuários do SaaS  

**5. Qual formato usaremos no primeiro importador do CoverageRadar PHP?**

A) PDF  
B) ZIP de repositórios  
C) Clover XML  
D) Arquivos executáveis  

**6. Cobertura de 100% garante ausência de bugs?**

A) Sim, sempre  
B) Apenas no Windows  
C) Não: execução não prova que todas as expectativas foram verificadas  
D) Apenas com PCOV  

**7. Em Clover, onde encontramos as métricas gerais do projeto?**

A) /coverage/project/metrics  
B) /coverage/file/name  
C) /html/body  
D) /phpunit/password  

**8. Por que não executaremos código PHP enviado por clientes no MVP?**

A) Porque Clover exige Java  
B) Porque não existe PHPUnit no servidor  
C) Para reduzir riscos e complexidade de executar código não confiável  
D) Porque relatórios XML não têm métricas  

### Gabarito comentado

| Questão | Resposta | Comentário |
|---|---|---|
| 1 | B | O driver conversa com PCOV ou Xdebug. |
| 2 | B | O filtro delimita o código elegível. |
| 3 | A | select() recebe um valor da enum Granularity. |
| 4 | B | A fachada expõe as diferentes saídas. |
| 5 | C | Clover é o primeiro contrato de importação. |
| 6 | C | Cobertura é um sinal de execução, não prova de correção. |
| 7 | A | Esse caminho evita confundir métricas globais com as de classes e arquivos. |
| 8 | C | Executar código arbitrário impõe controles operacionais e de segurança muito mais complexos. |

## 16. Checklist de conclusão

Considere a Aula 2 concluída quando conseguir:

- [ ] Desenhar a arquitetura da coleta até o relatório.
- [ ] Distinguir Filter de Driver.
- [ ] Explicar o papel de CodeCoverage.
- [ ] Reconhecer os três valores de Granularity.
- [ ] Identificar os métodos deprecated do Selector.
- [ ] Executar o Laboratório A com um driver compatível.
- [ ] Produzir ao menos um Clover XML.
- [ ] Gerar o resumo JSON usando nosso leitor.
- [ ] Explicar o denominador da cobertura.
- [ ] Identificar limitações de segurança e escopo do protótipo.
- [ ] Responder às oito questões de revisão.

## 17. Próxima lição da trilha

Na **Aula 3 — Construindo o núcleo do CoverageRadar PHP com testes**, vamos refatorar o protótipo em classes independentes e desenvolver uma suíte própria.

Nossa meta será construir **CloverImporter**, **CoverageSummary** e **FileCoverage** com PHPUnit, incluindo casos de erro e relatórios de teste. Assim, estudaremos boas práticas usando, na prática, o mesmo conceito que queremos vender como serviço.

Depois avançaremos para:

- persistência de resultados em MySQL;
- projetos e builds com histórico;
- detecção de quedas de cobertura;
- importador de JSONL com verificação de schemaVersion;
- dashboard web;
- integração com CI/CD;
- publicação e eventual monetização.

**Marco desta lição:** começamos a transição do estudo de uma biblioteca reconhecida para a engenharia de uma ferramenta própria, sem confundir protótipo didático com produto pronto para produção.

---

## Referências oficiais e leitura complementar

- [Repositório php-code-coverage](https://github.com/sebastianbergmann/php-code-coverage)
- [Release 14.4.1 — 03/10/2026](https://github.com/sebastianbergmann/php-code-coverage/releases/tag/14.4.1)
- [Código-fonte CodeCoverage.php — tag 14.4.1](https://github.com/sebastianbergmann/php-code-coverage/blob/14.4.1/src/CodeCoverage.php)
- [Código-fonte Filter.php — tag 14.4.1](https://github.com/sebastianbergmann/php-code-coverage/blob/14.4.1/src/Filter.php)
- [Código-fonte Driver/Selector.php — tag 14.4.1](https://github.com/sebastianbergmann/php-code-coverage/blob/14.4.1/src/Driver/Selector.php)
- [Código-fonte Driver/Granularity.php — tag 14.4.1](https://github.com/sebastianbergmann/php-code-coverage/blob/14.4.1/src/Driver/Granularity.php)
- [Código-fonte Report/Facade.php — tag 14.4.1](https://github.com/sebastianbergmann/php-code-coverage/blob/14.4.1/src/Report/Facade.php)
- [Código-fonte Report/Clover.php — tag 14.4.1](https://github.com/sebastianbergmann/php-code-coverage/blob/14.4.1/src/Report/Clover.php)
- [Changelog 14.4: JSONL e desempenho do merge](https://github.com/sebastianbergmann/php-code-coverage/blob/14.4.1/ChangeLog-14.4.md)
- [Documentação oficial PHPUnit — Code Coverage](https://docs.phpunit.de/en/13.0/code-coverage.html)
- [PHP — DOMDocument::loadXML](https://www.php.net/manual/pt_BR/domdocument.loadxml.php)
- [Composer — documentação](https://getcomposer.org/doc/)

**Créditos e licença:** o php-code-coverage é um projeto de Sebastian Bergmann e colaboradores, distribuído sob a licença BSD-3-Clause. Os códigos do laboratório e do importador apresentados aqui foram elaborados didaticamente para esta trilha; não representam uma cópia da implementação interna da biblioteca.

**Metadescrição:** Domine a arquitetura do PHP Code Coverage 14.4.1, entenda Filter, Driver e ReportFacade e crie um importador Clover em PHP para um futuro SaaS.

**Tags:** PHP, PHPUnit, cobertura de código, PHP Code Coverage, Xdebug, PCOV, Clover, XML, JSONL, Composer, testes automatizados, qualidade de software, CoverageRadar PHP, SaaS.
