---
# Please do not edit this file directly; it is auto generated.
# Instead, please edit 05-testthat.md in _episodes_rmd/
title: "Unit-Testing And Test-driven Development"
teaching: 45
exercises: 15
questions:
- "What is the benefit of unit-testing my code?"
- "How do I create and run unit tests?"
- "Why would I change my code after I got it to run?"
objectives:
- "Formalise documented examples as tests."
- "Use testthat functions to create and run tests."
keypoints:
- "Changing code is not always necessary, but often useful."
- "Tests provide a safety net for changing code."
- "Practice more TDD at [exercism.io](http://exercism.io/languages/)"
source: Rmd
---



### Unit testing with the `testthat` package

Computer code evolves. Functions may need to be updated to new usage goals,
sped up when more data needs to be crunched, or cleaned up to make their code
more readable for collaborators, reviewers, etc. You probably know the saying
"Never change a running system!" We surely invested a lot of time already to make
sure our functions work fine now. It is natural to be cautious about changing
computer code and it rightly shouldn't be done on a whim.

However, as you have probably learned in the Git lesson, branching is one way to
try out code changes in a way that allows you to recover from mistakes, and only
merge successful changes.

Unit tests are another way to "span a safety net", and in R, the [`testthat` package][testthat]
is commonly used. Because adding tests requires an expansion of the folder and file
structure of an R package, we are going to use a helper package to do this for
us: [`usethis`][usethis].


~~~
install.packages(c("testthat", "usethis"))
# several can be combined, but only for installations
library("testthat")
library("usethis")
~~~
{: .language-r}

Afterwards, type:


~~~
use_test("center")
~~~
{: .language-r}

A new file should be created and the console should show:

~~~
✔ Adding 'testthat' to Suggests field in DESCRIPTION
✔ Creating 'tests/testthat/'
✔ Writing 'tests/testthat.R'
✔ Writing 'tests/testthat/test-center.R'
● Modify 'test-center.R'
~~~
{: .source}

The file is pre-filled with a little example. While
following the explanation of each part, please delete the contents of that example
test in order to prepare inserting our own. The string within `test_that("…", …)`
is an explanation of what this test actually tests. In case of our `center()`
function we expect that "centering works".

Next comes a specific `expect_`ation, usually calling a given function with
a defined set of arguments (inputs) to check whether that result is `_equal`
to a known return value (outputs).
In the function comments, we have already formalised a few of these as examples.
Copy-and-paste them into the test file, remove the `# should return [1]`
indicators of the expected results, and wrap the result values into a `c()` to
enable R to compare the expected values with the output of the two `center()` tests.


~~~
test_that("centering works", {
  expect_equal(center(c(1, 2, 3), 0), c(-1, 0, 1))
})
~~~
{: .language-r}

Don't worry that there is no output when you execute either one or both of the
`expect_equal()` statements, or  the whole `test_that()` block. In good UNIX
tradition, `testthat` will only bother you in case some expectations are _not_
met, i.e. one or more unit tests fail.

> ## Testing our rescaling functions
>
> Apply the above example of converting the examples into unit tests for the
> `rescale` function as well.
>
> > ## Solution
> > ~~~
> > test_that("rescaling works", {
> >   expect_equal(rescale(c(1, 2, 3)), c(0.0, 0.5, 1.0))
> >   expect_equal(rescale(c(1, 2, 3, 4, 5)), c(0.0, 0.25, 0.5, 0.75, 1.0))
> > })
> > ~~~
> > {: .r}
> {: .solution}
> 
> Think about the [DRY principle]({{ page.root }}/03-func-R/#composing-functions).
> Is it necessary to keep the `@examples` in the documentation when you are using
> them in the tests? Which factors would you consider in your decision?
>
> > ## Hint
> > 
> > Examples are a good starting point, but not every test case will be a useful
> > example, and vice versa. The examples help users to figure out how to apply
> > your functions. The test cases help the package developer(s) improve the code.
> > 
> {: .solution}
{: .challenge}

To conclude this section about creating unit tests, let's again commit our results,
for example as "Span safety net for TDD".


### Test-driven development (TDD)

Remember that we updated `rescale()` with lower and upper bounds and default
values at the end of [the functions episode][ep-func]? We had to manually test that change with a
new example back then. We were repeating ourselves a bit more often than necessary
back then, weren't we? Of course, there are ways to automate the testing of code
changes, which gives you quick feedback whether your changes worked, or broke
anything.

To do that, we use `testthat`'s `auto_test_package()`
in the console, or in RStudio's `Build` pane the `More > Test Package` menu option, 
and notice the hopefully all green and `OK` `Results`.

With this safety net enabled, we will first update `test-center.R`, and then
update the code in `center.R` with the `desired = 0` default argument. This strategy of (re)writing (new)
tests before (re)writing the code-to-be-tested is called
"[test-driven design/development][TDD]". It is intended to reduce confirmation
bias when coding. If one already worked hard to get the code to run, one may be
_biased to_ let the tests _confirm_ what the code currently _does_, instead of
challenging it with tests about what it _should_ do. The latter is similar to the
scientific method of trying to disprove hypotheses, so that the truth remains.

[TDD]: https://en.wikipedia.org/wiki/Test-driven_development

> ## Update `test-center.R` with the argument default from [`center2.R`][ep-func].
>
> Think about which parts of the test code you need to change, to "break" the 
> existing implementation (with only the `desired` argument, but no `= 0` default)
> but to get it working again once the default is added to the function's code.
> 
> > ## Solution
> > ~~~
> > test_that("centering works with and without default arguments", {
> >   expect_equal(center(c(1, 2, 3) # `, 0` has to be deleted
> >                                        ), c(-1, 0, 1))
> >   expect_equal(center(c(1, 2, 3), 1), c(0, 1, 2))
> > })
> > ~~~
> > {: .r}
> {: .solution}
{: .challenge}

Save the file and if `auto_test_package()` is still running, you should see one
`Failed` `Result`. If you run the `expect_that("…", …)` or `expect_equal(…)`
block interactively, an `Error` should appear. This exactly what we want to see
before working on the code. Why?

> ## Update `center.R` with the argument default from [`center2.R`][ep-func].
>
> Don't copy-paste the code from earlier! Try instead to rely on the safety net
> and update the function code interactively, saving every now and then to
> trigger `auto_test_package()`. Don't forget to update the roxygen comments 
> and to `roxygenise()` again.
> 
> > ## Solution
> > 
> > See the ["Defining Defaults" section in the functions episode]( {{ site.root }} /03-func-R/#defining-defaults).
> > 
> {: .solution}
{: .challenge}


> ## Also update both `rescale` files with the argument defaults example from [`rescale2`][ep-func].
>
> You can either wrap this example in its own `test_that()` with a fitting
> description, or replace one of the existing tests. Either way, the test 
> explanation(s) should afterwards be checked whether they still reflect the 
> test code.
> 
> > ## Solution
> > ~~~
> > test_that("rescaling works with non-default arguments", {
> >   expect_equal(rescale(c(1, 2, 3), 1, 2), c(1.0, 1.5, 2.0))
> > })
> > ~~~
> > {: .r}
> {: .solution}
{: .challenge}

> ## Update `rescale.R` with a lower and upper bound argument and default values
>
> Don't copy-paste the code from earlier! Try instead to rely on the safety net
> and update the function code interactively, saving every now and then to
> trigger `auto_test_package()`.
> 
> > ## Solution
> > See the ["Defining Defaults" section in the functions episode]( {{ site.root }} /03-func-R/#a-function-with-default-argument-values).
> {: .solution}
{: .challenge}

While `auto_test_package()` is still running, play around with the code a bit more:

- change the placement of `(` parentheses `)`
- remove the `return()` statement to find out how R can implicitly return
  the last calculated value within a function
- shorten the code to 2 lines, or even only 1
- rename arguments to something more self-explanatory (basic "[refactoring]")

Some of those changes will result in errors, some will make the code more readable,
some will make it faster or slower.
To stop `auto_test_package()` in the console, press <kdb>Esc</kdb> or the red `STOP` button.
With automatic testing we can more quickly reach a sensible balance between investing
our time and energy into testing code improvement, while recovering from mistakes.

[ep-func]: {{ page.root }}/03-func-R/
[refactoring]: https://en.wikipedia.org/wiki/Code_refactoring
[usethis]: https://usethis.r-lib.org/
[testthat]: https://testthat.r-lib.org/
