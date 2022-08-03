---
tags:[presentation, tdd]

---

---
<iframe width="560" height="315" src="https://www.youtube.com/embed/EZ05e7EMOLM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

**The Problem**

- Some good developers don't want to write tests
- We often write more test code than implementation code
	- Sometimes 2-3 times as much
- We were slower when we wrote tests to deliver
- We ended up breaking a lot of tests when we attempted to make changes
	- Often times these were the tests with a lot of mocks involved
	- The tests became an obstacle to change when we changed implementation details
	- Surely [[Martin Fowler]] and [[Kent Beck]] were expecting refactors to occur without breaking tests
- [[Fred George]] "If [[Extreme Programming]] is good then lets turn the dial up to 11" 
	- [[Programmer Anarchy]] fire anyone who is not a developer, let developers talk to customers, and write very small pieces of code called [[Microservice]]s that only have about 100 lines of code without tests because you're bound to get it right, and if it breaks we'll just throw it away
	- [[Test Driven Development|TDD]] is something you do when you're scaling
- Why is the [[Duct Tape Programmer]] winning?
	- He get's solutions out the door very fast
	- His engineering is rubbish but he gets all the credit from product
	- It's hard to maintain his code
- When we return to our tests - because they are broken it is often difficult to understand their intent
	- Default to 'the best thing to do is just delete the test' if it's broken and we don't know why
- We have large ATDD suites written in Fit or Gherkin SpecFlow etc.
	- They spend much of their life red
	- Customer never looked at it because they don't know, Is it red because it hasn't been implemented yet or is it red because it's broken?
	- Our ATDD suites are slow to run and increase the cost of the check-in dance
	- Broke your code flow process because of how long it took to take
	- Developers start ignoring red results
	- Developers don't want to write them
	- Because they can't see the value return on their effort

[[David Heinemeier Hansson]] wrote [TDD is dead. Long live testing.](https://dhh.dk/2014/tdd-is-dead-long-live-testing.html)

His complaint is that I'm being forced by people

[[Active Record Pattern]] people tell me this is no good, because I can't cleanly separate their domain object models from their testing framework code. I used to write my unit tests and they'd talk to the database and I was told that was wrong as well.

He used to like [[Test Driven Development|TDD]] when it first came out but now I'm constantly being told I'm doing it wrong so I'm giving up on [[Test Driven Development|TDD]].

[[Kent Beck]] responded with [RIP TDD](https://www.facebook.com/notes/1063422864115918/)

[[Martin Fowler]] mediated a discussion between [[David Heinemeier Hansson]] and [[Kent Beck]] in [Is TDD Dead?](https://martinfowler.com/articles/is-tdd-dead/)

**TDD Rebooted**

TDD has answers to all the problems we have with TDD, there are patterns and practices we've put on TDD that have caused these issues.

Recommend reading [Test-Driven Development By Example](https://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530) by [[Kent Beck]]

*Avoid testing implementation details, test behaviors*

Many practice [[Test Driven Development|TDD]] by using 'adding a new method to a class' as the trigger to write a test. *This is wrong*

The trigger for creating a new test is you have a requirement you want to implement.

Test the public API
	- The contract your software has with the world
	- How you implement a requirement is unstable, but the requirement is generally stable
	- Not an HTTP API necessarily; but the exports from a module

Use a Given When Then model

The [[System Under Test]] is not a class, it's the exports from a module - it's façade

You should avoid [[Overspecified Test]]s which is when you know all about the internal implementations of a particular module through mocks etc.

Unit here really means a module - the old target of unit testing, not a class

A class may be the façade, but many classes are implementation details of the module.

*Do not write tests for implementation details - these change!*
*Write tests only against the stable contract of the API*
*The key is the refactoring step*

*Only writing tests to cover the implementation details when you need to better understand the refactoring of the simple implementation we start with **then delete these tests***

[[Dan North]] wrote a post about [[Behavior Driven Development]], I often find when I go to implement [[Test Driven Development|TDD]] in places people are often confused about it. The word Test throws them, if I change that word to Behavior, people implement them correctly.

[[Kent Beck]], [[Test Driven Development By Example]]
>What behavior will we need to produce the revised report? Put another way, what set of tests, when passed, will demonstrate the presence of code we are confident will compute the report correctly?
>We need to be able to add amounts in two difference currencies and convert the result given a set of exchange rates.
>We need to be able to multiple an amount (price per share) by a number (number of shares) and receive an amount

He is not talking about a method on a class, he's talking about a behavior in a system. *This is what your test should be looking at*

> When we write a test, we imagine the perfect interface for our operation. We are telling ourselves a story about how the operation will look from the outside. Our story won't always come true, but it's better to start from the best-possible application program interface ([[API]]) and work backward than to make things complicated, ugly, and "realistic" from the get-go.

For [[Kent Beck]] a [[Unit Test]] is a test that 'runs in isolation' from other tests. *The [[Unit of Isolation]] is the test, NOT the class under test* ^unit-of-isolation

This means that [[Mocking]] is not the way to [[Isolation]] as that's about isolating the classes functionality under test.

It is **NOT** to be confused with the classical [[Unit Test]] definition of targeting a module and an [[Integration Test]] being a composition of modules working together.

We avoid file system, database, simply because these 'shared fixture' elements prevent us running in isolation from other tests, or cause our tests to be slow.

But touching is fine in a developer test - with the shared fixture and slow constraint.

[[Kent Beck]]'s consideration is that if we touch a shared database, then we potentially impact the [[#^unit-of-isolation|isolation of our tests]]

This means we might have an in memory database that we use for our unit tests that we setup and tear down per test as opposed to a shared one

We should isolate for concerns of speed.

Focusing on methods creates tests that are hard to maintain. We don't capture the behavior we want to preserve.
We can't refactor easily because implementation details are exposed to tests (if we're focusing on the methods for tests)

Mocks means we likely know too much about our implementation details

**Red-Green-Refactor**

1. **Red** write a little test that doesn't work, and perhaps doesn't even compile at first
2. **Green** Make the test work quickly, committing whatever sins necessary in the process
3. **Refactor** Eliminate all of the duplication created in merely getting the test to work ^refactor-step

When you are trying to move from red to green, you're basically trying to solve the problem. You want to solve it as fast as possible to satisfy the behavior. *This should dominate everything else. Write sinful code. Emulate the [[Duct Tape Programmer]]*

In this moment, speed trumps design.

> Now I'm worried. I've given you a license to abandon all the principles of good design. Off you go to your teams -- "Kent says all that design stuff doesn't matter." Halt. The cycle is not complete. a four-legged Aeron chair falls over. The first four steps of the cycle won't work without the fifth. **Good design at good times. Make it run, make it right.**  [[Kent Beck]] [[Test Driven Development By Example]]

**Clean Code When?**

The [[#^refactor-step|refactoring]] Step is when we produce clean code.

> It's when you remove duplication [[Kent Beck]]

> It's when you sanitize the [[Code Smell]]s [[Martin Fowler]]

> It's when you apply [[Design Pattern]]s [[Joshua Kerievsky]]

When you [[Refactor]], you do not write any new tests. Refactoring should not impact the behavior that the tests are trying to maintain.

You are not introducing public classes. You are not expanding the API of the module.

> [[Dependency]] is the key problem in software development at all scales. [[Kent Beck]], [[Test Driven Development By Example]]

[[Coupling]] is a worse enemy for you than [[DRY]]

**Refactoring to Patterns**

This allows us to refactor implementations without changing tests, don't bake implementation details into tests!

Don't test internals. Don't make everything public in order to test it.

Do not make your internals visible to your tests!

[[Refactoring Improving the Design of Existing Code]] from [[Martin Fowler]]

> Refactoring is the process of changing a software system in such a way that it does not alter the external behavior of the code yet improves its internal structure. It is a disciplined way to clean up code that minimizes the chances of introducing bugs. In essence when you refactor you are improving the design of the code after it has been written. [[Martin Fowler]] in [[Refactoring Improving the Design of Existing Code]]

[[Refactoring to Patterns]] from [[Joshua Kerievsky]] suggests we don't try to implement patterns in the [[System Under Test|SUT]]. We implement patterns as improvements to the code in the [[refactoring]] step.



**Ports and Adapters**
What you want is the [[Testing Pyramid]]
[[Ice-Cream Cone Anti-Pattern]]
![[Pasted image 20220712180629 1.png]]

[[Hexagonal Architecture]], [[Clean Architecture]], [[Onion Architecture]] are all effectively [[Ports and Adapters]] architecture.

![[Pasted image 20220712180940.png]]

The Port is a use-case boundary, ands thus a behavior boundary
We unit test at the port, driving the application domain
Integration Tests are used to make sure that you're able to communicate correctly with an Adapter
Don't test Adapters they're usually 3rd party code

**Gears**
 
It's ok to write tests if you're using them to help guide you, but afterwards delete them, as they'll be a burden to the next developer. You should only leave tests that are focused on behaviors.

**ATDD**

> There is also a social problem with application test-driven development (ATDD). Writing tests is a new responsibility for users (by which I really mean a team that includes users), and that responsibility comes at a new place in the development cycle -- namely, before implementation begins. [[Kent Beck]] [[Test Driven Development By Example]]

> Another aspect of ATDD is the length of the cycle between test and feedback. If a customer wrote a test and ten days later it finally worked, you would be staring at a red bar most of the time. I think.
> I would still want to do programmer-level TDD, so that I got immediate green bars and I simplified the internal design

> One of the weaknesses of TDD as originally described is that can devolve into a programmer's technique used to meet a programmer's needs. Some programmers take a broader view of TDD, facilely shifting between levels of abstraction for their tests. However, with ATDD there is no ambiguity, this is a technique for enhancing communication with people for whom programming languages are foreign. The quality of our relationships, and the communication that underlies those relationships, encourages effective software development. ATDD can be used to take a step in the direction of clearer communication. [[Kent Beck]] forward on [[ATDD By Example]]

There are 2 problems, customers don't care, and its more overhead for the developer.

Instead the programmers should use these examples to inform their test-driven development. [[James Shore]] [The Problems with Acceptance Testing](https://www.jamesshore.com/v2/blog/2010/the-problems-with-acceptance-testing)

We should drop ATDD and instead retool our TDD practices towards behaviors.

**Behavior Driven Development**

This is basically what TDD should be.

**Mocks**

These should be used when resources are expensive to create. we should **not** be using these to confirm implementation details.

Mocks will couple your tests to your software **this is rarely worth it**

Leads to over-complexity through use if [[IoC Container]]s to resolve complex dependency chains, when just new'ing up the dependency in implementation code would have been better. Use DI, when the patterns refactored to demand it, ie. Open-Closed Principle

[[IoC Container]]s are overused
