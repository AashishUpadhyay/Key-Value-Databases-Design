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

- Write cost is O(1)
- Read cost is O(N)

### Define an Index

![alt text](SimpleDatabaseWithIndex.png)

- Write cost is O(1)
- Read cost is O(1)

