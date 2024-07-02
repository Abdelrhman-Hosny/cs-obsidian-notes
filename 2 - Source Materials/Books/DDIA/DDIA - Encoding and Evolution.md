## Reference Information

- Kleppmann, Martin. "Chapter 4" in _Designing Data-Intensive Applications_, O'Reilly Media, 2017.

---
## DDIA - Encoding and Evolution

Encoding (Serialization) is the way data is presented to the user or can be used to store database file like how MongoDB uses BSON to store documents.

This often affects how you want to store the data in the system, as adding or removing a field will require code changes.

You have to be careful during the deployment of those code changes, if multiple clients share the same database, you have to make sure that the code supports both versions after modifying a schema.

- With client-side apps (mobile), if the user doesn't update the app, this might result to improper encoding and decoding.

## Language Specific Encoding Formats

Examples are `pickle`, `java.io.Serializable` for python and java.

It is often a **bad idea** to use a language specific format
- they are intended for ease of use not for being used in production
- you are tying yourself to a specific language and making integration with other systems harder
- Versioning is often an afterthought for these libraries

## Binary Encoding
Binary encoding is smaller in size than other forms like JSON and XML.
However the trade off is not worth it in small datasets.

Several approaches came like BSON and WBXML 

## Thrift and Protocol Buffers
Apache Thrift and protobuf are binary encoding libraries.
You must have a schema to use them unlike JSON which has it's schema inside the file as a key value pair.

```
# thrift schema
struct Person {
  1: required string userName,
  2: optional i64 favoriteNumber,
  3: optional list<string> interests
}
```

The number before the field name is called the `fieldTag`, it is used as a unique id for that field. So when deserializing, we don't identify the field by the name instead we do so using the field tag.

This reduces the size of the data stored as you don't have to store the field of the name with each file.

---
### Field tags and schema evolution

How do we deal with schema changes while keeping forward and backward compatibility ?

The name can be freely changed as we use the field tag for identification purposes..

Adding a new field is easy as long as you give it a new field tag. The code in place will not know about that field and will ignore it, which will be okay as this feature wasn't there in the previous version. Having the datatype tells the parser how much bytes it needs to skip

This maintains **forward** compatibility: old code can read records that were written by new code.

What about **backward** compatibility ? (new code reading values written by old code)

As long as the fields added are optional, they can be read by older versions of code.


The same applies when removing fields for the forward and backward compatibility.

### Datatypes and schema evolution

That may be possible for some fields, but some other fields will lose precision (i32 -> i64)

## Avro

```
record Person {
  string  userName;
  union {null, long} favNumber = null;
  array<string> interests;
}
```

This is the most compact format, you'll notice that **there are no tags**.

To parse the binary, you go through the fields in the order they appear in the schema, and use the schema to tell you the datatype of each field.

This means to **decode data correctly** you need to use the **same exact schema**

### Schema Evolution
Avro has two schemas
1. reader schema (used by reader)
2. writer schema
The idea is that reader and writer schema don't have to be the same, they just have to be compatible
![[avro-schemas.png]]

#### Schema Evolution Rules
To maintain forward and backward compatibility, you can only add or remove fields that have a default value.
#### What is a writer's schema ?
How does reader know the writer's schema for the first time ?
Including writer schema with every request defeats the purpose of using binary format for compression

##### Large files with lots of records
Send the writer's schema once at the beginning then send all the rest of the files
##### Database with individually written records
Databases write at different points of time with different schemas.

The simplest solution is to add a version number and keep a list of schema versions

##### Sending records over a network connection

Negotiation of schema version can be done before sending data.

---
#### Dynamically Generated Schemas

Avro's encoding protocol offers an advantage over Protocol Buffers and Thrift by not requiring tag numbers in its schema. This feature is particularly beneficial for dynamically generated schemas.

For instance, when exporting the contents of a relational database to a binary format, Avro simplifies the process by allowing easy generation of a schema from the relational schema. Each database table is represented as a record, with each column mapped to a field in the Avro schema. If the database schema changes (e.g., a column is added or removed), a new Avro schema can be generated without needing to manually adjust the schema mapping. The data export process remains unaffected by these schema changes, as Avro handles field identification by name.

In contrast, using Thrift or Protocol Buffers would require manual assignment of field tags whenever the database schema changes. Automating this process is possible but complex, as it requires careful management to avoid reusing field tags.

---
## Modes of Dataflow

Dataflow can be done through
- Databases
- Service Calls
- Asynchronous message passing
I don't think that the information here is relevant at the moment.
---






