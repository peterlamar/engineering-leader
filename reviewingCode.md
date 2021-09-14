# Set of guidelines and tips for reviewing code

1. [Obvious Code](#obvious-code)
1. [Naming](#naming)
1. [Red Flags](#red-flags)
1. [References](#references)

## Obvious Code

Things that make code more obvious. Software should be designed for ease of reading, not
ease of writing

* Good, specific and precise names
* Generous use of whitespace
```cpp
for(int pass=1;pass>0&&!empty;pass--)
for (int pass = 1; pass >= 0 && !empty; pass--)
```
* Use of comments to provide information that is nonobvious
    - For example: Event driven or deep Object Oriented calls to code. Such calls should be highlighted with comments

```cpp
/*
* This method is invoked in the dispatch thread by a transport if a 
* transport level error prevents an RPC from completing
*/
void Transport::RpcNotifier::failed(){
}
```

### AntiPatterns

* Misusing Generic Containers

Grouping variables that do not have a relationship for convienence of 
passing or returning. 
```java
return new Pair<Integer, Boolean>(someTerm, false);
```
* Different types for declaration and allocation

Reader may expect var is a List and not an ArrayList
```java
private List<Message> incommingMessageList;
incomingMessageList = new ArrayList<Message>();
```
* Code that violates reader expectations
Code doesn't exit as implied by void type, but actually spawns threads
```java
public static void main(String[] args){
    new RaftClient(myAddress,serverAddresses); // Spawn threads
}
```
General ways to make code more obvious
- Reduce information that is needed such as abstraction and eliminating special cases
- Take advantage of known information through conventions and conforming to expectations
- Present information in code through naming and comments

## Naming

Names should be precise and consistent. 

```cpp
int widgetManager getCount();
```
GetCount of what? getActiveWidgets or numWidgets would be better

```cpp
// true when cursor visible
bool blinkstatus = true;
```
cursorVisible would be better

If its hard to pick a name, perhaps the variable does not have a clear 
definition or purpose. Consider refactoring into several variables

## Red Flags

* Shallow Modules - Interface for class/method is not much simplier than implementation. i.e.
a class barely does enough to justify learning its API
* Information Leakage - The same specific knowledge is needed in multiple places such as a
file type
* Overexposure - API for a commonly used feature forces users to learn about other features that are rarely used, increasing cognitive load for all users
* Pass through method - Method does nothing but pass its arguments to another
method with a similar signature
* Conjoined Methods - It takes reading two methods to understand whats going one,
methods should be able to be understood by themselves
* Comment repeats code - Information from comment is obvious 
* Vague Naming - Variable or method name broad enough to refer to many different
things, thus making it likely a future developer will misuse it
* Hard to describe - If a method or function is hard to describe, perhaps a
refactor could be warrented

## References

1. [Awesome Code Review](https://github.com/joho/awesome-code-review)
1. [Flatten code arrows](https://blog.codinghorror.com/flattening-arrow-code/) Excellent and practical on the all too prevelant nested if's
1. [A Philosophy of Software Design](https://hackernewsbooks.com/book/a-philosophy-of-software-design/12be0ff26a765aa737d6b042043aa079) A great design book with lots of examples from class experience