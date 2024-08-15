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
