# enumerate

## Examples
`enumerate("Firday")` => `[(0, 'F'), (1, 'i'), (2, 'r'), (3, 'd'), (4, 'a'), (5, 'y')]`

`enumerate("Firday", 6)` => `[(6, 'F'), (7, 'i'), (8, 'r'), (9, 'd'), (10, 'a'), (11, 'y')]`

`enumerate(["Spring", "Summer", "Autumn", "Winter"])` => `[(0, "Spring"), (1, "Summer"), (2, "Autumn"), (3, "Winter")]`

## Implementation
```
%dw 2.0
 
fun enumerate(iterable: Array<Any>, start = 0) = 
    iterable map (item, index) -> [index + start, item]

fun enumerate(iterable: String, start = 0) = 
    enumerate(iterable splitBy "", start)
```

# range

## Examples
`range(7)` => `[0, 1, 2, 3, 4, 5, 6]`

`range(1, 7)` => `[1, 2, 3, 4, 5, 6]`

`range(1, 7, 2)` => `[1, 3, 5]`

`range(7, 1, -2)` => `[7, 5, 3]`

## Implementation
```
%dw 2.0

fun range_(start: Number, stop: Number, step: Number, sequence: Array<Number>): Array<Number> = 
    if (start == stop)
        [] // error
    else if (step == 0)
        [] // error
    else if (step > 0 and start > stop)
        [] // error
    else if (step < 0 and start < stop)
        [] // error
    else if (sizeOf(sequence) == 1 and sequence[0] != start)
        [] // error
    else if (step < 0)
        range_(stop - step, start - step, -step, [])[-1 to 0] // flip
    else if (isEmpty(sequence))
        range_(start, stop, step, [start]) // initialize
    else if(sequence[-1] + step >= stop )
        sequence // done
    else
        range_(start, stop, step, sequence ++ [sequence[-1] + step]) // continue

fun range(start: Number, stop: Number, step: Number): Array<Number> = 
    range_(start, stop, step, [])

fun range(start: Number, stop: Number): Array<Number> =
    range(start, stop, 1)

fun range(stop: Number): Array<Number> = 
    range(0, stop)
```
