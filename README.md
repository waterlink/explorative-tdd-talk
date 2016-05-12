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


Knowledge change (Mutation) is a test for the test.



## Mutational testing


1. Narrow scope to single granular piece of knowledge
2. Break this knowledge (simple change, Mutation)
3. Make sure there is a test suite failure
4. Restore knowledge to the original state



## Reverse TDD


1\. Narrow scope to some manageable knowledge and isolate it (method/function/class/module)


2\. Read the code and try to understand one granular piece of knowledge it is doing


3\. Write a test to verify your assumption


4\. Make sure it passes (by fixing your assumption, or fixing production code (bugs))


5\. Apply Mutational Testing to each related granular piece of knowledge to verify that your understanding (and the test is correct)

(this may introduce more tests)


6\. Go back to 2


### Recap

1. Narrow scope and isolate it
2. Read, try to understand, pick granular piece of knowledge
3. Write a test
4. Make sure test passes
5. Apply Mutational Testing repeatedly
6. Go back to 2


### I want to see it in action!

*(FIXME: step-by-step example should be here)*



## Q & A



## Thank you

Twitter: [twitter.com/waterlink000](https://twitter.com/waterlink000)

Github: [github.com/waterlink](https://github.com/waterlink)

Blog: [tddfellow.com](http://tddfellow.com)
