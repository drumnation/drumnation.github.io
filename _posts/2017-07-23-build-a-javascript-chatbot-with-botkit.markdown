---
layout: post
title: "Build a Javascript Chatbot with Botkit"
date: 2017-07-23 11:39:06 -0400
---
![chat bot](/images/chat-bots/chatbot.jpg)

Chatbots are useful, helpful, and can be used in really clever and interesting ways. I'm going to discuss chatbots in general and an npm called botkit that tries to make it a bit easier to create one of your own. 

Wikipedia defines them as:

> A chatbot (also known as a talkbot, chatterbot, Bot, chatterbox, IM bot, interactive agent, or Artificial Conversational Entity) is a computer program which conducts a conversation via auditory or textual methods.

> Such programs are often designed to convincingly simulate how a human would behave as a conversational partner, thereby passing the Turing test. Chatbots are typically used in dialog systems for various practical purposes including customer service or information acquisition. 

> Some chatterbots use sophisticated natural language processing systems, but many simpler systems scan for keywords within the input, then pull a reply with the most matching keywords, or the most similar wording pattern, from a database.

## Things I find useful about chatbots

A chatbot is programmed to listen for certain words or characters and to respond in certain ways accordingly.

### Chatbot as a remote control

Every word begins a tree of possible conversation in which each text response from the user triggers some kind of response by the chatbot.

Therefore, I think you one can think about chatbots in a few different ways.  In a very basic scenario an SMS chatbot could function as a remote, triggering various server side automation scripts to begin running.  The chatbot could ask for a word to look up, take your text response, look it up in a browser instance, scrape the description, and then send it back to you.

### Chatbot as a text based menu system

The second way you can think of them is similar to the first, but instead of thinking remote, you can think of it as a sort of menu system.  A chatbot could ask a series of straight forward questions about a problem and use those responses to create a whole document with all the correct boxes checked and text input.

### Chatbot as a monitoring tool

A chatbot receiving data from Reddit threads or Slack conversations can on the fly gather useful information, log something, or perform some other kind of automated action.  

Chatbots as you can see can be pretty useful for a lot of different purposes you might not have thought to use them for.  They can be designed to emulate a human as well which at the very least can provide a smoother user experience with the user.

## Botkit npm for Node.js

To install the [botkit](https://github.com/howdyai/botkit) npm just type:

    npm install --save botkit

### If you visit the repo you'll see that they have a few examples for integrations:

1. Slack
2. Cisco Spark
3. Facebook Messenger
4. Twilio SMS
5. Twilio IPM
6. Microsoft Bot Framework

## A botkit conversation looks something like this:

``` javascript
// respond when a user sends an SMS to the bot that says "hello" or "hi"
controller.hears(['hi', 'hello'], 'message_received', (bot, message) => {
    bot.startConversation(message, (err, convo) => {
            convo.say('Hi, I am Jeeves, an SMS bot! :D')
            convo.ask('What is your name?', (res, convo) => {
            convo.say(`Nice to meet you, ${res.text}!`)
            convo.next()
        })
    })
})

// if the bot hears anything that isn't hi or hello respond like this
controller.hears('.*', 'message_received', (bot, message) => {
    bot.reply(message, 'huh?')
})
```
### So Why Use Botkit?

    Botkit is designed to ease the process of designing and running useful, creative bots that live inside Slack, Facebook Messenger, Twilio IP Messaging, and other messaging platforms like Twilio's Programmable SMS.

It's designed to ease many integration headaches and create a framework that can be applied to many different data sources.  This speeds up development time and allows you to focus more on scripting the conversations and actions, which to be honest is where all the real complexity lies.  

If you're looking for a place to start, botkit will get you closer to the meat of chatbot development much quicker than if you tried to build a chatbot from scratch and integrate it from these sources.  Give this a shot and enjoy creating your very own digital helper.