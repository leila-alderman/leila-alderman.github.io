---
layout: post
title: Arrays vs. Hashes
categories:
 - Ruby
tags:
 - Ruby
 - basics
---

[Hashes](https://ruby-doc.org/core-2.6/Hash.html) and [arrays](https://ruby-doc.org/core-2.6/Array.html) both keep track of collections of information, and they can use many of the same methods. So what are the differences between these two? Let's explore how arrays and hashes differ through a lunch time parable!

*Note:* I originally wrote this post as part of the new Ruby lessons for [The Odin Project](https://www.theodinproject.com), and I'm simply reprinting it here.

## Lunch Time!

You're sitting in your cubicle, diligently working away (because you would never dream of slacking off at work, right?), when lunch time rolls around. You need to grab a bite to eat, but how are you going to go about requesting food? For the purposes of this parable, you have two options: a vending machine or a nice restaurant. 

## The Vending Machine Array

If you were to go to the vending machine, you would see nice, orderly rows of food where each option is labeled with a number. These labels are the indices of the vending machine array. It's important to note that the indices are not interchangable: "12" will always come before "13" and after "11". You request your food by using an index to tell the vending machine what you want. It understands the index and returns whatever lives in that spot. Mmmmm, nothing like a lunch of Flamin' Hot Cheetos and Diet Coke! You are a programmer, after all.

## The Menu Hash

Your other option is to sit yourself down at a table covered with a nice white tablecloth, where a pleasant waiter will see to your every need. The first thing they will do is bring you a menu, which for those of you that have only been eating out of vending machines so far in your life, lists out all of your dining options labeled with the name of the dish, such as ["sublimated artichoke frittata" or "whole pork belly, market acorns, and activated shell bean"](http://www.brooklynbarmenus.com/). In this menu hash, the dish names are called **keys**: they are the labels that are used to identify your dining options. The food that those dish names represent are the **values** that the keys point to. To order your food, you give your waiter the key (you tell him the name of the dish you want), and he returns the value of that key (food that matches the description on the menu). Mmmmm, nothing like a lunch of free-range bison with corn and peach compote and an IPA! You are a programmer, after all.

## How They Differ

There are two important differences to note between the vending machine array and the menu hash. First, it's far easier for us to use the names of things to find what we're looking for than to have to translate what we want into numerical indices. This is a huge advantage of using a hash: no more having to count out array elements to request what we want! Second, the items on a menu can appear in any order, and we'll still get exactly what we want as long as we use the correct name. This unordered aspect of hashes isn't true for arrays, which are highly dependent on order. 

Understanding how both arrays and hashes work will greatly help you on your programming journey. As you dive deeper into learning Ruby, you'll start to see both of these data collections everywhere. You'll also come to greatly appreciate how powerful they can be, especially when combined with their many useful methods. 