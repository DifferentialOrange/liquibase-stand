# Perform MongoDB migration with Liquibase

Start a DB instance and perform migration when it's ready.

```bash
docker compose up
```

```
liquibase-1   | UPDATE SUMMARY
liquibase-1   | Run:                          1
liquibase-1   | Previously run:               0
liquibase-1   | Filtered out:                 0
liquibase-1   | -------------------------------
liquibase-1   | Total change sets:            1
liquibase-1   | 
liquibase-1   | Liquibase: Update has been successful. Rows affected: 1
liquibase-1   | Liquibase command 'update' was executed successfully.
liquibase-1 exited with code 0
```

Connect to DB and introspect schema.

```bash
mongosh -u root -p example
```

```sql
test> db.getCollectionInfos()
```
```bson
[
  {
    name: 'towns_yaml',
    type: 'collection',
    options: {},
    info: {
      readOnly: false,
      uuid: UUID('15d13942-4503-4eef-8a2c-5afb256762bb')
    },
    idIndex: { v: 2, key: { _id: 1 }, name: '_id_' }
  },
  {
    name: 'DATABASECHANGELOGLOCK',
    type: 'collection',
    options: {
      validator: {
        '$jsonSchema': {
          bsonType: 'object',
          description: 'Database Lock Collection',
          required: [ '_id', 'locked' ],
          properties: {
            _id: { bsonType: 'int', description: 'Unique lock identifier' },
            locked: { bsonType: 'bool', description: 'Lock flag' },
            lockGranted: {
              bsonType: 'date',
              description: 'Timestamp when lock acquired'
            },
            lockedBy: { bsonType: [Array], description: 'Owner of the lock' }
          }
        }
      },
      validationLevel: 'strict',
      validationAction: 'error'
    },
    info: {
      readOnly: false,
      uuid: UUID('49bd01b2-fd95-4487-9c01-2c7f3fa804e9')
    },
    idIndex: { v: 2, key: { _id: 1 }, name: '_id_' }
  },
  {
    name: 'DATABASECHANGELOG',
    type: 'collection',
    options: {
      validator: {
        '$jsonSchema': {
          bsonType: 'object',
          description: 'Database Change Log Table.',
          required: [ 'id', 'author', 'fileName', 'execType' ],
          properties: {
            id: {
              bsonType: 'string',
              description: 'Value from the changeSet id attribute.'
            },
            author: {
              bsonType: 'string',
              description: 'Value from the changeSet author attribute.'
            },
            fileName: {
              bsonType: 'string',
              description: 'Path to the changelog. This may be an absolute path or a relative path depending on how the changelog was passed to Liquibase. For best results, it should be a relative path.'
            },
            dateExecuted: {
              bsonType: [Array],
              description: 'Date/time of when the changeSet was executed. Used with orderExecuted to determine rollback order.'
            },
            orderExecuted: {
              bsonType: [Array],
              description: 'Order that the changeSets were executed. Used in addition to dateExecuted to ensure order is correct even when the databases datetime supports poor resolution.'
            },
            execType: {
              bsonType: 'string',
              enum: [Array],
              description: 'Description of how the changeSet was executed.'
            },
            md5sum: {
              bsonType: [Array],
              description: 'Checksum of the changeSet when it was executed. Used on each run to ensure there have been no unexpected changes to changSet in the changelog file.'
            },
            description: {
              bsonType: [Array],
              description: 'Short auto-generated human readable description of changeSet.'
            },
            comments: {
              bsonType: [Array],
              description: 'Value from the changeSet comment attribute.'
            },
            tag: {
              bsonType: [Array],
              description: 'Tracks which changeSets correspond to tag operations.'
            },
            contexts: {
              bsonType: [Array],
              description: 'Context expression of the run.'
            },
            labels: { bsonType: [Array], description: 'Labels assigned.' },
            deploymentId: {
              bsonType: [Array],
              description: 'Unique identifier generate for a run.'
            },
            liquibase: {
              bsonType: [Array],
              description: 'Version of Liquibase used to execute the changeSet.'
            }
          }
        }
      },
      validationLevel: 'strict',
      validationAction: 'error'
    },
    info: {
      readOnly: false,
      uuid: UUID('e137c841-2352-453c-ad3c-20f079eed1a5')
    },
    idIndex: { v: 2, key: { _id: 1 }, name: '_id_' }
  }
]
```
