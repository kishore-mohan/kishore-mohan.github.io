---
layout: post
title: "Rubyist Common Mistakes and Learnings"
excerpt: "As a Rubyist came across lots of common mistakes and learnings. Thought of sharing with other Rubyists, So everyone avoid this mistakes."
date: 2016-06-25
status: publish
comments: true
share: true
categories:
- Ruby
tags:
- "Ruby, Mistakes, Errors, Hash, irb, kishore-mohan, kishore.M"
image:
  feature: common-mistakes-min.jpg
---

After long break and busy weekends, again back with useful blog. Hope definitely it will helpful to others as well. Lets quickly get into code.

`Developers mean to make mistakes and correct mistakes`, it may sound Harsh but eventually everybody agree at some point. 
Let me share some of the dumb annoying mistakes I came across and some are really dangerous hard to debug and 
find from Unit test cases.

### Condition become nightmare sometimes
___
{% highlight ruby %}
# use .blank? only when knew about the object.

#All this works perfect.
nil.blank? => true
object.blank? => true
"".blank? => true
[].blank? => true

#But it acts weird when object was boolean!!
true.blank? => false
false.blank? => true

{% endhighlight %}
___

Double negative condition is hard to understand like `!user.blank?` but rails provides just opposite of .blank? that's .present?.

`blank? ! = present?`. 

Use .present? for double negative condition of .blank?.

### Includes with Select doesn't work.

select with includes does not produce consistent behavior. It appears that if the included association returns no results, select will work properly, if it returns results, the select statement will have no effect. In fact, it will be completely ignored, such that your select statement could reference invalid table names and no error would be produced. select with joins will produce consistent behavior.

For reference <a href="https://gist.github.com/kishore-mohan/708b336964a3d6247334e16f04bed309" target="_blank">gist-select with includes</a>

### Use map it's Ruby.

As like other language no need to much on map value in Array/Hash. Ruby Iterative methods are powerful.

I’ve seen such code many times:

___

{% highlight ruby %}
users = []
posts.each do |post|
  users << post.user
end

#This is exactly the case for using map, which is shorter and more idiomatic:

users = posts.map do |post|
  post.user
end

{% endhighlight %}

### Use fetch to access Hash

Use fetch to access hash, it make sense when key doesn't exit it throughs proper error message.
___

{% highlight ruby %}
name = {my: {name: {is: "kishore"}}}

name[:my][:name][:is]
#=> "kishore"

name[:ur][:name][:is]
#NoMethodError: undefined method `[]' for nil:NilClass

name.fetch(:ur).fetch(:name)
#KeyError: key not found: :ur

{% endhighlight %}
___

### Ruby Array subset contains all element??

{% highlight ruby %}
a = [5, 1, 6, 14, 2, 8]
b = [2, 6, 15]

a-b
=> [5, 1, 14, 8] # But where is 15?? Hard to find this issue very dangerous

a - b | b -a 
=> [5, 1, 14, 8, 15] # this works.
{% endhighlight %}

### ActiveRecord::Base::exists

Sometime we just need to check for existence of any record in the database. And I have seen coders are writing something like:
To check whether published books are present in the database or not:

{% highlight ruby %}

books = Book.where(:published => true)
books.count > 0 # will return true if published books exists in datatbase.
{% endhighlight %}
Instead use ActiveRecord::Base::exists method as:
{% highlight ruby %}
Book.exists?(:published => true)
{% endhighlight %}

### Use pluck instead of map

We often need to find a single column from the database, So many of us make use of map method on ActiveRecord::Relation.
Like:
`User.active.select(:name).map(&:name)`

Or, even worse:

`User.active.map(&:name)`

Instead use pluck method:
Example:

`User.active.pluck(:name)`

### Use find_each

From time to time, when a query is done, a post process needs to be performed. In cases where the retrieve quantity is small, it is ok to use each; once the result becomes massive, it is much better to use find_each because it gives the result in batches of 1000, making a lower load to the memory. In case you need a whole update on a table, it is also recommended to use update_all; this method will improve the performance up to 100x, as opposed to doing it one by one.

Example:
{% highlight ruby %}
Book.where(:published => true).find_each do |book|
  puts "Do something here!"
end
{% endhighlight %}
