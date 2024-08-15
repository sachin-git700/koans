# Important Points

## Cheatsheet

### Array & array_assignment

```ruby
arr = [1,2,3,4,5]

arr.slice(2,2) # [3, 4] # i.e., start with 2 and take 2 elements
arr[2..4] # [3, 4, 5] # 2..4 -> Range class - Slicing with range
arr[2...4] # [3, 4]
(1..5).to_a # [1,2,3,4,5]
first_name, last_name = last_name, first_name # swapping elements
arr << 333 # [1,2,3,4,5,333]
arr.unshift(0) # adds element at the beginning
arr.shift # adds element at the end
arr.push(:last) # returns entire array
last_element = arr.pop
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

class Dog7
  attr_reader :name

  def initialize(initial_name)
    @name = initial_name
  end

  def get_self
    self # refers to containing object
  end

  def to_s # used in string interpolation, supported by all Object
    @name
  end

  def inspect # supported by all Object. Provide more complete string version
    "<Dog named '#{name}'>"
  end
end

# to_s is used in string manipulation
dog1 = Dog7.new
"My dog name is #{dog1}" # it calls dog1.to_s

# Array
[1,2,3].to_s # "[1, 2, 3]"
[1,2,3].inspect # "[1, 2, 3]"

# String
"String".to_s # "String"
"String".inspect # "\"String\""

# Classes inside classes is possible in Ruby
```

### Constants

```ruby
C = "top level"
class AboutConstants
  C = "nested"
end

# Nested constants are referenced with relative paths
C # nested

# Top level constants are referenced by double colons
::C # "top level"

class MyAnimals
  LEGS = 2

  class Bird < Animal
    def legs_in_bird
      LEGS
    end
  end
end

 MyAnimals::Bird.new.legs_in_bird # 2
```

### Dice Project

```ruby
rand(1..6) # rolling a dice

# comparing array (could be another object)
.equal? # checks object_id
.eql? # checks value & type

a = [1,2,3]
a_copy = a
b = [1,2,3]

a.equal?(a_copy) # true
a.equal?(b) # false

a.eql?(a_copy) # true
a.eql?(b) # true
```

### Expections

```ruby
# ancestors
# - returns an array of modules and classes that are in the inheritance chain of the class (or module) it is called on
# - This includes the class itself, its parent classes, and any modules that have been mixed in via include or extend.

Dog.ancestors
# [Dog, Object, JSON::Ext::Generator::GeneratorMethods::Object, PP::ObjectMixin, Kernel, BasicObject]

RuntimeError.ancestors
# [RuntimeError, StandardError, Exception, Object, JSON::Ext::Generator::GeneratorMethods::Object, PP::ObjectMixin, Kernel, BasicObject]

# -------------------

# 'raise' and 'fail' are synonyms/alias
# it is used to raise RuntimeError
fail "Oops" # raises RuntimeError with message "Oops"
raise "Oops"

begin
  fail "Oops"
rescue StandardError => ex # ex.class = RuntimeError
  result = :exception_handled
end

result = nil
begin
  fail "Oops"
rescue StandardError
  # no code here
ensure # like javascript finally
  result = :always_run
end


is_a? # looks in the inheritance chain (the one returned by .ancestors)
RuntimeError.new.is_a?(StandardError) # true # true
RuntimeError.new.is_a?(Exception) # true
RuntimeError.new.is_a?(Object) # true
RuntimeError.new.is_a?(JSON::Ext::Generator::GeneratorMethods::Object) # true
RuntimeError.new.is_a?(PP::ObjectMixin) # true
RuntimeError.new.is_a?(Kernel) # true
RuntimeError.new.is_a?(BasicObject) # true
```

### Hashes

```ruby
hash.size
hash[:key1]
hash.fetch(:key1) # Error raise: KeyError
hash.class # Hash
hash.merge({ ... }) # does not modify hash
hash.merge!({ ... }) # modifies hash
Hash.new # creates a new hash
Hash.new("dos") # default value | hash1[:non_existence_key] -> dos

hash = Hash.new
hash.default = "dos"

# important - 1
hash = Hash.new([]) # default value is []
hash[:one] << "uno" # hash[:one] returns default value [] & "uno" is pushed to it
hash[:two] << "dos"
hash[:three] # output -> ["uno", "dos"]

# important - 2
# for default or non existing value the block will be ran
# which mean for every new key a new empty array will be returned
hash = Hash.new {|hash, key| hash[key] = [] }
hash[:one] << "uno"
hash[:two] << "dos"
hash # { :one => ["uno"], :two => ["dos"] }
hash[:three] # [] -> block ran to return default value

---------------------------------------------
hash = {}
hash.fetch(:key1) # Error raise: KeyError
KeyError.ancestors
# [DidYouMean::Correctable, KeyError, IndexError, StandardError, Exception, Object, JSON::Ext::Generator::GeneratorMethods::Object, PP::ObjectMixin, Kernel, BasicObject]

# Error Classes
KeyError
IndexError
StandardError
Exception
NoMethodError
```

### Ancestors (include vs prepend)

```ruby
class Animal
end

module Mammal
  def fly
    puts "Mammal is flying..."
  end
end

module Organism
  def fly
    puts "Organism is flying"
  end
end


class Cat < Animal
  prepend Mammal
  include Organism

  def fly
    puts "Cat is flying..."
  end
end

Cat.ancestors
# [Mammal, Cat, Organism, Animal, Object, JSON::Ext::Generator::GeneratorMethods::Object, PP::ObjectMixin, Kernel, BasicObject]
cat1 = Cat.new
cat1.fly # "Mammal is flying..."

```

### inheritance

```ruby
# Inheritance does not work cross method
class Dog
  def bark
    "WOOF"
  end
end

class GreatDane < Dog
  def growl
    super.bark + ", GROWL"
  end
end

gd = GreatDane.new
gd.growl # NoMethodError. 'growl' no superclass method growl
```
