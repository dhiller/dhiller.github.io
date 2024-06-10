---
anchor: kubevirt_summit_2023
title: Squash the flakes! - How does the flake process work? What tools do we have? How do we minimize the impact?
location: KubeVirt Summit 2023, virtual event
view: https://youtu.be/eA81Q7evtMA
download: https://github.com/dhiller/presentations/tree/master/KubeVirt-Summit-2023
---


Squash the flakes! - How does the flake process work? What tools do we have? How do we minimize the impact?

Daniel Hiller, Red Hat

Flakes aka tests that don’t behave deterministically, i.e. they fail sometimes and pass sometimes, are an ever recurring problem in software development. This is especially the sad reality when running e2e tests where a lot of components are involved. There are various reasons to why a test can be flaky, however the impact can be as fatal as CI being loaded beyond capacity causing overly long feedback cycles or even users losing trust in CI itself.

We want to remove flakes as fast as possible to minimize number of retests required. This should lead to shorter time to merge, reduce CI user frustration, improve trust in CI, while at the same time decrease overall load for the CI system. We start by generating a report of tests that have failed at least once inside a merged PR, meaning that in the end all tests have succeeded, thus flaky tests have been run inside CI. We then look at the report to separate flakes from real issues and forward the flakes to dev teams. As a result retest numbers have gone down significantly over the last year.

After attending the session the user will have an idea of what our flake process is, how we exercise it and what the actual outcomes are.
