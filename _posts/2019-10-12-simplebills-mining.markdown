---
layout: post
title:  "SimpleBills is mining on your computer"
date:   2019-10-12
categories: jekyll update
---

Quick and easy explanation:

## What is SimpleBills?
According to the Founder and President of SimpleBills, Kevin Jones, it's "...a complete utility management service that helps properties realize efficiencies with data-based decisions while *providing residents a customer service experience you can be proud of*."

I only know about it because my apartment forced it on me (along with an initial $60 charge for its services).

## How did you find out?
Symantec, pretty much. The mining JS script is located in the sign in page located [here](https://app.simplebills.com/signin).
The HTML source contains a `<script>` tag with the following `src`.

`https://www.hostingcloud.racing/gA51.js`

It doesn't matter whether you're sign in or not. Also, the JS script will eat up your CPU usage.

Here's a screenshot of my poor CPU feeling the effects:

![SimpleBills killing my CPU](https://i.imgur.com/6Y4T2Nk.png)

## What's in the JS script?
The JS script just contains some obfuscated code (lol) and I'm not bothered to identify the script's name but just to let you know, Symantec identifies it as JSCoinMiner.

Here's a [gist of the source code](https://gist.github.com/Maktm/9518eece4b6190a439d513396e622e0a).

## Bonus

The script connects to the following endpoint `wss://s11.hostcontent.live/3ce92Z`.

After connecting, the requests it sends look like the following:

![Requests form miner](https://i.imgur.com/E0jf2Mu.png)

So, it might be CoinImp but I don't know enough about mining to say for sure.