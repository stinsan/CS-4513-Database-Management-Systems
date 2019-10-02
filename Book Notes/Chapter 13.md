# Chapter 13 | Data Storage Structures
## 13.1 | Database Storage Architecture
We did not cover this section in lecture.

## 13.2 | File Organization
A databases is stored as a collection of **files**, with each file having a sequence of **records**, and each record being a sequence of fields.

Each file is partitioned into fixed-length storage units called **blocks**. We assume that a file cannot be larger than the block size and that a file will be contained in a single block.

There are two ways to store a record:
- fixed-length
- variable-length

### Fixed-Length Records
Let us look at a file of _instructor_ records for our university database:

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-64.png)

Assume that each character occupies 1 byte and that numeric (8,2) occupies 8
bytes. Suppose that instead of allocating a variable amount of bytes for the attributes
_ID_, _name_, and _dept_name_, we allocate the maximum number of bytes that each attribute
can hold. Then, the _instructor_ record is 53 bytes long. A simple approach is to use the
first 53 bytes for the first record, the next 53 bytes for the second record, and so on.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-65.png)

However, there are two problems with this implementation:
1. If the block size is not a multiple of 53, some records will cross into the next block. This will require two block accesses to read/write that specific record.
2. Deleting records is a hassle. The deleted record must be filled up so that memory is still contiguous, or a way of ignoring deleted files must be implemented.

To solve the first problem, we allocate only as many records as can fit into the block. We could do this by dividing the block size by the record size and discarding the fractional part of the answer.

The solve the second problem of record deletion, there are three possible solutions:
1. Move the record that comes after it into the space formerly occupied by the deleted record, and so on, until every record following
the deleted record has been moved. This solution is shown in Figure 13.2.
2. Move the last record of the file into the space formerly occupied by the deleted record. This solution is shown in Figure 13.3.
3. The usage of a file header.

Since solutions 1 and 2 require multiple block accesses, solution 3 is the most appealing.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-66.png)

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-67.png)

Since insertions tend to be more
frequent than deletions, it is acceptable to leave open the space occupied by the deleted
record and to wait for a subsequent insertion before reusing the space. To keep track of open spaces, we use an additional structure a file, which is often allocated a certain number of bytes at the beginning of a file.
A **file header**


