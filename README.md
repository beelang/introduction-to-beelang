Introduction
============

This book acts as an introduction to the programming language named Dragon. The Your First Program chapter will teach you how to write your first Dragon program. It's perfect for people new to programming. More advanced programmers will want to skim that chapter and go straight to the chapter named Types.

Dragon is an organism oriented programming language focusing on simplicity in written intent. It's goals are threefold:

  1. Be unsurprising.
  2. Make compartmentalization easy.
  3. Strive to allow building complex things that can rest on simple foundations.

In more dry terms Dragon is an imperative, prototype-based, object oriented, optionally dynamically typed, and reflective language. All values are objects and all messages are dynamic.

Here is a sample Dragon program:

```
# First we assign the twitter client library to a local constant:
Twitter: Library() load("twitter.com/client")

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
