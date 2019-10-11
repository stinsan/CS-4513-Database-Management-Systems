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

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-78.png)

### Indices on Multiple Keys
We skipped this section in lecture.

## 14.3 | B+-Tree Index Files
As the size of index-sequential files grow, their performance degrades. This can usually be solved through the reorganization of the file, but frequent reorganizations are undesirable.

The **B+-tree index** structure is the most widely used of several index structures that
maintain their efficiency despite insertion and deletion of data. A B+-tree index takes
the form of a **balanced tree** in which every path from the root of the tree to a leaf of
the tree is of the same length.

We shall see that the B+-tree structure imposes performance overhead on insertion
and deletion and adds space overhead.

### Structure of a B+-Tree
A B+-tree index is a multilevel index, but it has a structure that differs from that of the
multilevel index-sequential file.

A B+-tree contains up to _n - 1_ seach-key values _K<sub>1</sub>, K<sub>2</sub>, ..., K<sub>n-1</sub>_, and _n_ pointers
  _P<sub>1</sub>, P<sub>2</sub>, ..., P<sub>n</sub>_ . The search-key values within a node are kept in sorted order; thus if _i < j_,
then K<sub>i</sub> < K<sub>j</sub>.
                                                                                                                                     
![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-79.png)

The structure of the **leaf nodes** of a B+-tree is as follows: <br/>
- For _i_ = 1, 2, ..., _n - 1_, pointer _P<sub>i</sub>_ points to a file record with search-key _K<sub>i</sub>_. 
Note how the final pointer, _P<sub>n</sub>_ is not included; this pointer plays a special purpose that will be discussed later.

- Each leaf can hold a maximum of _n - 1_ value and a minimum of _ciel((n - 1) / 2)_ values.

- If _L<sub>i</sub>_ and _L<sub>j</sub>_ are leaf nodes and i < j (that is, _L<sub>i</sub>_ is to the left of _L<sub>j</sub>_ in the tree), then every search-key value _v<sub>i</sub>_ in _L<sub>i</sub>_ is less than every search-key value _v<sub>j</sub>_ in _L<sub>j</sub>_.

- B+-trees are often used as a dense index. Thus, every search-key value must appear in some leaf node.

- The final pointer in a leaf node, _P<sub>n</sub>_ is used to chain together the leaf nodes in search-key order since there is a linear order on the leaves based on the search-key values.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-80.png)

The **nonleaf nodes** of the B+-tree for a multilevel, sparse index on the leaf nodes.
The structure of nonleaf nodes is the same as leaf nodes, except that all pointers point to other nodes. A nonleaf node holds up to _n_ pointers and must hold at least _ceil(n / 2)_ pointers.

Consider a node containing _m_ pointers such that _m <= n_. For _i = 2, 3, ..., m - 1_, the pointer _P<sub>i</sub>_ points to a subtree contains search-key values less that _K<sub>i</sub>_ and greater than or equal to _K<sub>i-1</sub>_. Pointer _P<sub>m</sub>_ points to the part of the subtree that contains those key values greater than or equal to _K<sub>m-1</sub>_, and pointer _P<sub>1</sub>_ points to the part of the subtree with search-key values less than _K<sub>1</sub>_.

Root nodes can hold fewer than _ceil(n / 2)_ pointers, but must contain at least two, unless there is only one node in the tree.

Figure 14.9 shows a complete B+-tree for the _instructor_ file with _n = 4_.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-81.png)

Figure 14.10 shows a B+-tree for the _instructor_ file with _n = 6_, note how the height of this tree is shorter than the previous.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-82.png)

### Queries on B<sup>+</sup>-Trees
Suppose that we wish to find a record with a given value _v_ for the search key. Figure 14.11 presents pseudocode for a
function _find(v)_ to carry out this task, assuming there are no duplicates.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-83.png)

Starting with the root as the current node, the function repeats the
following steps until a leaf node is reached. First, the current node is examined, looking
for the smallest _i_ such that search-key value _K<sub>i</sub>_ is greater than or equal to _v_. Suppose
such a value is found; then, if _K<sub>i</sub>_ is equal to _v_, the current node is set to the node pointed
to by _P<sub>i+1</sub>_, otherwise _K<sub>i</sub> > v_, and the current node is set to the node pointed to by _P<sub>i</sub>_. 
If no such value _K<sub>i</sub>_ is found, then _v > K<sub>m−1</sub>_, where _P<sub>m</sub>_ is the last nonnull pointer in the
node. In this case the current node is set to that pointed to by _P<sub>m</sub>_. The above procedure
is repeated, traversing down the tree until a leaf node is reached.

B+-trees can also be used to find all records with search key values in a specified
range [_lb, ub_], such queries are called **range queries**.
To execute such queries, we can create a procedure _findRange (lb, ub)_, shown in
Figure 14.12.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-84.png)

The procedure does the following: it first traverses to a leaf in a manner
similar to _find(lb)_; the leaf may or may not actually contain value _lb_. It then steps
through records in that and subsequent leaf nodes collecting pointers to all records
with key values _C.K<sub>i</sub>_ such that _lb ≤ C.K<sub>i</sub> ≤ ub_ into a set _resultSet_. The function stops when
_C.K<sub>i</sub> > ub_, or there are no more keys in the tree.
