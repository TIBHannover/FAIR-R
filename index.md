---
layout: lesson
root: .
---

The [FAIR principles](https://blogs.tib.eu/wp/tib/2017/09/12/the-fair-data-principles-for-research-data/)
are not exactly framed around software source code, so we will be interpreting some aspects into them.

Our real goal isn't to teach you R,
but a few good practices for any kind of programming: using functions, 
as well as documenting, packaging and testing them.

We are basing this lesson on [swcarpentry/r-novice-inflammation](https://swcarpentry.github.io/r-novice-inflammation/)
which is about a fictional daily inflammation study in patients using data in the
[comma-separated values]({{ page.root }}/reference/#comma-separated-values-csv) (CSV) format.

We want to:

*   create some functions that help us analyse this data set (**a**cessible)
*   document the purpose and usage of these functions (**a**cessible)
*   package them to support sharing and publishing the code (**i**nteroperable & **r**eusable)
*   write automatic tests to check the code's correctness

> ## Prerequisites
>
> Learners need to understand the concepts of files and directories
> (including the working directory).
> We recommend RStudio to teach and follow this lesson, because some GUI elements will be used.
{: .prereq}
