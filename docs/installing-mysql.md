# MySQL

```sh
socket=/var/lib/mysql/mysql.sock


## INNODB Config
innodb_buffer_pool_size=5G # silk
innodb_flush_method=O_DIRECT # silk
innodb_thread_concurrency=8 # silk
innodb_log_file_size=256M
innodb_flush_neighbors=0
innodb_flush_log_at_trx_commit=2 # silk
query_cache_type=1

transaction-isolation=READ-COMMITTED # silk

# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
```
