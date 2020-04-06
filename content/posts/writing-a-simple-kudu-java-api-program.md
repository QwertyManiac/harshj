---
title: "Writing a simple Kudu Java API Program"
date: "2015-10-01"
---

This post is about using [Apache Kudu service](http://kudu.apache.org), via its [Java API interface](https://kudu.apache.org/apidocs/), in a Maven-based Java project.

<!--more-->

Kudu ships with a [Java client library](https://github.com/apache/kudu/tree/master/java) that is ready to use via an `Apache Maven` build configuration.

The `dependency` to add in a `pom.xml` would be:
```xml
<dependency>
  <groupId>org.apache.kudu</groupId>
  <artifactId>kudu-client</artifactId>
  <version>1.0.0</version>
</dependency>
```

To start off, you will need to create a [KuduClient](https://kudu.apache.org/apidocs/org/apache/kudu/client/KuduClient.html) object, like such:

```java
KuduClient kuduClient =
  new KuduClientBuilder("kudu-master-hostname")
  .build();
```

To create a table, a schema needs to be built first, and then created via the client.

Lets say we have a simple schema for table `users`, with columns `username` with type (`string`, `key`) and `age` with type (`8-bit signed int`, `value`), then we can create it as shown below, utilising [ColumnSchema](https://kudu.apache.org/apidocs/org/apache/kudu/ColumnSchema.html) and [Schema](https://kudu.apache.org/apidocs/org/apache/kudu/Schema.html) objects via the previously created `kuduClient` object (above):

```java
ColumnSchema usernameCol = new ColumnSchemaBuilder("username", Type.STRING)
  .key(true)
  .build();
ColumnSchema ageCol = new ColumnSchemaBuilder("age", Type.INT8)
  .build();

List<ColumnSchema> columns = new ArrayList<ColumnSchema>();
columns.add(usernameCol);
columns.add(ageCol);

Schema schema = new Schema(columns);
String tableName = "users";

if (!kuduClient.tableExists(tableName)) {
  client.createTable(tableName, schema);
}
```

Data work (such as inserts, updates or deletes) in Kudu are done over [Sessions](https://kudu.apache.org/apidocs/org/apache/kudu/client/KuduSession.html). The below shows how to insert a row after creating a `Session` and a [KuduTable](https://kudu.apache.org/apidocs/org/apache/kudu/client/KuduTable.html) connection for the table `users` and applying an insert instruction through it:

```java
KuduSession session = kuduClient.newSession();
KuduTable table = kuduClient.openTable(tableName);

Insert insert = table.newInsert();
insert.getRow().addString("username", "harshj");
insert.getRow().addInt("age", 25);

session.apply(insert);
```

Likewise, you can update existing rows:

```java
Update update = table.newUpdate();
// Specify the key (unchanged)
update.getRow().addString("username", "harshj");
// Change age from value '25' previously written to '26'
update.getRow().addInt("age", 26);

session.apply(update);
```

Or even delete them by key (`username` column):

```java
Delete delete = table.newDelete();
delete.getRow().addString("username", "harshj");

session.apply(delete);
```

Reading rows can be done via the [KuduScanner](https://kudu.apache.org/apidocs/org/apache/kudu/client/KuduScanner.html) class. The below example shows how to fetch only the key column data (all rows):

```java
List<String> columnNames = new ArrayList<String>();
columnNames.add("username");

KuduScanner scanner = kuduClient.newScannerBuilder(tableName)
  .setProjectedColumnNames(columnNames)
  .build();

while (scanner.hasMoreRows()) {
  for (RowResult row : scanner.nextRows()) {
    System.out.println(row.getString("username"));
  }
}
```

Another fully runnable example can be found on Kudu's [kudu-examples](https://github.com/cloudera/kudu-examples/tree/master/java/java-sample) repository.
