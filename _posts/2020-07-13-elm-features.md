---
layout: post
title:  "Why should I try Elm?"
date:   2020-07-13 08:00:10 +0200
categories: elm javascript functional
canonical_url: http://modzelewski.dev/posts/elm-features
---
![image](/assets/images/2020-07-13-elm-features.png)

Hi there!

In [the last post]({% post_url 2020-07-06-elm-intro %})
 I've shared a few resources that are worth checking out when you want to start discovering Elm.
 
The question comes up: why would I try Elm?<br/>
The answer that first pops into my mind is: *For its safety!*

In my opinion, the number one feature of Elm is its compiler.
It will protect you from different sorts of bugs, and you will have more time
 to focus on the core value of your app.   

Let's look at some examples where Elm's compiler comes in handy.

We have an `add` function and two function calls.<br/>
First, a snippet in JavaScript.
```javascript
function add(a, b) { return a + b }

add(1, 2) // 3
add(1, "hello") // "1hello"
```

Both function calls will work out just fine, but the result of the second one
might be surprising. This one is quite obvious, but imagine that you're passing
variables instead of literals. You'll know about this strange behaviour only
after executing the code. 
 
The same example in Elm.
```elm
add a b = a + b

add 1 2 -- 3
add 1 "ok"
``` 
The first function call looks ok, but the second one won't even compile.<br/>
The compiler will give us this nice, descriptive error:
```text
The 2nd argument to `add` is not what I expect:

4|   add 1 "ok"
           ^^^^
This argument is a string of type:

    String

But `add` needs the 2nd argument to be:

    number

Hint: I always figure out the argument types from left to right. If an argument
is acceptable, I assume it is “correct” and move on. So the problem may actually
be in one of the previous arguments!

Hint: Try using String.toInt to convert it to an integer?
```
Thanks, compiler!

That's just a very simple example but with more complex data structures the benefits are even higher.

The same level of security you're getting with properties names.
If you were to reference a property that does not exist in the object, 
the compiler will let you know that something's wrong.

```elm
type alias User =
    { firstName : String
    , lastName : String
    }

fullName : User -> String
fullName user =
    user.firstNme ++ user.lastName
```

In this example, we have a `User` record and function that concatenates first and last names.<br/>
If we try to compile that, we will get an error:
```text
This `user` record does not have a `firstNme` field:

10|       user.firstNme ++ user.lastName
               ^^^^^^^^
This is usually a typo. Here are the `user` fields that are most similar:

    { firstName : String
    , lastName : String
    }

So maybe firstNme should be firstName?
``` 
The compiler saved us hours, again!

Of course, the compiler won't help in case of logic errors, but that's the only concern left on your mind.

Some may say that all statically typed languages have these advantages.
Agreed, but static typing is not that common in the frontend world where JS is the king.
Even with TypeScript, there is the `any` type where you can just throw anything and 
all the type checking goes bananas.

To sum up, I think that Elm might be an interesting alternative for writing frontend apps
as it will make you more secure.
Besides the compiler features Elm offers immutable data structures and pure functions 
which will help you create a straightforward and [honest code][honest-code]{:target='_blank'}.

At the end I would like to recommend watching [an interesting presentation][presentation]{:target='_blank'}
 from Richard Feldman about his journey towards Elm.

Happy coding! 😀

[presentation]: https://www.youtube.com/watch?v=5CYeZ2kEiOI
[honest-code]: https://michaelfeathers.silvrback.com/functional-code-is-honest-code
