# **Basics of PostgreSQL**

After installing **PostgreSQL** and **pgAdmin4**, proceed by creating a
user with **admin privileges**.\
Next, create a new database --- ensure that the **locale provider** is
set to **ICU**.\
Once the database is ready, open the **Query Tool** in pgAdmin4 to begin
practicing SQL commands.

------------------------------------------------------------------------

## **Basic Syntax**

### **Creating and Deleting Databases**

``` sql
-- Create a new database
CREATE DATABASE db_name;

-- Delete an existing database
DROP DATABASE db_name;
```

------------------------------------------------------------------------

### **Creating Tables**

``` sql
CREATE TABLE actors (
    actor_id SERIAL PRIMARY KEY, 
    first_name VARCHAR(150),
    last_name VARCHAR(150) NOT NULL,
    gender CHAR(1),
    date_of_birth DATE,
    add_date DATE,
    update_date DATE
);

CREATE TABLE movies (
    movie_id SERIAL PRIMARY KEY,
    movie_name VARCHAR(100) NOT NULL,
    movie_length INT,
    movie_lang VARCHAR(10),
    age_certificate VARCHAR(10),
    release_date DATE,
    director_id INT REFERENCES directors(director_id)
);

CREATE TABLE movies_revenue (
    revenue_id SERIAL PRIMARY KEY,
    movie_id INT REFERENCES movies(movie_id),
    revenues_domestic NUMERIC(10,2),
    revenues_international NUMERIC(10,2)
);
```

------------------------------------------------------------------------

### **Reading Data**

``` sql
SELECT * FROM movies_actors;
```

> The asterisk (`*`) means "all columns."

------------------------------------------------------------------------

### **Junction Table**

``` sql
CREATE TABLE movies_actors (
    movie_id INT REFERENCES movies(movie_id),
    actor_id INT REFERENCES actors(actor_id),
    PRIMARY KEY (movie_id, actor_id)
);
```

> When a **primary key** consists of two or more columns, it's called a
> **composite primary key**.\
> In this example, it prevents the same actor from being assigned to the
> same movie more than once.

------------------------------------------------------------------------

### **Altering Tables**

``` sql
ALTER TABLE public.accounts
    ALTER COLUMN is_enabled TYPE character(2) COLLATE pg_catalog."default";

ALTER TABLE IF EXISTS public.accounts
    ALTER COLUMN is_enabled SET DEFAULT 'Y';
```

> Ensure `'Y'` is enclosed in quotes --- otherwise PostgreSQL may
> interpret it as an identifier instead of a character.

------------------------------------------------------------------------

### **Renaming Columns**

``` sql
ALTER TABLE IF EXISTS public.accounts
    RENAME added_date TO add_date;
```

------------------------------------------------------------------------

## **Key Concepts**

| Concept | Description |
|----------|-------------|
| **Primary Key** | A unique identifier for each row in a table. |
| **Foreign Key** | A reference to a primary key in another table, declared using `REFERENCES`. |
| **SERIAL** | Automatically creates an integer column with an auto-incrementing sequence. |
| **VARCHAR(n)** | A variable-length string with a maximum of *n* characters (e.g., `VARCHAR(150)`). |
| **CHAR(n)** | A fixed-length string with exactly *n* characters (e.g., `CHAR(2)`). |
| **DATE** | A date type without time information. |
| **NUMERIC(10,2)** | A numeric value with up to 10 total digits, including 2 digits after the decimal point. |

