# Chapter 14: Indexing

## 14.1 | Basic Concepts
Indices are critical for efficient processing of queries in databases. Without indices,
every query would end up reading the entire contents of every relation that it uses;
doing so would be unreasonably expensive for queries that only fetch a few records.

There are two types of indices:
- **Ordered indices**: Based on a sorted ordering of the values.
- **Hash indices**: Based on a uniform distribution of values across a range of buckets.
The bucket to which a value is assigned is determined by a function, called a hash
function.

Ordered indexing techniques can be evaluated on the following:
- **Access type**: The types of access that are supported efficiently. Access types can
include finding records or set of records with specified attribute values.
- **Access time**: The time it takes to find a particular data item, or set of items.
- **Insertion time**: The time it takes to insert a new data item. This value includes the time it takes to update the
index structure.
- **Deletion time**: The time it takes to delete a data item. This value includes the time
it takes to update the index structure.
- **Space overhead**: The additional space occupied by an index structure.

An attribute or set of attributes used to look up records in a file is called a **search
key**.

## 14.2 | Ordered Indices

An **ordered index** stores the values of the search keys in sorted order
and associates with each search key the records that contain it.

A **clustering index (primary index)** is an index whose search key
also defines the sequential order of the file.

A **nonclustering index (secondary index)** is an index whose search key
specifies an order different from the sequential order of the file.

Files with a clustering index on the search key
are called **index-sequential files**.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-71.png)

### Dense and Sparse Indices

An **index entry** consists of a search-key value and pointers to one or
more records with that value as their search-key value. The pointer to a record consists
of the identifier of a disk block and an offset within the disk block to identify the record
within the block.

There are two types of ordered indices:
- **Dense index**: In a dense index, an index entry appears for every search-key value
in the file. In a dense clustering index, the index record contains the search-key
value and a pointer to the first data record with that search-key value. The rest of
the records with the same search-key value would be stored sequentially after the
first record, since, because the index is a clustering one, records are sorted on the
same search key. In a dense nonclustering index, the index must store a list of pointers to all
records with the same search-key value.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-74.png)
![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-76.png)

- **Sparse index**: In a sparse index, an index entry appears for only some of the searchkey values. Sparse indices can be used only if the relation is stored in sorted order of the search key; that is, if the index is a clustering index. As is true in dense
indices, each index entry contains a search-key value and a pointer to the first data
record with that search-key value. To locate a record, we find the index entry with
the largest search-key value that is less than or equal to the search-key value for
which we are looking. We start at the record pointed to by that index entry and
follow the pointers in the file until we find the desired record.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-75.png)

Dense indices often take up a lot of overhead space, but make finding a record faster. On the other hand, sparse indices do not take up much overhead space, but are slower at finding records. A good tradeoff is to have a sparse index with one index entry per block.

### Multilevel Indices
There is an issue with dense indices in that if the primary index does not fit into main memory, access becomes expensive.

To deal with this problem, we treat the index just as we would treat any other
sequential file, and we construct a sparse outer index on the original index, which we
now call the inner index, as shown in Figure 14.5

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-77.png)

To locate a record, we first use
binary search on the outer index to find the record for the largest search-key value less than or equal to the one that we desire. The pointer points to a block of the inner index.
We scan this block until we find the record that has the largest search-key value less
than or equal to the one that we desire. The pointer in this record points to the block
of the file that contains the record for which we are looking.

If the databases grows, and the outer index can no longer fit in main memory, another level of indices can be created. This process can be repeated as many times as needed. y. Indices with two or more levels are called **multilevel indices**.

### Index Update
This section was skipped in lecture.

### Secondary Indices
Secondary indices must be dense, with an index entry for every search-key value, and a
pointer to every record in the file. If a
secondary index stores only some of the search-key values, records with intermediate
search-key values may be anywhere in the file and, in general, we cannot find them
without searching the entire file.

If the search key of a secondary index is not a candidate key, it is
not enough to point to just the first record with each search-key value. The remaining
records with the same search-key value could be anywhere in the file, since the records
are ordered by the search key of the clustering index, rather than by the search key
of the secondary index. Therefore, a secondary index must contain pointers to all the
records.
