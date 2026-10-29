# Wide-Column-db
- Eksemplet her tager udgangspunkt i Apache Cassandra

# Generic Info
- Wide-column databases expand the key-value store concept from two columns to multiple columns, but only the columns needed for that record.

> RDBMS vs. Wide-Column
- Mere fleksibelt fordi hver column ('row' i relationelle) kan have sine egne unikke columns i samme column-family ('tabel' i relationelle)
- Sagt på en anden måde, hver række bestemmer sine egne kolonner (og kan have forskellige datatyper ** TODO: INDSÆT BILLEDE EKSEMPEL HER ** )
- En column har

- Keyspace = Schema
- I keyspaces er der column-families = Tabeller
- Indenfor hver column-family har du Rows = Rækker
- I hver Row har du unikke Columns = I relationelle DB'er har rækkerne i en tabel eksakt samme Kolonner
- En Row har et unikt ID 'row key' som identificer rækken unikt = Primary Key
- Hver Column har Name, Value & Timestamp

* Cassandra specifikt har 'Composite Columns', som gør det muligt at nest'e objekter i en kolonne
 - F.eks. 

# Opsætning


# CRUD Operations Eksempler


# Pros & Cons
> Pros:
- Open-source
- Top 1 på DB-Engine ranking
- 


> Cons:
- Term-forvirring - Wide column forvirres typisk med Columnar (f.eks. ved Googling af 'Wide column store vs. relational database')
- Wide column store databaser også kan være multi-model, så man skal være opmærksom på at de kan indeholde extra funktionalitet udover wide-column-specifik
- 

# Refleksion


# Yderligere Emner
> Skalérbarhed
-
> Performance
> Håndtering af store datamængder


# Extra Stuff
https://cassandra.apache.org/_/quickstart.html
