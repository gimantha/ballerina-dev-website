---
layout: ballerina-persist-documentation-left-nav-pages-swanlake
title: Bal persist overview
description: The section gives an overview of the `bal persist` feature.
keywords: ballerina, programming language, ballerina packages, persist, persist model, persist cli, persist client api
permalink: /learn/ballerina-persist/persist-overview/
active: persist_overview
intro: The `bal persist` feature allows you to store data in different data stores and retrieve them when needed. A data store can be a database, an in-memory cache, or a file system. The `bal persist` feature currently supports in-memory tables, MySQL databases, and Google Sheets as data stores. The important point is you can use the same syntax to access data in all these data stores. Therefore, you don't need to learn different syntaxes to access data in different data stores.
redirect_from:
- /learn/ballerina-persist/persist-overview/
---
This feature has three main components: Persist Model, Persist CLI, and Persist Client API. The Persist Model is used to define the data model. The Persist CLI is used to generate the client API for the data model. The Persist Client API is used to access the data in the data store.

## Persist Model

The Persist Model is used to define the data model. The data model is defined in a Ballerina source file inside the `persist` directory in the project root directory. The data model is defined using the Ballerina record type. The following example shows how to define a data model for a table named `Employee` with the fields `id`, `name`, `age`, and `salary`.

```ballerina
type Employee record {
    readonly int id;
    string name;
    int age;
    float salary;
}
```

The `readonly` keyword is used to define a field as a primary key. You need to have at least one primary key field in the `Entity` record. The primary key field is used to uniquely identify a record in the table. The primary key field is used to generate the `get`, `update`, and `delete` operations in the client API. You can define multiple primary key fields in the `Entity` record.

Learn about how to define relationships between entity records and more about the persist model in the [Persist Model](/learn/persist-model/) section.

> **Note:** The Ballerina VS Code plugin provides validation and code actions for the persist model. Therefore, you can easily create the Persist Model using the Ballerina VS Code plugin.

## Persist CLI

The `bal persist` CLI is used to generate the client API for the data model. Based on the data store, additional configurable files and setup scripts are generated. For example, if you are using MySQL as the data store, the `persist_db_config.bal` file and the `script.sql` script are generated. The `persist_db_config.bal` file is used to configure the MySQL database connection. The `script.sql` file is used to create the table in the MySQL database. The client API is generated in the `generated` directory in the project root directory.

The `bal persist` has two main built-in CLI commands: `persist init` and `persist generate`. The `persist init` command is used to initialize `bal persist` in the Ballerina project. The `persist generate` command is used to generate the client API for the data model. Additionally, there is an experimental `persist migrate` command to generate the SQL scripts for changing the table structure in the database when the data model is changed.

Learn more about `bal persist` CLI in the [Persist CLI](/learn/persist-cli/) section.

## Persist Client API

The Persist Client API is used to access the data in the data store. The client API is generated using the Persist CLI and it is located in the `generated` directory in the project root directory. The client API consists of a Ballerina client object with a resource for each entity record modeled in the Ballerina project. For each client resource, there are resource methods to perform CRUD operations on the data store.

For example, if you have an `Employee` entity record in the Ballerina project, you can perform CRUD operations on the `Employee` table in the data store using the client object as follows.

```ballerina
// Create a new `employee` record.
EmployeeInsert employee = {id: 1, name: "John", age: 30, salary: 3000.0};
int[]|error employeeId = sClient->/employees.post([employee]);

// Get the `employee` record with the ID 1.
Employee|error employee = sClient->/employees/1;

// Update the `employee` record with the ID 1.
Employee|error updated = sClient->/employees/1.put({salary: 4000.0});

// Delete the employee record with the ID 1.
Employee|error deleted = sClient->/employees/1.delete();

// Get records of all employees.
stream<Employee, error?> employees = sClient->/employees;
```

Learn more about persist client APIs in the [Persist Client API](/learn/persist-client-api/) section.

If you want to get started with a practical introduction and learn about `bal persist`, head over to the [Quick Start Guide](quick-tour.md) section.