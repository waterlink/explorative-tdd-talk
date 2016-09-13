# Explorative Test-Driven Development

Note:
Welcome everyone, thank you for having me here.



<img src="../my-presentation-template/me.jpeg" class="photo-me">

## Oleksii Fedorov

Software Craftsperson

[@waterlink000](https://twitter.com/waterlink000)

Note:
My name is Oleksii Fedorov. I am a Software Craftsperson and this is my twitter handle.



<img src="pivotal.png" class="pivotal-logo">

Note:
I work as a Software Engineer @ Pivotal Labs.



## Eliminate Fear of Changing Legacy Code

- Increase test coverage
- Increase understanding of the code
- Test-drive your tests

Note:
Today you will learn how to eliminate fear of changing legacy code. You will learn how to confidently and iteratively understand legacy code better and increase test coverage in the process. While code examples will be in Ruby, the technique is language-agnostic. Shall we get the ball rolling?



## Agenda

1. Knowledge in Code
2. Mutation
3. Code <-> Test Relationship
4. Knowledge Test Coverage
5. Mutational Testing
6. Explorative TDD
7. Step-by-step Example
8. Outside of Legacy Code



### Knowledge in Code


<div class="presented-code">
some_var = some.value(from, other, source)
if some_var.predicate?
  something.do_action(some, arguments, here)
elsif some_var == SPECIAL_VALUE
  return UNKNOWN
else
  some_var.split(",").each do |item|
    collaborator.send_item(item)
  end
end
</div>


<div class="presented-code presented-code--with-highlights">
<span class="presented-code__highlight">some_var = some.value(from, other, source)</span>
if some_var.predicate?
  something.do_action(some, arguments, here)
elsif some_var == SPECIAL_VALUE
  return UNKNOWN
else
  some_var.split(",").each do |item|
    collaborator.send_item(item)
  end
end
</div>


<div class="presented-code presented-code--with-highlights">
some_var = some.value(from, other, source)
if <span class="presented-code__highlight">some_var.predicate?</span>
  something.do_action(some, arguments, here)
elsif some_var == SPECIAL_VALUE
  return UNKNOWN
else
  some_var.split(",").each do |item|
    collaborator.send_item(item)
  end
end
</div>


<div class="presented-code presented-code--with-highlights">
some_var = some.value(from, other, source)
if some_var.predicate?
  <span class="presented-code__highlight">something.do_action(some, arguments, here)</span>
elsif some_var == SPECIAL_VALUE
  return UNKNOWN
else
  some_var.split(",").each do |item|
    collaborator.send_item(item)
  end
end
</div>


<div class="presented-code presented-code--with-highlights">
some_var = some.value(from, other, source)
if some_var.predicate?
  something.do_action(<span class="presented-code__highlight">some, arguments, here</span>)
elsif some_var == SPECIAL_VALUE
  return UNKNOWN
else
  some_var.split(",").each do |item|
    collaborator.send_item(item)
  end
end
</div>


<div class="presented-code presented-code--with-highlights">
some_var = some.value(from, other, source)
if some_var.predicate?
  something.do_action(some, arguments, here)
elsif <span class="presented-code__highlight">some_var == SPECIAL_VALUE</span>
  return UNKNOWN
else
  some_var.split(",").each do |item|
    collaborator.send_item(item)
  end
end
</div>


<div class="presented-code presented-code--with-highlights">
some_var = some.value(from, other, source)
if some_var.predicate?
  something.do_action(some, arguments, here)
elsif some_var == <span class="presented-code__highlight">SPECIAL_VALUE</span>
  return UNKNOWN
else
  some_var.split(",").each do |item|
    collaborator.send_item(item)
  end
end
</div>


<div class="presented-code presented-code--with-highlights">
some_var = some.value(from, other, source)
if some_var.predicate?
  something.do_action(some, arguments, here)
elsif some_var == SPECIAL_VALUE
  <span class="presented-code__highlight">return UNKNOWN</span>
else
  some_var.split(",").each do |item|
    collaborator.send_item(item)
  end
end
</div>


<div class="presented-code presented-code--with-highlights">
some_var = some.value(from, other, source)
if some_var.predicate?
  something.do_action(some, arguments, here)
elsif some_var == SPECIAL_VALUE
  <span class="presented-code__highlight">return</span> UNKNOWN
else
  some_var.split(",").each do |item|
    collaborator.send_item(item)
  end
end
</div>


<div class="presented-code presented-code--with-highlights">
some_var = some.value(from, other, source)
if some_var.predicate?
  something.do_action(some, arguments, here)
elsif some_var == SPECIAL_VALUE
  return <span class="presented-code__highlight">UNKNOWN</span>
else
  some_var.split(",").each do |item|
    collaborator.send_item(item)
  end
end
</div>


<div class="presented-code presented-code--with-highlights">
some_var = some.value(from, other, source)
if some_var.predicate?
  something.do_action(some, arguments, here)
elsif some_var == SPECIAL_VALUE
  return UNKNOWN
else
  <span class="presented-code__highlight">some_var.split(",").each do |item|
    collaborator.send_item(item)
  end</span>
end
</div>


<div class="presented-code presented-code--with-highlights">
some_var = some.value(from, other, source)
if some_var.predicate?
  something.do_action(some, arguments, here)
elsif some_var == SPECIAL_VALUE
  return UNKNOWN
else
  <span class="presented-code__highlight">some_var.split(",")</span>.each do <span class="presented-code__highlight">|item|</span>
    collaborator.send_item(item)
  end
end
</div>


<div class="presented-code presented-code--with-highlights">
some_var = some.value(from, other, source)
if some_var.predicate?
  something.do_action(some, arguments, here)
elsif some_var == SPECIAL_VALUE
  return UNKNOWN
else
  some_var.split(",").each do |item|
    <span class="presented-code__highlight">collaborator.send_item(item)</span>
  end
end
</div>


## And so on.

Note:
I think you get the idea.



## Mutation

Mutation - granular change of knowledge, that changes behavior of the system.


### Example


<div class="presented-code">
if cell_is_alive
  do_this
else
  do_some_other_thing
end
</div>


<div class="presented-code presented-code--with-highlights">
if <span class="presented-code__highlight">cell_is_alive</span>
  do_this
else
  do_some_other_thing
end
</div>


<div class="presented-code presented-code--with-highlights">
if <span class="presented-code__highlight mutant">true</span> <span class="presented-code__highlight">cell_is_alive</span>
  do_this
else
  do_some_other_thing
end
</div>


<div class="presented-code presented-code--with-highlights">
if <span class="presented-code__highlight mutant">false</span> <span class="presented-code__highlight">condition</span>
</div>

Note:
Other available mutations here: Changing `if` condition to always be `false`.


<div class="presented-code presented-code--with-highlights">
if <span class="presented-code__highlight mutant">!condition</span> <span class="presented-code__highlight">condition</span>
</div>

Note:
Inverting `if` condition.


<div class="presented-code presented-code--with-highlights">
if condition
  <span class="presented-code__highlight mutant"># if_body</span>
  <span class="presented-code__highlight">if_body</span>
else ...
</div>

Note:
Commenting out `if` body


<div class="presented-code presented-code--with-highlights">
if condition
  if_body
else
  <span class="presented-code__highlight mutant"># else_body</span>
  <span class="presented-code__highlight">else_body</span>
end
</div>

Note:
Commenting out `else` body



## Code and Test Suite Relationship


- Production code is the most important part
- Test suite makes sure production code is correct
- Test suite enables easy refactoring
- Test suite gives courage to introduce a change (new feature, bug fix, etc.)
- Test suite is coupled to the code it tests


- Knowledge in the production code should be verified by test suite
- Knowledge change in code should lead to a test failure
- Knowledge change is a test for the test.



## Agenda

<ol>
  <li class="done">Knowledge in Code</li>
  <li class="done">Mutation</li>
  <li class="done">Code <-> Test Relationship</li>
  <li class="next">Knowledge Test Coverage</li>
  <li>Mutational Testing</li>
  <li>Explorative TDD</li>
  <li>Step-by-step Example</li>
  <li>Outside of Legacy Code</li>
</ul>



## Verifying knowledge coverage

Note:
Just to rephrase it:
How can one verify if specific knowledge in production code is covered by test suite?


### Break it.

Introduce a mutation.

Note:
Introduce a very small change to the knowledge. The test suite should fail.
If it doesn't - knowledge is not covered well enough.


### Semantic Test Stability



## Mutational testing


1. Narrow scope to single granular piece of knowledge
2. Break this knowledge (simple change, Mutation)
3. Make sure there is a test suite failure
4. Restore knowledge to the original state

Note:
I think this is a good time to have some questions...
I always felt that it should be possible to apply Mutational Testing to support the process of understanding the Legacy Code. That is where I started to notice, how experienced engineers deal with untested code. I was able to aggregate it to the technique called Explorative Test-Driven Development...


### Example

1\. Narrow scope:

```ruby
if cell_is_alive
  # .. do this ..
else
  # .. do that ..
end
```


2\. Break this knowledge

```ruby
if true
  # .. do this ..
else
  # .. do that ..
end
```


3\. Make sure there is a test failure

```
$ rake test
....

Finished in 0.02343 seconds (files took 0.11584 seconds to load)
4 examples, 0 failures
```


By adding new test:

```ruby
context "when cell is not alive" do
  it "does that" do
    cell_is_alive = false
    expect(subject).to do_that
  end
end
```

*do_that is a custom matcher*


And run the test again:

```
$ rake test
....F

Finished in 0.02343 seconds (files took 0.11584 seconds to load)
5 examples, 1 failure
```


4\. Restore the knowledge to the original state

```ruby
if cell_is_alive
  # ...
```


and make sure the tests pass:

```
$ rake test
.....

Finished in 0.02343 seconds (files took 0.11584 seconds to load)
5 examples, 0 failures
```



## Explorative TDD

The technique used to increase code coverage and understanding of the knowledge in the code.


1\. Narrow scope to some manageable knowledge and isolate it

Note:
*(manageable knowledge = method/function/class/module)*


2\. Read and try to understand one piece of knowledge


3\. Write a test to verify this assumption


4\. Make sure it passes

Note:
by altering the assumption, or fixing production code (bugs)


5\. Apply Mutational Testing repeatedly

Note:
Apply Mutational Testing to each related granular piece of knowledge to verify that the understanding (and the test) is correct
(this may introduce more tests)


6\. Go back to 2


### Recap

1. Narrow scope and isolate it
2. Read, try to understand, pick granular piece of knowledge
3. Write a test
4. Make sure test passes
5. Apply Mutational Testing repeatedly
6. Go back to 2

NOTE:
I think this is a good time to have some questions...
I think we should go through a small example...



## Agenda

<ol>
  <li class="done">Knowledge in Code</li>
  <li class="done">Mutation</li>
  <li class="done">Code <-> Test Relationship</li>
  <li class="done">Knowledge Test Coverage</li>
  <li class="done">Mutational Testing</li>
  <li class="done">Explorative TDD</li>
  <li class="next">Step-by-step Example</li>
  <li>Outside of Legacy Code</li>
</ul>



## I want to see it in action!

*(step-by-step example)*


### Narrow & Isolate


Means of isolation:

- Extract completely to module/class/package of its own.
- Duplicate the code under the test and put it into function/method(s) with
  different distinguishable name.


#### Our scope


![original code](original_code.png)


### Step 1: Duplicate code to different method

```ruby
class User
  def notifications
    notifications = Database
      .where("notifications") do |x|
    ...
  end

  # full copy of User#notifications
  def notifications__isolated__
    notifications = Database
      .where("notifications") do |x|
    ...
  end
end
```


### Step 2: Read, try to understand and pick knowledge to test

```ruby
(x[1][0] == "followed_notification" && x[1][2] == id.to_s) ||
...
```

```ruby
if kind == "followed_notification"
  {
    kind: kind,
    follower: User.find(values[1].to_i),
    user: User.find(values[2].to_i),
  }
elsif ...
```


### Step 3: Write the first test

```ruby
it "looks like it loads some notifications from the database" do
  user = User.new(email: "john@example.org", password: "welcome")
  Database.insert("notifications", [
    "followed_notification", "986", user.id.to_s]
  )

  notifications = user.notifications__isolated__

  expect(notifications.count).to eq(1)
end
```


### Step 4: Make sure test passes

```
.

Finished in 0.02343 seconds (files took 0.11584 seconds to load)
1 example, 0 failures
```


### Step 5: Apply Mutational Testing repeatedly

```ruby
(x[1][0] == "followed_notification" && x[1][2] == id.to_s) ||
...
```

```ruby
if kind == "followed_notification"
  {
    kind: kind,
    follower: User.find(values[1].to_i),
    user: User.find(values[2].to_i),
  }
elsif ...
```


Break first granular piece of knowledge:

```ruby
# (x[1][0] == "followed_notification" && x[1][2] == id.to_s) ||
...
```


And run tests:

```
F

Failures:

  1) User#notifications looks like it loads some notifications from the database
     Failure/Error: expect(notifications.count).to eq(1)

       expected: 1
            got: 0

       (compared using ==)
     # ./lemon_spec.rb:63:in `block (3 levels) in <top (required)>'

Finished in 0.03511 seconds (files took 0.11877 seconds to load)
1 example, 1 failure
```


Great. This mutation says, that our tests are good to go. Other options:

- replace condition with `false`
- replace condition with `true`


Replacing condition with `true` results in:

```
.

Finished in 0.02343 seconds (files took 0.11584 seconds to load)
1 example, 0 failures
```


Now we have to either:

- change our understanding if it is not what we expect, or
- change our test to cover that, or
- add more tests.


Adding another test does the job:

```ruby
it "ignores records of an invalid kind" do
  user = User.new(email: "john@example.org", password: "welcome")
  Database.insert("notifications", [
    "invalid", "986", user.id.to_s
  ])

  notifications = user.notifications__isolated__

  expect(notifications.count).to eq(0)
end
```


```
.F

Failures:

  1) User#notifications ignores records of invalid kind
     Failure/Error: expect(notifications.count).to eq(0)

       expected: 0
            got: 1

       (compared using ==)
     # ./lemon_spec.rb:131:in `block (3 levels) in <top (required)>'

Finished in 0.21531 seconds (files took 0.11582 seconds to load)
2 examples, 1 failure
```


Undo the mutation:

```ruby
(x[1][0] == "followed_notification" && x[1][2] == id.to_s) ||
...
```

and run tests:

```
..

Finished in 0.02343 seconds (files took 0.11584 seconds to load)
2 examples, 0 failures
```


### Apply Mutational Testing repeatedly


```ruby
...
kind = values[0]
...
```

change to

```ruby
...
kind = values[1]
...
```


And run tests:

```
..

Finished in 0.02343 seconds (files took 0.11584 seconds to load)
2 examples, 0 failures
```


Another non-failing mutation, let's add test:

```ruby
it "loads followed notifications with a correct kind" do
  user = User.new(email: "john@example.org", password: "welcome")
  Database.insert("notifications", [
    "followed_notification", "986", user.id.to_s
  ])

  notifications = user.notifications__isolated__

  expect(notifications[0][:kind]).to eq("followed_notification")
end
```


And run tests:

```
..F

Failures:

  1) User#notifications loads followed notifications with correct kind
     Failure/Error: expect(notifications[0][:kind]).to eq("followed_notification")

     NoMethodError:
       undefined method `[]' for nil:NilClass
     # ./lemon_spec.rb:72:in `block (3 levels) in <top (required)>'

Finished in 0.14636 seconds (files took 0.12127 seconds to load)
3 examples, 1 failure
```

Which is what we expect!


Undo mutation and run tests:

```
...

Finished in 0.14636 seconds (files took 0.12127 seconds to load)
3 examples, 0 failures
```


### Continue applying mutational testing

(until there is enough confidence)


### Go back to step 2 and repeat

(until there is enough confidence)


This step-by-step example can be viewed as commit history here:

https://github.com/waterlink/lemon/pull/6

Note:
This concludes our example and I think it is time for questions...
With that done, we should see, if that technique can be used outside of the context of Legacy Code...



## Outside of the context of Legacy Code


Useful during big refactorings

(extract class/module/package)


Useful when refactoring tests

- verify that test suite is still correct after refactoring
- verify that test suite is not rigid (and identify parts requiring refactoring)

*(rigid = one change -> 70% tests fail)*

Note:
Let's recap the technique itself and draw a bottom line.



## Recap & Q & A

1. Narrow scope and isolate it
2. Read, try to understand, pick granular piece of knowledge
3. Write a test
4. Make sure test passes
5. Apply Mutational Testing repeatedly
6. Go back to 2

Note:
It is time for questions now.



## Thank you

Twitter: [twitter.com/waterlink000](https://twitter.com/waterlink000)

Github: [github.com/waterlink](https://github.com/waterlink)

Blog: [tddfellow.com](http://tddfellow.com)
