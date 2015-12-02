# The Struct Class in Ruby

The struct class is a handy way to bundle data and attributes together (using attribute accessor methods), for when an explicit class is just too heavy.

Here's what a struct looks like declared:

```ruby
Cat = Struct.new(:name, :breed)
```

Structs can encapsulate methods as well, within a block:

```ruby
Cat = Struct.new(:name, :breed) do 
  def say_hello
    "Meow, my name is #{name}!"
  end
end
```

Structs are instantiated much like classes are:

```ruby
maro = Cat.new("Maro", "Scottish Fold")

maro.name
=> "Maro"
maro.breed
=> "Scottish Fold"
maro.say_hello
=> "Meow, my name is Maro!"
```

## Usage

Above we've been making structs much like how we make classes. But most commonly, Structs are declared within classes to group together attributes that always relate to each other. 

[Steve Klabnik's](http://blog.steveklabnik.com/posts/2012-09-01-random-ruby-tricks--struct-new) example is clearest, so let's work through that.

You have a Person class. A person has a name and a birthday. But a birthday is actually three separate attributes: day, month, and year. There's something a bit sloppy about this:

```ruby
class Person
  attr_accessor :name, :day, :month, :year

  def initialize(attributes = {})
    @name = attributes[:name]
    @day = attributes[:day]
    @month = attributes[:month]
    @year = attributes[:year]
  end

  def birthday
    "#@day/#@month/#@year"
  end
end
```

Why? "day", "month", and "year" on their own aren't really attributes of a person. Instead, a person should have a `:birthday` attribute. Enter, a struct that groups the day, month, and year attributes within the person class:

```ruby
class Person
  attr_accessor :name, :birthday

  Birthday = Struct.new(:day, :month, :year)

  def initialize(attributes = {})
    @name = attributes[:name]
    @birthday = Birthday.new(
      attributes[:day], 
      attributes[:month], 
      attributes[:year]
    )
  end
end
```

This pattern of using a struct to handle one specific functionality within a class fits into a best practices principle called "separation of concerns": the birthday attributes and functionality are useful for the person class, but not directly a part of that. A struct is one way to keep things organized within a class.

## Resources

* [Ruby Docs: Struct](http://www.ruby-doc.org/core-2.1.3/Struct.html)
* [Steve Klabnik on the Struct Class](http://blog.steveklabnik.com/posts/2012-09-01-random-ruby-tricks--struct-new)

<a href='https://learn.co/lessons/ruby-struct-class-readme' data-visibility='hidden'>View this lesson on Learn.co</a>
