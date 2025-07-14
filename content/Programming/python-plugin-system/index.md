+++
title = "Python Plugin System with Entrypoints"
slug = "python-plugin-system"
date = 2025-07-14
+++



## Intro
I am working on vllm HW plugin for LPU(NPU of HyperAccel). During studying vllm, I was curious how python plugin system works.

In this post, I will go through how python plugin system works with some simple examples.

## Plugin System
Let's say you want to build a opensource app that handles `Vehicle` Class. You implemented `Car`, `Drone` class in your application.

At this point, other person wanted to add other vehicles(`Boat`) to your app. What should he/she do?

There are two possible ways:
1. Implement a boat in the opensource app, and make a PR(Pull Request) in Github.
2. If opensource app supports plugin, then just implement the `Boat` in plugin module.

First method seems fair, but it costs a lot for maintainer and person who wants to add a boat.

For second method, if opensource app is well structured to accept plugins, then maintainers won't have to take care about the plugin. And also person who wants to add a boat can take minimum effort to add `Boat` class.

Python already have a feature called `Entry-points` for python plugin ecosystem. Please checkout the example code.

## Example code
Checkout the example code base:

[python-entrypoints-plugin-example](https://github.com/jinho-choi123/python-entrypoints-plugin-example)
