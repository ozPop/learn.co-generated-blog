---
layout: post
title: Getting to know Pry
---

A "prymer" to Pry
- Alex Griffith

*Target audience:* Ruby beginner, someone with little to no experience using Pry.

Pry is a powerful and feature rich REPL. This blog post is an intro to Pry as 
well as to some of the features it offers.

Discussion points:

* Description of REPL, IRB, and Pry
* Use case examples of `binding.pry`
* How to access documentation while in Pry


**What are REPL, IRB and Pry?**

----

* REPL stands for *read-eval-print loop*. It offers an interface for a 
user to input expressions, which are then evaluated and displayed back to the 
user. The process is then repeated until program session is closed.

    REPL's serve many different programming languages, however they are 
    particularly common with scripting languages.(1)

* IRB comes packaged with ruby. It is a native REPL that any ruby user has 
access to. It allows for rapid experimentation in real-time and is especially 
popular with new ruby developers.(2)

    ![example of irb](http://i.imgur.com/4lbMJmN.png)

* [Pry](http://pryrepl.org/) is an alternative to IRB. It packs a number of 
exciting features that differentiate it from IRB and make it a very useful 
tool for a any rubyist.

    Some of the key features are:

    * Better indentation
    * Color coding
    * Start REPL mid execution
    * Edit files from within a Pry session
    * Quick access to documentation
    * Input history search
    * Run shell commands
    * Explore ruby objects

    In IRB a user has to input code before that code gets evaluated; in other 
    words, you deliver code to REPL session. Pry, unlike IRB, can create a REPL 
    session around your existing code.(3)

    To install Pry, run the command `gem install pry`.


**Useful Commands:**

----

`pry` - launches a Pry session in your shell.

`require "pry"` - used at the top of the file to import Pry into the file.

`binding.pry`- used inside file to indicate where execution will enter Pry.

*while in Pry console*:

`help` - list various keywords, aliases and commands with descriptions.

`exit` - in a loop, moves up one iteration or executes until next `binding.pry` 
statement or exits Pry session.

`exit-program` or `exit!` or `!!!` - exits the program immediately.


NOTE: Pry offers a lot of what IRB offers and more. Arguably, one 
of the more powerful features of Pry is **the ability to stop a procedure during 
execution and give the user an opportunity to debug or verify what is happening 
at that exact place in the code**.


**Use case examples**

----

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
each iteration.

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

The above image demonstrates how a user can navigate through a hash iteration 
by typing `exit`. In the above case it takes 3 iterations, at which point the user 
hits the next `binding.pry` statement. Note line 23 in the first three 
snipets and then note line 26 in the fourth one.


**Documentation from within Pry**

----

Another useful feature is examining documentation while in Pry. To do that, 
first install the required gem using the command `gem install pry-doc`. While 
in Pry, type `show-doc Class_name#method_name` which will display related 
documentation. A shortcut for show doc is `?` which results in `? Array#map!`.(4)

![show-doc](http://i.imgur.com/fIJzpVy.png)

NOTE: 

* `show-source Class_name#method_name` will return source code
* `show-method method_name` is a good way to inspect your own methods

![show-method](http://i.imgur.com/KAefRLk.png)

**Conclusion**

----

This introduction just scratched the surface of what Pry can do for you as a 
ruby developer. This blog listed a number of features, however I only covered a 
couple of them. If some of the things seem confusing, playing around with Pry 
should make things more clear.


Look out for another blog entry in which I will cover other features of Pry.


*Sources:*

1. [REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)
2. [IRB](https://en.wikipedia.org/wiki/Interactive_Ruby_Shell)
3. [Turning IRB on its head with Pry](https://banisterfiend.wordpress.com/2011/01/27/turning-irb-on-its-head-with-pry/)
4. [Pry Wiki](https://github.com/pry/pry/wiki)