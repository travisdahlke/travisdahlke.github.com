---
layout: post
title: "Shadowing Gotchas"
date: 2013-11-07 09:00
comments: true
categories: Ruby
published: true
---
*Crossposted from [Coding in the Crease](http://www.codinginthecrease.com/news_article/show/307337)*

Recently, while working on one of those *"WHY ISN'T THIS WORKING?!"* bugs, I stumbled on what I believe is a little understood fact about Ruby variable scoping.

The code went something like this:

```ruby
def foo; "foo"; end

foo = "bar" if foo.nil?
foo # => nil
```

How is this possible you ask? It turns out that in Ruby, **local variables are determined to be in scope at parse time, not at runtime**.

```ruby
defined? foo # => nil
foo = "foo" if false
defined? foo # => "local-variable"
```

Without an explicit receiver or arguments, Ruby will always attempt to dereference the local variable if it is in scope.

```ruby
foo # => nil
self.foo # => "foo"
foo() # => "foo"
```

To avoid this problem in practice, you should avoid shadowing method names with local variables in all but the simplest of cases. We actually had this guideline in our [styleguide](http://sportngin.github.io/styleguide/ruby.html#syntax_L20). 

![](http://aiminglow.com/wp-content/uploads/2011/06/peter-pan.jpg)

When writing or reviewing code, be on the lookout for shadows. You never know when they'll sneak up on you.