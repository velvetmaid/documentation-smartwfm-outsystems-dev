---
title: Avoid Aggregate Looping
layout: home
parent: Query & Aggregate
nav_order: 2
---

# Avoid Aggregate Looping
Let's say you encountered a situation where you must(or happened to) write logic like this when you cannot join the table for any reason :

| ![Aggregate inside for loop](https://smartwfm-1.gitbook.io/~gitbook/image?url=https%3A%2F%2F2965834173-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F8bJznGZQyIrzjCOGYuPg%252Fuploads%252Fsh57HnU2eE9gqMNoaH9b%252Fimage.png%3Falt%3Dmedia%26token%3Da2358382-90c5-4ffc-b77e-6ace31674990&width=150&dpr=2&quality=100&sign=b845af6a&sv=1) | 
|:------:| 
| *Aggregate inside for loop* |

Often times, you can easily join the two table together to return the data you needed. But there's several condition where you cant join the table because several reason like the table is on another server or source data for the list was a List datatype.

This example should be avoided at any cost. The reason for avoiding this type of logic is it could lead to performance issues, if the data is somewhat large, it will cause timeout when calling this action.

There are several ways for you to rewrite this type of logic to improve application performance such as using Advanced SQL with "IN", or using index() function to search the data.

___

## Method 1 : Using Index() function

![](https://smartwfm-1.gitbook.io/~gitbook/image?url=https%3A%2F%2F2965834173-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F8bJznGZQyIrzjCOGYuPg%252Fuploads%252FFqEFhm1QBbetonwPo8Q3%252Fimage.png%3Falt%3Dmedia%26token%3Dfac9a5d7-ead2-499b-9051-8b3d1b271b5a&width=400&dpr=2&quality=100&sign=b47294a&sv=1)

Perhaps you have some function that takes Site list as input parameter and ticket list as the output.

![](https://smartwfm-1.gitbook.io/~gitbook/image?url=https%3A%2F%2F2965834173-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F8bJznGZQyIrzjCOGYuPg%252Fuploads%252FMhM4ROMPivlWTqgfyBCL%252Fimage.png%3Falt%3Dmedia%26token%3D70f5edfc-3d42-483c-8f65-df83136e7f3a&width=400&dpr=2&quality=100&sign=b9c08a2b&sv=1)

Using the ***Low Code*** mindset, you will create logic like the picture above. However, its not best practice for application performance. To improve this, we can rewrite the logic so it doesn't loop the aggregate

### Joining SiteId as single string

Purpose of this new variable is to store site id list in new format. We can convert the data using _StringJoin()_ function and set separator with ";" (you can use any other symbol as separator)

![](https://smartwfm-1.gitbook.io/~gitbook/image?url=https%3A%2F%2F2965834173-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F8bJznGZQyIrzjCOGYuPg%252Fuploads%252FB9eBrGteiqnYhadYkLad%252Fimage.png%3Falt%3Dmedia%26token%3Dc85f811f-f2e7-4829-a601-eb349c83280d&width=400&dpr=2&quality=100&sign=cc83ce0d&sv=1)

So the data would look like `ABC123;ABC124;ABC125;ABC188`

Now we can modify the filter in aggregate.

| ![](https://smartwfm-1.gitbook.io/~gitbook/image?url=https%3A%2F%2F2965834173-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F8bJznGZQyIrzjCOGYuPg%252Fuploads%252FQTl4D7IqofvS65DMp3N2%252Fimage.png%3Falt%3Dmedia%26token%3D4acc82ff-7651-4868-b7d6-80ecd6b9ae33&width=400&dpr=2&quality=100&sign=579b718b&sv=1) | 
|:------:| 
| *Before, we filter by site id and loop the aggregate* |

To this : 

| ![](https://smartwfm-1.gitbook.io/~gitbook/image?url=https%3A%2F%2F2965834173-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F8bJznGZQyIrzjCOGYuPg%252Fuploads%252FQjjt3TNnlRYl2Ye9Hniy%252Fimage.png%3Falt%3Dmedia%26token%3De60ebd58-1d01-4366-8298-d9c995511bbf&width=400&dpr=2&quality=100&sign=9c129cfd&sv=1) | 
|:------:| 
| *After, use index function* |

So we can modify the logic to this 

| ![](https://smartwfm-1.gitbook.io/~gitbook/image?url=https%3A%2F%2F2965834173-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F8bJznGZQyIrzjCOGYuPg%252Fuploads%252FekvAuzLT9zeBMVh9zHWO%252Fimage.png%3Falt%3Dmedia%26token%3D8fa3e765-34d7-47c0-adb7-1c6b693476c9&width=400&dpr=2&quality=100&sign=5c1cecd8&sv=1) | 
|:------:| 
| *Removed loop and change ListAppend to ListAppendAll* |

___

## Method 2 : Using "IN" in Advanced SQL
First, we must add the dependency from "Sanitization" module and assign SiteID list as input to the action.

![](https://smartwfm-1.gitbook.io/~gitbook/image?url=https%3A%2F%2F2965834173-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F8bJznGZQyIrzjCOGYuPg%252Fuploads%252FoJCOUXNd4rYzAiu6tvU2%252Fimage.png%3Falt%3Dmedia%26token%3D40dc7e30-a4cf-41d5-84bd-f7b58672c56a&width=400&dpr=2&quality=100&sign=32ee92d&sv=1)

Then, create query using Advance SQL like this with input parameter text and expand inline set to "Yes"

| ![](https://smartwfm-1.gitbook.io/~gitbook/image?url=https%3A%2F%2F2965834173-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F8bJznGZQyIrzjCOGYuPg%252Fuploads%252FymCr8iANBpJP6wFxSd9H%252Fimage.png%3Falt%3Dmedia%26token%3Dffbd9e4a-4be5-42fe-8494-2435926d7f53&width=400&dpr=2&quality=100&sign=cc9b022b&sv=1) | 
|:------:| 
| *Don't forget to set "Expand Inline" properties to Yes* |

And the final result will be like this

![](https://smartwfm-1.gitbook.io/~gitbook/image?url=https%3A%2F%2F2965834173-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F8bJznGZQyIrzjCOGYuPg%252Fuploads%252Fo1TF31WoqdU0anlBLWrQ%252Fimage.png%3Falt%3Dmedia%26token%3Da95a241b-c453-46b2-b663-a9dbada95612&width=400&dpr=2&quality=100&sign=5bd0c235&sv=1)