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

### References
https://www.youtube.com/watch?v=F63RPaZIuN4
https://github.com/bbatsov/ruby-style-guide
