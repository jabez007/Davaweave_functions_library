# Davaweave_functions_library
Library of Dataweave functions I've found useful

## damerauLevenshteinDistance
https://en.wikipedia.org/wiki/Damerau%E2%80%93Levenshtein_distance

https://reference.wolfram.com/language/ref/DamerauLevenshteinDistance.html

https://www.guyrutenberg.com/2008/12/15/damerau-levenshtein-distance-in-python/

### Examples
`damerauLevenshteinDistance("mccann", "mccann")` => `0`

`damerauLevenshteinDistance("mccann", "mccan")` => `1`

`damerauLevenshteinDistance("mccann", "mcan")` => `2`

`damerauLevenshteinDistance("mccann", "mcacnn")` => `1`

### Implementation
the [range](ported_from_python#range) and [enumerate](ported_from_python#enumerate) functions can be found in [ported_from_python](ported_from_python)
```
fun damerauLevenshteinMatrix(s1: String, s2: String) = do {
    var di = range(-1, sizeOf(s1) + 1) reduce (i, acc = {}) -> acc ++ {
        "$(i), -1": i + 1
    }
    var dj = range(-1, sizeOf(s2) + 1) reduce (j, acc = {}) -> acc ++ {
        "-1, $(j)": j + 1
    }
    ---
    enumerate(s1) reduce (enu1, acc1 = di ++ dj) -> acc1 ++ do {
        var i = enu1[0]
        var cs1 = enu1[1]
        ---
        enumerate(s2) reduce (enu2, acc2 = {}) -> acc2 ++ do {
            var j = enu2[0]
            var cs2 = enu2[1]
            
            var cost = if (cs1 == cs2) 0 else 1
            
            var tmp = acc1 ++ acc2
            var dijMin = min([
                (tmp["$(i - 1), $(j)"] as Number) + 1, // deletion
                (tmp["$(i), $(j - 1)"] as Number) + 1, // insertion
                (tmp["$(i - 1), $(j - 1)"] as Number) + cost // substitution
            ])
            ---
            if (i > 0 and j > 0 and cs1 == s2[j - 1] and s1[i - 1] == cs2) {
                "$(i), $(j)": min([
                    dijMin,
                    (tmp["$(i - 2), $(j - 2)"] as Number) + cost // transposition
                ])
            } else  {
                "$(i), $(j)": dijMin
            }
        }
    }
}

fun damerauLevenshteinDistance(s1: String, s2: String): Number = 
    damerauLevenshteinMatrix(s1, s2)["$(sizeOf(s1) - 1), $(sizeOf(s2) - 1)"] as Number

fun damerauLevenshteinDistanceNormalized(s1: String, s2: String): Number = do {
    var maxDist = max([sizeOf(s1), sizeOf(s2)]) default 0
    ---
    if (maxDist > 0)
        1 - (damerauLevenshteinDistance(s1, s2) / maxDist)
    else
        0
}
```
