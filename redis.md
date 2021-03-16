# Copy keys to another database

```
# MIGRATE host port key|"" destination-db timeout [COPY] [KEYS key [key ...]]

redis-cli --scan | xargs redis-cli MIGRATE localhost 6379 '' 1 10000 COPY KEYS
```

Due to a bug in old Redis versions, the command will timeout but will still succeed.
