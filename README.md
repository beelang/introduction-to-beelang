Introduction
============

This book acts as an introduction to the programming language named Dragon. The Your First Program chapter will teach you how to write your first Dragon program. It's perfect for people new to programming. More advanced programmers will want to skim that chapter.

Dragon is an language focusing on simplicity in written intent. It's goals are threefold:

  1. Be unsurprising.
  2. Make compartmentalization easy.
  3. Strive to allow building complex things that can rest on simple foundations.

In more dry terms Dragon:

  - Dragon has actors
  - Dragon has immutability
  - Dragon has functions
  - Dragon has streams
  - Dragon has objects

Here's what a labeling a value looks like:

``` coffee
name: "Kurtis Rainbolt-Greene"
```

When this book wants to show you the return value of something it'll look like this:

``` coffee
name #=> "Kurtis Rainbolt-Greene"
```

We can use this in a sentence and show off interpolation:

``` coffee
"My name is {{name}}." #=> "My name is Kurtis Rainbolt-Greene."
```

But what if I want this to be dynamic instead? Here's what a function looks like:

``` coffee
greeting: function()
  "My name is {{name}} and I'm age {{age}}."
#=> <Function>

greeting()
  name: "Kurtis Rainbolt-Greene"
  age: 27
#=> "My name is Kurtis Rainbolt-Greene and I'm age 27."
```

Alright, while that's probably a weird and new syntax lets keep going. Your program is more complex. You want to define multiple behaviors and keep them co-located. In Dragon we call these centers Objects and they are wrappers around streams:

``` coffee
Person: Object clone()
  greeting: function(name, age)
    "My name is {{name}} and I'm age {{age}}."
#=> <Person>
jordan = Person new()
  name: "Jordan Howitzer"
  age: 31
#=> <Person name: "Jordan Howitzer", age: 31, greeting: <Function>>

jordan greeting()
#=> "My name is Jordan Howitzer and I'm age 31."
```

Remember however that Objects are just streams:

``` coffee
jordan name: "Jorda Howitzer"
jordan greeting()
#=> "My name is Jorda Howitzer and I'm age 31."
```

By defining the `Person` object you've created a stream for those types of objects. Any new object created within that `Person` becomes an event on the stream. By "creating" a new person as `jordan` you essentially created a unique identifier that can be watched for on the stream. When you "changed" the name of `jordan` you didn't mutate the object, you put a new event on the stream with the identifier and the data changed, and replaced the old object with the new person using the same data. The `greeting` function you defined is an event handler. Whenever an event on the `Person` stream with the matching id the object it's resting on, and the values defined in the function definition have changed, then it reevaluates.

Here is a sample Dragon program:

```
# First we assign the twitter client library to a local constant:
Twitter: Library() load("github.com/twitter/client.dov")

# Now we setup a new Twitter client to connect to the API:
client: Twitter() Client(key: "...", secret: "...")

# We can post to our account now:
status: Twitter() Status() build(text: "Hello, world!", client: client())
status() save()

# We can talk to the Stream API too!
# In this example we want to create a thing to count how many
# times the language is mentioned

# First we create a counter object that holds the amount of times we see the phrase
Counter: Object() clone()
  # Every object runs the `setup()` method after being built
  setup: behavior()
    # This creates a private piece of data, accessible only by the object
    private(name: "@amount", value: 0)
  # This behavior is accessible externally, by other objects
  tick: behavior()
    # It increments the amount and thens tores the amount
    store(name: "@amount", value: @amount() increment())

# Now we create a new instance of the counter
ticker: Counter() build()

# We start watching the stream for the phrase and give it a process to run
stream: Twitter() Stream() build(filter: "#dragonlang")
  ticker() tick()

# And now we watch the stream
stream watch()
```
