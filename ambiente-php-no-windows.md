---
title: "Como configurar um ambiente PHP profissional no Windows"
seo_title: "Ambiente PHP no Windows: guia profissional completo"
slug: "ambiente-php-no-windows"
meta_description: "Aprenda a configurar um ambiente PHP no Windows com PHP 8, Composer, MySQL, Git, VS Code, autoload PSR-4 e um projeto prático."
focus_keyphrase: "ambiente PHP no Windows"
related_keywords:
  - "instalar PHP no Windows"
  - "configurar PHP no Windows"
  - "instalar Composer no Windows"
  - "PHP com MySQL"
  - "ambiente de desenvolvimento PHP"
  - "PHP Back-End"
category: "Programação"
tags:
  - "PHP"
  - "Back-End"
  - "Composer"
  - "MySQL"
  - "Git"
  - "Windows"
  - "Visual Studio Code"
excerpt: "Um guia prático para instalar e validar PHP, Composer, MySQL, Git e VS Code no Windows, criar um projeto com autoload PSR-4 e executar uma aplicação que responde em JSON."
featured_image_url: "https://upload.wikimedia.org/wikipedia/commons/thumb/2/27/PHP-logo.svg/1280px-PHP-logo.svg.png"
featured_image_alt: "Ambiente PHP no Windows representado pelo logotipo da linguagem PHP"
featured_image_title: "Ambiente PHP profissional no Windows"
featured_image_caption: "Logotipo da linguagem PHP, utilizado para representar a configuração de um ambiente de desenvolvimento Back-End."
---

# Como configurar um ambiente PHP profissional no Windows

Montar um **ambiente PHP no Windows** não significa apenas instalar um programa e criar um arquivo com a extensão `.php`. Um desenvolvedor Back-End precisa trabalhar com um conjunto de ferramentas que permita executar aplicações, instalar dependências, conectar bancos de dados, analisar erros e versionar o código.

Nesta primeira lição da trilha de preparação para vagas de desenvolvimento PHP, vamos configurar:

- PHP 8 em uma versão oficialmente suportada;
- Composer para gerenciamento de dependências;
- MySQL para persistência dos dados;
- Git para controle de versão;
- Visual Studio Code como editor;
- autoload PSR-4 para carregar classes automaticamente;
- um pequeno endpoint que responde em JSON.

Ao final, você terá um projeto funcional e uma base adequada para estudar PHP puro, orientação a objetos, APIs REST, Laravel, segurança, testes e desempenho.

## O que você aprenderá nesta lição

Ao concluir a configuração do **ambiente PHP no Windows**, você deverá ser capaz de:

1. executar o PHP pelo terminal;
2. identificar qual arquivo `php.ini` está sendo carregado;
3. habilitar extensões essenciais;
4. usar o Composer em qualquer diretório;
5. criar um banco e um usuário de aplicação no MySQL;
6. iniciar um repositório Git;
7. organizar classes com namespace e autoload PSR-4;
8. executar o servidor local do PHP;
9. retornar uma resposta JSON pelo navegador;
10. validar a sintaxe dos arquivos antes do commit.

## Ferramentas necessárias

Para acompanhar o artigo, você precisará das seguintes ferramentas:

| Ferramenta | Finalidade |
|---|---|
| **PHP 8** | Executar a linguagem e a aplicação |
| **Composer** | Gerenciar dependências e gerar o autoload |
| **MySQL** | Armazenar dados relacionais |
| **Git** | Controlar o histórico do código |
| **Visual Studio Code** | Editar e analisar os arquivos do projeto |
| **PowerShell** | Executar os comandos apresentados |

Prefira uma versão estável e oficialmente suportada do PHP. Para uma instalação nova, PHP 8.4 ou 8.5 são escolhas adequadas. Evite começar um projeto com versões que já chegaram ao fim do suporte de segurança.

## 1. Diagnóstico inicial do computador

Abra o PowerShell e execute:

```powershell
php -v
composer --version
git --version
mysql --version
code --version
```

Não há problema caso alguns comandos ainda não sejam reconhecidos. O objetivo é descobrir quais ferramentas já estão instaladas.

No final da lição, repetiremos os mesmos comandos para validar a configuração.

## 2. Como instalar PHP no Windows

A instalação manual ajuda a compreender onde o PHP está localizado, como o arquivo de configuração funciona e de que maneira as extensões são carregadas.

### Escolha do pacote

Na página oficial de downloads do PHP para Windows, escolha:

- uma versão estável do PHP 8;
- arquitetura **x64**;
- compilação **Non Thread Safe — NTS** para uso por linha de comando, servidor embutido ou FastCGI;
- arquivo compactado em formato ZIP.

A compilação Thread Safe é necessária em cenários específicos, como o carregamento do PHP diretamente como módulo do Apache. Nesta trilha utilizaremos inicialmente o terminal e o servidor embutido, portanto a versão NTS é suficiente.

### Extração dos arquivos

Extraia o conteúdo para uma pasta permanente. Um exemplo é:

```text
C:\tools\php
```

Você pode escolher outro local. O requisito importante é manter o PHP em uma pasta que não será apagada ou movida logo depois da configuração.

Confirme se o executável existe:

```text
C:\tools\php\php.exe
```

### Criar o arquivo php.ini

Dentro da pasta do PHP, localize:

```text
php.ini-development
```

Copie o arquivo e renomeie a cópia para:

```text
php.ini
```

Pelo PowerShell, considerando o caminho de exemplo:

```powershell
Copy-Item "C:\tools\php\php.ini-development" "C:\tools\php\php.ini"
```

O arquivo `php.ini-development` possui configurações apropriadas para desenvolvimento local. Em produção, a exibição de erros e outras opções precisam ser revisadas.

### Configurar o diretório das extensões

Abra o `php.ini` e procure:

```ini
;extension_dir = "ext"
```

Remova o ponto e vírgula:

```ini
extension_dir = "ext"
```

O ponto e vírgula indica um comentário em arquivos INI. Enquanto ele estiver presente, a configuração não será aplicada.

### Habilitar extensões importantes

Procure as extensões abaixo e remova o `;` do início das linhas:

```ini
extension=curl
extension=fileinfo
extension=mbstring
extension=mysqli
extension=openssl
extension=pdo_mysql
extension=zip
```

Essas extensões serão utilizadas em diferentes momentos da trilha:

| Extensão | Utilização |
|---|---|
| `curl` | Comunicação com serviços e APIs externas |
| `fileinfo` | Identificação segura do tipo de arquivo |
| `mbstring` | Manipulação de textos com caracteres multibyte |
| `mysqli` | Comunicação com MySQL pela extensão MySQLi |
| `openssl` | Criptografia e conexões seguras |
| `pdo_mysql` | Comunicação com MySQL usando PDO |
| `zip` | Leitura e criação de arquivos compactados |

### Configurar o fuso horário

Procure:

```ini
;date.timezone =
```

Para o horário de Brasília, configure:

```ini
date.timezone = America/Sao_Paulo
```

A aplicação deve trabalhar com datas de forma consciente. Em sistemas distribuídos, é comum persistir datas em UTC e convertê-las apenas na apresentação. Nesta primeira configuração, usaremos o fuso local para facilitar os exercícios.

### Adicionar PHP ao PATH

Adicionar o PHP ao `PATH` permite executar `php` em qualquer pasta.

No Windows:

1. pesquise por **Variáveis de Ambiente**;
2. abra **Editar as variáveis de ambiente do sistema**;
3. clique em **Variáveis de Ambiente**;
4. selecione `Path` nas variáveis do usuário;
5. clique em **Novo**;
6. informe a pasta que contém `php.exe`;
7. confirme as alterações;
8. feche e abra novamente o PowerShell.

Considerando o exemplo deste artigo, o valor seria:

```text
C:\tools\php
```

Não substitua todo o conteúdo da variável `Path`. Apenas acrescente uma nova entrada.

### Validar a instalação do PHP

Execute:

```powershell
php -v
php --ini
```

O primeiro comando exibe a versão do PHP. O segundo informa qual arquivo de configuração foi carregado.

Verifique também as extensões:

```powershell
php -m | Select-String "curl|fileinfo|mbstring|mysqli|openssl|PDO|pdo_mysql|zip"
```

Caso uma extensão não apareça:

1. confirme se o `php.ini` correto está sendo carregado;
2. verifique se o ponto e vírgula foi removido;
3. confirme o valor de `extension_dir`;
4. feche e abra o terminal;
5. execute `php -m` novamente e observe possíveis mensagens de erro.

## 3. Instalação do Composer

Composer é o gerenciador de dependências do ecossistema PHP. Ele permite declarar bibliotecas necessárias, controlar versões e carregar classes automaticamente.

No Windows, use o instalador oficial `Composer-Setup.exe`. Durante o processo, informe o caminho do `php.exe` configurado anteriormente.

Depois da instalação, abra um novo PowerShell e execute:

```powershell
composer --version
composer diagnose
```

O comando `composer diagnose` verifica problemas comuns, como configurações incorretas do PHP, conectividade, certificados e permissões.

### Composer não é um framework

É importante não confundir os conceitos:

- **PHP** é a linguagem e o runtime;
- **Composer** gerencia dependências;
- **Laravel**, **Symfony** e **CodeIgniter** são frameworks;
- **Packagist** é o principal repositório público de pacotes usado pelo Composer.

Mesmo projetos feitos com PHP puro podem e devem utilizar Composer.

## 4. Instalação e configuração do Git

Git registra a evolução dos arquivos e permite criar branches, comparar alterações, recuperar versões e colaborar por meio de plataformas como GitHub e GitLab.

Verifique se ele já está disponível:

```powershell
git --version
```

Caso não esteja instalado, utilize o instalador oficial do Git para Windows ou o Windows Package Manager:

```powershell
winget install --id Git.Git -e --source winget
```

Configure sua identidade substituindo os valores de exemplo:

```powershell
git config --global user.name "Seu Nome"
git config --global user.email "seu-email@example.com"
```

Defina `main` como nome padrão da branch inicial:

```powershell
git config --global init.defaultBranch main
```

No Windows, você também pode configurar o tratamento das quebras de linha:

```powershell
git config --global core.autocrlf true
```

Confira as configurações:

```powershell
git config --global --list
```

Use preferencialmente o mesmo endereço de e-mail associado à sua conta do GitHub. Caso deseje manter o e-mail privado, o GitHub fornece um endereço `noreply` para commits.

## 5. Visual Studio Code para PHP

O Visual Studio Code pode ser usado como editor principal da trilha.

Verifique a instalação:

```powershell
code --version
```

Instale as extensões:

- **PHP Intelephense** para análise, navegação e sugestões de código;
- **PHP Debug** para integração com Xdebug, que será configurado em outra lição;
- **EditorConfig** para padronizar indentação e final de linha;
- **GitLens** para ampliar a visualização do histórico Git.

Depois, configure o caminho do executável PHP nas configurações do VS Code quando necessário. O editor também pode usar o comando `php -l` para validar a sintaxe.

## 6. Instalação do MySQL

Para novos ambientes, prefira uma versão estável do MySQL Community Server, como a série 8.4 LTS. No Windows, o servidor pode ser instalado por um pacote MSI e configurado com o MySQL Configurator.

Também é possível instalar o MySQL Workbench separadamente para administrar bancos por uma interface gráfica.

Durante a configuração local, utilize valores semelhantes a estes:

```text
Tipo de configuração: Development Computer
Porta: 3306
Autenticação: senha forte
Serviço do Windows: inicialização automática
```

A porta `3306` é a porta convencional do MySQL. Ela pode ser alterada quando já existe outro serviço usando o mesmo endereço.

### Não use root na aplicação

O usuário `root` deve ser reservado para administração. Uma aplicação deve possuir uma conta própria e apenas as permissões necessárias.

Abra o MySQL Workbench ou o cliente de linha de comando e crie o banco da trilha:

```sql
CREATE DATABASE php_backend_senior
    CHARACTER SET utf8mb4
    COLLATE utf8mb4_0900_ai_ci;
```

Crie um usuário exclusivo:

```sql
CREATE USER 'php_app'@'localhost'
IDENTIFIED BY 'SUBSTITUA_POR_UMA_SENHA_FORTE';

GRANT ALL PRIVILEGES
ON php_backend_senior.*
TO 'php_app'@'localhost';

FLUSH PRIVILEGES;
```

A senha acima é apenas um marcador. Não use esse texto literalmente e nunca envie credenciais para o Git.

Verifique o cliente pelo terminal:

```powershell
mysql --version
```

Se o comando não for encontrado, adicione ao `PATH` a pasta que contém `mysql.exe`. O servidor e o Workbench podem estar funcionando mesmo quando o cliente ainda não está no `PATH`.

## 7. Criar o primeiro projeto PHP

Utilizaremos a pasta `dev` dentro do diretório do usuário. Isso evita depender de uma letra de unidade ou de um nome de usuário específico.

No PowerShell:

```powershell
New-Item -ItemType Directory -Force -Path "$HOME\dev\php-backend-senior"
Set-Location "$HOME\dev\php-backend-senior"

New-Item -ItemType Directory -Force -Path "public"
New-Item -ItemType Directory -Force -Path "src"
New-Item -ItemType Directory -Force -Path "tests"

code .
```

A estrutura inicial será:

```text
php-backend-senior/
├── public/
├── src/
└── tests/
```

Cada pasta terá uma responsabilidade:

- `public`: ponto de entrada acessível pelo navegador;
- `src`: classes da aplicação;
- `tests`: testes automatizados que serão adicionados posteriormente.

## 8. Configurar o Composer e o autoload PSR-4

Na raiz do projeto, crie o arquivo `composer.json`:

```json
{
    "name": "seu-usuario/php-backend-senior",
    "description": "Trilha prática de preparação para vagas PHP Back-End",
    "type": "project",
    "require": {
        "php": ">=8.4 <9.0"
    },
    "autoload": {
        "psr-4": {
            "App\\": "src/"
        }
    }
}
```

Substitua `seu-usuario` pelo nome utilizado em seu GitHub ou por outro identificador válido no formato esperado pelo Composer.

Execute:

```powershell
composer install
```

O Composer criará:

```text
composer.lock
vendor/
vendor/autoload.php
```

### O que é PSR-4?

PSR-4 é um padrão de autoload para classes PHP. Nesta configuração:

```json
"App\\": "src/"
```

estamos informando que classes cujo namespace começa com `App\` serão procuradas dentro da pasta `src`.

Por exemplo:

```text
Classe: App\EnvironmentCheck
Arquivo: src/EnvironmentCheck.php
```

Sem autoload, seria necessário espalhar vários comandos `require` e `include` pela aplicação.

## 9. Criar uma classe de diagnóstico

Crie o arquivo:

```text
src/EnvironmentCheck.php
```

Adicione:

```php
<?php

declare(strict_types=1);

namespace App;

final class EnvironmentCheck
{
    /**
     * @return array<string, mixed>
     */
    public static function report(): array
    {
        return [
            'php_version' => PHP_VERSION,
            'sapi' => PHP_SAPI,
            'operating_system' => PHP_OS_FAMILY,
            'timezone' => date_default_timezone_get(),
            'generated_at' => date(DATE_ATOM),
            'extensions' => [
                'curl' => extension_loaded('curl'),
                'fileinfo' => extension_loaded('fileinfo'),
                'mbstring' => extension_loaded('mbstring'),
                'openssl' => extension_loaded('openssl'),
                'pdo' => extension_loaded('pdo'),
                'pdo_mysql' => extension_loaded('pdo_mysql'),
                'zip' => extension_loaded('zip'),
            ],
        ];
    }
}
```

### Entendendo a classe

A instrução:

```php
declare(strict_types=1);
```

ativa um comportamento mais rigoroso para tipos escalares nas chamadas realizadas a partir daquele arquivo.

O namespace:

```php
namespace App;
```

corresponde ao prefixo configurado no `composer.json`.

A declaração:

```php
final class EnvironmentCheck
```

impede que a classe seja herdada. Isso não é obrigatório para todas as classes. Neste caso, comunica que a classe foi criada como utilitário fechado, sem extensão por herança.

A função:

```php
extension_loaded('pdo_mysql')
```

verifica se a extensão necessária para conectar PHP e MySQL por PDO está ativa.

## 10. Criar o ponto de entrada da aplicação

Crie:

```text
public/index.php
```

Adicione:

```php
<?php

declare(strict_types=1);

use App\EnvironmentCheck;

require dirname(__DIR__) . '/vendor/autoload.php';

header('Content-Type: application/json; charset=utf-8');

$environment = EnvironmentCheck::report();
$pdoMySqlAvailable = $environment['extensions']['pdo_mysql'] === true;

if (!$pdoMySqlAvailable) {
    http_response_code(500);
}

$response = [
    'status' => $pdoMySqlAvailable ? 'ok' : 'error',
    'message' => $pdoMySqlAvailable
        ? 'Ambiente PHP configurado com sucesso.'
        : 'A extensão pdo_mysql não está habilitada.',
    'environment' => $environment,
];

echo json_encode(
    $response,
    JSON_PRETTY_PRINT
    | JSON_UNESCAPED_UNICODE
    | JSON_THROW_ON_ERROR
);
```

### Carregamento automático das classes

Esta linha carrega o arquivo gerado pelo Composer:

```php
require dirname(__DIR__) . '/vendor/autoload.php';
```

Depois disso, a classe `App\EnvironmentCheck` pode ser utilizada sem um `require` específico para o arquivo dela.

### Cabeçalho da resposta

A linha:

```php
header('Content-Type: application/json; charset=utf-8');
```

informa ao cliente que a resposta contém JSON codificado em UTF-8.

### Status HTTP

Quando `pdo_mysql` não está disponível, o código executa:

```php
http_response_code(500);
```

O status HTTP 500 informa que o servidor não conseguiu atender corretamente à requisição. Em APIs profissionais, o status HTTP e o corpo JSON precisam representar a mesma situação.

## 11. Executar o servidor local

Na raiz do projeto, execute:

```powershell
php -S localhost:8000 -t public
```

Abra no navegador:

```text
http://localhost:8000
```

O resultado deverá ser semelhante a:

```json
{
    "status": "ok",
    "message": "Ambiente PHP configurado com sucesso.",
    "environment": {
        "php_version": "8.x.x",
        "sapi": "cli-server",
        "operating_system": "Windows",
        "timezone": "America/Sao_Paulo",
        "generated_at": "2026-07-26T12:00:00-03:00",
        "extensions": {
            "curl": true,
            "fileinfo": true,
            "mbstring": true,
            "openssl": true,
            "pdo": true,
            "pdo_mysql": true,
            "zip": true
        }
    }
}
```

O servidor embutido do PHP é apropriado para desenvolvimento e exercícios locais. Ele não deve ser usado como servidor público de produção.

Para interrompê-lo, volte ao terminal e pressione:

```text
Ctrl + C
```

## 12. Validar a sintaxe do código

Antes de executar ou versionar os arquivos, use o lint do PHP:

```powershell
php -l src/EnvironmentCheck.php
php -l public/index.php
```

O resultado esperado é:

```text
No syntax errors detected
```

O comando `php -l` encontra erros de sintaxe, mas não garante que a regra de negócio esteja correta. Mais adiante, adicionaremos análise estática e testes automatizados.

## 13. Criar o arquivo .gitignore

Na raiz, crie `.gitignore`:

```gitignore
/vendor/
/.idea/
/.vscode/
.env
*.log
```

A pasta `vendor` não deve ser versionada porque pode ser reconstruída com:

```powershell
composer install
```

Já o arquivo `composer.lock` deve ser enviado ao repositório em projetos de aplicação. Ele registra as versões exatas instaladas e ajuda a manter ambientes reproduzíveis.

Credenciais e segredos devem ficar fora do Git. Quando criarmos a conexão com o banco, utilizaremos um arquivo `.env`, que já está ignorado.

## 14. Criar os primeiros commits

Inicialize o repositório:

```powershell
git init
git add .
git status
git commit -m "chore: configura ambiente inicial PHP"
```

Confira o histórico:

```powershell
git log --oneline
```

O commit deve representar uma alteração coerente. Evite mensagens vagas como `alterações`, `teste` ou `arquivos novos`.

Um padrão simples para mensagens é:

```text
tipo: descrição objetiva
```

Exemplos:

```text
chore: configura ambiente inicial PHP
feat: adiciona diagnóstico das extensões
fix: trata ausência da extensão pdo_mysql
```

## Exercício guiado

Adicione ao relatório a quantidade de memória máxima permitida pelo PHP.

No método `report()`, inclua:

```php
'memory_limit' => ini_get('memory_limit'),
```

A resposta deverá conter algo semelhante a:

```json
"memory_limit": "128M"
```

Depois:

1. valide a sintaxe;
2. abra a aplicação no navegador;
3. confira o novo campo;
4. faça um commit separado.

```powershell
php -l src/EnvironmentCheck.php
git add .
git commit -m "feat: adiciona limite de memória ao diagnóstico"
```

## Desafio da lição

Adicione uma propriedade chamada `ready` à resposta.

Ela deverá ser:

```json
"ready": true
```

somente quando todas estas extensões estiverem habilitadas:

- `curl`;
- `mbstring`;
- `openssl`;
- `pdo`;
- `pdo_mysql`.

Caso uma delas esteja ausente:

- `ready` deverá ser `false`;
- `status` deverá ser `error`;
- a resposta deverá utilizar HTTP 500;
- o JSON deverá informar quais extensões estão ausentes.

Uma saída possível é:

```json
{
    "status": "error",
    "ready": false,
    "missing_extensions": [
        "pdo_mysql"
    ]
}
```

Tente resolver o desafio sem copiar uma implementação pronta. O objetivo é praticar arrays, condições e construção de respostas HTTP.

## Perguntas comuns em entrevistas PHP

### Qual é a diferença entre PHP e Composer?

PHP é a linguagem e o ambiente de execução. Composer é uma ferramenta que gerencia bibliotecas, versões e autoload.

### Para que serve o php.ini?

O `php.ini` controla o comportamento do PHP. Ele permite configurar extensões, fuso horário, limites de memória, upload, exibição de erros e outras opções.

### O que é uma extensão PHP?

É um módulo que adiciona funcionalidades ao runtime. Exemplos incluem conexão com bancos, criptografia, compactação e manipulação avançada de texto.

### O que faz declare(strict_types=1)?

Ele torna mais rigorosa a coerção de tipos escalares nas chamadas feitas a partir daquele arquivo. Não transforma o PHP inteiro em uma linguagem de tipagem estrita e não substitui testes ou análise estática.

### Para que serve o autoload?

O autoload encontra e carrega classes quando elas são utilizadas, reduzindo a necessidade de incluir manualmente cada arquivo.

### O que é PSR-4?

É uma especificação que relaciona namespaces a diretórios e nomes de arquivos. Ela é amplamente usada pelo Composer.

### O servidor php -S pode ser usado em produção?

Não. Ele foi criado para desenvolvimento e testes locais, sem os recursos necessários para operar como servidor público de produção.

### Por que uma aplicação não deve acessar o MySQL como root?

Porque o princípio do menor privilégio determina que cada aplicação receba somente as permissões necessárias. Uma credencial administrativa aumenta o impacto de falhas e invasões.

### Qual é a diferença entre mysqli e PDO?

MySQLi é voltada ao MySQL. PDO oferece uma interface consistente para diferentes bancos suportados por drivers. Nesta trilha, utilizaremos principalmente PDO com prepared statements.

## Checklist de conclusão

Considere a configuração do **ambiente PHP no Windows** concluída quando estes comandos funcionarem:

```powershell
php -v
php --ini
composer --version
composer diagnose
git --version
mysql --version
code --version
```

Também confirme que:

- o PHP carrega o `php.ini` esperado;
- as extensões importantes aparecem em `php -m`;
- `composer install` cria a pasta `vendor`;
- o autoload encontra `App\EnvironmentCheck`;
- `php -S localhost:8000 -t public` inicia o servidor;
- o navegador recebe JSON;
- o campo `status` apresenta `ok`;
- os arquivos passam pelo comando `php -l`;
- o projeto possui ao menos um commit Git.

## Próximos passos

Com o ambiente pronto, a próxima lição pode avançar para o funcionamento de uma aplicação web:

- cliente e servidor;
- requisições e respostas;
- HTTP e HTTPS;
- métodos GET, POST, PUT, PATCH e DELETE;
- cabeçalhos;
- cookies;
- sessões;
- códigos de status HTTP.

Essa base será usada posteriormente na criação de formulários, autenticação, APIs REST e integrações entre PHP, MySQL e interfaces desenvolvidas com Bootstrap e jQuery.

## Referências oficiais

- [Versões do PHP com suporte](https://www.php.net/supported-versions.php)
- [Instalação manual do PHP no Windows](https://www.php.net/manual/pt_BR/install.windows.manual.php)
- [PHP no terminal do Windows](https://www.php.net/manual/pt_BR/install.windows.commandline.php)
- [Servidor web embutido do PHP](https://www.php.net/manual/pt_BR/features.commandline.webserver.php)
- [Instalação do Composer](https://getcomposer.org/doc/00-intro.md)
- [Especificação PSR-4](https://www.php-fig.org/psr/psr-4/)
- [Git para Windows](https://git-scm.com/install/windows)
- [Instalação do MySQL no Windows](https://dev.mysql.com/doc/refman/8.4/en/windows-installation.html)
