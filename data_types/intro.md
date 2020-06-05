# 🔥Data Types

<img alt="data types" width="700px" src="/assets/images/data_types_1.png" />
<img alt="data types" width="700px" src="/assets/images/data_types.png" />

### ✳String Types

> Common Datatypes in Strings are

| DATA TYPE  | LENGTH(MAX)       |
| ---------- | ----------------- |
| CHAR(x)    | fixed length      |
| VARCHAR(x) | 65,535 characters |
| MEDIUMTEXT | 16MB              |
| LONGTEXT   | 4GB               |
| TINYTEXT   | 255 bytes         |
| TEXT       | 64 KB             |

?> **_Be Consistent:_**<br>
**VARCHAR(50)** for short strings <br>
**VARCHAR(255)** for medium-length strings.

!> If you store more than 64KB in **VARCHAR**, it will get truncated.

?>**MEDIUMTEXT** is ideal to store **JSON** objects, **CSV** strings and short to medium length books.

?>**LONGTEXT** is ideal to store **large books** and years of **log files**.

All the string types support international characters

- **English** : 1 byte
- **European & Middle-eastern** : 2 bytes
- **Asian** : 3 bytes

### ✳Integer Types

| DATATYPE         | LENGTH | VALUES      |
| ---------------- | ------ | ----------- |
| TINYINT          | 1b     | [-128, 127] |
| UNSIGNED TINYINT | 1b     | [0, 255]    |
| SMALLINT         | 2b     | [-32K, 32K] |
| MEDIUMINT        | 3b     | [-8M, 8M]   |
| INT              | 4b     | [-2B, 2B]   |
| BIGINT           | 8b     | [-9Z, 9Z]   |

!> If you try to save value in the field which is exceeding the range of data type, MySQL will throw **out of range error** and our insert and update operations will fail

?> 💡 Use smallest datatype that suits your needs. It will eventually increase performance of our operations.

### ✳Fixed-point and Floating-point Types

| DATATYPE     | DESCRIPTION                 |
| ------------ | --------------------------- |
| DECIMAL(p,s) | DECIMAL(9, 2) => 1234567.89 |
| DEC          | Synonym of DECIMAL          |
| NUMERIC      | Synonym of DECIMAL          |
| FIXED        | Synonym of DECIMAL          |
| FLOAT        | 4b size                     |
| DOUBLE       | 8b size                     |

?> DECIMAL are used for fixed point numbers.

?> FLOAT and DOUBLE are used for scientific calculations(approximate values) and this will allow us to store very small and large values.

### ✳Boolean Types

| DATATYPE | VALUE                                      |
| -------- | ------------------------------------------ |
| BOOL     | 1 is equal to TRUE <br>0 is equal to FALSE |
| BOOLEAN  | Synonym to BOOL                            |

> You can use **(0, 1) or (false, true)**. MySQL will convert false and true values to 0 and 1 before storing in database.

### ✳Enum and Set Types

?> 💡 If you want to restrict column to store only certain values then we use **ENUM** type. In the following query **size** column will allow only 3 values **(small, medium, large)**

```sql
ALTER TABLE sql_store.products
ADD COLUMN size ENUM('small', 'medium', 'large') NULL AFTER unit_price;
```

?> 💡 A **SET** is a string object that can have zero or more values, each of which must be chosen from a list of permitted values specified when the table is created. SET column values that consist of multiple set members are specified with members separated by commas (,). A consequence of this is that SET member values should not themselves contain commas.

```sql
CREATE TABLE myset (col SET('a', 'b', 'c', 'd'));

INSERT INTO myset (col) VALUES
('a,d'), ('d,a'), ('a,d,a'), ('a,d,d'), ('d,a,d');
```

!> It is better to avoid using ENUM and SET types because it is hard to modify values inside them and this will result in rebuilding entire table(expensive operation). And we cannot select all values inside them. Instead we can create separate table with different values we need.

### ✳Date and Time Types

| DATATYPE  | VALUE                |
| --------- | -------------------- |
| DATE      |                      |
| TIME      |                      |
| DATETIME  | 8b                   |
| TIMESTAMP | 4b (up to year 2038) |
| YEAR      |                      |

?> 💡 If you have to store date more than year 2038, please use **DATETIME** for that.

### ✳Blob Types

?> 💡 We use blog types to large amounts of binary data like images, videos, pdfs, word files etc.,

| DATATYPE   | VALUE |
| ---------- | ----- |
| TINYBLOB   | 255b  |
| BLOB       | 65KB  |
| MEDIUMBLOB | 16MB  |
| LONGBLOB   | 4GB   |

!> It is better to avoid storing binary data in databases. Databases are designed to store relational data not binary data. Instead store paths to files on your disk in database.

**Problems with storing binary data in Database**

1. Increased database size.
2. Slower backups.
3. Performance problems.
4. More code to read/write images.

### ✳JSON Types

?> **JSON**: Lightweight format for storing and transferring data over internet.

> MySQL introduced JSON DataType in version 8.

```json
{
	"key": value
}
```

```sql
CREATE TABLE products (product_id INT, properties JSON);

UPDATE products SET properties = '
{
    "dimentions": [1,2,3],
    "weight": "10",
    "manufacturer": {"name": "sony"}
    }
';

-- Alternative
UPDATE products SET properties =
    JSON_OBJECT(
        'weight', 10,
        'dimentions', JSON_ARRAY(1, 2, 3),
        'manufacturer', JSON_OBJECT('name', 'sony')
    )
;
```

```sql
SELECT JSON_EXTRACT(properties, '$.weight') as weight
FROM products;

-- Alternative

SELECT properties-> '$.weight' as weight
FROM products;

SELECT properties-> '$.dimentions[0]' as dimention
FROM products;

SELECT properties-> '$.manufacturer.name' as Company
FROM products; # "sony"

-- If you want to remove quotes around values
SELECT properties->> '$.manufacturer.name' as Company
FROM products; # sony

-- If you want to update only certain properties in the JSON object

UPDATE products
SET properties = JSON_SET(
    properties,
    '$.weight', 20,
    '$.age', 10
)
WHERE product_id = 1;

-- If you want to remove certain properties in the JSON object

UPDATE products
SET properties = JSON_REMOVE(
    properties,
    '$.age'
)
WHERE product_id = 1;
```
