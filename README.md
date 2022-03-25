# Key Value Database Design

## Simple database implemented using two bash functions:
```
#!/usr/bin/env bash

db_set (){
    echo "$1, $2" >> database
}

db_get(){
    grep "^$1," database | sed -e "s/^$1,//" | tail -n 1
}
```

- Write cost is O(1), we only append to a file
- Read cost is O(N)

## Define an inmemory Index

![alt text](SimpleDatabaseWithIndex.png)

- Write cost is O(1) 
- Read cost is O(1)

This approach is used in bitcask the storage engine for Riak

- To avoid running out of disk space the disk files are broken into segments. Once a segment becomes big enough, its sealed and later writes are performed on a different segment file
- Merge and compaction is performed on segments to remove duplicate values 
![alt text](MergeCompaction.png)

- Each segment has its own Index file
- During read the key is first searched in the most recent segment and if not found later segments are searched. To improve search performance techniques like bloom filters are used. Bloom filters are probabilistic data strcuture that may produce false positives but never false negatives

**Cons** 
- The Index size is limited by the memory and therefore suitable for use cases where unique keys are few
- Inefficient range queries


