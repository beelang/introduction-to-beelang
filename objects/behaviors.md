Behaviors
=========


``` coffee
Person: Object clone()
  hug: Behavior build()
    setup: method(person: Huggable)
      person: person || default
    call: method()
      person hugged(true)
    default: caller
```
