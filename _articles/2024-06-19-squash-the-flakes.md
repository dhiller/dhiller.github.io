---
anchor: stackconf_eu_2024
title: Squash the flakes! - how to minimize the impact of flaky tests
location: stackconf.eu, Berlin, Germany
---


squash the flakes!
how to minimize the impact of flaky tests

Daniel Hiller, Red Hat

Flakes aka tests that don’t behave deterministically, i.e. they fail sometimes and pass sometimes, are an ever recurring problem in software development. This is especially the sad reality when running e2e tests where a lot of components are involved. There are various reasons why a test can be flaky, however the impact can be as fatal as CI being loaded beyond capacity causing overly long feedback cycles or even users losing trust in CI itself.

For the KubeVirt project we want to remove flakes as fast as possible to minimize the number of retests required. This leads to shorter time to merge, reduces CI user frustration, improves trust in CI, while at the same time it decreases the overall load for the CI system.

We start by generating a report of tests that have failed at least once inside a merged PR, meaning that in the end all tests have succeeded, thus flaky tests have been run inside CI. We then look at the report to separate flakes from real issues and forward the flakes to dev teams.

As a result retest numbers have gone down significantly over the last year.

After attending the session the user will have an idea of what our flake process is, how we exercise it and what the actual outcomes are.
