# PostgreSQL

## Adding New Column**
```SQL
ALTER TABLE my_table
ADD COLUMN new_field varchar(20);
```

## Copying Data From Column to Another**
```SQL
UPDATE my_table --format schema.my_table
SET 
field = my_tableOther.field
FROM my_tableOther-- mention schema name
WHERE my_tableOther.field = my_table.field 
```

## Create Index**
```SQL
-- Criar index
CREATE INDEX idx_fieldAAA
ON my_table (fieldAAA);

-- Listar indices tabela
SELECT * FROM pg_indexes WHERE tablename = 'my_table ';
```

## Sequences
Sequence is a special type of data created to generate unique numeric identifiers.
```SQL
CREATE SEQUENCE user_id_seq;
```

```SQL
CREATE TABLE tb_user(id INTEGER DEFAULT NEXTVAL('user_id_seq'));
```
### Delete Sequence of a Table
```SQL
ALTER TABLE tb_userALTER COLUMN id SET DEFAULT NULL;
DROP SEQUENCE phonebook_id_seq;
```


### Add Sequence to Existing Column Postgres
```SQL
CREATE SEQUENCE foo_a_seq;
ALTER TABLE foo ALTER COLUMN a SET DEFAULT nextval('foo_a_seq');
ALTER TABLE foo ALTER COLUMN a SET NOT NULL;

ALTER SEQUENCE foo_a_seq OWNED BY foo.a;    -- 8.2 or later
```

### Type of Sequences
-   **nextval(' sequence_name ')**  - this command will increment the value of the specified sequence and return the new value as an integer
    
-   **currval(' sequence_name ')**  - this command will return the last returned value from the "nextval" command. If the nextval still hasn't been used, no value will be returned
    
-   **setval(' sequence_name ', n)**  - the "setval" command will set the current value of the sequence to n.
