# Elasticsearch Best Practices

- flatten all your SQL tables or JSON data. So queries will be faster. data might get duplicate in every rows
- never think joins in elasticsearch, use flat json
