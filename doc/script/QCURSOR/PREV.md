# ?CURSOR/PREV

{% method -%}

Sets the cursor at the previous key value

Input stack: `cursor`

Output stack: `[key value]` or `[]`

If there is a previous key/value pair in the database, `[key value]` will be pushed onto the stack.
Otherwise, `[]` will be pushed. Useful in conjunction with [UNWRAP](../UNWRAP.md),
[SOME?](../SOMEQ.md) and [NONE?](../NONEQ.md).

{% common -%}

```
PumpkinDB> ["1" "2" ASSOC "2" "2" ASSOC COMMIT] WRITE [CURSOR 'c SET c ?CURSOR/LAST DROP c ?CURSOR/PREV] READ UNWRAP
"1" "2"
```

{% endmethod %}
## Allocation

Allocates for values to be put onto the stack

## Errors

NoTransaction error if there's no current write transaction

InvalidValue error if the cursor identifier is incorrect or expired

## Tests

```test
works : ["1" "2" ASSOC "2" "2" ASSOC COMMIT] WRITE [CURSOR 'c SET c ?CURSOR/LAST DROP c ?CURSOR/PREV] READ ["1" "2"] EQUAL?.
requires_txn : ["1" ?CURSOR/PREV] TRY UNWRAP 0x08 EQUAL?.
empty_stack : [[?CURSOR/PREV] TRY] READ UNWRAP 0x04 EQUAL?.
invalid_cursor : [["1" ?CURSOR/PREV] READ] TRY UNWRAP 0x03 EQUAL?.
```
