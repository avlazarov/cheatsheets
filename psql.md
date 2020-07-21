## Dump into CSV

```
COPY (SELECT id FROM users)
TO '/path' DELIMITER ',' CSV [HEADER]
```

or from the command line

```
$ psql -c "\COPY ..."
```
