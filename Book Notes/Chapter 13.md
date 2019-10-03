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
record and to wait for a subsequent insertion before reusing the space. To keep track of open spaces, we use an additional structure called a file header, which is often allocated a certain number of bytes at the beginning of a file.

A **file header** contains a variety of information about the file, but the one we are interested in is the location of the first record whose contents are deleted. The first deleted record will then point to the location of the second deleted element, and so on. This forms a linked list of deleted locations, which is often referred to as a **free list**.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-68.png)

## Variable-Length Records
Variable-length records exists because of several reasons such as strings, repeating fields like arrays, and multiple record types within a file.

There are two problems when implementing variable-length records:
1. How to represent a single record in such a way that individual attributes can be extracted easily, even if they are of variable length.
2. How to store variable-length records within a block, such that records in a block can be extracted easily.

There are two parts in the representation of a record with variable-length attributes:
- First, there is an initial part with fixed-length information
- Second, there are the actual contents of the variable-length attributes.

Fixed-length attributes are allocated as many bytes as required. Variable-length attributes are represented in the initial part of the record by a pair (_offset, length_), where _offset_ is the location where the attribute starts, and _length_ is length of the attribute in bytes.

An example of such a record representation is shown in Figure 13.5. The figure
shows an _instructor_ record whose first three attributes _ID_, _name_, and _dept_name_ are
variable-length strings, and whose fourth attribute _salary_ is a fixed-sized number. We
assume that the _offset_ and _length_ values are stored in two bytes each, for a total of 4
bytes per attribute. The _salary_ attribute is assumed to be stored in 8 bytes, and each
string takes as many bytes as it has characters.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-69.png)

Figure 13.5 also shows a **null bitmap**, which indicates which attributes of the record have a null value. Since the _salary_ attribute is the fourth attribute stored in the first section of memory, a null bitmap of 0001 would set the _salary_ attribute to null. Since Figure 13.5 only shows four attributes, the null bitmap can be stored in one byte, but more memory will be necessary for records with more attributes.

Next, we address the structure of a block. The **slotted-page structure** is commonly used for organizing records within a block and is shown in Figure 13.6.

![](https://github.com/stinsan/CS-4513-Database-Management-Systems/blob/master/Screenshots/databases-70.png)

The block's header contains the following information:
- The number of records entries
- The location of the end of the free space in the block
- An array whose entries contain the location and size of each record.

The records themselves are stored contiguously starting from the end of the block. There is free space between the final entry in the header array and the first record. When a record is inserted, memory is allocated for it at the end of the free space, and an entry containing its location is size is added to the header. If a record is deleted, the space that the record once occupied is freed, and the records in the block before the deleted record are moved so that the free space remains contiguous.

The slotted-page structure requires that there be no pointers that point directly to
records. Instead, pointers must point to the entry in the header that contains the actual
location of the record.
