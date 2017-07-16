---
layout: post
title: "ES6 Promises/.then() vs. ES7 Async/Await"
date: 2017-07-15 3:18:06 -0400
comments: true
categories: javascript, asynchronous functions, es7
---

# Ditch Promises/.then() for ES7's Async/Await

Now that [Node 8](https://nodejs.org/en/blog/release/v8.0.0/) has been released Async/Await is fully supported. asyncawait addresses the problem of callback hell in Node.js JavaScript code. Inspired by C#'s Async/Await feature, Async/Await enables you to write functions that appear to block at each asynchronous operation, waiting for the results before continuing with the following statement.

Here's the [post that convinced me](https://hackernoon.com/6-reasons-why-javascripts-async-await-blows-promises-away-tutorial-c7ec10518dd9) to give the refactor a try. I'll summarize the key points the author made.

## 6 Reasons why Async/Await is better

### 1. Concise and clean
No annonymous callback function, function nesting, or unused variable like 'data'

### 2. Error handling
Catch block can now handle asynchronous errors

### 3. Conditionals
Greatly simplify conditionals and eliminate nesting, extra braces, and redundant return statements.

### 4. Intermediate Values
In situations where promise 3 uses data from promise 2 to make it's request, async await can cut the necessary code to 1/4th the lines.

### 5. Error Stacks
With long promise chains, promises/then makes it very difficult to determine which promise broke. 
try/catch makes it possible to report on any failure in the chain without duplicating any code.

### 6. Debugging
Arrow functions can't return expressions, async await lets you avoid arrow functions, using 'debugger' works properly with async await / weird step over functionality with then

## Code Examples

I was writing a SeleniumWebdriver script and began by getting it to almost work properly using promises and .then().  I decided to try refactoring it into async await structure.  Turns out, not only did this seem to be a much cleaner way of writing it, but it totally fixed the strange timing based behavior I was experiencing. It fixed my bug.

## ES6 Promises/.then()

``` javascript
function bookClass(convo) {
    browser.findElement(By.css('.tooltip.tooltip--center.mt-.bt.bt--lg.bt--width-full')).click()
    .then( () => {
        browser.switchTo().frame(0)
        .then( () => { 
            browser.sleep(2000)
            browser.findElement(By.linkText("Reserve this class")).click()
            .then( () => {
                const reservationFailed = browser.findElements(By.xpath("//*[contains(text(), 'Reservation failed')]")) ? true : false
                if (reservationFailed) {
                    convo.next()
                    convo.say('Sorry. No more spots are available. Reservation Failed.')
                } else {
                    convo.next()
                    convo.say("Congrats! You're registered.")
                }
            })
        })
    })
}
```

## ES7 Async/Await

``` javascript
const bookClass = async (convo) => {
    try {
        await browser.findElement(By.css('.tooltip.tooltip--center.mt-.bt.bt--lg.bt--width-full')).click()
        await browser.switchTo().frame(0)
        browser.sleep(2000)
        await browser.findElement(By.linkText("Reserve this class")).click()
        await browser.switchTo().activeElement()
        await browser.findElements(By.xpath("//*[contains(text(), 'Sorry, no more spots are available.')]")).then(function(element) {
            if (element.length > 0) {
                convo.next()
                convo.say('Sorry. No more spots are available. Reservation Failed.')
            } else {
                convo.next()
                convo.say("Congrats! You're registered.")
            }
        })
    } catch (err) { console.log(err) }
}
```

## So you can see that...

1. I was able to remove all arrow functions

2. Flatten 3 layers of nesting into 1

3. Add error handling to 5 promises with 1 line of code

I really like ES7's async await and definitely recommend that you give it a try. You can rest assured that If something goes wrong, someone will patiently wait and catch your errors.