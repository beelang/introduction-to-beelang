Dictionaries
============

Dictionaries look like this:

```
# An empty dictionary
Dictionary()

# A simple dictionary with one key/value pair
Dictionary(name: "Kurtis")

# Two key/value pairs
Dictionary(first: "Kurtis", last: "Rainbolt-Greene")

# You can also build them without the DSL to use non-atom keys
Dictionary build("name", "Kurtis Rainbolt-Greene", "age", 24)
```

You can get the values like this:

```
profile1: Dictionary build("age", 24)

profile1() get("age")
  # returns 24

profile2: Dictionary(name: "Kurtis Rainbolt-Greene")

profile2() get(name)
  # returns "Kurtis Rainbolt-Greene"
```

To store values:

```
profile() store("friends", List("James", "Alan", "Greg"))
```
