<!--
author:   André Dietrich

version:  0.0.1

email:    LiaScript@web.de

logo:     logo.png

edit:     true

comment:  This template allows you to create database diagrams using dbdiagram.io
          directly in LiaScript with the DBML (Database Markup Language) syntax.

@onload
window.dbdiagram = function (dbml) {
    // Unicode-safe: UTF-8 → base64 → URL-encode
    const base64 = btoa(unescape(encodeURIComponent(dbml)));
    const c = encodeURIComponent(base64);
    return `https://dbdiagram.io/embed?c=${c}`
}
@end

@dbdiagram
<script style="width: 100%; display: block" run-once modify="false">
const dbml = `@'0`;

// Unicode-safe: UTF-8 → base64 → URL-encode
const base64 = btoa(unescape(encodeURIComponent(dbml)));
const c = encodeURIComponent(base64);

`HTML: 
<iframe
  src="https://dbdiagram.io/embed?c=${c}"
  width="100%"
  height="600px"
  style="border: 0"
  referrerpolicy="no-referrer"
  allowfullscreen
></iframe>
<center><a style="font-size: smaller" target="_blank" href="https://dbdiagram.io/embed?c=${c}">dbdiagram.io</a></center>`
</script>
@end

@dbdiagram.edit
<script style="width: 100%; display: block" run-once modify="//---\n" type="text/sql">
const dbml = `//---
@0
//---
`;

// Unicode-safe: UTF-8 → base64 → URL-encode
const base64 = btoa(unescape(encodeURIComponent(dbml)));
const c = encodeURIComponent(base64);

`HTML: 
<iframe
  src="https://dbdiagram.io/embed?c=${c}"
  width="100%"
  height="600px"
  style="border: 0"
  referrerpolicy="no-referrer"
  allowfullscreen
></iframe>
<center><a style="font-size: smaller" target="_blank" href="https://dbdiagram.io/embed?c=${c}">dbdiagram.io</a></center>`
</script>
@end

@dbdiagram.eval
<script>
const dbml = `@'input`;

// Unicode-safe: UTF-8 → base64 → URL-encode
const base64 = btoa(unescape(encodeURIComponent(dbml)));
const c = encodeURIComponent(base64);

console.html(`<iframe
  src="https://dbdiagram.io/embed?c=${c}"
  width="100%"
  height="600px"
  style="border: 0"
  referrerpolicy="no-referrer"
  allowfullscreen
></iframe>
<center><a style="font-size: smaller" target="_blank" href="https://dbdiagram.io/embed?c=${c}">dbdiagram.io</a></center>`)
"LIA: stop"
</script>
@end

-->

# dbdiagram

    --{{0}}--
dbdiagram.io is a powerful database design tool that uses DBML (Database Markup
Language) to create entity-relationship diagrams. It allows you to quickly design
and visualize database schemas with a simple and intuitive syntax. This template
brings dbdiagram.io's embedding capabilities directly into LiaScript.

For more information about DBML syntax, visit the [dbdiagram.io
documentation](https://dbdiagram.io/docs).

- __Try it on LiaScript:__

  https://liascript.github.io/course/?https://raw.githubusercontent.com/LiaTemplates/dbdiagram/main/README.md

- __See the project on Github:__

  https://github.com/LiaTemplates/dbdiagram

- __Experiment in the LiveEditor:__

  https://liascript.github.io/LiveEditor/?/show/file/https://raw.githubusercontent.com/LiaTemplates/dbdiagram/main/README.md

---

    --{{1}}--
Like with other LiaScript templates, there are three ways to integrate dbdiagram,
but the easiest way is to copy the import statement into your project. For more
information, see the [Sec. Implementation](#implementation).

                           {{1}}
1. Load the latest macros via (this might cause breaking changes)

   `import: https://raw.githubusercontent.com/LiaTemplates/dbdiagram/main/README.md`

   or the current version 0.0.1 via:

   `import: https://raw.githubusercontent.com/LiaTemplates/dbdiagram/0.0.1/README.md`

2. __Copy the definitions into your Project__

3. Clone this repository on GitHub


## `@dbdiagram`

    --{{0}}--
This is the most common way to embed database diagrams in LiaScript. It allows you
to create a database diagram directly from a DBML code block. The diagram is
rendered as an interactive iframe that displays your database schema with tables,
columns, relationships, and constraints.

``` sql @dbdiagram
Table users {
  id int [pk, increment] // primary key with auto-increment
  username varchar(50) [not null, unique]
  email varchar(100) [not null, unique]
  created_at timestamp [default: `now()`]
  
  indexes {
    (username, email) [unique]
  }
}

Table posts {
  id int [pk, increment]
  title varchar(200) [not null]
  content text
  user_id int [not null, ref: > users.id] // many-to-one relationship
  status varchar(20) [default: 'draft']
  created_at timestamp [default: `now()`]
  updated_at timestamp
}

Table comments {
  id int [pk, increment]
  post_id int [not null, ref: > posts.id]
  user_id int [not null, ref: > users.id]
  content text [not null]
  created_at timestamp [default: `now()`]
}

Table tags {
  id int [pk, increment]
  name varchar(50) [not null, unique]
}

Table post_tags {
  post_id int [ref: > posts.id]
  tag_id int [ref: > tags.id]
  
  indexes {
    (post_id, tag_id) [pk]
  }
}
```

    --{{1}}--
The diagram is fully interactive and supports zooming, panning, and exploring
relationships. Users can click on the link below the diagram to open it in a new
tab on dbdiagram.io for even more features.

## `@dbdiagram.edit`

    --{{0}}--
This macro allows your users to inspect and modify the DBML code that generates the
database diagram. It is useful for educational purposes, where you want to show the
underlying code and allow users to experiment with it. The difference is indicated
by the different background color of the code block. Simply double-click onto the
border of the diagram to reveal the code and modify it.

``` sql @dbdiagram.edit
Table products {
  id int [pk, increment]
  name varchar(100) [not null]
  description text
  price decimal(10,2) [not null]
  stock_quantity int [default: 0]
  category_id int [ref: > categories.id]
  created_at timestamp [default: \`now()\`]
}

Table categories {
  id int [pk, increment]
  name varchar(50) [not null, unique]
  parent_id int [ref: > categories.id] // self-referencing for hierarchy
}

Table orders {
  id int [pk, increment]
  customer_name varchar(100) [not null]
  customer_email varchar(100) [not null]
  total_amount decimal(10,2) [not null]
  status varchar(20) [default: 'pending']
  created_at timestamp [default: \`now()\`]
}

Table order_items {
  id int [pk, increment]
  order_id int [not null, ref: > orders.id]
  product_id int [not null, ref: > products.id]
  quantity int [not null]
  unit_price decimal(10,2) [not null]
  
  indexes {
    (order_id, product_id)
  }
}
```

    --{{1}}--
This interactive mode is perfect for teaching database design concepts, allowing
students to modify table structures, add or remove relationships, and see the
changes reflected immediately in the visual diagram.

## `@dbdiagram.eval`

    --{{0}}--
This macro turns a code block into an executable DBML editor. It is useful for
quickly testing and experimenting with database designs. Simply write your DBML
code in a code block and append `@dbdiagram.eval` after the closing backticks. When
you click the execute button, the diagram will be displayed in the terminal below.

``` sql
Table employees {
  id int [pk, increment]
  first_name varchar(50) [not null]
  last_name varchar(50) [not null]
  email varchar(100) [not null, unique]
  hire_date date [not null]
  department_id int [ref: > departments.id]
  manager_id int [ref: > employees.id] // self-referencing
  salary decimal(10,2)
}

Table departments {
  id int [pk, increment]
  name varchar(100) [not null, unique]
  location varchar(100)
  budget decimal(12,2)
}

Table projects {
  id int [pk, increment]
  name varchar(100) [not null]
  description text
  start_date date
  end_date date
  budget decimal(12,2)
}

Table employee_projects {
  employee_id int [ref: > employees.id]
  project_id int [ref: > projects.id]
  role varchar(50)
  hours_allocated int
  
  indexes {
    (employee_id, project_id) [pk]
  }
}
```
@dbdiagram.eval

## DBML Syntax Guide

    --{{0}}--
DBML (Database Markup Language) is a simple, readable syntax for defining database
schemas. Here are the key features supported by dbdiagram.io:

### Tables

Define tables with columns and their properties:

```sql
Table table_name {
  column_name column_type [settings]
}
```

### Column Types

Common column types include:

- `int`, `integer`
- `varchar(n)`, `char(n)`
- `text`
- `decimal(p,s)`, `numeric(p,s)`
- `timestamp`, `datetime`, `date`, `time`
- `boolean`, `bool`

### Column Settings

- `pk` - Primary key
- `not null` - Not nullable
- `null` - Nullable (default)
- `unique` - Unique constraint
- `increment` - Auto-increment
- `default: value` - Default value

### Relationships

Define relationships between tables:

```sql
Table orders {
  user_id int [ref: > users.id] // many-to-one
}

// Or separately:
Ref: orders.user_id > users.id

// Relationship types:
// >  : many-to-one
// <  : one-to-many
// -  : one-to-one
// <> : many-to-many
```

### Indexes

Define indexes for better query performance:

```sql
Table table_name {
  indexes {
    column1 [type: btree]
    (column1, column2) [unique]
    column3 [pk]
  }
}
```

### Notes and Documentation

Add notes to tables and columns:

```sql
Table users {
  id int [pk, note: 'Unique identifier']
  
  Note: 'This table stores user information'
}
```

## Implementation

The LiaScript implementation of dbdiagram uses the dbdiagram.io embedding API,
which encodes DBML code in base64 format and passes it as a URL parameter to their
iframe embed service. The implementation handles Unicode characters safely to
support international text in table and column names.

```markdown
@onload
window.dbdiagram = function (dbml) {
    // Unicode-safe: UTF-8 → base64 → URL-encode
    const base64 = btoa(unescape(encodeURIComponent(dbml)));
    const c = encodeURIComponent(base64);
    return `https://dbdiagram.io/embed?c=${c}`
}
@end

@dbdiagram
<script style="width: 100%; display: block" run-once modify="false">
const dbml = `@0`;
const base64 = btoa(unescape(encodeURIComponent(dbml)));
const c = encodeURIComponent(base64);
`HTML: 
<iframe
  src="https://dbdiagram.io/embed?c=${c}"
  width="100%"
  height="600px"
  style="border: 0"
  referrerpolicy="no-referrer"
  allowfullscreen
></iframe>
<center><a style="font-size: smaller" target="_blank" href="https://dbdiagram.io/embed?c=${c}">dbdiagram.io</a></center>`
</script>
@end

@dbdiagram.edit
<script style="width: 100%; display: block" run-once modify="//---\n" type="text/sql">
const dbml = `//---
@0
//---
`;
const base64 = btoa(unescape(encodeURIComponent(dbml)));
const c = encodeURIComponent(base64);
`HTML: 
<iframe
  src="https://dbdiagram.io/embed?c=${c}"
  width="100%"
  height="600px"
  style="border: 0"
  referrerpolicy="no-referrer"
  allowfullscreen
></iframe>
<center><a style="font-size: smaller" target="_blank" href="https://dbdiagram.io/embed?c=${c}">dbdiagram.io</a></center>`
</script>
@end

@dbdiagram.eval
<script>
const dbml = `@input`;
const base64 = btoa(unescape(encodeURIComponent(dbml)));
const c = encodeURIComponent(base64);
console.html(`<iframe
  src="https://dbdiagram.io/embed?c=${c}"
  width="100%"
  height="600px"
  style="border: 0"
  referrerpolicy="no-referrer"
  allowfullscreen
></iframe>
<center><a style="font-size: smaller" target="_blank" href="https://dbdiagram.io/embed?c=${c}">dbdiagram.io</a></center>`)
"LIA: stop"
</script>
@end
```

