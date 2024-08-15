# Important Points

## Cheatsheet

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
