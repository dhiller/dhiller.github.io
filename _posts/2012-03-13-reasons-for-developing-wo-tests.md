---
layout: post
title: Reasons For Developing Without Tests
---

I've come to write this short article because I had some discussion with a developer about why he does not test. This discussion made me remember that many teams either don't do testing, neither on unit nor on integration level, or do it very sparsely. So I've come to write down a list of reasons that I've encountered and some questions that anyone hiding behind those reasons should have answers for.

Before you go right on to flaming against automated tests, please bear this in mind:

A script or program
works in no more time than required
when repeated does exactly the same
A human
may be distracted
interprets, thus may not follow the test script exactly
takes shortcuts because he or she is bored of repetitive tasks

So here they are:

*I don't have time for this*

But you take the time for testing every change you make by hand each time, via the debugger or through the user interface? Or you don't and hope it will work? You want to continue doing a job manually that could be automated?
Don't you have some more important work to do?

*I don't know the testing framework/tools*

Learn them. The basic ramp up time for creating a unit test is very small. Every major IDE or build tool has support for testing.

Later on, when you start to integrate your tests into the build process, you will find out that Ant and maven both support JUnit out of the box.

*I don't know if the boss will allow it*

Ask him. Maybe he's glad that you asked and takes this as a sign of you taking responsibility for what you do.

*I know the boss won't allow it*

So you get paid to do a job that a script could also do, in less time and with fewer errors. Do you think he will like that when he discovers that?

*I fear it will take too long*

Try it in small steps. Consider that if you do nothing about it, it will take much longer in the long term.

*I don't know where to start*

Just start where you are, right now. The worst thing you can do is not do it, because it won't get better.

You will see, it's not as hard as you thought.

Are there any other reasons you know? Please provide them, I will see if I find an answer that convinces you.
