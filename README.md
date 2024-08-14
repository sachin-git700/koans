# Important Points

## Cheatsheet

### Array

```ruby
arr = [1,2,3,4,5]

arr.slice(2,2) # [3, 4] # i.e., start with 2 and take 2 elements
arr[2..4] # [3, 5] # 1..2 -> Range class - Slicing with range
arr[2...4] # [3, 4]
(1..5).to_a # [1,2,3,4,5]
first_name, last_name = last_name, first_name # swapping elements
arr << 333 # [1,2,3,4,5,333]
arr.unshift(0) # adds element at the beginning
arr.shift # adds element at the end
arr.push(:last)
arr.pop
first_name, *last_name = ["John", "Smith", "III"] # splat operator (rest operator)
```
