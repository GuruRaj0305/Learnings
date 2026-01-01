# What is Normalization?
Normalization is the steps/process of making database specific and distinct to reduce data redundancy. It makes the database easier to maintain and more efficient.

## Normal Forms

### 1NF (First Normal Form)
- Each column should have atomic (indivisible) values.
- Each record should be unique.

### 2NF (Second Normal Form)
- Must be in 1NF.
- All non-key attributes should depend on the entire primary key (no partial dependencies).

### 3NF (Third Normal Form)
- Must be in 2NF.
- No transitive dependencies (non-key attributes should not depend on other non-key attributes).

### BCNF (Boyce-Codd Normal Form)
- A stricter version of 3NF.
- Every determinant must be a candidate key ( For every functional dependency A → B, A must be a candidate key. ).

### 4NF (Fourth Normal Form)
- Deals with multi-valued dependencies.
- Ensures that no table contains more than one multi-valued dependency.

### 5NF (Fifth Normal Form)
- Also called Project-Join Normal Form.
- Ensures that any join dependencies are also represented correctly, so no loss of data occurs when splitting or joining tables.

In practice, databases often aim for at least 3NF or BCNF to ensure efficiency and data integrity.

