
<h3 style='color:yellowgreen'>Index function</h3>

`index` finds the element index for a given value in a list .

`index(list, value)`

the returned index is zero-based. this function produces an error if the given value is not present in the list.

```
> index(["a", "b", "c"], "b")
1
```

<h3 style='color:yellowgreen'>min functions</h3>

How you can use min function ?
min takes one or more numbers and returns the smallest number from the set 
` min (12,54,3)` result is 3

if the numbers are in a list or set value, use ... to expand the collection to individual arguments:
`min ([12,54,3]...)` result is 3

<h3 style='color:yellowgreen'>Zipmap function</h3>

`zipmap` constructs a map from a list of keys and a corresponding list of values.

`zipmap(keyslist, valueslist)`

Both keyslist and valueslist must be of the same length. keyslist must be a list of strings, while valueslist caan be a list of any type.

```
> zipmap(["a", "b"], [1, 2])
{
  "a" = 1,
  "b" = 2,
}

``` 

<h3 style='color:yellowgreen'>Lookup function</h3>

`lookup` retrieves the value of a single element from a map, given its key.if the given key does not exist the given default value is returned instead.

``` 

> lookup({a="ay", b="bee"}, "a", "what?")
ay
> lookup({a="ay", b="bee"}, "c", "what?")
what?

```