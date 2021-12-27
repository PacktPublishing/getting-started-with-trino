# Running Trino

## Introduction 
This tutorial will cover running Trino and the TPCH connector

## Steps

### Running Services

First, you want to start the services. Make sure that you are in the 
`chapter-1/trino-tpc/` directory. Now run the following
command:

```
docker-compose up -d
```

You should expect to see the following output (you may also have to download
the Docker images before you see the "done" message):

```
[+] Running 2/2
 ⠿ Network trino-tpc_default                Created                                                                                                0.1s
 ⠿ Container trino-tpc_trino-coordinator_1  Started
```

### Open Trino CLI

Once this is complete, you can log into the Trino coordinator node. We will
do this by using the [`exec`](https://docs.docker.com/engine/reference/commandline/exec/)
command and run the `trino` CLI executable as the command we run on that
container. Notice the container id is `trino-mysql_trino-coordinator_1` so the
command you will run is:

```
docker container exec -it trino-tpc_trino-coordinator_1 trino
```

When you start this step, you should see the `trino` cursor once the startup
is complete. It should look like this when it is done:
```
trino>
```


Now, let's run a query from the TPCH catalog. The TPC connectors generate data 
on the fly so that we can run simple tests like this. First, we must check that
these catalogs are available.

### Viewing catalogs in Trino

First, run a command to show the catalogs to see the `tpch` and `tpcds` catalogs
since these are what we will use in the query.

```
SHOW CATALOGS;
```

You should see that the tpch catalog is available, along with the tpcds catalog. 
Now that we know these catalogs are available. We need to check what schemas are
available. Schemas in the Trino context is the next layer in the containment 
hierarchy. Where catalogs deliniate which system you wish to query, the schema
is typically contains the databases of in the system its pointing to. For TPC
connectors, it is instead used to group different sizes of benchmark data to be
used. This is helpful when you're just trying to learn and test as you can
choose the samllest schema called, tiny. 

### Viewing schemas in Trino

```
SHOW SCHEMAS in tpch;
```

You should see various scale factor schemas. We will use tiny schema for speed 
and simplicity.

### Viewing tables in Trino

The lowest containment hierarchy container is called what you would expect in a 
database, a table. Tables equivalently map to tables in relational databases and
data lake systems like Hive. NoSQL systems such as Elasticsearch call these 
indices, but ultimatley these will show up as Tables to Trino.

```
SHOW TABLES in tpch.tiny;
```

### Querying Trino

Now that we know the catalog and the schema that will hold our table, we now can 
create our first query. 

Optional: To view your queries run, log into the
[Trino UI](http://localhost:8080) and log in using any username (it doesn't
 matter since no security is set up).

```
SELECT * FROM tpch.tiny.customer LIMIT 5;
```

The results should look like this:
```
trino> SELECT * FROM tpch.tiny.customer LIMIT 5;
 custkey |        name        |                address                 | nationkey |      phone      | acctbal | mktsegment |
---------+--------------------+----------------------------------------+-----------+-----------------+---------+------------+---------------------------
    1126 | Customer#000001126 | 8J bzLWboPqySAWPgHrl4IK4roBvb          |         8 | 18-898-994-6389 | 3905.97 | AUTOMOBILE | se carefully asymptotes. u
    1127 | Customer#000001127 | nq1w3VhKie4I3ZquEIZuz1 5CWn            |        10 | 20-830-875-6204 | 8631.35 | AUTOMOBILE | endencies. express instruc
    1128 | Customer#000001128 | 72XUL0qb4,NLmfyrtzyJlR0eP              |         0 | 10-392-200-8982 | 8123.99 | BUILDING   | odolites according to the
    1129 | Customer#000001129 | OMEqYv,hhyBAObDjIkoPL03BvuSRw02AuDPVoe |         8 | 18-313-585-9420 | 6020.02 | HOUSEHOLD  | pades affix realms. pendin
    1130 | Customer#000001130 | 60zzrBpFXjvHzyv0WObH3h8LhYbOaRID58e    |        22 | 32-503-721-8203 | 9519.36 | HOUSEHOLD  | s requests nag silently ca
(5 rows)
```

So now you have a basic working Trino instance up and running. From
here you can read more about the 
[Trino Concepts](https://trino.io/docs/current/overview/concepts.html) 
to learn more about Trino.

See trademark and other [legal notices](https://trino.io/legal.html).
