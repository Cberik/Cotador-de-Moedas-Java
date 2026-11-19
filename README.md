# 💱 Cotador de Moedas — Java

## 📌 Sobre o projeto

Projeto desenvolvido em Java com o objetivo de automatizar a consulta de cotações de moedas estrangeiras e registrar os resultados em um arquivo CSV.

A aplicação realiza a leitura de um arquivo de texto contendo códigos de moedas, consulta suas respectivas cotações por meio de uma API externa e registra os valores obtidos em um arquivo de saída.

O projeto utiliza a API **Frankfurter**, que disponibiliza informações de taxas de câmbio, tendo o dólar americano (USD) como moeda de referência.

## 🎯 Objetivo

Desenvolver uma aplicação capaz de integrar operações de entrada e saída de arquivos com requisições HTTP assíncronas, aplicando recursos modernos da plataforma Java.

O processamento contempla:

- Verificação dos arquivos e diretórios necessários.
- Leitura e tratamento de códigos de moedas.
- Validação do formato dos códigos informados.
- Consulta simultânea de cotações por meio de requisições assíncronas.
- Tratamento de falhas individuais nas consultas.
- Extração de informações de respostas JSON.
- Registro das cotações obtidas em um arquivo CSV.

## 🛠️ Tecnologias e recursos utilizados

- **Java 21:** linguagem utilizada no desenvolvimento.
- **Maven:** gerenciamento e configuração do projeto.
- **NIO2:** manipulação de arquivos e diretórios utilizando `Path` e `Files`.
- **HttpClient:** comunicação HTTP com a API externa.
- **CompletableFuture:** execução e coordenação de operações assíncronas.
- **Streams API:** processamento e filtragem de coleções.
- **Records:** representação estruturada dos dados de cotação.
- **JUnit 5:** desenvolvimento de testes automatizados.
- **AssertJ:** biblioteca para validação dos resultados dos testes.
- **Mockito:** criação de objetos simulados para testar as operações sem depender de chamadas reais à API.

## ⚙️ Funcionamento da aplicação

### 1. Leitura do arquivo de entrada

A aplicação utiliza o arquivo `entrada/moedas.txt`, que contém os códigos das moedas que devem ser consultadas.

Exemplo:

```text
BRL
EUR
GBP
JPY
```

Durante a leitura, os dados passam por um processo de tratamento:

- Remoção de espaços desnecessários com `trim()`.
- Conversão dos códigos para letras maiúsculas.
- Filtragem de códigos que não possuem exatamente três letras.

### 2. Consulta à API Frankfurter

A classe `ClienteCambio` realiza as requisições HTTP utilizando `HttpClient` e o método `sendAsync()`.

A aplicação utiliza a seguinte API:

https://api.frankfurter.dev/v1/latest?base=USD&symbols=

Cada código de moeda é acrescentado à URL para consultar sua cotação em relação ao dólar americano.

As consultas são realizadas de maneira assíncrona, permitindo que várias requisições sejam processadas sem a necessidade de aguardar individualmente cada resposta.

### 3. Processamento assíncrono

A classe `CotadorDeMoedas` utiliza `CompletableFuture` para coordenar as consultas.

O método `CompletableFuture.allOf()` permite aguardar a conclusão de todas as operações antes de reunir os resultados.

As consultas que apresentam falhas são representadas por `Optional.empty()` e não impedem o registro das demais cotações válidas.

### 4. Processamento do JSON

A classe `JsonParser` realiza a extração da taxa de câmbio diretamente da resposta JSON, utilizando operações de manipulação de Strings.

O resultado é representado por um `Optional<Double>`, permitindo tratar situações em que a taxa não foi encontrada ou não pôde ser convertida.

### 5. Geração do arquivo CSV

As cotações obtidas são armazenadas no arquivo:

`saida/cotacoes.csv`

O arquivo utiliza a seguinte estrutura:

```csv
moeda,valor,coletadoEm
BRL,5.43,2026-01-30T12:00
EUR,0.92,2026-01-30T12:00
GBP,0.79,2026-01-30T12:00
```

O arquivo é criado automaticamente e recebe um cabeçalho com os campos:

- `moeda`: código da moeda consultada.
- `valor`: taxa de câmbio obtida.
- `coletadoEm`: data e horário do registro.

## 📂 Estrutura do projeto

```text
atividade2/
│
├── entrada/
│   └── moedas.txt
│
├── saida/
│   └── cotacoes.csv
│
├── pom.xml
├── mvnw
├── mvnw.cmd
│
└── src/
    ├── main/
    │   └── java/
    │       └── br/edu/utfpr/
    │           ├── Main.java
    │           ├── ClienteCambio.java
    │           ├── Cotacao.java
    │           ├── CotadorDeMoedas.java
    │           └── JsonParser.java
    │
    └── test/
        └── java/
            └── br/edu/utfpr/
                └── CotadorDeMoedasTest.java
```

## 🧪 Testes automatizados

O projeto utiliza JUnit 5, AssertJ e Mockito para verificar o comportamento das principais operações.

Os testes contemplam:

- Verificação do caminho padrão do arquivo de entrada.
- Criação do diretório de saída.
- Leitura e normalização dos códigos de moedas.
- Descarte de códigos com formato inválido.
- Tratamento de arquivos inexistentes.
- Registro das cotações válidas mesmo quando uma consulta falha.
- Criação automática do arquivo CSV.
- Comportamento da aplicação quando não existem moedas para consultar.
- Verificação de que nenhuma consulta é realizada quando a lista está vazia.

O Mockito permite simular respostas das consultas, evitando a dependência de uma conexão real com a API durante esses testes.

## ▶️ Como executar

O projeto utiliza Maven Wrapper, permitindo executar a aplicação sem precisar instalar o Maven manualmente.

**1. Configure o arquivo de entrada**

Edite `entrada/moedas.txt` e informe os códigos das moedas que deseja consultar, um por linha.

**2. Execute os testes**

No Windows, utilizando PowerShell:

```powershell
.\mvnw.cmd test
```

**3. Execute a aplicação**

```powershell
.\mvnw.cmd compile exec:java
```

**4. Consulte os resultados**

Após a execução, o arquivo `saida/cotacoes.csv` será gerado com as cotações obtidas.

É necessário possuir conexão com a internet para consultar a API Frankfurter.

## 📚 Conhecimentos desenvolvidos

Durante a implementação desta atividade, foram trabalhados conceitos importantes da plataforma Java:

- Manipulação de arquivos com NIO2.
- Utilização das classes `Path` e `Files`.
- Leitura e escrita de arquivos.
- Criação de diretórios.
- Comunicação HTTP com `HttpClient`.
- Requisições assíncronas com `sendAsync()`.
- Programação assíncrona com `CompletableFuture`.
- Coordenação de tarefas com `CompletableFuture.allOf()`.
- Tratamento de resultados opcionais com `Optional`.
- Processamento de coleções com Streams API.
- Utilização de Records.
- Manipulação e extração de dados JSON.
- Escrita de arquivos CSV.
- Testes automatizados e simulação de dependências.

## 💡 Considerações finais

O desenvolvimento desta atividade demonstra como os recursos modernos do Java podem ser utilizados para construir aplicações que integram arquivos locais e serviços externos.

A combinação de NIO2, HttpClient e CompletableFuture permite desenvolver um fluxo de processamento que realiza consultas assíncronas, trata possíveis falhas e organiza os resultados em um formato estruturado para armazenamento.

---

**Projeto acadêmico desenvolvido durante os estudos de Java II — Pós-graduação.**
