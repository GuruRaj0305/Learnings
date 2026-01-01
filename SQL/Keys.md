# Types of Keys in a Database

## Primary Key
- A column or set of columns that uniquely identifies each record in a table.
- Must be unique and not null.

## Foreign Key
- A field in one table that references the primary key in another table.
- Maintains referential integrity between tables.

## Candidate Key
- Any column or set of columns that could qualify as a primary key.
- A table can have multiple candidate keys.

## Alternate Key
- A candidate key that is not chosen as the primary key.
- Still unique but not the main key.

## Composite Key
- A primary key made up of two or more columns.
- Used when one column alone isn't enough to uniquely identify each row.

## Super Key
- Any set of columns that can uniquely identify a record.
- Includes the primary key but may have extra columns.

## Surrogate Key
- An artificial key, often a generated number, used as a unique identifier.
- Not derived from application data.

## Unique Key
- A key that enforces uniqueness for a column but is not the primary key.
- Ensures no duplicate values in the specified column.

So in total, that gives you eight common types of keys to remember: primary, foreign, candidate, alternate, composite, super, surrogate, and unique keys.

