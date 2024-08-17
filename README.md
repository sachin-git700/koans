# Important Points

## Cheatsheet

### Array & array_assignment

```ruby
arr = [1,2,3,4,5]

arr.slice(2,2) # [3, 4] # i.e., start with 2 and take 2 elements
arr[2,2] # [3, 4] - same as slice
arr[2..4] # [3, 4, 5] # 2..4 -> Range class - Slicing with range
arr[2...4] # [3, 4]

# method_name = find_by_product_name -> get "product_name"
method_name.slice(8..-1) # product_name
method_name[8..-1] # product_name
method_name.split('_', 3).last # product_name

method_name.split('_', 3) # ["find", "by", "product_name"]

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

# Classes
# BasicObject, Object, Kernel
# NilClass

# Modules
# Kernel
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

### Iteration

Important terms: Enumerable

```ruby
# map (collect) select (find_all), reduce(0) (inject(0)), find

# Important concept
Enumerable.instance_methods.include?(:map) # true
Enumerable.instance_methods.include?(:select) # true
Enumerable.instance_methods.include?(:find) # true

Enumerable.instance_methods.include?(:each) # false
Array.instance_methods.include?(:each) # true

# important
# Array.instance_methods === Array.new.instance_methods

# Why "each" is part of Array and not Enumerable class
# - The Enumerable module provides a collection of methods that allow us to iterate over a collection.
# - Enumerable itself does not define how to iterate over elements.
# - Instead, it requires the class that includes Enumerable to define an each method.
# - Why?
# - Enumerable is designed to be included in many different kinds of collections (arrays, hashes, ranges, etc.), it leaves the actual implementation of each to the including class.

class Array
  include Enumerable

  def each
    # each method logic goes here
  end
end

# iterating over a collection vs iterating over elements
array.each do |element|
  puts element
end
# - The each method is responsible for iterating over the collection.
# - The block { |element| puts element } is responsible for iterating over each element (specifically, outputting each one).

# In other words
# - Iterating over the collection means looping through the array itself, from start to finish.
# - Iterating over elements focuses on what happens when you get to each individual number (1, 2, 3, etc.) in that array.

# Note
# break works in each style iterators
# aliases:
# - map & collect
# - select & find_all
# - reduce & inject -> [2, 3, 4].inject(0) { |sum, item| sum + item }

Array.ancestors
# [Array, JSON::Ext::Generator::GeneratorMethods::Array, Enumerable, Object, JSON::Generator::GeneratorMethods::Object, PP:ObjectMixin, Kernel, BasicObject]

Enumerable.ancestors
# [Enumerable]
# Enumerable.new.methods or Enumerable.instance_methods

# this way closes the file after block has executed, even if there was an exception
File.open('filename') do |file|
  # code to read 'file'
end
```

### interoperability - JRuby

```ruby
include Java

class AboutJavaInterop
  def method1
    java_array = java.util.ArrayList.new
    java_array.class
  end
end

# For this JRuby is required
# Def:
# - JRuby is an implementation of the Ruby programming language that runs on the Java Virtual Machine (JVM). It provides a bridge between Ruby and Java, allowing Ruby code to interact with Java libraries and applications.

# JRuby vs. MRI (Matz's Ruby Interpreter)
# Threading: JRuby supports true parallel threading, while MRI has a Global Interpreter Lock (GIL) that can limit concurrency.
```

### Keyword Arguments

```ruby
def method1(one:, two: 'two')
  [one, two]
end

method1(one: 'one') # ['one', 'two']
```

### Message Passing

```ruby
send # Object class
__send__# BasicObject class. to avoid conflict, in code or through library send method might have been overwritten. Readability.

dog1.send(:bark)
dog1.send("bark")
dog1.send("bar" + "k")
dog1.send("BARK".downcase) # upcase, downcase

dog1.respond_to?(:bark) # true
dog1.respond_to?(:send) # true -> since it knows how to respond_to send method

class AllMessageCatcher
  def method_missing(method_name, *args, &block)
    "Someone called #{method_name} with <#{args.join(", ")}>"
  end
end
catcher = AllMessageCatcher.new
catcher.foobar # will not throw error
catcher.respond_to?(:foobar) # false

User.respond_to?(:find_by_email) # true -> the method_missing will define this method at run time and so it returns true

# playing around - creating a new class
class AllMessageCatcher
  def method_missing(method_name, *args, &block)
    self.class.define_method(method_name) do |*args|
      puts "defined #{method_name} with args #{args}"
    end

    # find_by_name dynamic methods creation
    #  def method_missing(method_name, *args, &block)
    #   if method_name.to_s[0,7] == "find_by" && check_if_valid_column(method_name)
    #     code goes here...
    #   else
    #     super(method_name, *args, &block)
    #   end
    # end

    send(method_name, *args)
  end
end

catcher = AllMessageCatcher.new
catcher.foo(1,2)

```

### Methods

```ruby
eval # to evaluate a string; dangerous!

def method_1; end
private method_1
NoMethodError # error raised when private method is called
ArgumentError # if number of arguments are less or more - Runtime error and not Syntax error
```

### Modules

```ruby
include
prepend
.new # NoMethodError -> Modules can't be instantiated
```

### Nil

```ruby
nil.methods # returns a lot of methods, which makes sense as it inherits from Object
NilClass.class.instance_methods
nil.nil?
nil.to_s # ""
nil.inspect # "nil"
```

### Objects

```ruby
# Everything is an object - Prove
1.is_a?(Object) # 1.object_id
1.5.is_a?(Object)

# For integer, object_id = 2n+1
1.object_id # 3
2.object_id # 5
100.object_id # 201

# clone
obj = Object.new
copy = obj.clone
obj.object_id != copy.object_id # true
# Note: clone won't work for integer as they have a fix object_id
a = 1 # a.object_id = 3
b = a.clone # b.object_id = 3
```

### Open Classes

One can open any class to add more constants & methods.

```ruby
class ::Integer
  def answer_to_life_universe_and_everything?
    self == 42
  end
end

1.answer_to_life_universe_and_everything? # false
42.answer_to_life_universe_and_everything? # true
```

### Proxy Object

```ruby
# ----------------- USING SEND METHOD ------------------------
class Calculator
  def sum(*args)
    args
  end
end

c1 = Calculator.new
args = [1,2,3]
c1.send(:sum, *args) # args will be passed as individual values instead of an array

class Welcome
  def greet(name1, name2, &block)
    [block.call, name1, name2].join(" ")
  end
end

w1 = Welcome.new
block = lambda { "Hello" }
w1.send(:greet, "Alice", "Bob", &block)
w1.send(:greet, "Alice", "Bob") do ||
  "Hello"
end
# ----------------- USING SEND METHOD END ------------------------
```

### Regular Expression

```ruby
# d - digit, D - not digit
# s - whitespace, S - not whitespace
# w - word character, W - not character; [a-zA-Z0-9_]
# [^0-9] - ^ negate i.e., not containing number

# Regexp - Regex Pattern class
/anything/.class # Regexp
"abc def"[/de/] # de
""[/abc/] # nil

# match must start from beginning of string - use anchor (A); z for end
"start end"[/start/] # "start"
"start end"[/end/] # "end"
"start end"[/\Astart/] # "start"
"start end"[/\Aend/] # nil
"start end"[/end\z/] # "end"

# ?/+/* (optional, one or more, zero or more)
"aaabcde"[/ab?/] # "a" -> matches from L to R, and stops at first match. b is optional so "a" is the first match
"aaabcde"[/bc?/] # "bc"
"aaabcccde"[/bc+/] # "bccc"
"aaabde"[/bc*/] # "b"
"aaabde"[/bc*d/] # "bd"

# select all words that ends with "at" & first letter is in "cbr"
["cat", "bat", "rat", "zat"].select { |w| w[/^[cbz]at/] } # ["cat", "bat", "rat"]

# Caret "^", dollar "$", \b - word boundary,
"variable_1 = 42"[/[^a-zA-Z0-9_]+/] # " = " -> here negate
"num 42\n2 lines"[/^\d+/] # "2" -> caret anchors to start of lines
"42", "2 lines\nnum 42"[/\d+$/] # "42"
"bovine vines"[/\bvine./] # "vines" -> word boundary

# group content
"ahahaha"[/(ha)+/] # "hahaha"

# digits
# /[0123456789]/ -> returns only one matching
# /[0123456789]+/ -> 1 or more match
"the number is 42"[/[0123456789]+/] # 42
"the number is 42"[/[0-9]+/] # 42
"the number is 42"[/\d+/]

# whitespace i.e., space, tab, newline
" \t\n", "space: \t\n"[/\s+/]

# word character
"variable_1 = 42", "variable_1 = 42"[/[a-zA-Z0-9_]+/] # "variable_1"
"variable_1 = 42", "variable_1 = 42"[/\w+/] # "variable_1"

"variable_1 = 42".[/[a-zA-Z0-9_]+/] # "variable_1"
"variable_1 = 42".match(/[a-zA-Z0-9_]+/) # "variable_1"
"variable_1 = 42".scan(/[a-zA-Z0-9_]+/) # ["variable_1", "42"]

# sub - like find & replace
"one two-three".sub(/(t\w*)/) { $1[0, 1] } # "one t-three"

# gsub - like find & replace all
"one two-three".sub(/(t\w*)/) { $1[0, 1] } # "one t-t"
```

### Sandwich code

```ruby
# Sandwich code
# some code needs to run at begin & some at end, the middle can differ
# eg: open a file, do something, close -> "do something" can change, open & close code remains same


def count_lines(file_name)
  file = open(file_name)
  count = 0

  while line = file.gets
    count += 1
  end
  count
ensure
  file.close if file
end

# -----------------
def file_sandwich(file_name)
  file = open(file_name)
  yield(file)
ensure
  file.close if file
end

def count_lines2(file_name)
  file_sandwich(file_name) do |file|
    # rest of the code...
  end
end

# ----------------------------------------------

# best way - use block
open(file_name) do |file|
  # code goes here
end # closes file (even if there are error while processing file)
```

### Scope

```ruby
# access instance variables
Dog.new.instance_variable_get("@name")
Dog.const_get("PI") # get a constant variable explicitly
Dog.constants # get all constants

# Note: class names are just constants
module A
  class B
  end
end
A.constants # [:B]
```

### Scoring Project (Dice game)

#### Important learning

Write test cases first. Test cases will tell you what code to write. It basically shows that you were successfully able to break the requirement. Write some code and make them pass. Now you go back and refactor the code.

```ruby
# Red, Green, Refactor -> TDD
```

### String

```ruby
[].is_a?(Object) # true
[].kind_of?(Object) # true
[].instance_of?(Object) # true

a = %{flexible quotes can handle both ' and " characters;
and also new line
}
b = <<EOS # End Of String, differs from flexible quotes in terms of \n only
It is good.
It is bad.
EOS

hello = "Hello, "
world = "World"
hello += "World" # "Hello, World"
hello << "World" # "Hello, World"

string = "\n" # output: "\n", length 1
string = '\n' # output: "\\n", length 2 -> \ is treated as literal \ and not a new line, single quotes does not interpret escape charaters
string = '\\\'' # output: "\\'" -> single character only escapes \', as it things that ' might have been escaped to avoid unintentional string termination

string = "Hello"
string[1,2] # "el"
string[1..2] # "el"

?a # "a"
?a == 97 # false
?a.ord # 97
"a".ord # 97
"hello world".count
"the:rain:in:spain".split(/:/) # ["the", "rain", "in", "spain"]
```

### symbols

```ruby
# Symbol class
symbol1 = :a_symbol
symbol2 = :a_symbol
symbol1.object_id == symbol2.object_id # true
"cats".to_sym # :cats
symbol = :"cats and dogs" # :"cats and dogs"

# symbol does not have string methods
sym = :abc
sym.respond_to?(:each_char) # false
sym.respond_to?(:reverse) # false


# method names become symbols
# constants names become symbols
# ---------- DETAIL ----------
def hello
end

Symbol.all_symbols.map(&:to_s).include?("hello") # all method names become symbols
Symbol.all_symbols # returns all symbols - long list - as all method names become symbols

Symbol.all_symbols.map(&:to_s).include?("hello") # true
Symbol.all_symbols.map(&:to_s).include?("hello2") # false
Symbol.all_symbols.include?(:hello2) # true -> as during comparision, :hello2 symbol gets created and so true
# --------------------
```

<!-- ---------------------------------------------------- -->
<!-- ---------------------------------------------------- -->
<!-- ---------------------------------------------------- -->
<!-- ---------------------------------------------------- -->
<!-- ---------------------------------------------------- -->
### Performance

```ruby
# string concatination
+= vs << # prefer << as it mutates current string & does not create a new string. No memeory rellocation.

# using symbols for hash - object_id remains same

# SQL query optimization
# exists? - eg. whether any employee exists who has not been assigned any project

# SQL optimization
```
