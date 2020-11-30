---
layout: post
title: "Improve *ngFor performance with trackBy"
date: 2020-11-30 13:30:10 UTC
categories: angular typescript
canonical_url: https://climbingdev.com/posts/angular-ngfor-trackby
---
![image](/assets/images/2020-11-30-angular-ngfor-trackby.png)

Whenever you are using `*ngFor` in your Angular application consider setting `trackBy` function for it.

By default, Angular uses reference comparison for objects in the list. 
This might not be the optimal approach, especially when you update your list's data from the server. 

Instead, you could be using some unique and stable identifier that your objects have.
Thanks to comparing by the id, Angular won't need to rerender all elements every time
you fetch an updated list from the server (object references will be changed but ids won't).

*Less computations -> performance gain!*

How to use the `trackBy` then?

You need to create a function that matches `TrackByFunction` interface. 
```typescript
interface TrackByFunction<T> {
  (index: number, item: T): any;
}

// might be something like this
function trackByTransaction(index: number, item: Transaction): string {
  return item.id;
}
```
And use it in the template:
```angular2html
<div *ngFor="let transaction of transactions; trackBy: trackByTransaction">
  <!--  transaction view  -->
</div>
```

That's it! Now go and optimise your lists :)

Happy coding! 😀
