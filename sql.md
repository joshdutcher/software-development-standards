# Company Development Standards - SQL Syntax

### General SQL syntax
* snake_case. Period.
* More likely than not, Entity Attribute Value stores are not meant for (My)SQL
* Do not use double quotes ("), use single quotes instead (')
* Use `#` to comment out single lines, along with `/*` and `*/` to comment out multiple lines
* OOP doesn't generally translate well into (My)SQL. Schemas should not be designed around the OOP code, rather it should be designed around the data it's storing

### Naming Conventions
* Spell out words, avoid abbreviations
* Must not end with an underscore
* Use only alpha-numeric characters (and underscores)
  * Use underscores where you would typically use a space
* Use plural words for tables, singular words for columns
  * If possible, use a collective name (staff), instead of the plural (employees)
  * A table should never share a name with one of its columns
* All tables should have a Primary Key, preferably an `id` column.
  * An allowable exception is a 1:many relationship (lookup) table, where the "1" column may be the primary column
* A relationship (lookup) table should mention, in the name, both tables it is linking
* Aliases need to relate to what they are abbreviating (first_name -> fn, etc)
  * If you are aliasing a `SUM()`, `AVG()`, `COUNT()`, etc, use the original column name as the alias

### Query Syntax
* Use aliases whenever possible
* Use uppercase letters for sql reserved words such as `SELECT` and `WHERE`
* Use new lines after each sql reserved word (`SELECT`, `FROM`, `WHERE`, `GROUP BY`, `AND`, etc)
  * The `WHERE` clause might need to be split up into multiple lines, ~80 chars/line
* Spaces should surround equals signs, comparison signs, commas, etc
* Sub queries need to be indented one tab per sub query, per level
* Use ` characters to wrap around table names and column names when they are separated

### Optimizations
* Avoid `SELECT *` in queries, specifically spell out columns
* `INSERT` statements need column names specified
* Use `BETWEEN` whenever possible instead of multiple `AND`s
* Use `IN()` whenever possible instead of multiple `OR`s
* If a value needs to be evaluated before "leaving" the database, use `CASE` to organize the results into the necessary logical structure
* Avoid TEMP tables whenever possible
* Use `INT` or `DECIMAL` data types over `REAL` or `FLOAT`, as they might cause rounding errors
* Find alternatives for the `BLOB` datatype (reference a file path to a server file, etc)
* Indexing a `VARCHAR` column is based off of the creation statement, not by how many characters are actually being used
* Default values should be the same type as the column, ie a `decimal` should not have an `int` default
* Avoid 1:1 lookup tables whenever possible, as this adds query time
* Use `NULL` values over empty string values
* Running `SELECT * FROM <table_name> procedure analyse();` might give aid to building a more efficient table (once your data is in it)

### Constraints
* The "parent" referenced column should be a unique (and possibly) a primary key
* Both the parent and "child" columns need to be the same data type (`INT(11)` and `INT(11)`, `VARCHAR(25)` and `VARCHAR(25)`, etc)
* Constraint names need to reference (in this case, abbreviations are allowed)
  * The parent table
  * The parent column
  * The child column
* When defining a constraint, it should be placed right below the child column it represents in the create table statement
* Specify `ON DELETE` before `ON UPDATE` (but specify both)
* Constraints can specify multiple columns in multiple tables, or just be used as a data integrity check
* Where applicable, use `LIKE`, `SIMILAR TO`, and/or `CHECK` to ensure integrity, i.e.
	```
	CONSTRAINT pens_total_check
		CHECK(pens_total >0 AND pens_total <= 100)
	```

### Acceptable suffixes
* **_id:** Typically used to foreign key to a primary id in another table
* **_status:** Typically a flag value, such as player_status or shipment_status
* **_total:** A total or collection of values
* **_num:** A numerical field, but not mandatory (ie years_active)
* **_name:** Specific type of name, ie middle_name
* **_date:** A date field, such as start_date,
* **_size:** ie file_size, shoe_size
* **_addr:** An appropriate abbreviation for address, such as home_addr

### Odds and ends
* When defining a stored procedure, function, or event:
  * Declare all of your variables at the very beginning, one per line
* When defining a table, stored procedure, function, or event:
  * Add a `Comment = 'x'` clause briefly explaining the purpose of said object
  * For multi-step procedures, functions, or events you must at least use single (try to avoid multiple) line comments to explain what the next step is doing
