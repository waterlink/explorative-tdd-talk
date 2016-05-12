# Reverse Test-Driven Development

by That TDD Fellow

Oleksii Fedorov

[@waterlink000](https://twitter.com/waterlink000)



## Code <- Test Relationship


Production code is the most important part


Test suite makes sure production code is correct


Test suite enables easy refactoring


Test suite gives courage to introduce a change (new feature, bug fix, etc.)


Test suite is coupled to the code it tests



## Verifying knowledge coverage

How can one verify if specific knowledge in production code is covered by test suite?


### Knowledge in Production Code?


```ruby
variable_name = some.value(from, other, source)
```

this is the knowledge


```ruby
if <knowledge>
...
```


```ruby
if ...
  <knowledge>
...
```


```ruby
...
elsif <knowledge>
...
```


```ruby
...
else
  <knowledge>
end
```


```ruby
something.doAction(this, is, a, knowledge, too)
```


```ruby
<knowledge>.each do |item|
  ...
end
```


```ruby
....each do |item|
  <knowledge>
end
```


```ruby
return <knowledge>
```


### Got it. How to verify it?


### Break it.


Introduce a very small change to the knowledge. Test suite should fail.

If it doesn't - knowledge is not covered well enough.



## Code -> Test Relationship


Knowledge in the production code should be verified by test suite


Knowledge change is an act of verification if test suite is correct (or thorough enough)



## Thank you

Twitter: [twitter.com/waterlink000](https://twitter.com/waterlink000)

Github: [github.com/waterlink](https://github.com/waterlink)

Blog: [tddfellow.com](http://tddfellow.com)
