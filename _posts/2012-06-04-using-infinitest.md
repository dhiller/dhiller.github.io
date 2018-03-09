---
layout: post
title: Using Infinitest for real
summary: Using Infinitest for real
tags: [tdd,infinitest]
---

#### Making your Test driven development faster in real projects
When you are practicing TDD like I do you may blindly know your keyboard shortcut to start your test runs after you've changed something. Or do you still use your mouse? :-)

[Infinitest](http://infinitest.github.com/) is a great tool to get rid of those shortcuts. Whenever a change occurs in a class it runs the tests that accompany it. Well, that's already awesome but fasten your seatbelt: Infinitest also runs tests of classes that are affected by your change.

How they do this magic I haven't elaborated but I can assure you of the effect: When using Infinitest you will notice net effects of your changes much faster.

#### But...
I always loved Infinitest for it's incredibly short feedback loop, yet disabled it in my current maven multi module project for seemingly obvious reasons: "No, I ~~am too lazy~~ don't have time to figure out how to properly set it up" (That of course I would not confess to myself.)

This meant I didn't figure out how to configure it so it wouldn't always mess up my own dev environment or, being quite more of an impact, the integration test environment.

As you might guess I have successfully set up Infinitest for this monster of a project.

#### The project
The Project is a maven multi module project with a dozen modules and specifically a module containing **most** of the integration tests. It is completely stored in a remote git repository. I have set up a local virtual machine containing a database for testing and a local memcached. The project is configured via a set of -D properties that specify which of these to use during integration tests, so that these can be run locally or on the dedicated test instances running on the integration server during the Jenkins CI build. The CI build runs on every push to the server repository.

**And here comes the problem: Whenever a local integration test fails to run on the local database engine, but uses the integration instance instead, it may interfere with a running CI build.**

For a thing like Infinitest, which runs a couple of tests on each change, the test runs (including the integration tests) may happen quite often. So I have to make sure Infinitest propagates the properties of i.e. where to find the database to the test cases.

The easiest solution (read: solution of the lazy one) is of course: switch it off.

**Beware:** configuring Infinitest for such a project requires a bit of work if you don't have it enabled directly after you started. But it can be done and is well worth the effort.

There are a couple of constraints you have to consider when using Infinitest:

When you are working in Eclipse you can not switch it on or off on a per project basis
Settings are configured using files
You have to have a set of files for each project
Test cases may be filtered by
Using regular expressions and/or
Using TestNG groups
Arguments are passed as -D properties in a files
See the [Infinitest user guide](http://infinitest.github.com/user_guide.html) for more information.

#### Preparations
The best way is: Have your integration tests in a single module. If you're using TestNG, use a group attribute in the test annotation. If you can't do that, move them into a separate package. If you can't do that and are not using TestNG, use a naming convention for your integration tests so you can easily filter them out. If you can't do that, you're screwed (well, I'm open for suggestions :-).

#### Setting it up
At first create a file called `infinitest.args` that contains all properties that have to be propagated to the test cases. Then create a file `infinitest.filters` that contains all filters for your integration tests (i.e. TestNG group or package regular expression or regular expression for the test case name. These files I'll call the **master files** for reference later on.

Put these two files into the top level maven project folder. Then make soft links in each module to these files. You can't do that? Well, you are on your own now ;-)

**Hint:** A very nice property of git is that it stores symbolic links in the repository. So you will share the links with the other project participants.

Don't put your master files into version control, or others will share the settings, which you probably don't want. But maybe you don't care whether they're using your database, do you? So you just put them into the `.gitignore` file.

Remember to not ignore the links in the module directories, of course.

So you should be fine right now. Just check the "Continuously test" option and enjoy the short feedback cycle from now on :-)

Hint: A slight modification which you may consider: For each project where you don't want to execute any tests just put an `infinitest.filters` with content `.*` instead of the symbolic link into the module directory.

So I hope this short guide helps you to get faster in test driven development and getting rid of manually executing tests via the mouse or a keyboard shortcut or whatever else you use. Please let me know if you have any corrections or suggestions.

My final notice: if you want to get really fast, just start using Infinitest whenever you start a new project. It's worth it!

Thanks for reading, anyway :-)
