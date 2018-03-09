---
layout: post
title: Rules for test driven bug fixing
summary: Rules for test driven bug fixing
tags: [tdd,bugfix]
---

Just a quick note to myself for how to do bug fixing:

1. **Write an integration test that reproduces the bug**  
This should create a test failure which will guard against regressions later on
1. **Write integration tests for the other cases**  
You should write at least one test. This should prove that it works the other way so you'll feel safe you don't break behavior later on
1. **Track down the problem to unit level**  
Debug to the lowest level where you can find the real cause of the problem
1. **Write a unit test that reproduces the bug**  
This should create a test failure which will guard against regressions later on
1. **Write unit tests for the other cases**  
You should write at least one test. There may be a chance that other corner cases have slipped as well, so examine closely. Keep in mind that unit tests are cheap!
1. **Now, fix the bug**  
Change it so all unit tests are green
1. **Clean up**  
Tidy up, don't leave a mess.  
Remember: Someone certainly will have to come back here. If that one is you, you will spend much more time, cursing the one who left that mess than if you do it right now!
1. **Run the unit tests again**  
Verify your clean up didn't break anything
1. **Run the integration tests**  
These should be green by now
1. **Integrate changes**
1. **Create release**
