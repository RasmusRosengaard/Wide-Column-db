# HINT TIL NÆSTE GANG
- Sidste session, oprettede Googel Docs for at kunne multi-edit'e samtidig ; hence whyfor README'n her ik' er updated yet med de aendringer
- Saa note to selves: Husk at tage udgangspunkt i dét naeste gang! (Probs' torsdag d. 12/11)

# Wide-Column-db
- Eksemplet her tager udgangspunkt i Apache Cassandra
- Andre Wide Column DB'er: ScyllaDB, HBase, BigTable

# Generic Info
- Link til opgavens udgangspunkt fra Kenneth's Magiske Verden:
https://ucl.kjc.dk/softwaredev/2026/dfu/dfu-exercise-other-databases
- Wide-column databases expand the key-value store concept from two columns to multiple columns, but only the columns needed for that record/row.

- Data model (Cassandra terms):
Keyspace → Table → Partition → Row → Column

## Termforvirring at være obs på
- Cassandra kaldes nogle gange for en Columnar DB - det er det IKKE! ; Wide-column DB'er storer en partition's rows sammen så lookups bliver hurtigere by key, mens Columnar DB'er opbevarer hver column's værdier sammen, for at man kan lave hurtigere analytics across mange rækker.
- I Cassandra bruges 'column family' som et gammelt navn for tables, som har et defined schema men en row store kun de kolonner den har vaerdier til. I HBase/BigTable er 'column family' en gruppe af columns i en table.

## RDBMS vs. Wide-Column
- Mere fleksibelt fordi hver row kan have sine egne unikke columns, samlet i samme column-family ('tabel' i relationelle)
- Eksempel:
"user1" -> [name: "Ana", email: "ana@odense.dk"]
"user2" -> [name: "Michael P.", phone: "12345678", city: "Vejle"]

- Sagt på en anden måde, hver række bestemmer sine egne kolonner (og kan have forskellige datatyper ** TODO: INDSÆT BILLEDE EKSEMPEL HER ** )

// NEW



// OLD
- Keyspace = Schema
- I keyspaces er der column-families = Tabeller
  * NOTE - Termforvirring: I Cassandra bruges 'column family' som et gammelt navn for tables, som har et defined schema men en row store kun de kolonner den har vaerdier til. I HBase/BigTable er 'column family' en gruppe af columns i en table.
- Indenfor hver column-family har du Rows = Rækker
- I hver Row har du unikke Columns = I relationelle DB'er har rækkerne i en tabel eksakt samme Kolonner
- En Row har et unikt ID 'row key' som identificer rækken unikt = Primary Key
- Hver Column har Name, Value & Timestamp

* Cassandra specifikt har 'Composite Columns' (også kaldet 'Clustering Columns'), som gør det muligt at nest'e objekter i en kolonne
 - F.eks. 

# Opsætning


# CRUD Operations Eksempler

## Create et Keyspace

## Create en Column-Family

## Create en Row i Column-Family'en

## Create Columns i en Row

## Create en Row med Composite/Clustered Columns


# Pros & Cons
## Pros:
- Open-source
- Top 1 på DB-Engine ranking
- Compression: Column store DB'er er meget effektive til data-compression
- Aggregation Queries: Column store DB'er er gode til disse, f.eks. Sum, Count & Average
- Scalability: Column store DB'er er gode til parallel processering, hvilket indebærer at data er fordelt på mange clusters af maskiner
- Fast to Load & Query: En milliard rows kan loades indenfor et par sekunder & derfor kan Wide Column stores være gode til Big Data
- Vi query'er mod kun de kolonner vi søger, da de er adskilte, modsat en relationel DB hvor en tabel skal igennem 

## Cons:
- Term-forvirring - Wide column forvirres typisk med Columnar (f.eks. ved Googling af 'Wide column store vs. relational database')
- Wide column store databaser også kan være multi-model, så man skal være opmærksom på at de kan indeholde extra funktionalitet udover wide-column-specifik
 

# Refleksion


# Yderligere Emner
## Skalérbarhed

## Performance
Håndtering af store datamængder


# Extra Stuff
https://cassandra.apache.org/_/quickstart.html

https://www.scylladb.com/glossary/wide-column-database/

https://database.guide/what-is-a-column-store-database/
