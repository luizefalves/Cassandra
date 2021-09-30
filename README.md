# Projeto: Modelagem de dados com Apache Cassandra

## Comentários sobre bancos de dados relacionais, não relacionais e o Apache Cassandra:

Bancos de dados do tipo **relacional** quase sempre utilizam operações de consulta do tipo SQL,
que são fundamentadas na Álgebra de Relações.
Isso implica em grande simplicidade, contudo, traz consigo todas as limitações de sua estrutura, os valores de uma
tupla relacional não podem conter nenhum tipo de estrutura além das tuplas (ex: registro aninhado). 
Estruturas mais ricas permitem uma maior quantidade de informações a partir de uma única consulta.

Bancos de dados relacionais não foram projetados para serem executados em clusters, o que dificulta o escalonamento
horizontal, mais barato e resiliente. O tipo relacional utiliza transações ACID para lidar com consistência em
todo o banco de dados, o que é inerentemente **conflitante com o ambiente de clusters**.

Os bancos de dados **NoSQL**, em sua maioria, são orientados pela necessidade de execução em clusters, não utilizam SQL
e atuam sem um esquema (_"schema"_), permitindo que sejam adicionados livremente, campos aos registros do banco de
dados, sem ter de definir mudanças na estrutura.

O **Apache Cassandra** é um banco de dados NoSQL com armazenamento orientado a famílias de colunas.
Ao invés de utilizar  as linhas como unidade de armazenamento (o que aumenta o desempenho de gravação),
o armazenamento em grupos de colunas é ideal para cenários onde as gravações são raras mas é preciso ler
frequentemente  algumas colunas de muitas linhas de uma só vez.

**Notas sobre o Cassandra:** 

1. Apesar de _schemaless_,possui uma liguagem de consulta semelhante ao SQL,
o CQL (_"Cassandra Query Language"_).
2. Não possui um cluster mestre, todos os clusters tem o mesmo status, tornando-o um sistema altamente disponível.
3. O custo da mudança em consultas pode ser maior do que na mudança de esquema, de modo que ele é pouco apropriado 
para os primeiros protótipos de um projeto.
 



## Visão Geral do Projeto:

Esse projeto é parte do curso **Udacity Data Engineering Nanodegree** e tem por objetivo trabalhar com algumas
funcionalidades básicas do Apache Cassandra.

O projeto é constituído de duas etapas:

1. Pré-processamentos de dados extraídos de um arquivo .csv contendo dados históricos de um aplicativo de músicas fictício

2. Realização de consultas à partir de perguntas feitas aos alunos do curso.

O modelo responde a três perguntas bastante simples, sendo elas:

1. Utilizando os dados históricos, responda o artista, a música e a duração das músicas que foram ouvidas na 
sessão de **ID igual a 338**.
2. Para o usuário de **ID igual a 10** e **sessão 182**, responda o nome do artista, a música ordenada por itemInSession
e o usuário, com primeiro e último nome.
3. Imprima o nome e sobrenome de todos os usuários que ouviram a música _"All Hands Against His Own"_

As respostas a estas perguntas estão nas seguintes tabelas:

1. **`songinfo_by_session_by_item`**
2. **`songinfo_by_user_by_session`**
3. **`userinfo_by_song`**


## Para rodar em um container Docker:


```{bash}
docker pull cassandra

docker run --name cassandra-container -p 9042:9042 -d cassandra:latest
```

Isso irá permitir devolver o modelo a partir do Jupyter sem precisar alterar o código fornecido nesse 
projeto, uma vez que mantem a porta padrão do localhost.

```{python}
from cassandra.cluster import Cassandra

cluster = Cluster(['127.0.0.1'])
session = cluster.connect()
```

