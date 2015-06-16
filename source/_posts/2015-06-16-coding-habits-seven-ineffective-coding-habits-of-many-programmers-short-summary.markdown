---
layout: post
title: "[Clean Code] Short summary - Seven Ineffective Coding Habits of Many Programmers"
date: 2015-06-16 13:04:44 +0300
comments: true
categories: [code style, presentations, video, external links]
---

Recently I’ve stumbled upon to the interesting talk by Kevlin Henney: [Seven Ineffective Coding Habits of Many Programmers](http://www.infoq.com/presentations/7-ineffective-coding-habits).

Talk itself is very interesting, and I would agree with Kevlin on most of his points. Among other things, author talks about efficiency of source code reading, clean code without much noise, signal-to-noise ratio, etc. Interestingly, talk itself took more than one hour, which could be interpreted as noisy itself :) In a modern world for many people it’s quite difficult to allocate more than 1 hour in a row to watch such presentation.

So, we’ve decided to write a short summary for the above talk, to help people to quickly go through the main points of the above talk.

<ol>
<li>
### Noisy code
Try to minimize number of lines of “noise” in your code, try to make code as simple and "noiseless" as possible. Keep in mind that your code will be read by humans, and they need to understand it easily. In a lot of noise it's hard to find and identify real important code issues.  
Do not write obvious comments. They only produce more noise to the code.
Examples of bad comments: `i = i + 1; /* Add one to i */` and `System.out.println(“Hello World!”); // display the string`
{% blockquote Kevlin Henney https://twitter.com/KevlinHenney/status/381021802941906944 %}
A common fallacy is to assume authors of incomprehensible code will somehow be able to express themselves lucidly and clearly in comments.
{% endblockquote %}
</li>

<li>
### Unsustainable Spacing
Respect **“80 column rule”** - it’s not about screen sizes, it’s more about “ape vision”. It’s difficult for human eyes to read long lines of text. That’s why magazines, newpapers, modern websites are all using limited width for a content. Your code should leverage that knowledge too.
Example of a good formatting, according to Kevlin:
```java
public ResultType arbitraryMethodName(
    FirstArgumentType firstArgument,
    SecondArgumentType secondArgument,
    ThirdArgumentType thirdArgument)
{
    LocalVariableType localVariable =
        method(firstArgument, secondArgument);
    if (localVariable.isSomething(
        thirdArgument, SOME_SHOUTY_CONSTANT)) 
    {
        doSomething(localVariable);
    }
    return localVariable.getSomething();
}
```
</li>

<li>
### Lego Naming
Choose moderate (not very short, but not too long) names for your classes, variables, methods, etc. Below you can find some of standard Java Exception classes and recommended by Kevlin alternatives (shorter and more clear) names for these classes:
`ClassNotFoundException` -> **`ClassNotFound`** <br/>
`ArithmeticException` -> **`IntegerDivisionByZero`** <br/>
`ArrayStoreException` -> **`IllegalArrayElementType`** <br/>
`ClassCastException` -> **`CastToNonSubclass`** <br/>
`InstantiationException` -> **`ClassCannotBeInstantiated`** <br/>
`NullPointerException` -> **`NullDereferenced`** <br/>
`SecurityException` -> **`SecurityViolation`**
</li>

<li> 
###￼ Underabstraction
{% blockquote Kevlin Henney http://www.infoq.com/presentations/7-ineffective-coding-habits Seven Ineffective Coding Habits of Many Programmers %}
Take your code base, stripout all of the string literals, stripout all of the comments, and then take your source code and put it in a tag cloud generator. And then see what it’s look like. 
{% endblockquote %}
Ask yourself a question “What is your system about?”.
Your codebase should represent your **domain knowledge**, not language syntax. More info: http://philcalcado.com/2009/04/29/tag-clouds-see-how-noisy-your-code-is/ <br/>
Good example of underabstracted code:
```Java
// Initial code
if (portfolioIdsByTraderId.get(trader.getId()) .containsKey(portfolio.getId())) {
    ...
}
// Refactored code
if (trader.canView(portfolio)) {
    ... 
}
```
</li>

<li>
###￼ Unencapsulated State
Do no use **Singletons**, they are almost always bad.
{% blockquote The Lost boys %}
￼Don't ever invite a vampire into your house, you silly boy. It renders you powerless.
{% endblockquote %}
￼</li>
<li>
### Getters and Setters
“set” is NOT the opposite of “get”, the opposite is “reset” or “unset”. These two words has the most entries in English dictionary, so they are very ambiguous, try to limit usage of above words in your code.
For example, instead of:
```Java
public class Money implements ...
{
    public int getUnits() ...
    public int getHundredths() ...
    public Currency getCurrency() ...

    public void setUnits(int newUnits) ...
    public void setHundredths(int newHundredths) ...
    public void setCurrency(Currency newCurrency) ...
}
```
Write it like the following:
```Java
public final class Money implements ...
{
    public int getUnits() ...
    public int getHundredths() ...
    public Currency getCurrency() ...
}
```
</li>

<li>
### Uncohesive Tests
{% blockquote Nat Pryce and Steve Freeman "Are Your Tests Really Driving Your Development?" %}
￼Everybody knows that TDD stands for Test Driven Development. However, people too often concentrate on the words "Test" and "Development" and don't consider what the word **"Driven"** really implies. For tests to drive development they must do more than just test that code performs its required functionality: they must clearly express that required functionality to the reader. That is, they must be clear specifications of the required functionality. Tests that are not written with their role as specifications in mind can be very confusing to read.
{% endblockquote %}
Do NOT test methods, **test classes**. A test case should be just that: it should correspond to a single case.

PS: I do recommend to watch full talk, 'cos it contains much more information that presented in this short summary.
