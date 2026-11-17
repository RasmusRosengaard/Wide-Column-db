# Wide-Column-db
- Eksemplet her tager udgangspunkt i Apache Cassandra
- Andre Wide Column DB'er: ScyllaDB, HBase, BigTable

# Generic Info
- Link til opgavens udgangspunkt fra Kenneth's Magiske Verden:
https://ucl.kjc.dk/softwaredev/2026/dfu/dfu-exercise-other-databases
- Wide-column databases expand the key-value store concept from two columns to multiple columns, but only the columns needed for that record/row.

- Data model (Cassandra terms):
Keyspace → Table → Partition → Row → Column

## Termforvirring at være obs på 🤔
- Cassandra kaldes nogle gange for en Columnar DB - det er det IKKE! ; Wide-column DB'er storer en partition's rows sammen så lookups bliver hurtigere by key, mens Columnar DB'er opbevarer hver column's værdier sammen, for at man kan lave hurtigere analytics across mange rækker.
- I Cassandra bruges 'column family' som et gammelt navn for tables, som har et defined schema men en row store kun de kolonner den har vaerdier til. I HBase/BigTable er 'column family' en gruppe af columns i en table.

## WHAT er Wide Column ?!
NoSQL-DB, organiserer data i fleksible kolonner - modsat relationel DB kan kolonner variere i navn og format fra række til række.

DB’en fordeles på tværs af servere/noder.

Designudgangspunktet er i dine Queries - Ikke i relationerne.

Det er IKKE en Columnar DB !!!

## Struktur
Clusters
▼
Keyspace
▼
Table *
▼
Partition (find via partition key)
▼
Row (find via clustering key)
▼
Column

## WHEN gi’r Wide Column DB mening at bruge ?
IoT/Sensor data:
Masser af data i sekundet | Partition: device, cluster: time

Timeseries data:
Masser af data, men typically kun writes ; læs per timerange

Chat & messaging:
Messages opdelt i conversations, read ‘latest N messages’
[Discord brugte Cassandra för rykket til ScyllaDB, begge er wide column]

Logging & monitoring:
Skrives ofte, opdel per timerange

Recommendation & personalization:
‘Hvad bruger Michael_P har sidst set & clicket på’ [Netflix]

Shopping carts & sessions:
Simple lookup some bör være Highly Available, på tværs af datacentre

## WHEN ville man nok vælge en anden DB ?
Ved normalisering

Hvis höj skrive-mulighed IKKE er vigtigt for dig

Hvis data ska’ vaere ens i en tabel

Hvis du ska’ bruge JOINS eller TRANSAKTIONER

Frekvente sletninger og opdateringer på dataerne

Hvis du skal have striks ACID-overholdelse


# Opsaetning A - Docker
docker network create cassandra

docker run --rm -d --name cassandra --hostname cassandra --network cassandra cassandra

docker exec -it cassandra cqlsh

CREATE KEYSPACE IF NOT EXISTS chat WITH REPLICATION =
{ 'class' : 'SimpleStrategy', 'replication_factor' : '1' };

USE chat;

CREATE TABLE IF NOT EXISTS messages (
  chat_id int,
  sent_at timestamp,
  sender text,
  body text,
  PRIMARY KEY (chat_id, sent_at)
) WITH CLUSTERING ORDER BY (sent_at DESC);

INSERT INTO messages (chat_id, sent_at, sender, body)
VALUES (1, '2026-11-05 10:00:00+0000', 'Ana', 'Hej');

# Opsaetning B - Softwareintegrerings-approach
DataStax C# Driver

https://docs.datastax.com/en/developer/csharp-driver/3.4/index.html

NuGet:
CassandraCSharpDriver

var cluster = Cluster.Builder()
                     .AddContactPoints("host1")
                     .Build();
// Connect to the nodes using a keyspace
var session = cluster.Connect("sample_keyspace");
// Execute a query on a connection synchronously
var rs = session.Execute("SELECT * FROM sample_table");
// Iterate through the RowSet
foreach (var row in rs)
{
  var value = row.GetValue<int>("sample_int_column");
  // Do something with the value
}

# Glossary Help
Partition Key			(A.K.A. “Row Key” i gammel-Cassandra-tid)
Bestemmer hvilken node indeholder dataen & grupperer rows ind i en partition

Clustering Key
Styrer sort order af rows i en partition & gør hver row unik

Primary Key
Particion Key + Clustering Key(s)

PRIMARY KEY 	((chat_id), sent_at)
				partition    clustering


# CRUD Operations Eksempler

## Create et Keyspace

## Create en Column-Family

## Create en Row i Column-Family'en

## Create Columns i en Row

## Create en Row med Composite/Clustered Columns


# Pros & Cons
## Pros:
Open-Source

#1 by score på DB-Engines

Effektiv til data-compression

Scalability: Parallel processering af data fordelt på mange clusters af maskiner

Vi query’er kun mod data vi søger da dataen er opdelt = Bedre performance

## Cons:
Nogle forskelle i CQL fra standard SQL (Cassandra/ScyllaDB specifik)

Ineffektiv opdatering af rækker

Ingen JOINS

Svær data modeling
I.e. dårlige partition keys giver hot’e eller for store partitioner

Kompleks at operere
(clusters, repair, tuning)

SQL er nemmere för datastörrelsen kraever skalering


# Yderligere Emner
## Skalérbarhed

## Performance
Håndtering af store datamængder


# Semi-helpful links
https://cassandra.apache.org/_/quickstart.html

https://www.scylladb.com/glossary/wide-column-database/

https://database.guide/what-is-a-column-store-database/
