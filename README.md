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
Today you will learn how to eliminate fear of changing legacy code. You will learn how to confidently and iteratively understand legacy code better and increase test coverage in the process. While code examples will be in Ruby, the technique is language-agnostic.



## Legacy Code

- Hard to understand
- No tests
- Brings value

Note:
For the purposes of this talk I need to define what Legacy Code means. It is hard to understand. It has no tests or almost no tests and it brings value to the business and customers.

Let's take a look at what we will be going through today:



## Agenda

1. Knowledge in Code
2. Mutation
3. Code <-> Test Relationship
4. Most Useful Coverage Metric
5. Mutational Testing
6. Explorative TDD
7. Step-by-step Example
8. Outside of Legacy Code

Note:
We will define what Knowledge in the ProdCode and Mutation means. We will take a look at the relationship of the ProdCode and TestSuite. Then we will see what is the most useful coverage metric is. Then we will explore Mutational Testing and Explorative TDD techniques. And we will go through the example. Shall we get started?



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
I think you get the idea. And if we change small piece of knowledge, we are introducing...



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

Note:
..Explain what this code does..

So let's pick a first bit of knowledge here:


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


### How does Test Suite affect Production Code?


- Makes sure Code is correct
- Enables refactoring
- Gives courage to introduce change
- Coupled to Code


### How Does Production Code affect Test Suite?


- Knowledge should be verified by Test Suite
- Mutation should lead to a Test failure


### Knowledge Change

### ==

### Test for the Test



## Agenda

<ol>
  <li class="done">Knowledge in Code</li>
  <li class="done">Mutation</li>
  <li class="done">Code <-> Test Relationship</li>
  <li class="next">Most Useful Coverage Metric</li>
  <li>Mutational Testing</li>
  <li>Explorative TDD</li>
  <li>Step-by-step Example</li>
  <li>Outside of Legacy Code</li>
</ul>

Note:
..Great point to stop, give audience chance to ask questions and drink some water..



## Knowledge Coverage


### How to check if knowledge is covered?


### Break it.

Introduce a mutation.

Note:
Introduce a very small change to the knowledge. The test suite should fail.
If it doesn't - knowledge is not covered well enough. And that leads us to the term called...


### Semantic Test Stability

Note:
Semantic Test Stability. Test Suite can be considered semantically stable if for any mutation to any bit of knowledge it tests there is a failing test. There are techniques that allow us to keep this metric up high. One of them is...



## Mutational testing


1. Narrow scope to single granular piece of knowledge
2. Break this knowledge (simple change, Mutation)
3. Make sure there is a test suite failure
4. Restore knowledge to the original state

Note:
Let's see it in action.


### Example


<div class="presented-code">
if cell_is_alive
  do_this
else
  do_some_other_thing
end
</div>

Note:
First, we need to narrow our scope to a single bit of knowledge.


<div class="presented-code presented-code--with-highlights">
if <span class="presented-code__highlight">cell_is_alive</span>
  do_this
else
  do_some_other_thing
end
</div>

Note:
Second, we need to introduce a mutation:


<div class="presented-code presented-code--with-highlights">
if <span class="presented-code__highlight mutant">true</span> <span class="presented-code__highlight">cell_is_alive</span>
  do_this
else
  do_some_other_thing
end
</div>

Note:
Third, we need to make sure there is a test failure:


<div class="presented-code presented-code--with-highlights">
$ rake test
<span class="presented-code__highlight">....</span>

Finished in 0.02343 seconds (files took 0.11584 seconds to load)
4 examples, <span class="presented-code__highlight">0 failures</span>
</div>

Note:
Oh no, it didn't fail, so we have a "failing test" for our test suite. In this case we need to add the test for the negative case:


<div class="presented-code presented-code--with-highlights">
<span class="presented-code__highlight">cell_is_alive = false</span>
expect(did_some_other_thing).to eq(true)
</div>


<div class="presented-code presented-code--with-highlights">
cell_is_alive = false
expect(<span class="presented-code__highlight">did_some_other_thing</span>).to eq(true)
</div>


<div class="presented-code presented-code--with-highlights">
cell_is_alive = false
expect(<span class="presented-code__highlight">did_some_other_thing</span>).to eq(<span class="presented-code__highlight alternative">true</span>)
</div>


<div class="presented-code presented-code--with-highlights">
$ rake test
<span class="presented-code__highlight">....</span><span class="presented-code__highlight failure">F</span>

Finished in 0.02343 seconds (files took 0.11584 seconds to load)
5 examples, <span class="presented-code__highlight failure">1 failure</span>
</div>


<div class="presented-code presented-code--with-highlights">
if <span class="presented-code__highlight">true</span>
  do_this
else
  do_some_other_thing
end
</div>


<div class="presented-code presented-code--with-highlights">
if <span class="presented-code__highlight mutant">cell_is_alive</span> <span class="presented-code__highlight">true</span>
  do_this
else
  do_some_other_thing
end
</div>


<div class="presented-code">
if cell_is_alive
  do_this
else
  do_some_other_thing
end
</div>


<div class="presented-code presented-code--with-highlights">
$ rake test
<span class="presented-code__highlight">.....</span>

Finished in 0.02343 seconds (files took 0.11584 seconds to load)
5 examples, <span class="presented-code__highlight">0 failures</span>
</div>

Note:
Usually, to accomplish any useful behavior we would like to combine multiple bits of knowledge. So if we want to understand how system works better, we need to focus on groups of pieces of knowledge. This is what Explorative TDD is about:



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
  <li class="done">Most Useful Coverage Metric</li>
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


<div class="presented-code">
class User
  def notifications
    notifications = Database
      .where("notifications") do |x|
</div>


<div class="presented-code">
      (x[1][0] == "followed_notification" &&
        x[1][2] == id.to_s) ||
      (x[1][0] == "favorited_notification" &&
        StatusUpdate.find(x[1][2].to_i)
            .owner_id == id) ||
      (x[1][0] == "replied_notification" &&
        StatusUpdate.find(x[1][2].to_i)
            .owner_id == id) ||
      (x[1][0] == "reposted_notification" &&
        StatusUpdate.find(x[1][2].to_i)
            .owner_id == id)
</div>


<div class="presented-code">
    end.map do |row|
      id, values = row
      kind = values[0]
</div>


<div class="presented-code">
      if kind == "followed_notification"
        {
          kind: kind,
          follower: User.find(values[1].to_i),
          user: User.find(values[2].to_i),
        }
</div>


<div class="presented-code">
      elsif kind == "favorited_notification"
        {
          kind: kind,
          favoriter: User.find(values[1].to_i),
          status_update: StatusUpdate
              .find(values[2].to_i),
        }
</div>


<div class="presented-code">
      elsif kind == "replied_notification"
        {
          kind: kind,
          sender: User.find(values[1].to_i),
          status_update: StatusUpdate
              .find(values[2].to_i),
          reply: StatusUpdate
              .find(values[3].to_i),
        }
</div>


<div class="presented-code">
      elsif kind == "reposted_notification"
        {
          kind: kind,
          reposter: User.find(values[1].to_i),
          status_update: StatusUpdate.find(values[2].to_i),
        }
      end
    end
</div>


<div class="presented-code">
  Analytics.tag({name: "fetch_notifications",
                count: notifications.count})
  notifications
end
</div>

Note:
As you might guess, this code is really overwhelming. So, let's start by duplicating the whole method in an isolated method to test it:


<div class="presented-code presented-code--with-highlights">
class User
  def notifications
    notifications = Database
      .where("notifications") do |x|
    ...
  end
</div>


<div class="presented-code presented-code--with-highlights">
class User
  <span class="presented-code__highlight">def notifications
    notifications = Database
      .where("notifications") do |x|
    ...
  end</span>
</div>


<div class="presented-code presented-code--with-highlights">
class User
  <span class="presented-code__highlight">def notifications ... end</span>

  <span class="presented-code__highlight alternative">def notifications_isolated
    notifications = Database
      .where("notifications") do |x|
    ...
  end</span>
</div>

Note:
Next we need to read the code and try to understand a concrete part of the behavior it has. We need to identify related knowledge for this behavior as a whole:


<div class="presented-code presented-code--with-highlights">
...
<span class="presented-code__highlight">(x[1][0] == "followed_notification" &&
  x[1][2] == id.to_s)</span> ||
...
if kind == "followed_notification"
  {
    kind: kind,
    follower: User.find(values[1].to_i),
    user: User.find(values[2].to_i),
  }
elsif ...
</div>


<div class="presented-code presented-code--with-highlights">
...
<span class="presented-code__highlight">(x[1][0] == <span class="presented-code__highlight alternative">"followed_notification"</span> &&
  x[1][2] == id.to_s)</span> ||
...
if kind == <span class="presented-code__highlight alternative">"followed_notification"</span>
  {
    kind: kind,
    follower: User.find(values[1].to_i),
    user: User.find(values[2].to_i),
  }
elsif ...
</div>


<div class="presented-code presented-code--with-highlights">
...
<span class="presented-code__highlight">(x[1][0] == <span class="presented-code__highlight alternative">"followed_notification"</span> &&
  x[1][2] == id.to_s)</span> ||
...
<span class="presented-code__highlight">if kind == <span class="presented-code__highlight alternative">"followed_notification"</span>
  {
    kind: kind,
    follower: User.find(values[1].to_i),
    user: User.find(values[2].to_i),
  }</span>
elsif ...
</div>

Note:
These bits of knowledge indeed look related, so let's try to guess what behavior they are responsible for:


<div class="presented-code presented-code--with-highlights">
it "<span class="presented-code__highlight">can load followed notifications</span>" do
  # TODO
end
</div>

Note:
Wait. I think we are making to big assumption. There is a smaller assumption that we need to validate here:


<div class="presented-code presented-code--with-highlights">
it "<span class="presented-code__highlight">can load <span class="presented-code__highlight alternative mutant">some</span> <span class="presented-code__highlight">followed</span> notifications</span>" do
  # TODO
end
</div>


<div class="presented-code presented-code--with-highlights">
it "can load some notifications" do
  <span class="presented-code__highlight"># TODO</span>
end
</div>


<div class="presented-code presented-code--with-highlights">
it "can load some notifications" do
  <span class="presented-code__highlight mutant">user = User.new(email: "john@example.org",
                  password: "welcome")</span>
  <span class="presented-code__highlight"># TODO</span>
end
</div>


<div class="presented-code presented-code--with-highlights">
it "can load some notifications" do
  user = User.new(email: "john@example.org",
                  password: "welcome")
  <span class="presented-code__highlight">Database.insert("notifications",
    ["followed_notification", "986",
      user.id.to_s])</span>
end
</div>


<div class="presented-code presented-code--with-highlights">
it "can load some notifications" do
  user = User.new(email: "john@example.org",
                  password: "welcome")
  Database.insert("notifications",
    ["followed_notification", "986",
      user.id.to_s])
  <span class="presented-code__highlight">notifications = user.notifications_isolated</span>
end
</div>


<div class="presented-code presented-code--with-highlights">
it "can load some notifications" do
  user = User.new(email: "john@example.org",
                  password: "welcome")
  Database.insert("notifications",
    ["followed_notification", "986",
      user.id.to_s])
  notifications = user.notifications_isolated
  expect(<span class="presented-code__highlight">notifications.count</span>).to eq(1)
end
</div>


<div class="presented-code presented-code--with-highlights">
it "can load some notifications" do
  user = User.new(email: "john@example.org",
                  password: "welcome")
  Database.insert("notifications",
    ["followed_notification", "986",
      user.id.to_s])
  notifications = user.notifications_isolated
  expect(<span class="presented-code__highlight">notifications.count</span>).to eq(<span class="presented-code__highlight alternative">1</span>)
end
</div>

Note:
Next step is to make sure that this test passes:


<div class="presented-code presented-code--with-highlights">
$ rake test
<span class="presented-code__highlight">.</span>

Finished in 0.02343 seconds (files took 0.11584 seconds to load)
1 example, <span class="presented-code__highlight">0 failures</span>
</div>

Note:
Now we need to apply mutational testing repeatedly to these bits of knowledge until we are confident that it is well-tested. For example:


<div class="presented-code">
(x[1][0] == "followed_notification" &&
  x[1][2] == id.to_s) ||
...
if kind == "followed_notification"
  {
    kind: kind,
    follower: User.find(values[1].to_i),
    user: User.find(values[2].to_i),
  }
elsif ...
</div>

Note:
Let's take a closer look at this boolean condition:


<div class="presented-code presented-code--with-highlights">
<span class="presented-code__highlight">(x[1][0] == "followed_notification" &&
  x[1][2] == id.to_s)</span> ||
...
if kind == "followed_notification"
  {
    kind: kind,
    follower: User.find(values[1].to_i),
    user: User.find(values[2].to_i),
  }
elsif ...
</div>

Note:
For example, We could remove it:


<div class="presented-code presented-code--with-highlights">
<span class="presented-code__highlight mutant"></span><span class="presented-code__highlight">(x[1][0] == "followed_notification" &&
  x[1][2] == id.to_s) ||</span>
...
if kind == "followed_notification"
  {
    kind: kind,
    follower: User.find(values[1].to_i),
    user: User.find(values[2].to_i),
  }
elsif ...
</div>


<div class="presented-code presented-code--with-highlights">
$ rake test
<span class="presented-code__highlight failure">F</span>

Finished in 0.03511 seconds (files took 0.11877 seconds to load)
1 example, <span class="presented-code__highlight failure">1 failure</span>
</div>

Note:
So it fails, great! Let's take a detailed look at the failure itself:


<div class="presented-code presented-code--with-highlights">
  1) User#notifications looks like it loads some notifications from the database
     Failure/Error: <span class="presented-code__highlight failure">expect(notifications.count).to eq(1)</span>

       expected: 1
            got: 0

       (compared using ==)
     # ./lemon_spec.rb:63
</div>


<div class="presented-code presented-code--with-highlights">
  1) User#notifications looks like it loads some notifications from the database
     Failure/Error: <span class="presented-code__highlight failure">expect(notifications.count).to eq(1)</span>

       <span class="presented-code__highlight">expected: 1</span>
            got: 0

       (compared using ==)
     # ./lemon_spec.rb:63
</div>


<div class="presented-code presented-code--with-highlights">
  1) User#notifications looks like it loads some notifications from the database
     Failure/Error: <span class="presented-code__highlight failure">expect(notifications.count).to eq(1)</span>

       <span class="presented-code__highlight">expected: 1</span>
            <span class="presented-code__highlight failure">got: 0</span>

       (compared using ==)
     # ./lemon_spec.rb:63
</div>

Note:
This mutant is not surviving, which means that our test is good. Or is it? Let's see what other mutations we can introduce for this bit of knowledge:


<div class="presented-code presented-code--with-highlights">
<span class="presented-code__highlight mutant">false</span> <span class="presented-code__highlight">(x[1][0] == "followed_notification" &&
  x[1][2] == id.to_s)</span> ||
...
if kind == "followed_notification"
  {
    kind: kind,
    follower: User.find(values[1].to_i),
    user: User.find(values[2].to_i),
  }
elsif ...
</div>


<div class="presented-code presented-code--with-highlights">
<span class="presented-code__highlight mutant">true</span> <span class="presented-code__highlight">(x[1][0] == "followed_notification" &&
  x[1][2] == id.to_s)</span> ||
...
if kind == "followed_notification"
  {
    kind: kind,
    follower: User.find(values[1].to_i),
    user: User.find(values[2].to_i),
  }
elsif ...
</div>

Note:
Replacing condition with `true` results in:


<div class="presented-code presented-code--with-highlights">
$ rake test
<span class="presented-code__highlight">.</span>

Finished in 0.02343 seconds (files took 0.11584 seconds to load)
1 example, <span class="presented-code__highlight">0 failures</span>
</div>


Now we have to either:

- change our understanding if it is not what we expect, or
- change our test to cover that, or
- add more tests.

Note:
In this case, adding another test does the job:


<div class="presented-code presented-code--with-highlights">
it "<span class="presented-code__highlight">ignores records of an invalid kind</span>" do
end
</div>


<div class="presented-code presented-code--with-highlights">
it "ignores records of an invalid kind" do
  <span class="presented-code__highlight">user = User.new(email: "john@example.org",
                  password: "welcome")</span>
end
</div>


<div class="presented-code presented-code--with-highlights">
it "ignores records of an invalid kind" do
  user = User.new(email: "john@example.org",
                  password: "welcome")
  <span class="presented-code__highlight">Database.insert("notifications",
    ["invalid", "986", user.id.to_s])</span>
end
</div>


<div class="presented-code presented-code--with-highlights">
it "ignores records of an invalid kind" do
  user = User.new(email: "john@example.org",
                  password: "welcome")
  Database.insert("notifications",
    ["invalid", "986", user.id.to_s])
  <span class="presented-code__highlight">notifications = user.notifications_isolated</span>
end
</div>


<div class="presented-code presented-code--with-highlights">
it "ignores records of an invalid kind" do
  user = User.new(email: "john@example.org",
                  password: "welcome")
  Database.insert("notifications",
    ["invalid", "986", user.id.to_s])
  notifications = user.notifications_isolated
  expect(<span class="presented-code__highlight">notifications.count</span>).to eq(0)
end
</div>


<div class="presented-code presented-code--with-highlights">
it "ignores records of an invalid kind" do
  user = User.new(email: "john@example.org",
                  password: "welcome")
  Database.insert("notifications",
    ["invalid", "986", user.id.to_s])
  notifications = user.notifications_isolated
  expect(<span class="presented-code__highlight">notifications.count</span>).to eq(<span class="presented-code__highlight alternative">0</span>)
end
</div>


<div class="presented-code presented-code--with-highlights">
$ rake test
<span class="presented-code__highlight">.</span><span class="presented-code__highlight failure">F</span>

Finished in 0.21531 seconds (files took 0.11582 seconds to load)
2 examples, <span class="presented-code__highlight failure">1 failure</span>
</div>


<div class="presented-code presented-code--with-highlights">
  1) User#notifications ignores records of invalid kind
     Failure/Error: <span class="presented-code__highlight failure">expect(notifications.count).to eq(0)</span>

       expected: 0
            got: 1

       (compared using ==)
     # ./lemon_spec.rb:131
</div>


<div class="presented-code presented-code--with-highlights">
  1) User#notifications ignores records of invalid kind
     Failure/Error: <span class="presented-code__highlight failure">expect(notifications.count).to eq(0)</span>

       <span class="presented-code__highlight">expected: 0</span>
            got: 1

       (compared using ==)
     # ./lemon_spec.rb:131
</div>


<div class="presented-code presented-code--with-highlights">
  1) User#notifications ignores records of invalid kind
     Failure/Error: <span class="presented-code__highlight failure">expect(notifications.count).to eq(0)</span>

       <span class="presented-code__highlight">expected: 0</span>
            <span class="presented-code__highlight failure">got: 1</span>

       (compared using ==)
     # ./lemon_spec.rb:131
</div>

Note:
Great, it means that this mutation is covered by our tests too. It is important to undo the mutation and see all tests pass:


<div class="presented-code presented-code--with-highlights">
<span class="presented-code__highlight mutant">(x[1][0] == "followed_notification" &&
  x[1][2] == id.to_s)</span> <span class="presented-code__highlight">true</span> ||
...
if kind == "followed_notification"
  {
    kind: kind,
    follower: User.find(values[1].to_i),
    user: User.find(values[2].to_i),
  }
elsif ...
</div>


<div class="presented-code presented-code--with-highlights">
<span class="presented-code__highlight">..</span>

Finished in 0.02343 seconds (files took 0.11584 seconds to load)
2 examples, <span class="presented-code__highlight">0 failures</span>
</div>


### Apply Mutational Testing repeatedly


<div class="presented-code presented-code--with-highlights">
...
<span class="presented-code__highlight">kind = values[0]</span>
if kind == "followed_notification"
  {
    kind: kind,
    follower: User.find(values[1].to_i),
    user: User.find(values[2].to_i),
  }
elsif ...
</div>


<div class="presented-code presented-code--with-highlights">
...
kind = <span class="presented-code__highlight mutant">values[1]</span> <span class="presented-code__highlight">values[0]</span>
if kind == "followed_notification"
  {
    kind: kind,
    follower: User.find(values[1].to_i),
    user: User.find(values[2].to_i),
  }
elsif ...
</div>


<div class="presented-code presented-code--with-highlights">
...
<span class="presented-code__highlight alternative">kind</span> = <span class="presented-code__highlight mutant">values[1]</span> <span class="presented-code__highlight">values[0]</span>
if <span class="presented-code__highlight alternative">kind</span> == "followed_notification"
  {
    kind: <span class="presented-code__highlight alternative">kind</span>,
    follower: User.find(values[1].to_i),
    user: User.find(values[2].to_i),
  }
elsif ...
</div>


<div class="presented-code presented-code--with-highlights">
$ rake test
<span class="presented-code__highlight">..</span>

Finished in 0.02343 seconds (files took 0.11584 seconds to load)
2 examples, <span class="presented-code__highlight">0 failures</span>
</div>

Note:
It seems we need another test:


<div class="presented-code presented-code--with-highlights">
it "<span class="presented-code__highlight">loads notifications with a correct kind</span>" do
end
</div>


<div class="presented-code presented-code--with-highlights">
it "loads notifications with a correct kind" do
  <span class="presented-code__highlight">user = User.new(email: "john@example.org",
                  password: "welcome")
  Database.insert("notifications",
    ["followed_notification", "986",
      user.id.to_s])</span>
end
</div>


<div class="presented-code presented-code--with-highlights">
it "loads notifications with a correct kind" do
  user = User.new(email: "john@example.org",
                  password: "welcome")
  Database.insert("notifications",
    ["followed_notification", "986",
      user.id.to_s])
  <span class="presented-code__highlight">notifications = user.notifications_isolated</span>
  expect(<span class="presented-code__highlight">notifications[0][:kind]</span>)
    .to eq("followed_notification")
end
</div>


<div class="presented-code presented-code--with-highlights">
it "loads notifications with a correct kind" do
  user = User.new(email: "john@example.org",
                  password: "welcome")
  Database.insert("notifications",
    ["followed_notification", "986",
      user.id.to_s])
  notifications = user.notifications_isolated
  expect(<span class="presented-code__highlight">notifications[0][:kind]</span>)
    .to eq(<span class="presented-code__highlight alternative">"followed_notification"</span>)
end
</div>


<div class="presented-code presented-code--with-highlights">
<span class="presented-code__highlight">..</span><span class="presented-code__highlight failure">F</span>

Finished in 0.14636 seconds (files took 0.12127 seconds to load)
3 examples, <span class="presented-code__highlight failure">1 failure</failure>
</div>


<div class="presented-code presented-code--with-highlights">
1) User#notifications loads followed notifications with correct kind
   Failure/Error: <span class="presented-code__highlight failure">expect(notifications[0][:kind]).to eq("followed_notification")</span>

   NoMethodError:
     undefined method `[]' for nil:NilClass
   # ./lemon_spec.rb:72
</div>


<div class="presented-code presented-code--with-highlights">
1) User#notifications loads followed notifications with correct kind
   Failure/Error: <span class="presented-code__highlight failure">expect(notifications[0][:kind]).to eq("followed_notification")</span>

   <span class="presented-code__highlight failure">NoMethodError:
     undefined method `[]' for nil:NilClass</span>
   # ./lemon_spec.rb:72
</div>


<div class="presented-code presented-code--with-highlights">
1) User#notifications loads followed notifications with correct kind
   Failure/Error: expect(notifications[0][:kind]).to eq("followed_notification")

   NoMethodError:
     undefined method `[]' for nil:NilClass
   # <span class="presented-code__highlight">./lemon_spec.rb:72</span>
</div>

Note:
And this failure is pointing to the code in the test:


<div class="presented-code presented-code--with-highlights">
# <span class="presented-code__highlight">./lemon_spec.rb:72</span>
notifications[0][:kind]
</div>


<div class="presented-code presented-code--with-highlights">
# ./lemon_spec.rb:72
<span class="presented-code__highlight">notifications[0]</span>[:kind]
<span class="presented-code__highlight alternative">^      nil     ^</span>[:kind]
</div>


<div class="presented-code presented-code--with-highlights">
# ./lemon_spec.rb:72
notifications[0]<span class="presented-code__highlight">[:kind]</span>
^      nil     ^<span class="presented-code__highlight alternative">[:kind]</span>
</div>


<div class="presented-code presented-code--with-highlights">
# ./lemon_spec.rb:72
notifications[0]<span class="presented-code__highlight">[:kind]</span>
^      nil     ^<span class="presented-code__highlight alternative">[:kind]</span> => <span class="presented-code__highlight failure">NoMethodError</span>
</div>

Note:
We made slightly wrong assumption about what will happen after the mutation and the failing test has proven us wrong. We had to investigate what really has happened and therefore we have deepened our understanding of this knowledge in the code.


<div class="presented-code presented-code--with-highlights">
...
kind = <span class="presented-code__highlight">values[1]</span>
if kind == "followed_notification"
  {
    kind: kind,
    follower: User.find(values[1].to_i),
    user: User.find(values[2].to_i),
  }
elsif ...
</div>


<div class="presented-code presented-code--with-highlights">
...
kind = <span class="presented-code__highlight mutant">values[0]</span> <span class="presented-code__highlight">values[1]</span>
if kind == "followed_notification"
  {
    kind: kind,
    follower: User.find(values[1].to_i),
    user: User.find(values[2].to_i),
  }
elsif ...
</div>


<div class="presented-code presented-code--with-highlights">
<span class="presented-code__highlight">...</span>

Finished in 0.14636 seconds (files took 0.12127 seconds to load)
3 examples, <span class="presented-code__highlight">0 failures</span>
</div>


### Continue applying mutational testing

(until there is enough confidence)


### Go back to step 2 and repeat

Choose new group of bits of knowledge responsible for other behaviors of the code.

And repeat.

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
