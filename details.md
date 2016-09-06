This talk introduces the technique Test-Driven Exploration, that allows one to tackle the problem of most of the legacy code-bases - untested code.

Inherently, untested code, not being written in a Test-First or Test-Driven fashion, will be hard to test and, most probably, hard to understand. The readability and clarity of the untested code degrade over time because, the only effective tool to clean the code up - the refactoring - is too dangerous to use in the absence of the good test suite. This results in a very high probability of introducing bugs into this code while making a change in it. Thus, after a few mistakes, most engineers learn to avoid changes in old untested modules of the application. When these changes have to be done, they can only pray that QA or users will not find any new bugs.

This talk defines:

- "Legacy Code" as old untested code, that is unreadable and hard to understand.
- "Knowledge in Code" as smallest possible pieces of functionality, that represent a business rule or infrastructure logic to support business rules.
- "Test for a Test" as a simple change of the knowledge, that should alter (break) the behavior of the module and result in a test failure, if and only if the knowledge is tested well.
- A way to test-drive the test suite for the untested module that is a subject for a pending change - Test-Driven Exploration. Also, this technique deepens the understanding of the code in question, allows to find bugs and unexpected features.

Also, this talk includes a step-by-step example of the application of Test-Driven Exploration technique in Ruby.

## Pitch

This technique severely reduces the amount of bugs introduced by changes to the Legacy Code. I think this is very helpful for any engineer working at late/post-startup or grown-up company, that has legacy systems running, that have big chunks of untested code.

I am doing Test-Driven Development for about 4 years and I was always wondering if it is possible to apply same or similar concepts to test-drive tests from the production code.

This technique is my attempt to solidify observations of how senior engineers deal with legacy code in a proper technique. I and my team use this technique every time when we are presented with untested code, that we need to change, for about half a year. The results are stumbling: nearly no bugs when changing untested code (which becomes well-tested after applying the technique) and generally higher morale and confidence in what we are doing.
