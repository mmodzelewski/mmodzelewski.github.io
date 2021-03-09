---
layout: post
title: "First take on Cloud Firestore Security Rules"
date: 2021-03-09 18:01:40 +01:00
categories: firebase, firestore, javascript, webdev
canonical_url: https://climbingdev.com/posts/firestore-rules
---
Hi!

I'm currently working on a small POC project and I thought Firebase will be a good candidate for quick prototyping. In the project I am using Cloud Firestore and Authentication.

In this post I'd like to show you what I've learned about the Firestore Rules. Hopefully you'll find it useful. Let's dig in.

The first version of the Firestore schema looks like this:

```json
{
  "clubs": {
    "<clubId>": {
      "name": "This is a public club",
      "ownerId": "<ownerId>",
      "visibility": "public",
      "members": {
        "<userId>": {
          "name": "My fancy nickname"
        }
      }
    }
  }
}
```

At the top level there is a collection of clubs. Each club has a name, owner and it can be either public or private. 

Of course the club must have members, which is a nested Firestore collection. The keys in this collection are users' ids from the Firebase Authentication module. A member for now has only a name.

For the above collections schema I have created the following rules:

```jsx
rules_version = '2';
service cloud.firestore {
 match /clubs/{club} {
    	allow create: if request.resource.data.ownerId == request.auth.uid
      allow delete, update: if request.auth != null && request.auth.uid == resource.data.ownerId
      allow read: if request.auth != null && (resource.data.visibility == 'public' || isClubMember(club, request.auth.uid))
      match /members/{member} {
      	allow read, write: if isClubOwner(club, request.auth.uid)
      }
    }    
    
    function isClubMember(clubId, userId) {
      return exists(/databases/$(database)/documents/clubs/$(clubId)/members/$(userId));
    }
    function isClubOwner(clubId, userId) {
    	let club = get(/databases/$(database)/documents/clubs/$(clubId));
      return club != null && club.data.ownerId == userId;
    }
  }
}
```

Let's look at them one by one.

```jsx
 match /clubs/{club} {
```

All rules within this block will refer to some club with id available under the `club` variable.

```jsx
allow create: if request.resource.data.ownerId == request.auth.uid
```

Anyone can create a club as long as they set themselves as the owner of the club. 

```jsx
allow delete, update: if request.auth != null && request.auth.uid == resource.data.ownerId
```

Only the owner can delete or update the club.

```jsx
allow read: if request.auth != null && (resource.data.visibility == 'public' || isClubMember(club, request.auth.uid))
```

Any authenticated user can see public clubs. Non public clubs can be seen by their members.

```jsx
match /members/{member} {
	allow read, write: if isClubOwner(club, request.auth.uid)
}
```

Only the owner can see or modify members of the club. 

In the last two rules I have used helper functions which are defined as follows:

```jsx
function isClubMember(clubId, userId) {
  return exists(/databases/$(database)/documents/clubs/$(clubId)/members/$(userId));
}
```

This function queries the members collection for specified club to see if the user belongs to it.  
**Important note**: this function will count towards the number of DB invocations and will influence billing.

```jsx
function isClubOwner(clubId, userId) {
  let club = get(/databases/$(database)/documents/clubs/$(clubId));
  return club != null && club.data.ownerId == userId;
}
```

This function uses `get` instead of `exists` and checks the property of the club to see if the specified user is an owner.

This is what I've come up with on the first encounter with Firestore rules. It's not perfect but it's a good start.

At the moment I'm not sure what is the best place to keep the `ownerId`. With the current set up every user that can see the club can also see the id of the owner and that's far from perfect.

If you have any comments or suggestions on how this structure could be improved please let me know in the comments. 

Happy coding! 🙂

Additional resources:  
[https://firebase.google.com/docs/rules](https://firebase.google.com/docs/rules){:target='_blank'}  
[https://firebase.google.com/docs/firestore/security/rules-query](https://firebase.google.com/docs/firestore/security/rules-query){:target='_blank'}
