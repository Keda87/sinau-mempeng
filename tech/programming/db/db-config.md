# DB Config

Pro tip: If you are setting a PostgreSQL or MySQL, don't use the default database settings because those are meant for personal computers or notebooks.

One example is the Buffer Pool size:

- ✅ For MySQL, increase the innodb_buffer_pool_size
- ✅ For PostgreSQL, increase shared_buffers and effective_cache_size to match your OS cache size.

Referensi: https://vladmihalcea.com/postgresql-performance-tuning-settings/
