# Bloom Filter

Bloom filter helps prune segments that do not contain any record matching a **EQUALITY** predicate, e.g.

`SELECT COUNT(*) from baseballStats where playerID = 12345`

There are 3 parameters to configure the bloom filter:

* `fpp`: False positive probability of the bloom filter \(from `0` to `1`, `0.05` by default\). The lower the `fpp` , the higher accuracy the bloom filter has, but it will also increase the size of the bloom filter.
* `maxSizeInBytes`: Maximum size of the bloom filter \(unlimited by default\). If a certain `fpp` generates a bloom filter larger than this size, we will increase the `fpp` to keep the bloom filter size within this limit.
* `loadOnHeap`: Whether to load the bloom filter using heap memory or off-heap memory \(`false` by default\).

There are 2 ways of configuring bloom filter for a table in the table config:

* Configure bloom filter columns with default settings

```javascript
{
  "tableIndexConfig": {
    "bloomFilterColumns": [
      "playerID",
      ...
    ],
    ...
  },
  ...
}
```

* Configure bloom filter columns with customized parameters

```javascript
{
  "tableIndexConfig": {
    "bloomFilterConfigs": {
      "playerID": {
        "fpp": 0.01,
        "maxSizeInBytes": 1000000,
        "loadOnHeap": true
      },
      ...
    },
    ...
  },
  ...
}
```

Currently bloom filter can only be applied to the dictionary-encoded columns. Bloom filter support for raw value columns is WIP.

