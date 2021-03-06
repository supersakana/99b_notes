# 99 Bottles OOP Notes

## Chapter 1 | Rediscovering Simplicity

Explores how to decide if code is "good enough", reveals metrics

### 1.1 Simplifying Code

- There is a spectrum between concrete and abstract, the best solutions lie somewhere in the middle

- "methods" are defined on an object and contains behavior. "message" is sent by an object to invoke behavior (#song method sends the #verses message to the reciver)

- **Incomprehensibly Concise** : Inconsistant and duplicative, conditionals are inconsistant (ternary verses if/else). Too abstract. Hard to identify verse variants, how they're alike/different, and order.

- **Speculatively General** : Use of lambdas can easily be eliminated to solve this. As this code is more complex than it should be, it is easier to identify verse variants, how they're alike/different, etc... but too much unecessary code. Many levels of indirection. It's not the verse templates that are the problem, its the way of choosing the template.

- **Concretely Abstract** : Way too many small methods, hard to tell the verse variants, alikeness/differences. One simple change could change the rest of the code. Demonstrates overusing the DRY principle. The name of the methods are also not abstract enough (what if the developer wants to change the beverage?)

- **Shameless Green** : Simple and easy to read. Clearly 4 variants, easy to discern which ones are different/alike. Easy to write, understand. NOTE when you dry our code (writting methods to name a bit of code), you add levels of indirection that increase abstraction.

### 1.2 Judging Code

- To evaluate code base on fact, we use metrics. Metrics are like crowd sourced opinions about the quality of code.

- **Source lines of Code** : Measures the total effort needed to develop a method/project/softwear.

- **Cyclomatic Complexity** : Test how difficult it would be to test/maintain. Counts the number of unique execution paths.

- **Assignments, Branches, and Conditions (ABC)** : Code assigns values to vars and sends messages. These things add up and can be difficult to understand. Assignments is count of variable assignments. Branches counts function calls or message sends. Conditions counts conditional logic. Flog is used to measure the ABC metric.

## Chapter 2 | Test Driving Shameless Green

Primer for TDD which was used to fund Shameless Green

### 2.1 Understanding Testing

- The practice of writing tests before writing code become known as test driven development (TDD)

  1. Write a failing test
  2. Write the code to make the test pass
  3. Each time your test returns green, refactor

- Incrementally creating tests can drive the development of code to create simple solutions.

### 2.2 Writing the First Test

- While it is important to consider the problem and sketch a plan before writting the test, dont overthink it

- The purpose of some of your tests might be to prove bad ideas.

- The first test can look simple, expecting a verse output given the correct arguments. There are three steps to creating this type of test...

  1. **Setup** : create the specific environment required for the test.
  2. **Do** perform the action to be tested
  3. **Verify** Confirm the result is as expected.

- Test each time you create the method (Between defining the method and having an actual wokring solution.

- While it may seem pointless to write a temporary piece of code, this is the essence of TDD

- While writting tests, you keep your eye on the big picture, while writing code, you pretend to know nothing other than what the requirements specified by the tests at hand.

### 2.3 Removing Duplication

- When adding another conditional, the cyclomatic complexity increases(another execution branch exists).

### 2.4 Understanding Transformations

- **Transformations** are simple operations that change the behavior of code

- Interpolating a variable into a string is simpler than adding a new conditional

- Notice how verses 99 through 3 are the same but differ in numbers. Unlike verses 2, 1, 0 which all require different outputs. Becuase they output differently, these paths need to be tested (each is unique)

### 2.5 Tolerating Duplication

- Adding a new test that expects 2 to output '1 bottle` instead of '1 bottles` will fail with the current interpolated method. 2's output is unique

- There are two different ways to pass this test. Add a starck conditional or use the value number is some way within it (embeding interpolated logic into the verse string).

- The metrics cast a vote to the interpolated conditional but in this case, the metrics are misleading. As theinterpolated solution is better, it's harder to understand and clearly see the verse differences and variants compared to adding another conditional.

- In a situation discerning whether to DRY code, ask these three questions...
  1. Does the change I'm implementing make the code harder to understand?
  2. What is the future cost of doing nothing now?
  3. When whill the future arrive, how soon will I get more information?

### 2.6 Hewing to the Plan

- Now that we've decided to branch into a different conditional with 2, we will do the same with 1 and convert our if/else into a case statement.

- The use of if/elsif implies that each condition varies in a meaningful way. case implies that ever condition checks for equality against an explicit value.

- Just like how we added 1 and 2 unique verses, we do the same with 0. The code passes all tests for each verse and is easy to understand because there are not many levels of indirection.

### 2.7 Exposing Responsibilities

- The plan for verses(a, b) is to take two arguments. These are numbers that specify the range of verses for which the method should generate lyrics.

- What actual arguments make most sense to test in verses?

- The responsibility of verses is to accepts a start/end number and return the correct verse template according to each number

- Kent Beck describes different ways to make tests pass. Three of his "Green Bar Patterns" are:

  1. **Fake it ("Til you make it")** : Having the code pass the tests even though you know the solution is incomplete

  2. **Obvious Implementation** : implementing a solution that is obvious (duh), if you know the solution, no need to fake it, just do it. _CAUTION_ if your obvious solution is incorect and you skip the incremental steps, it will cause pain.

  3. **Triangulate** : Described as conservatively driving abstraction with tests. This requires writting several tests at once (Meaning multiple simulations broken tests).

### 2.8 Choosing Names

- `song` is a method that calls verses between 99 and 0... but why would we need this method if the user can just call `verses(99, 0)`?

- As it is true that `verses(99, 0)` and `song` return the same output, _they differ widely in the amount of knowledge they require from the sender_

- Knowledge that one object has about another creates a dependency. Dependencies tie objects together, exacerbating the cost of change.

- The goal for a messege sender is to incur a limited number of dependencies and the method providers obligation is to inflict few.

### 2.9 Revealing Intentions

- " The distinction between intentiona nd implementation allows you to understand a computation first in essence and later, if necessary, in detail." - Kent Beck

- In the shameless green solution, `song` is the intention and `verses(99, 0)` is the implementation. There is a difference between wanting lyrics for a range of verses and wanting the lyrics for the entire song.

### 2.10 Writting Cost-Effective Tests

- TDD promises straightforward, bug-free software that can be confidently and easily changed.

- There can be a great deal of pain that originates from tieing test too close to code.

- The first step in learning the art of testing is to understand how to write tests that confirm what your code does without any knowledge of how your code does it.

### 2.11 Avoiding the Echo-Chamber

- The output of `song` is a string of one hundred very similar verses.

- Asserting that `song` returns the expected lyrics is very different from asserting that `song` returns the same thing as `verses`.

- Not only does tesing `song` to equal `verses(99, 0)` tightly couple to the Bottles implementation, it does't even force you to write the right code.

- **The best way to test `song` is to test exactly what it is responsible for (returning a string of verses from 99 to 0).**

### 2.12 Considering Options

- If you find duplication distressing, here are the alternative choices (that are not as favorable as the bold point above)...

  1. Assert that the expected output matches that of some other method. (Makes dependencies which make the Bottles class more vulnerable to changes)

  2. Assert that the expected output matches a dynamically generated string. (Implementing logic to dynamically generate a lyric string means that a change to bottles might break the song test in an unexpected way.)

- Again, the best way to test `song` is to hard code the string. An output that is unambiguously stated where the test has no dependencies.

- Tests are not the place for abstractions. Tests are for concretions.

### 2.13 Summary

- Testing done well, speeds up development and lowers costs however, flawed tests can slow you down and cost money.

- **Get good at testing!**

## Chapter 3 | Unearthing Concepts

Introduces new requirement (six-pack), how to decide where to start when changing code. Explores Open/Closed, code smells, flocking.

### 3.1 Unearthing Concepts

- The arival of a new requirement tells you two things, onve very specific, the other more general.

- A new requirement tells you exactly how code should change. Code that never changes obviously does not need to be very changable, but the bare is rasied when a new requirement enters and the code must be changable then.

- Requirement: Add a feature that outputs 6-pack when

- Conditionals are the bane of OO, as it is ok for Shameless Green's solution, but to add a new branch would turn the case frorm 4 to 6.

### 3.2 Start with the Open/Closed Principle

- "Open" is short for "Open/Closed" which in turn is short for _"open for extension and closed for modification"_.

- The SOLID Design Principles are listed below...

  1. **Single Responsibility**: The method in a class should bd cohesive around a single purpose.

  2. **Open-Closed**: Objects should be open for extension but closed for modification (do not conflate the process of moving code around during refactoring).

  3. **Liskov Subsitution**: Subclasses should be substitutable for their superclasses.

  4. **Interface Segregation**: Objects should not be forced to depend on methods the do not use.

  5. **Dependency Inversion**: Depend on abstractions, not on concretions.

- SO, the current Bottles class ins _not_ open to the 6-packs requirement because adding new verse variants require editing the conditional.

- To make it open, we need to remove the easiet to fix/best understood code smell.

- If you can identify smells in code, you isolate flaws and correct them one by one

### 3.3 Recognizing Code Smells

- Code flaws are also recognized as 'code smell'

- The easiest way to uncover flaws in code is to list all the flaws you dislike

### 3.4 Identifying the Best Point of Attack

- The current implementation of Shameless Green is not "open" (open for extension, closed for modification) it's currently a case statement and we dont want to add another set of conditionals.

- If unclear on how to make code open, the way forward is to start removing code smells.

- Switch statements smell. And in the current case, it contains many duplicated strings. Duplication is the most straight forward smell so will be tackled first. The current task is to remove the duplication in hope and expectation that the resulting code will be more open to six pack.

### 3.5 Refactoring Systematically

- Refactoring is the process of changing softwear in such a way that does not alter the external behavior of the code, yet imporves the internal structure.

- Adding new requirements should be implemented in 2 steps. First rearrange exisitng code to be open to new requirements. Next write new code to meet requirement.

- NOTE: Safe refactoring relies upon tests.

- Test are the wall at your back that help with refactoring safely.

### 3.6 Following the Flocking Rules.

- The case statement currently has 4 branches that contain a verse template.

- Each variant is merely a verse in the song; in that sense they are all the same!

- Underlying each concrete variant is a generalized verse abstraction. If you could find this abstraction, you could use it to reduce the four branch case statement to a single line of code.

- Flocking Rules...

  1. Select the things that are most alike.

  2. Find the smallest difference between them.

  3. Make the simplest vhange that will remove that difference.

- Changes to code can be subdivided into four distinct steps...

  1. parse the new code

  2. parse and execute it

  3. parse, execute and use its result

  4. delete unused code

- As you're folowing thr flocking rules...

  1. For now, change only one line at a time

  2. Run the tests after every change.

  3. If the tests fail, undo and make a better change.

- Why is it called flocking? The result of a flock in nature is nothing more than a bunch of small decisions making up a larger impression.

### 3.7 Converging on Abstractions

- Differences hold the key to understanding. If two concrete examples represent the same abstraction and contain a difference, that difference must represent a smaller abstraction within the larger one. If you can name the difference, you've identified the smaller abstraction.

- It's commmon to find that hard problems are hard only because the easy ones have not been solved yet.

- Making a slew of simultaneous changes is not refactoring - it's rehacktoring. It would be much easier to make a series of tiny changes and run the tests after each. If the tests fail, you know the exact change that caused the failure.

- The flocking rules concentrate on turning difference into sameness, and thus are useful tools for unearthing abstractions.

## Chapter 4 | Practicing Horizontal Refactoring

Continues the step by step refactoring. uses flocking rules and stumbles across the need for Liskove Subsitution Principle

### 4.1 Replacing Differences with Sameness

- The refactoring rules start by choosing the cases that are most alike.

- Just as we created the `container` method for the verse 2 and else statment to be merged, we can do the same with the 1 verse. By making `container` interpolated on the top line of the verse.

### 4.2 Equivocating About Names

- We added a `pronoun` method that returns "it" or "one" depending on the number of bottles. The way we created it was almost identical to creating our `container` method. By identifying the abstraction between "one" and "it" (pronoun), then interpolating the method in our logic.

### 4.3 Deriving Names From Responsibilities

-

## Chapter 5

Inventories the existing code for smells, chooses the most prominent one, and uses it to trigger the creation of a new class.
