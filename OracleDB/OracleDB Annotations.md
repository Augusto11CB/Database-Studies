

## Oracle  `CREATE SEQUENCE`  statement

The  `CREATE SEQUENCE`  statement allows the creation a new sequence in the database.

The basic syntax of the  `CREATE SEQUENCE`  statement:

`CREATE SEQUENCE schema_name.sequence_name
[INCREMENT BY interval]
[START WITH first_number]
[MAXVALUE max_value | NOMAXVALUE]
[MINVALUE min_value | NOMINVALUE]
[CYCLE | NOCYCLE]
[CACHE cache_size | NOCACHE]
[ORDER | NOORDER];`


CYCLE - allow the sequence to generate value after it reaches the limit, min value for a descending sequence and max value for an ascending sequence. When an ascending sequence reaches its maximum value, it generates the minimum value. On the other hand, when a descending sequence reaches its minimum value, it generates the maximum value.

NOCYCLE - Use to stop generating the next value when it reaches its limit. This is the default.

CACHE - Specify the number of sequence values that Oracle will preallocate and keep in the memory for faster access.



## References

[Oracle Create Sequence](https://www.oracletutorial.com/oracle-sequence/oracle-create-sequence/)
