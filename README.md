## Ruby best practices and guidelines

### Develop the most generic as possible
Always try to develop the most generic as possible, at least for those elements that are not
strictly related to your domain. That is, think if the piece of code you're developing can be used
in other application and how far it can go.

### Do not repeat your code
Don't repeat yourself. Whenever possible, re-use the code you wrote, generalize the most as possible
and extract functionality to methods.

### Do not use `nil` object
Why? Because it will eventually add more complexity to your code, that is because nested validations
would appear. Take a look at the following example:

#### With nils (bad)
```ruby
class User
  def contact_address
    if self.contact.nil?
      'The user has no contact information'
    elsif self.contact.address.nil?
      'The user contact has no address'
    else
      self.contact.address
    end
  end
end
```
As you can see, there are multiple nested validations, which can be avoided. For instance, using the
following solutions:

#### Without nils (good)

```ruby
class User
  def initialize(contact)
    if contact.nil? || contact.address.nil?
      raise ArgumentError, 'Contact address is required'
    end
    @contact = contact
  end
end
```
In this case we are not allowing to create a an `User` object if the given contact is `nil`, in this
way we won't need to validate for a contact anymore when working with that particular instance.

####  Without nils (good)
```ruby
class User
  def contact
    @contact ||= UnknownContact.new
  end
end
```
Using default values could be a good way to avoid validations. In the above example we are using the
class `UnknownContact` in case that the current user's contact is `nil`.
*Notice that the* `UnknownContact` *instance is assigned to the user once it is required by the
first time.*

### Constantly refactor your code
Try to keep your code clean, and the best way to do it is not to postpone refactoring, because it
will get accumulated. Refactor the simplest piece of code everyday, always as possible.

## Language Structure and Layout
In this section you'll find some guidelines about the language layout, what is recommended, how to
write a good syntax, etc.

##### Tab size
Use **two (2)** spaces to indent your code
```ruby
class Example
  some_code # I am a two-indented line.
end
```
##### Space between...
Use spaces between operators *(except exponential)*, commas, colons, semicolons, and even after and
before `{` and `}`, why? it makes your code more readable, look at the example below:
```ruby
# Wrong
var>1?'yes':'no'

# Good
var > 1 ? 'yes' : 'no'
```
As you can see the wrong way looks ugly and becomes hard to read, let's look another couple of
examples:
```ruby
# Wrong
result = (2*5) ** 5

# Good
result = (2 * 5)**5
```
```ruby
# Wrong
my_array.map{|element| element*2}

# Good
my_array.map { |element| element * 2 }
```
Notice how clean is your code when you use correctly the spaces.

##### Assignment indentation
When assigning a conditional expression to a variable, keep the indentation deep as needed.
```ruby
# Wrong
name = case hour
when 0..5 then 'early morning'
when 6..11 then 'morning'
when 12..18 then 'afternoon'
else 'night'
end

# Good
name = case hour
       when 0..5 then 'early morning'
       when 6..11 then 'morning'
       when 12..18 then 'afternoon'
       else 'night'
       end
```

##### Chaining a method call
When you are chaining multiple method calls, keep the `.` in the second line, so that one doesn't
have to read the first line to understand what's going on in the second one.
```ruby
# Wrong
user.contacts.most_recent.
  call

# Good
user.contacts.most_recent
  .call
```

##### Align parameters when calling a method
Maybe you had wondered how to split a lot of parameters when calling a function. Okay, lets assume
that we have this line with a function call and many parameters which exceeds my characters per
line limit:
```ruby
# Wrong
User.create(name: 'Santiago', last_name: 'Vanegas', phone: '2222222', gender: 'male', city: 'Medellín', country: 'Colombia')
```
Now we can use two methods to split those parameters in multiple lines, as follows:
```ruby
# Good
User.create(name: 'Santiago',
            last_name: 'Vanegas',
            phone: '2222222',
            gender: 'male',
            city: 'Medellín',
            country: 'Colombia')
```
Or
```ruby
# Good
User.create(
  name: 'Santiago',
  last_name: 'Vanegas',
  phone: '2222222',
  gender: 'male',
  city: 'Medellín',
  country: 'Colombia'
)
```

##### Columns limit
Most of the guidelines recommend to use **80** characters limit in your editor, but I personally
recommend to user **100** when using a large resolution screen, because it will perfectly fit even
if working in split screen mode.

## Syntax
Do not miss syntax details, we will cover a couple of guidelines below.

##### Use parentheses when arguments are present
When defining a method, use parentheses if and only if no arguments are received:
```ruby
# Bad
def wave()
  some_content
end

# Good
def wave
  some_content
end
```
```ruby
# Bad
def wave speed, repeat
  some_content
end

# Good
def wave(speed, repeat)
  some_content
end
```

##### Multiline conditional without `then`
When you are using a multiline conditional `if/unless` do not use `then`
```ruby
# Bad
if truth? then
  "yes"
end

# Good
if truth?
  "yes"
end
```
##### Ternary operator instead of conditionals
Consider using ternary operator (`?:`) instead of single line `if/then/else/end` statements.
```ruby
# Bad
value = if truth? then "yes" else "no" end

# Good
value = truth? ? "yes" : "no"
```

##### `if` v.s. `unless`
When you have a statement with a negation, consider using the opposite conditional:
```ruby
# Bad
if !valid_account
  negative_content
end

# Good
unless valid_account
  positive_content
end
```
**Note:** It is not recommended to use `unless` when there is an `elsif` or `else` statement right
after it.
```ruby
# Bad
unless valid_account
  positive_content
else
  negative_content
end

# Good
if !valid_account
  positive_content
else
  negative_content
end
```

##### Single line conditionals and loops
Consider using single line conditionals and loops when able:
```ruby
# Bad
if example_condition
  some_content
end

until done
  keep_working
end

# Good
some_content if example_condition

keep_working until done
```

##### Avoid using `return` when it is not needed
If you are not doing a flow control, you can omit the `return` keyword in a method:

```ruby
# Bad
def my_method(param)
  return param.apply_method
end

# Good
def my_method(param)
  param.apply_method
end
```

##### Do not use `||=` in boolean variables
If a boolean variable is false, it will set the given default value:
```ruby
# Bad - it would assign true to enabled even though it was false
enabled ||= true

# Good
enabled = true if enabled.nil?
```

## Names
##### Name your variables as understandable english words

```ruby
# Bad
x = 1.6
a = val * x

# Good
taxes = 1.6
total = value * taxes
```

##### Use `snake_case` for symbols, methods and variables

##### Use `CamelCase` for classes and modules

##### Use `SCREAMING_SNAKE_CASE` for constants

##### Boolean methods
A method that returns a boolean value should end with a question mark
```ruby
# Bad
def enabled
  some_boolean_value
end

def enabled?
  some_boolean_value
end
```

## Annotations
Always leave a white space between the comment character `#` and the comment text:
```ruby
# Bad
hello_world #As always

# Good
hello_world # As always
```

##### Avoid redundant comments
Most of the times we expect to understand the code with only reading it, we don't need a comment for
something obvious
```ruby
# Bad
return 1 # returns 1
```

## Rails best practices and guidelines
As you can see, all previous guidelines and best practices applies to Ruby language itself, coming
up next we'll cover a some of the Rails specific framework guidelines and best practices.

### Controllers
Lets start talking about controllers in Rails:

##### Skinny controllers
Try to keep your controllers with no business logic code, they shouldn't contain it, that means
controllers should be skinny and only retrieve data for the view layer.

##### One single method
If your controller action calls only one method you're doing it perfectly right (beyond *find* or
*new*)

##### Sharing instance variables
Try not to share more than two (2) instance variables between a particular view and its controller.

### Models
A model is the main connection point with your database.

##### Model names
Name your model in a way that it is understandable, meaningful but short. Try not to use any
abbreviation.

##### Active record defaults
Do not modify or alter ActiveRecord defaults, such as table names, primary keys, foreign keys, etc.

##### Group macro-style methods
Set all macro-style methods such as `validates`, `has_many`, `belongs_to`, etc, at the beginning of
the class definition.

##### Use the new way of validations
```ruby
# Bad
validates_presence_of :name
validates_length_of name, minimum: 2

# Good
validates :name, presence: true, length: { minimum: 2 }
```

##### Generic validators
And we return again to the same topic, about don't repeating your code and keeping generic software.
If you have built some validator that you think could work for any other application, consider
moving it to a **separate gem**.

### ActiveRecord queries
Short time ago I was writing a query for a JSON field (postgres), I used string interpolation and
I realized that something was wrong there. I started to look for a better way to do it and came up
with the parametrized queries. They will avoid SQL injection.
```ruby
# Bad
User.where("metadata -> #{params[:attribute]}")

# Good
User.where("metadata -> ?", params[:attribute])
```

##### `find` v.s. `where`
Consider using `find` if you only have to retrieve one single record by its id
```ruby
# Bad
User.where(id: id).first

# Good
User.find(id)
```

##### `find_by` v.s. `where`
Consider using `find_by` if you only have to retrieve one single record by some attributes
```ruby
# Bad
User.where(name: 'Santiago', gender: 'male').first

# Good
User.find(name: 'Santiago', gender: 'male')
```

##### Usage of `where.not` in queries
Consider using `where.not` statement over SQL on its pure form
```ruby
# Bad
User.where("name != ?", name)

# Good
User.where.not(name: name)
```

### Views
When talking about views you should keep in mind a couple of considerations:

##### Calling models
Do not ever call a model directly from a view.

##### Complex formatting
Avoid complex formatting in views, feel free to use methods to format in some helper or model.

##### Use layouts
Do you feel you are repeating code? Check what pieces of code you can extract and put in a partial
template or layouts.

### Mailers
When using a mailer, there are too some considerations:

##### Naming a mailer
Name your mailer with the *Mailer* suffix, that will help you to recognize immediately a mailer and
differentiate it from a standard view.

##### Providing templates
When you create a mail action, provide both HTML and plain-text view templates. Remember that there
are also some best practices about writing an email HTML, which unfortunately it is not our domain
in this guide.

### Time
At the time of doing this guide, I didn't know about the following practices, I think they are
really important because I've always had problems with time zones.

##### Configure timezone
In your `application.rb` file, configure your timezone accordingly.
```ruby
config.time_zone = 'Eastern European Time'
# Below value can be either :utc or :local, by default it is :utc
config.active_record.default_timezone = :local
```

##### Do not user `Time.parse`
If you use `Time.parse` it will assume time string given is n the system's time zone.
```ruby
# Bad
Time.parse('2015-01-01 12:01:19')

# Good
Time.zone.parse('2015-01-01 12:01:19')
# => Mon, 02 Mar 2015 19:05:37 EET +02:00
```

##### Do not use `Time.now`
If you use `Time.now` it ignores your configured time zone and returns a time in the system's one.
```ruby
# Bad
Time.now

# Good
Time.zone.now # => Sat, 13 Mar 2014 23:01:13 EET +02:00
Time.current  # => Sat, 13 Mar 2014 23:01:13 EET +02:00
```


### References
https://www.youtube.com/watch?v=F63RPaZIuN4

https://github.com/bbatsov/ruby-style-guide

https://github.com/bbatsov/rails-style-guide#one-method
