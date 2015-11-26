---
layout: post
title: "Ruby ENV variable Basics"
excerpt: "Ruby interpreter uses the following environment variables to control its behavior. The ENV object contains a list of all the current environment variables set. In this post we will explore how ENV variable works as well in which case it will not work"
date: 2015-11-24
status: publish
comments: true
share: true
categories:
- Ruby
tags:
- "Rails, Variables, Environments"
<!-- image:
  feature: ruby.jpg -->
---


###Ruby Environment Variable Basics

How we can effectively maintain our Rails environment with with many case as Production, Development and Staging?? if you understand the environement variable its as simple as it is to maintain different environments.

We would have used environment variable few years back occasionally, I hardly experienced environment variable. But nowdays on Infrastructure side we are trying to achieve everything automate and trying to make our deployment very simple.

In this post we will try to explore how environment variable works and DON'T work.