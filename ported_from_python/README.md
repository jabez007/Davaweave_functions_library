
# range

## Examples
`range(7)` => `[0, 1, 2, 3, 4, 5, 6]`

`range(1, 7)` => `[1, 2, 3, 4, 5, 6]`

`range(1, 7, 2)` => `[1, 3, 5]`

`range(7, 1, -2)` => `[7, 5, 3]`

## Implementation
```
%dw 2.0

fun _range(start: Number, stop: Number, step: Number, sequence: Array<Number>): Array<Number> = 
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
        range(stop - step, start - step, -step, [])[-1 to 0] // flip
    else if (sizeOf(sequence) == 0)
        range(start, stop, step, [start]) // initialize
    else if(sequence[-1] + step >= stop )
        sequence // done
    else
        range(start, stop, step, sequence ++ [sequence[-1] + step]) // continue

fun range(start: Number, stop: Number, step: Number): Array<Number> = 
    _range(start, stop, step, [])

fun range(start: Number, stop: Number): Array<Number> =
    range(start, stop, 1)

fun range(stop: Number): Array<Number> = 
    range(0, stop)
```
