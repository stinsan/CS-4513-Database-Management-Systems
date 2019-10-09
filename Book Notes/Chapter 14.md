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

- **Sparse index**: In a sparse index, an index entry appears for only some of the searchkey values. Sparse indices can be used only if the relation is stored in sorted order of the search key; that is, if the index is a clustering index. As is true in dense
indices, each index entry contains a search-key value and a pointer to the first data
record with that search-key value. To locate a record, we find the index entry with
the largest search-key value that is less than or equal to the search-key value for
which we are looking. We start at the record pointed to by that index entry and
follow the pointers in the file until we find the desired record.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-75.png)



