<h1 align="center">MongoDB</h1>

## Commands

- View the query plan selected

```bash
db.<collectionName>.find(<query>).explain("executionStats");
```

- Get indexes for a collection

```bash
db.<collectionName>.getIndexes();
```

- Drop all indexes from a collection

```bash
db.<collectionName>.dropIndexes("*");
```

- Import json data to a collection

```bash
# mongodbURL example mongodb://localhost:27017
mongoimport <mongodbURL> --collection <collectionName> --db <databaseName> <file>
```
