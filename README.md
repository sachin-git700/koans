# Important Points

## Cheatsheet

### Array & array_assignment

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

### Blocks

```ruby
yield # to execute the code of the block
block_given? # to find if a block was given

# passing arguments
def greet
  yield("Sachin")
  yield("Ram")
end

greet do |value|
  puts "Hello #{value}!!!"
end

# ^^^ this will call the greet function with the block
# Output
# Hello Sachin!!!
# Hello Ram!!!

# Lambda
greet_block = lambda { |value| puts "Hi #{value}" }
# greet greet_block
# ^^ Error: ArgumentError, wrong number of arguments provided. Expected 0, received 1
greet_block.call("Sachin")

full_name.call(user) # one can implement this

def greet2(&block)
  block.call("Sachin")
end

greet2 &greet_block # Hi Sachin
```

### Class & Class Methods

```ruby
is_a? # Dog.is_a?(Class), Dog.is_a?(Object)
dog1.is_a?(Object)
Dog.methods.size
Dog.new.methods.size
User.new.abc # NoMethodError class

# class statements return the value of their last expression
LastExpressionInClassStatement = class Dog
                                     21
                                   end
SelfInsideOfClassStatement = class Dog
                                self
                              end
LastExpressionInClassStatement # 21
SelfInsideOfClassStatement # Dog

# Interesting
class Dog
  attr_accessor :name, :age
end
# output: [:name, :name=, :age, :age=] <-- interesting output

dog1 = Dog.new
dog1.instance_variables # []
dog1.name = "fido"
dog1.instance_variables # [:@name] --- ????
dog1.age = 25
dog1.instance_variables # [:@name, :@age]

# 2 main ways to define class method
class Animal
  self.method1

  class << self
    def method2
    end
  end
end

# even if there is no getter, one can get instance variable value
dog1.instance_variable_get("@name")
dog1.instance_eval("@name")
dog1.instance_eval { @name }

# to_s is used in string manipulation
dog1 = Dog.new
"My dog name is #{dog1}" # it calls dog1.to_s
```
