## Reference Information
- https://www.youtube.com/watch?v=DJ5u5HrbcMk&list=PLSE8ODhjZXjbj8BMuIrRcacnQh20hmY9g&index=4
---
## 3 - Database Storage

### SQL QUERY Processing Overview
When you receive a query from the application it goes to the following stages

App -> Query Planning -> Operator Execution -> Access Methods -> Buffer Pool Manager -> Disk Manager

Each stage is exposed to the others using an API, these stages collected together form the DBMS

Query Planning: Optimise the query and turn them into a sequence of operations

Operator Execution: Start execution of each operation

Access Methods: This stage talks to the tables or indexes, whatever you want to access.

Buffer pool manager: Manages what happens in the memory.

Some operations might need information from all stages, like crash recovery.

---
### DBMS System Design Goals

- Allow DBMS to exceed amount of memory available (as if everything is already in memory)
	- This sounds a lot like Virtual Memory, but this is something we want to avoid using when designing a DBMS
- Do as much work on the memory before returning data to disk
- Maximize sequential access (i.e. minimize random access)
---
### Why not use the OS ?
TL;DR: 
Virtual memory doesn't know what happens inside the system, it sees everything as reads and writes.
Concepts like Transactions, Pages are not considered.
****
#### Problem #1: Transaction Safety
If we want to write multiple Pages, the OS doesn't know that some Pages may need to be written to disk before others
Also, it might write a page
#### Problem #2: I/O Stalls
If a thread tries to access a page that is not in memory, it gets a Page Fault Error and it gets stalled.
Meanwhile, the thread could do other things while waiting for MMU to get the page, but it can't since OS has the control
#### Problem #3: Error Handling
Any error results in a `SIGBUS` error that the DBMS must handle
#### Problem #4: Performance Issues
OS has its own internal data structure that may conflict with the best queries for the DBMS

#### DBMS (almost) always wants to control things itself and can do better than the OS
- Flushing dirty pages to disk in the **correct** order.
- Specialized prefetching
- Buffer replacement policy
- Thread/process scheduling
****
### Database Storage

We have two problems

1. How DBMS stores files on disk (This lecture)
2. How DBMS moves pages back-and-forth between disk and memory

****
### Storage Manager / Storage Engine
Responsible for maintaining a database's files.
- Some do their own scheduling for reads and writes to improve **spatial** and **temporal** locality of pages
It organizes the files as a collection of pages
- Tracks data read/written to pages
- Track available space
A DBMS typically does **NOT** maintain multiple copies of a page on disk

---
### Database Pages

A page is a **fixed-size block of data**
- It can contain **different types of data** but usually not at the same time
- It can contain tuples, meta-data, indexes, log records
- Some systems require a page to be self-contained
	- i.e. a page must contain the meta data required to read its content even if all other files of the database are lost.
Each page is given a **unique identifier**
- DBMS uses an indirection layer to map `Page Id -> Physical Location`
#### Three Types of Pages
1. **Hardware Pages** (usually 4 KB)
	The largest block of data that the **storage device** cam guarantee fail-safe writes
2. **OS Page** (usually 4 KB, x64 2 MB/1 GB)
	The pages used by the OS to keep track of locations in memory
3. **Database Page** (512 B - 32 KB)
	The page size used by the DBMS
##### No free lunch
A larger page makes sequential access better and is easier to read smaller data, but if you have a lot of reads from different parts of the system that are not contiguous, you'll find yourself performing worse.

---
### How DBMS Stores Files On Disk
 The DBMS stores a db as **one or more files** on disk typically in a **proprietary format**
- Portable file formats like parquet and Arrow
- Some DBMSs tried to build their own filesystem, but most newer DBMSs don't do that.

Different DBMSs manage pages in a different way

#### Heap File Organization

A collection of unordered pages with tuples stored in random order

Operations provided
- CRUD
- Must allow iterating over all pages
##### Finding a page
Pages are easy to find if there is a **single file**.
If I want page 10 then I go to the address `page_address = file_start_loc + 10 * page_size`

Things get harder when there are multiple files, so we need to keep some metadata in order to find these things quickly.

The thing we use for this is called a **Pages Directory**

###### Pages Directory

The page directory is a Map (dictionary) that keeps track of the **location** of different pages stored by the database.
-> We must make sure that the directory pages are in sync with the data pages (i.e. if we delete a data page, we shouldn't find its address in the pages directory)

The Pages Directory also records meta-data about **available space**:
- Number of free slots per page
- List of free / empty pages
#### Tree File Organization
#### Sequential / Sorted File Organization (ISAM)
Not as used anymore
#### Hashing File Organization

Hash table lookups for files

---
### Page Layout
![[page-header.png]]

### Tuple Layout inside Page

The most common way to store typles is to use a **slotted page**

#### Slotted Page
![[slotted-page-02.png]]

The slotted page contains a map `Tuple # -> Tuple Location`

The header keeps track of:
-> # of used slots
-> The offset of the starting location of the last used slot
-> End of free space
![[slotted-page-01.png]]
You can see as we need to create more tuple (assuming there is empty space), The Slot array and tuples occupy space in memory on different ends (just like stack and heap of an app)
****
Notice that this is self contained. If we decide to change the location of Tuple #3, we only need to change the address in the slot array. We don't need to notify all database objects pointing to Tuple #3 that its address has changed.
****

#### Record Ids
The database also has a way to uniquely identity the tuple based on the location (think of it as a primary key but **NOT FOR APPLICATION USER**)
This is called a Record ID

The DBMS assigns each logical tuple in a Slotted Page a unique Record ID that represents its **physical location** in the database
-> `(File Id, Page Id, Slot #)`
-> Most DBMSs do not store ids in the Tuple
-> SQLite uses Record ID as the **true primary key** and stores them as a **hidden attribute**

Applications **should never** rely on those IDs to mean anything.
A database can change them at any time.
****

![[slotted-page-layout.png]]
****
### Deleting tuples from a slotted page
If a tuple is deleted, we set the size of it to -1 (for example). Furthermore, the previous tuples are moved, so that the space by the deleted document is occupied. Then we adjust the pointer to the free space accordingly.

The cost of moving the tuples is not too high since the size of the block is limited. typical values are around 4 to 8 KB.
