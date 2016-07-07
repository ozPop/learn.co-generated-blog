---
layout: post
title: Getting to know Pry
---

*Target audiance:* Ruby begginer, someone with little to no experience using pry.

Pry is a very powerful and feature rich REPL. This blog post will be an intro
to pry as well as a "prymer" to some of the features begginers should get
comfortable with.(9)

Index:

* Description of REPL, IRB, and PRY
* Example of common usecase, "prying" into a method
* Modifying / Editing code from within PRY session
* Access to documentation


**What is REPL, IRB and PRY?**

----

* REPL acronym stands for read-eval-print loop. It offers an interface for a
user to input expressions, which are then evaluated and displayed back to the
user. The process is then repeated until program session is closed.(1)

    REPL's serve many different programming languages however, they are in particular
common with scripting languages.(1)

* IRB comes packaged with ruby. It is a native REPL that any ruby user has
access to. It allows for rappid experimentation in real-time and is especially
popular with new ruby developers.(2)

    ![Example or irb](http://i.imgur.com/u0dDdsz.png)

* [Pry](http://pryrepl.org/) is an alternative to irb. It packs a number of 
exciting features that differentiate it from IRB and makes it a very useful
tool for a ruby developer.

    Some of the key features are:

    * Better indentation
    * Color coding
    * Start REPL mid execution
    * Edit files from within a pry session
    * Quick access to documentation
    * Input history search
    * Run shell commands
    * Explore ruby ojects

    But.., how else is it different from IRB?

    One major difference is, an IRB user has to input code before that code
    gets evaluated, in other words you deliver code to REPL session. Pry, unlike
    IRB, creates a REPL session around your existing code.(3)


**Useful Commands:**

`help` - list various keywords, aliases and commands with descriptions

e.g.: `exit` - while in a loop, moves up one iteration or to next binding.pry

e.g.: `exit-program` or `exit!` or `!!!` - exits the program


NOTE: Pry offers a lot of what irb offers and quite a bit more. Arguably, one
of the more useful feartures of pry is the ability to stop a procedure during
execution and give yourself opportunity to debug or verify what is happening
at that exact place in your code.


**A common usecase example**

-----

*Example 1: Prying into a method.*

```
require 'pry' # <= remember to require

def say_hello(person_name)
    greeting = "Hi there!"
    puts "#{greeting} #{person_name}"
    binding.pry # <= stop execution and bring me into pry console
end
```

![image title](http://i.imgur.com/SiDXtEO.png)

Using the previous example we can stop the method before it returns and 
inspect the code more closely.

*Example 2: multiple bindings*

When iterating over nested data structure like the hash below, we can inspect
each step of iteration.

```
hash = {
  :key => "value",
  :key2 => "value2",
  :key3 => { # <= value3 is another hash
    :sub_key => "sub_value",
    :sub_key2 => "sub_value2"
  },
  :key4 => "value4"
}
```

```
def prying_hashes(some_hash)
  some_hash.each do |key, value|
    binding.pry # <= first binding
    if value.class == Hash
      value.each do |sub_key, sub_value|
        binding.pry # <= other binding
      end
    end
  end
end

prying_hashes(hash)
```


**Documetation from within pry**

----


*Sources*

1. [REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)
2. [IRB](https://en.wikipedia.org/wiki/Interactive_Ruby_Shell)
3. [Turning IRB on its head with Pry](https://banisterfiend.wordpress.com/2011/01/27/turning-irb-on-its-head-with-pry/)
9. Alex Griffith pun