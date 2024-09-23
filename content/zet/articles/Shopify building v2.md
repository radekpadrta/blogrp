---
author:
  - Radek Padrta
title: Reflection on Madeira
date: 2024-09-21
description:
tags:
  - Travel
  - Madeira
---

### Currency selector
After we built a language selector, I also wanted a currency selector. It should be good to have the option to send packages anywhere, and people don't want to see everything in Czech currency. 
We built markets in Shopify. I divided it into three pillars:

- Czech
- Europe
- International 
Europe and International contain a few countries which I selected, just those where I saw potential. 

When we set the markets, then I needed to send rates for shipping. I put there just test prices; this needs more time to correctly set it.

And that setting gives me a pretty boring currency header. I wanted to have it better... It should be nice if users already understand -> "here I can select currency for my country". 

 Then I start tinkering with a code and I put an image of currency there, and in the wrapper, we can search countries! When you hover over a particular country, you will see the currency which will be shown on the page. But this is not all.. I already set geolocation, so if you are, for example, from Canada, you should see everything with Canadian currency. It should be automatic, but you know, it's better to be prepared :)
 
 Below the currency is me, just a demo photo from 9th grade from primary school :) And the scroll bar needs some tuning too!

![[Pasted image 20240921202957.png]]

### Structure
Then I started playing with the structure of the website. Here is the plan:

We will have two pillars of products. Each pillar has its own collections. And when you click the collections, you will see products of each collection. 
It's maybe a lot for customers.. maybe you need to give them a lot of options that they can choose from on the main page. 

This can be a good approach if you want to show everything in one check, but I also would like to have some system in our e-shop, and I think this will provide a better user experience for customers, to navigate a little bit and really find the treasure they are searching for.












