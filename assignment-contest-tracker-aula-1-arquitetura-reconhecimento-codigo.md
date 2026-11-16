---
title: "Assignment Contest Tracker — Aula 1: Arquitetura e Reconhecimento do Código"
seo_title: "Arquitetura Full Stack: React, Express e MongoDB na prática"
slug: "assignment-contest-tracker-aula-1-arquitetura-reconhecimento-codigo"
meta_description: "Estude a arquitetura de um projeto full stack real com React, TypeScript, Express e MongoDB. Entenda as rotas REST, o fluxo HTTP e os riscos do código."
focus_keyphrase: "arquitetura full stack React Express MongoDB"
related_keywords:
  - "Assignment Contest Tracker"
  - "arquitetura de software na prática"
  - "React e Express"
  - "TypeScript full stack"
  - "Node.js e MongoDB"
  - "API REST com Express"
  - "análise de repositório GitHub"
  - "frontend e backend"
  - "segurança JWT"
  - "Codeforces CodeChef LeetCode"
category: "Desenvolvimento Full Stack"
tags:
  - "React"
  - "TypeScript"
  - "Node.js"
  - "Express"
  - "MongoDB"
  - "Mongoose"
  - "API REST"
  - "Arquitetura de Software"
  - "GitHub"
  - "Segurança"
  - "Aprendizado Prático"
excerpt: "Na Aula 1, vamos ler o código real do Assignment Contest Tracker, mapear frontend, backend, rotas e integrações, acompanhar uma requisição HTTP e identificar problemas antes de executar o projeto."
---

# Assignment Contest Tracker — Aula 1: Arquitetura e Reconhecimento do Código

Quando você encontra uma aplicação completa no GitHub, por onde começa: instala as dependências, abre o primeiro arquivo ou tenta entender o problema que ela resolve?

Nesta formação, vamos seguir um caminho diferente de simplesmente executar comandos. Nosso laboratório será o projeto público [dgvijaytorg/assignment](https://github.com/dgvijaytorg/assignment), denominado **Assignment Contest Tracker and Solution Uploader** e associado, no README original, ao nome *TLE Eliminators Assignment*.

Ele é um exemplo real de aplicação **full stack**: tem uma interface em React, um servidor em Express, persistência com MongoDB e integrações com serviços externos. Também contém limitações técnicas interessantes para o aprendizado.

Nesta primeira aula, nossa tarefa é **reconhecer a arquitetura, seguir o percurso de uma requisição e separar o que o código realmente faz do que sua documentação promete**. Não é necessário executar o projeto.

> **Nota editorial:** este é um estudo técnico independente, baseado na leitura do código público do repositório. Não é uma publicação oficial dos autores nem uma certificação. As análises de segurança são constatações de revisão estática: não representam uma auditoria completa nem um teste de invasão. Os exemplos didáticos adicionais são explicações próprias.

## 1. O que vamos aprender

Ao concluir a aula, você deverá conseguir:

- explicar a finalidade do Assignment Contest Tracker;
- distinguir **frontend**, **backend**, **persistência** e **APIs externas**;
- reconhecer os papéis de React, Vite, TypeScript, Express, Mongoose e MongoDB;
- navegar na estrutura de pastas do repositório;
- identificar o arquivo de entrada do servidor e o registro das rotas;
- acompanhar uma chamada HTTP, da interface até a resposta em JSON;
- entender a diferença entre dados consultados de terceiros e dados persistidos pela aplicação;
- reconhecer problemas iniciais de autenticação, configuração e tratamento de erros;
- justificar por que devemos revisar o código antes de executar dependências ou arquivos desconhecidos.

**Pré-requisito:** saber navegar por arquivos e diretórios no GitHub. Conhecimento prévio de JavaScript, React ou Node.js ajuda, mas não é obrigatório para esta etapa.

## 2. Qual problema o projeto tenta resolver?

Imagine acompanhar competições de programação em três plataformas: **Codeforces**, **CodeChef** e **LeetCode**.

Sem um agregador, seria necessário visitar sites distintos para verificar os próximos eventos, consultar competições encerradas e encontrar material relacionado.

O Assignment Contest Tracker reúne essas informações em uma única interface e oferece funcionalidades como:

1. listar competições futuras;
2. consultar competições anteriores;
3. filtrar resultados pela plataforma;
4. marcar competições como favoritas (*bookmarks*);
5. autenticar usuários por meio do Google;
6. associar links de vídeos com soluções de competições;
7. executar uma rotina agendada que procura vídeos e tenta relacioná-los a eventos.

Aqui há uma distinção importante: o nome e o README falam em *upload* de soluções, mas o código da rotina agendada que examinamos **consulta vídeos no YouTube e armazena links no MongoDB**. Ele não demonstra a publicação de um vídeo novo em um canal.

**Primeira lição de engenharia:** a descrição de um produto é uma hipótese sobre seu comportamento. O código, os testes e a execução controlada fornecem evidências mais concretas.

## 3. O que significa ser uma aplicação full stack?

Uma aplicação full stack possui componentes que trabalham em camadas diferentes.

| Parte | Responsabilidade | Tecnologias observadas |
|---|---|---|
| Frontend | Mostrar telas e reagir a ações do usuário | React, TypeScript, Vite |
| Comunicação | Transportar solicitações e respostas | HTTP, JSON, Axios |
| Backend | Expor rotas e executar regras da aplicação | Node.js, Express |
| Persistência | Armazenar usuários, favoritos e links | MongoDB, Mongoose |
| Integrações | Consultar dados externos | APIs de competições e YouTube |
| Agendamento | Executar tarefas sem interação manual | node-cron |

O frontend não precisa conhecer os detalhes internos de todas as plataformas de competição. Ele conversa com uma API própria, que pode realizar consultas e devolver uma representação padronizada dos resultados.

Essa separação reduz o acoplamento entre a interface e as diversas fontes de dados — embora não elimine a necessidade de tratar falhas de cada integração.

## 4. Desenhando a arquitetura do sistema

Uma representação simplificada do fluxo é:

~~~text
                  USUÁRIO
                     |
                     v
                NAVEGADOR
                     |
                     v
       FRONTEND — React + TypeScript
          páginas, hooks e estado
                     |
             HTTP / JSON (Axios)
                     |
                     v
         BACKEND — Node.js + Express
          roteadores e controladores
                 /       \
                /         \
               v           v
       MongoDB/Mongoose   APIs externas
       usuários, favoritos Codeforces
       e links de solução  CodeChef
                           LeetCode
                           YouTube
~~~

Há também um processo agendado no backend que busca vídeos e grava associações no banco.

Não confunda os conceitos:

- **React** é uma biblioteca de interface, não um banco de dados.
- **Express** é o framework HTTP que organiza o servidor, não a plataforma de competições.
- **Mongoose** é uma biblioteca que facilita o trabalho com documentos do MongoDB.
- **MongoDB** guarda documentos da aplicação; não é a origem de todas as competições exibidas.
- **Axios** envia requisições HTTP. Ele não substitui a API nem o banco.

A arquitetura acima é uma abstração didática das responsabilidades identificadas no código, não um diagrama oficial do projeto.

## 5. Explorando a estrutura real do repositório

O projeto possui dois diretórios principais e um README na raiz:

~~~text
assignment/
├── backend/
│   ├── src/
│   │   ├── controllers/
│   │   ├── cron/
│   │   ├── middleware/
│   │   ├── models/
│   │   ├── routers/
│   │   ├── types/
│   │   └── index.ts
│   ├── docker-compose.yml
│   ├── package.json
│   └── tsconfig.json
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── pages/
│   │   ├── store/
│   │   ├── types/
│   │   ├── App.tsx
│   │   └── main.tsx
│   ├── package.json
│   └── vite.config.ts
└── readme.md
~~~

Para explorar:

- [Código do backend](https://github.com/dgvijaytorg/assignment/tree/main/backend/src)
- [Código do frontend](https://github.com/dgvijaytorg/assignment/tree/main/frontend/src)
- [README original](https://github.com/dgvijaytorg/assignment/blob/main/readme.md)

**Como interpretar essas pastas?**

A pasta `routers` declara caminhos da API. A pasta `controllers` concentra funções que atendem às solicitações. Os `models` descrevem documentos usados com o MongoDB. Os arquivos em `cron` tratam tarefas agendadas.

No frontend, `pages` reúne telas; `components`, elementos reutilizáveis; `hooks`, comportamentos reutilizáveis do React; e `store`, estado compartilhado.

Esses nomes descrevem uma organização escolhida pelos autores. Eles não são uma regra obrigatória de React ou Express.

## 6. Encontrando o ponto de entrada do backend

Abra o arquivo [`backend/src/index.ts`](https://github.com/dgvijaytorg/assignment/blob/main/backend/src/index.ts).

Ele executa, em linhas gerais, as seguintes tarefas:

1. carrega bibliotecas e variáveis de ambiente;
2. cria a instância do Express;
3. habilita o processamento de JSON;
4. configura o CORS;
5. tenta se conectar ao MongoDB;
6. registra as rotas de autenticação, competições e usuários;
7. inicia o servidor HTTP;
8. inicia a rotina agendada de vídeos depois que o MongoDB se conecta.

Observe um trecho ilustrativo extraído da lógica do projeto:

~~~ts
const app = express();
app.use(express.json());
app.use(cors());

app.use("/auth", auth_router);
app.use("/contests", contest_router);
app.use("/user", user_router);
~~~

O primeiro comando cria a aplicação Express. O segundo prepara o servidor para interpretar corpos JSON. O terceiro habilita o middleware CORS com a configuração padrão. Os demais associam prefixos de URL a roteadores.

Assim, a rota `/contests/past_contest` combina o prefixo `/contests` com o caminho declarado no roteador.

**Atenção:** uma configuração aberta de CORS não equivale a autenticação. Mesmo com políticas de origem restritas, o backend precisa validar as permissões de quem chama endpoints sensíveis.

Outro detalhe: o servidor chama `app.listen` independentemente do sucesso da conexão ao banco. Por isso, a mensagem de que o servidor iniciou não comprova, sozinha, que todas as dependências estejam funcionando.

## 7. Entendendo a diferença entre rota e controlador

Vamos abrir [`backend/src/routers/contest_router.ts`](https://github.com/dgvijaytorg/assignment/blob/main/backend/src/routers/contest_router.ts).

O arquivo associa métodos e caminhos HTTP a funções que executam as operações:

~~~ts
contest_router.get(
  "/upcomming_contest",
  contest_controllers.fetchUpcommingContest
);

contest_router.get(
  "/past_contest",
  contest_controllers.fetchPastContest
);

contest_router.post(
  "/upload_solution",
  contest_controllers.uploadVideoSolutionLink
);
~~~

O **roteador** decide *qual função será chamada*. O **controlador** executa o processamento da requisição.

Note que o nome `upcomming_contest` aparece com uma grafia incomum de *upcoming*. Mantemos a grafia original ao estudar o código, porque alterar um caminho em apenas um lado da comunicação quebraria as chamadas existentes.

Em uma melhoria futura, poderíamos introduzir rotas padronizadas e migrar os consumidores de maneira controlada.

## 8. Acompanhando uma requisição do início ao fim

Vamos seguir a operação **Consultar próximas competições**.

### Passo 1 — A interface é renderizada

A página [`frontend/src/pages/Home.tsx`](https://github.com/dgvijaytorg/assignment/blob/main/frontend/src/pages/Home.tsx) utiliza um hook personalizado chamado `useUpcommingContests`.

### Passo 2 — O hook consulta a API

No arquivo [`frontend/src/hooks/UseUpcommingContests.tsx`](https://github.com/dgvijaytorg/assignment/blob/main/frontend/src/hooks/UseUpcommingContests.tsx), o Axios faz uma chamada GET para a rota de competições futuras:

~~~ts
const response = await axios.get(
  `${import.meta.env.VITE_PROD_BACKEND_URL}/contests/upcomming_contest`
);

setUpcomingContests(response.data.upcomingContests);
~~~

O código acima é uma leitura do comportamento existente, não uma recomendação para usar a URL de produção em desenvolvimento.

### Passo 3 — O Express localiza a rota

O prefixo `/contests` está registrado em `index.ts`. O roteador identifica `/upcomming_contest` e encaminha para `fetchUpcommingContest`.

### Passo 4 — O controlador consulta as fontes

No arquivo [`backend/src/controllers/contests.ts`](https://github.com/dgvijaytorg/assignment/blob/main/backend/src/controllers/contests.ts), a função chama as plataformas externas.

A implementação utiliza `Promise.all` para iniciar as requisições em conjunto. Depois, filtra e transforma os resultados de cada plataforma para uma estrutura equivalente.

### Passo 5 — O backend organiza e responde

Os dados são combinados e ordenados pela data de início da competição. A resposta HTTP inclui o campo `upcomingContests`.

### Passo 6 — O React atualiza a tela

O hook guarda o resultado no estado. A página `Home` filtra os dados conforme as plataformas selecionadas e os apresenta em cartões.

O percurso resumido é:

~~~text
Home.tsx
   |
   v
useUpcommingContests()
   |
   v
GET /contests/upcomming_contest
   |
   v
contest_router.ts
   |
   v
fetchUpcommingContest()
   |
   v
APIs externas → normalização → ordenação
   |
   v
JSON { upcomingContests: [...] }
   |
   v
React atualiza a interface
~~~

Se você conseguir explicar esse caminho sem consultar o diagrama, terá compreendido uma parte fundamental da arquitetura.

## 9. Por que padronizar os dados recebidos?

As plataformas não devolvem, necessariamente, os mesmos nomes de propriedades ou representações de datas.

O controlador resolve essa diferença criando objetos com campos como:

| Campo | Significado |
|---|---|
| `id` | Identificador da competição |
| `title` | Nome apresentado ao usuário |
| `type` | Plataforma de origem |
| `link` | Endereço da competição |
| `start_date` | Data e hora de início |
| `end_date` | Data e hora de término |
| `duration` | Duração |
| `video_solution` | Link associado, quando existir |
| `status` | Situação do evento |

Exemplo **fictício**, apenas para ilustrar a estrutura de um item:

~~~json
{
  "id": "12345",
  "title": "Competição de exemplo",
  "type": "Codeforces",
  "link": "https://codeforces.com/contests/12345",
  "start_date": "2026-11-20T14:00:00.000Z",
  "end_date": "2026-11-20T16:00:00.000Z",
  "duration": 120,
  "video_solution": "",
  "status": "upcoming"
}
~~~

O exemplo **não foi retornado pela API** e o identificador não representa uma competição verificada.

**Por que isso importa?** Uma interface única pode renderizar objetos de várias origens sem conhecer o formato de cada serviço externo. Esse processo é conhecido como **normalização** ou adaptação dos dados.

Uma limitação da implementação atual é o `Promise.all` conjunto: se uma das chamadas externas falhar, a operação pode cair no tratamento geral de erro, mesmo que outras fontes estejam disponíveis. Em uma refatoração, podemos avaliar `Promise.allSettled`, timeouts e respostas parciais.

## 10. Consultando competições anteriores e salvando favoritos

O controlador `fetchPastContest` consulta eventos encerrados. O código também procura links de soluções já armazenados no banco, associando-os às competições retornadas.

Os favoritos seguem outra lógica. O usuário seleciona uma competição, e o backend registra essa associação no MongoDB.

O arquivo [`backend/src/controllers/user.ts`](https://github.com/dgvijaytorg/assignment/blob/main/backend/src/controllers/user.ts) contém funções para obter o perfil, adicionar uma competição aos favoritos e recuperar os favoritos do usuário.

Esse contraste ajuda a compreender duas categorias de dados:

- **Dados de referência externos:** informações de competições que podem ser consultadas nas plataformas.
- **Dados específicos do produto:** perfil, seleção de favoritos e links de soluções associados à aplicação.

A arquitetura poderia, no futuro, armazenar cache de dados externos para melhorar disponibilidade e reduzir chamadas. Isso seria uma evolução; não devemos afirmar que já existe um cache completo sem encontrá-lo no código.

## 11. Quais endpoints REST identificamos?

A partir dos roteadores, encontramos:

| Método | Endpoint | Objetivo |
|---|---|---|
| GET | `/contests/upcomming_contest` | Consultar próximas competições |
| GET | `/contests/past_contest` | Consultar competições passadas |
| POST | `/contests/upload_solution` | Registrar ou atualizar link de solução |
| POST | `/auth/google` | Processar login via Google |
| GET | `/user/profile` | Consultar perfil |
| POST | `/user/bookmarks` | Adicionar favorito |
| GET | `/user/bookmarks` | Listar favoritos |

**GET** costuma representar consultas sem alteração intencional do estado do recurso. **POST** envia dados para processamento e frequentemente é utilizado em operações que criam ou modificam informações.

Essa tabela representa as rotas encontradas na revisão do código. Ela não substitui uma especificação formal de API com contratos, esquemas e respostas de erro.

## 12. Entendendo a automação com node-cron

O arquivo [`backend/src/cron/video_link_fetcher.ts`](https://github.com/dgvijaytorg/assignment/blob/main/backend/src/cron/video_link_fetcher.ts) agenda a verificação de vídeos.

A expressão de agendamento utilizada é:

~~~ts
cron.schedule("0 8 * * *", async () => {
  // Consulta vídeos e procura competições correspondentes.
});
~~~

Ela indica execução diária às 8 horas **no fuso horário usado pelo processo**, caso não exista uma configuração explícita que altere esse comportamento. Não devemos presumir que será sempre o horário de Brasília.

A rotina consulta vídeos de um canal específico usando a API do YouTube, analisa títulos com expressões regulares e procura correspondências com competições do Codeforces, CodeChef e LeetCode.

Quando reconhece uma associação e não encontra um registro existente para aquele identificador, grava o link da solução.

Isso mostra um caso prático de **automação recorrente**: integrar serviços, aplicar regras de correspondência e persistir resultados sem depender de um clique do usuário.

Também revela possíveis limitações: títulos podem mudar, padrões de nomenclatura podem variar, APIs podem impor limites e o agendamento pode deixar de funcionar se o processo estiver parado.

## 13. Segurança: o que precisa ser revisto antes de executar?

Uma parte importante do estudo é identificar riscos concretos sem transformar toda suspeita em vulnerabilidade comprovadamente explorada.

### 13.1 Decodificar JWT não é validar JWT

No arquivo [`backend/src/routers/auth_router.ts`](https://github.com/dgvijaytorg/assignment/blob/main/backend/src/routers/auth_router.ts), o sistema utiliza:

~~~ts
const json_data = jwt.decode(token);
~~~

A mesma abordagem aparece no [middleware de autenticação](https://github.com/dgvijaytorg/assignment/blob/main/backend/src/middleware/middleware.ts).

A função `decode` lê o conteúdo do token, mas **não verifica sua assinatura criptográfica**. Assim, confiar diretamente no e-mail declarado pelo token é uma decisão insegura.

Para autenticação com Google, a evolução apropriada é verificar o ID token no backend, incluindo assinatura, emissor, audiência e validade, usando uma biblioteca adequada, como `google-auth-library`.

Referência: [Google Identity — verificar ID tokens no backend](https://developers.google.com/identity/gsi/web/guides/verify-google-id-token).

### 13.2 Endpoint sensível sem middleware de autenticação

A rota `POST /contests/upload_solution` aparece declarada sem middleware de autenticação no roteador de competições.

O controlador modifica registros de links no MongoDB. Portanto, precisamos definir quem possui permissão para realizar essa alteração e aplicar autenticação e autorização no servidor.

### 13.3 Configuração previsível do banco

O arquivo [`backend/docker-compose.yml`](https://github.com/dgvijaytorg/assignment/blob/main/backend/docker-compose.yml) inclui credenciais previsíveis para o MongoDB e publica portas no host.

Em um ambiente local isolado, configurações de demonstração ajudam a explicar os serviços. Porém, não devem ser copiadas diretamente para um servidor acessível por outras pessoas.

Na etapa de instalação, vamos revisar credenciais, volumes, portas expostas e a necessidade de interfaces administrativas.

### 13.4 Tratamento de ausência de usuário

No arquivo [`frontend/src/App.tsx`](https://github.com/dgvijaytorg/assignment/blob/main/frontend/src/App.tsx), a busca do usuário atual pode retornar `null`. Em seguida, o código tenta acessar propriedades como `currentUser.name`, sem uma guarda explícita naquele trecho.

Isso pode causar erro quando a aplicação é aberta sem uma sessão válida.

Uma melhoria simples seria conferir se o objeto existe antes de ler seus campos e representar explicitamente o estado de visitante.

### 13.5 Ambientes de desenvolvimento e produção misturados

O frontend declara variáveis de ambiente para URLs de desenvolvimento e produção, mas alguns componentes chamam diretamente `VITE_PROD_BACKEND_URL`.

Além disso, a configuração de desenvolvimento aponta para a porta 4000, enquanto o backend usa a porta 5000 como padrão.

Essas escolhas podem provocar chamadas para o servidor errado e dificultar os testes locais.

### 13.6 Arquivo compactado desconhecido

O repositório contém o arquivo `frontend/src/lib/Software-1.9.zip` e o README oferece um link para download.

Não precisamos baixar ou executar esse pacote para estudar o projeto. Antes de utilizar um binário ou arquivo compactado de origem desconhecida, é necessário verificar sua procedência, conteúdo e riscos.

**Regra prática:** primeiro lemos e compreendemos o código; depois preparamos um ambiente isolado e controlado. Não há necessidade de executar esse ZIP nesta formação.

## 14. Como transformar problemas em plano de melhoria?

A análise inicial pode ser organizada assim:

| Área | Observação | Melhoria proposta |
|---|---|---|
| Autenticação | Uso de `jwt.decode` como base de confiança | Verificar criptograficamente ID tokens |
| Autorização | Rota de solução sem proteção declarada | Exigir identidade e permissão |
| Configuração | Credenciais previsíveis e portas publicadas | Isolar serviços e parametrizar segredos |
| Frontend | Acesso a propriedades quando usuário pode ser `null` | Tratar estado não autenticado |
| Integrações | `Promise.all` pode falhar integralmente | Considerar falhas parciais e timeout |
| Desenvolvimento | URLs e portas potencialmente inconsistentes | Centralizar configuração por ambiente |
| Qualidade | Backend não declara scripts úteis de teste e execução | Organizar build, execução e testes |

Essa lista é um **backlog didático preliminar**, não uma classificação definitiva de severidade baseada em exploração.

Vamos abordar esses tópicos de forma progressiva nas próximas aulas.

## 15. Exercício prático: investigação somente pela interface do GitHub

Nesta primeira prática, **não vamos instalar dependências nem iniciar contêineres**.

### Exercício 1 — Identifique a inicialização do servidor

Abra [`backend/src/index.ts`](https://github.com/dgvijaytorg/assignment/blob/main/backend/src/index.ts).

Encontre:

- onde a instância do Express é criada;
- onde o MongoDB é conectado;
- onde os roteadores são registrados;
- onde `app.listen` inicia o servidor;
- em que momento a rotina do YouTube é iniciada.

**Pergunta:** o servidor HTTP espera a conexão com MongoDB ser concluída para chamar `app.listen`?

### Exercício 2 — Siga uma rota

Abra [`backend/src/routers/contest_router.ts`](https://github.com/dgvijaytorg/assignment/blob/main/backend/src/routers/contest_router.ts).

Localize o endpoint `/upcomming_contest` e identifique o nome do controlador que ele chama.

Depois, abra [`backend/src/controllers/contests.ts`](https://github.com/dgvijaytorg/assignment/blob/main/backend/src/controllers/contests.ts) e encontre a função correspondente.

**Pergunta:** quais são as três plataformas consultadas?

### Exercício 3 — Encontre a ordenação

Ainda no controlador, procure o trecho que reúne as listas de competições futuras e as ordena pela data de início.

**Pergunta:** o código apresenta primeiro os eventos com início mais próximo ou os mais distantes?

### Exercício 4 — Identifique um risco

Abra o [middleware de autenticação](https://github.com/dgvijaytorg/assignment/blob/main/backend/src/middleware/middleware.ts).

Procure a chamada a `jwt.decode`.

**Pergunta:** por que ler o e-mail de um token não é suficiente para autenticar o usuário?

### Modelo de registro do laboratório

~~~text
Arquivo analisado:
Função ou rota encontrada:
O que a função faz:
De qual componente recebe dados:
Para qual componente envia dados:
Dúvida ou risco identificado:
~~~

Utilize esse pequeno registro sempre que estudarmos uma nova parte do projeto. Ele ajudará a documentar o raciocínio, não apenas decorar nomes de funções.

## 16. Questões de revisão

Tente responder antes de consultar o gabarito.

**1. Qual componente consulta as plataformas de competição e transforma os resultados?**

A) MongoDB  
B) Controlador do backend  
C) Navbar do React  
D) Docker Compose

**2. O que acontece quando o frontend chama `GET /contests/upcomming_contest`?**

A) O navegador acessa diretamente as coleções do MongoDB  
B) O Express encaminha a requisição ao controlador configurado  
C) O Docker Compose cria a interface  
D) O GitHub executa o cron

**3. Por que o backend normaliza os dados?**

A) Para que todas as APIs externas tenham de alterar seus formatos  
B) Para facilitar a apresentação de dados de fontes diferentes em uma estrutura comum  
C) Para eliminar a necessidade de HTTP  
D) Para substituir a autenticação

**4. O uso isolado de `jwt.decode` comprova a autenticidade de um token?**

A) Sim, porque o token tem três partes  
B) Sim, se contiver um e-mail conhecido  
C) Não, porque a assinatura não é verificada  
D) Sim, desde que seja armazenado no navegador

**5. O que a rotina de vídeos faz, conforme o código analisado?**

A) Publica vídeos novos automaticamente no YouTube  
B) Consulta vídeos, tenta associá-los a eventos e grava links  
C) Compila o React diariamente  
D) Cria usuários no Google

### Gabarito comentado

| Questão | Resposta | Explicação |
|---|---|---|
| 1 | **B** | O controlador realiza consultas, adapta dados e prepara a resposta. |
| 2 | **B** | Express encontra a rota e chama a função associada. |
| 3 | **B** | Normalizar dados permite tratar diferentes plataformas de maneira uniforme. |
| 4 | **C** | Decodificação não é verificação criptográfica. |
| 5 | **B** | O cron pesquisado associa links existentes, não publica vídeos. |

## 17. Checklist de aprendizagem

Antes de avançar, verifique se consegue:

- [ ] Explicar em uma frase a finalidade do projeto.
- [ ] Diferenciar frontend, backend e banco de dados.
- [ ] Identificar o papel do React, do Express e do MongoDB.
- [ ] Encontrar o arquivo `backend/src/index.ts`.
- [ ] Localizar o roteador de competições.
- [ ] Identificar o controlador chamado pelo endpoint de próximas competições.
- [ ] Descrever o caminho Home → hook → HTTP → Express → controlador → JSON.
- [ ] Explicar por que os dados de diferentes plataformas são normalizados.
- [ ] Distinguir busca de vídeos de publicação de vídeos no YouTube.
- [ ] Explicar por que `jwt.decode` não é suficiente para validar identidade.
- [ ] Justificar por que não devemos executar um ZIP desconhecido para começar o estudo.
- [ ] Responder às cinco questões de revisão.

Se algum item ficou difícil, retorne ao arquivo correspondente no GitHub e siga o fluxo novamente.

## 18. O que faremos na Aula 2?

Na próxima etapa, vamos estudar a **preparação segura do ambiente de desenvolvimento**.

O percurso previsto inclui Git, Node.js, TypeScript, editor de código, dependências do frontend e backend, variáveis de ambiente e a execução local de serviços.

Antes de iniciar a aplicação, vamos também revisar as configurações e definir correções mínimas para evitar o uso inseguro da autenticação, do banco e de arquivos não verificados.

O objetivo não será apenas fazer a aplicação *subir*, mas conseguir explicar **o que está sendo executado, quais recursos utiliza e como verificar o comportamento esperado**.

## 19. Conclusão: compreender antes de modificar

Estudar um projeto real exige mais do que reconhecer nomes de bibliotecas.

Nesta aula, conhecemos o Assignment Contest Tracker, identificamos suas camadas, percorremos uma requisição HTTP e observamos como informações externas e dados persistidos participam do funcionamento da aplicação.

Também encontramos situações que mostram por que revisão de código, segurança e qualidade precisam fazer parte da formação de um desenvolvedor.

A principal ideia é simples: **antes de corrigir ou ampliar uma aplicação, precisamos construir um mapa confiável de suas responsabilidades e de seus fluxos**.

Com esse mapa, os próximos passos se tornam mais seguros e mais fáceis de justificar.

---

## Fontes e materiais de estudo

**Projeto-base:**

- [dgvijaytorg/assignment — repositório original](https://github.com/dgvijaytorg/assignment)
- [README do projeto](https://github.com/dgvijaytorg/assignment/blob/main/readme.md)

**Arquivos analisados:**

- [Inicialização do backend — `src/index.ts`](https://github.com/dgvijaytorg/assignment/blob/main/backend/src/index.ts)
- [Rotas de competições — `contest_router.ts`](https://github.com/dgvijaytorg/assignment/blob/main/backend/src/routers/contest_router.ts)
- [Controlador de competições — `contests.ts`](https://github.com/dgvijaytorg/assignment/blob/main/backend/src/controllers/contests.ts)
- [Rotas de usuários — `user_router.ts`](https://github.com/dgvijaytorg/assignment/blob/main/backend/src/routers/user_router.ts)
- [Controlador de usuários — `user.ts`](https://github.com/dgvijaytorg/assignment/blob/main/backend/src/controllers/user.ts)
- [Autenticação — `auth_router.ts`](https://github.com/dgvijaytorg/assignment/blob/main/backend/src/routers/auth_router.ts)
- [Middleware de autenticação](https://github.com/dgvijaytorg/assignment/blob/main/backend/src/middleware/middleware.ts)
- [Agendamento do YouTube](https://github.com/dgvijaytorg/assignment/blob/main/backend/src/cron/video_link_fetcher.ts)
- [Aplicação React — `App.tsx`](https://github.com/dgvijaytorg/assignment/blob/main/frontend/src/App.tsx)
- [Página inicial — `Home.tsx`](https://github.com/dgvijaytorg/assignment/blob/main/frontend/src/pages/Home.tsx)
- [Hook de próximas competições](https://github.com/dgvijaytorg/assignment/blob/main/frontend/src/hooks/UseUpcommingContests.tsx)
- [Configuração Docker](https://github.com/dgvijaytorg/assignment/blob/main/backend/docker-compose.yml)

**Referências complementares:**

- [React — documentação](https://react.dev/)
- [Express — documentação](https://expressjs.com/)
- [TypeScript — documentação](https://www.typescriptlang.org/docs/)
- [MongoDB — documentação](https://www.mongodb.com/docs/)
- [Google Identity — verificação de ID tokens](https://developers.google.com/identity/gsi/web/guides/verify-google-id-token)
- [OWASP — API Security](https://owasp.org/API-Security/)

*Nota de escopo: a análise corresponde ao código observado no ramo principal do repositório durante a preparação desta aula. Alterações futuras do projeto original podem modificar comportamentos, nomes de rotas e configurações aqui descritos. Nenhum código do repositório original foi executado como parte desta revisão.*

**Metadescrição:** Estude a arquitetura de um projeto full stack real com React, TypeScript, Express e MongoDB. Entenda as rotas REST, o fluxo HTTP e os riscos do código.

**Tags:** React, TypeScript, Node.js, Express, MongoDB, Mongoose, API REST, Arquitetura de Software, GitHub, Segurança, Aprendizado Prático.
