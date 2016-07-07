---
layout: post
title: Getting to know Pry
---

A "prymer" to Pry.
- Alex Griffith

*Target audiance:* Ruby begginer, someone with little to no experience using Pry.

Pry is a powerful and feature rich REPL. This blog post is an intro to Pry as 
well as to some of the features begginers should strive to get comfortable with.

Index:

* Description of REPL, IRB, and Pry
* Examples of binding.pry
* How to access documentation in pry


**What is REPL, IRB and PRY?**

----

* REPL acronym stands for read-eval-print loop. It offers an interface for a
user to input expressions, which are then evaluated and displayed back to the
user. The process is then repeated until program session is closed.(1)

    REPL's serve many different programming languages, however they are particularly
common with scripting languages.(1)

* IRB comes packaged with ruby. It is a native REPL that any ruby user has
access to. It allows for rapid experimentation in real-time and is especially
popular with new ruby developers.(2)

    ![example or irb](http://i.imgur.com/4lbMJmN.png)

* [Pry](http://pryrepl.org/) is an alternative to IRB. It packs a number of 
exciting features that differentiate it from IRB and make it a very useful
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

    In IRB a user has to input code before that code gets evaluated, in other
    words you deliver code to REPL session. Pry, unlike IRB, can create a REPL
    session around your existing code.(3)

    To install pry run `gem install pry` command.


**Useful Commands:**

`pry` - launches a pry session in your shell.

`require "pry"` - used at the top of the file to import pry into the file.

`binding.pry`- used inside file to indicate where execution will enter pry.

*while in pry console*:

`help` - list various keywords, aliases and commands with descriptions.

`exit` - in a loop, moves up one iteration or executes until next binding.pry 
statement or exits Pry session.

`exit-program` or `exit!` or `!!!` - exits the program immediately.


NOTE: Pry offers a lot of what IRB offers and quite a bit more. Arguably, one
of the more powerful feartures of Pry is the **ability to stop a procedure during
execution**. This gives the user an opportunity to debug or verify what is happening
at that exact place in the code.


**Usecase examples**

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

![using binding.pry](http://i.imgur.com/7mRq3ex.png)

Using the previous example we can stop the method before it returns and 
inspect that section of code more closely.

*Example 2: multiple bindings*

When iterating over nested data structure like the hash below, we can inspect
each step of iteration.

```
hash = {
  :key1 => "value1",
  :key2 => "value2",
  :key3 => { # <-- "value3" is another hash
    :sub_key1 => "sub_value1",
    :sub_key2 => "sub_value2"
  },
  :key4 => "value4"
}
```

```
def prying_hashes(some_hash)
  some_hash.each do |key, value|
    binding.pry # <-- first binding
    if value.class == Hash
      value.each do |sub_key, sub_value|
        binding.pry # <-- other binding
      end
    end
  end
end

prying_hashes(hash)
```

![multiple bindings](http://i.imgur.com/k2WXWEC.png)

The above image demonstrates how a user can navigate through a hash itteration 
by typing `exit`, in the above case it takes 3 times, at which point the user 
hits the next binding.pry statement. Note the lines 23 in the first three 
snipets and then note the line 26 in the fourth one.


**Documetation from within Pry**

----

Another useful feature is examining documentation while in Pry. To do that,
first install the required gem using `gem install pry-doc` command. While in Pry
type `show-doc Class_name#method_name` which will display related documentation.
A shortcut for show doc is `?` which results in `? Array#map!`.

![show-doc](http://i.imgur.com/fIJzpVy.png)

NOTE: 

* `show-source Class_name#method_name` will return source code
* `show-method method_name` is a good way to inspect your own methods

![show-method](http://i.imgur.com/KAefRLk.png)

**Conclusion**

----

This introduction just scratched the surface of what Pry can do for you as a
ruby developer. In this blog listed a number of features however, I only covered a
couple of them. If some of the things seem confusing, playing around with pry 
should make things more clear.


Look out for another blog entry in which I will cover other useful aspects of Pry.


*Sources*

1. [REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)
2. [IRB](https://en.wikipedia.org/wiki/Interactive_Ruby_Shell)
3. [Turning IRB on its head with Pry](https://banisterfiend.wordpress.com/2011/01/27/turning-irb-on-its-head-with-pry/)
4. [Pry Wiki](https://github.com/pry/pry/wiki)