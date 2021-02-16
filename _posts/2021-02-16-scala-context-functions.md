---
layout: post
title:  "Scala 3 (aka Dotty) Context functions"
date:   2021-02-16 15:03:00 +0100
categories: scala dotty functional
---
![image](/assets/images/2021-02-16-scala-context-functions.png)

Recently I've watched an interesting presentation from [Martin Odersky on Plain Functional Programming](https://www.youtube.com/watch?v=YXDm3WHZT5g){:target='_blank'}.

In his talk Martin introduced Context functions which let him achieve 2 things.

1. Pass context to functions without explicitly passing it through function's arguments
2. Make function signatures more descriptive - by introducing Possibly he gave the function ability to throw an error

I highly recommend the talk if you want to get familiar with the full reasoning behind Context functions. [Also here's the link to docs about Context functions](https://dotty.epfl.ch/docs/reference/contextual/context-functions.html){:target='_blank'}.

You can check out the sample code for Martin's presentation [here](https://github.com/mmodzelewski/scala-examples/blob/master/context-functions/src/main/scala/Main.scala){:target='_blank'}.
I have updated the code to the current Scala syntax.

Happy coding 🙂
