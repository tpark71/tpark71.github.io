---
layout: post
title: "[Angular Concept 02] Angular Components"
subtitle: "Nothing to see here! This is for testing purposes!"
date: 2021-07-27
background: '/img/posts/03.jpg'
categories:
- en
- angular
lang:
- English
- Korean
---

## Components

Components are the combination of template view (html and css) and business logic (TypeScript). Each component is responsible of one section of the screen.

For example, if a component is a loginComponent, it will be responsible of displaying login page template and logic for login page.

## Benefits
1. Easy to reuse the style and logic.
2. Split each part of the screen
3. Easy to integrate
4. Easy to update and add components

## Code Partice of Creating Components

First, we will make new project named componentDemo.
~~~~
$ ng new componentDemo
~~~~

Now go inside the app directory and create new component named my-first-component
~~~~
$ cd ./src/app
$ ng g c my-first-component
~~~~