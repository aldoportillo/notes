# Storage Paradigms

## Blob Store (Binary Large Object Store)

A blob is a arbitrary piece of unstructured data. Large image, large video, large text file, large binary code file.

A Blob store is optimized for storing these blobs.

### Common Blob Store

- GCS - Google
- S3 - Amazon
- Azure Blob Storage - Microsoft

## Time Series DB

DB optimized for storing time series data. Useful for monitoring. Designing an IoT system (internet of things). Stock prices.

### Common Time Series DB

- [Influx DB](https://www.influxdata.com/)
- [Prometheus](https://prometheus.io/)

## Graph DB

A graph database is built upon a graph data model. Think about a tabular DB with a lot of relationships. It would definitely be better to use a graph DB. Think about a social network.

![graph visual](https://imgs.search.brave.com/6WzqGyAcuhu_8nsmQ0dMp-Oa9YpYuidDT1caNHtru78/rs:fit:860:0:0/g:ce/aHR0cHM6Ly9pbWFn/ZXMuY3RmYXNzZXRz/Lm5ldC9vN3h1OXdo/cnMwdTkvNjBrdkw5/UkVUc0R2M3IzU3BU/TWN2Ti8wZjE1NmMw/YTJjN2VjNjI4NjNh/MGJkNjAzZDk5MGI4/Ni9Db21wb25lbnRz/LW9mLWEtZ3JhcGgt/ZGF0YWJhc2UucG5n)

### Common Graph DB

- [Neo4j](https://neo4j.com/sandbox/)

## Spatial DB

A database that deals with geometric space. They rely on spatial indexes.

### Quadtree

A quadtree is a data structure in which each node had 4 children nodes or is a leaf node.

![quadtree visual](https://imgs.search.brave.com/XDfLlbOP_DNI-4JQ3tgNekv00LjjVTy8LgpZAcDMZtw/rs:fit:860:0:0/g:ce/aHR0cHM6Ly9vcGVu/ZHNhLXNlcnZlci5j/cy52dC5lZHUvT0RT/QS9Cb29rcy9FdmVy/eXRoaW5nL2h0bWwv/X2ltYWdlcy9QUmV4/YW1wLnBuZw)

As we can see, the outermost square is the parent node, and we continue to divide each quadrants into quadrants if a node is not a leaf node.

This is excellent because the time complexity of a spacial query is O(Log<sub>4</sub>(N)).

## Conclusion

Although all these databases are great, your current database might optimize some of these requirements out of the box so check those first. Seems like Postgresql offers a [spatial data optimization](https://www.sqlshack.com/getting-started-with-spatial-data-in-postgresql/).
