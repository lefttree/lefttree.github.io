---
layout: post
title: Python stack and queue
cover: cover.jpg
category: Python
tags: [python, data structure]
---

>from python official doc

### Stack

The list methods make it very easy to use a list as a stack, where the last element added is the first element retrieved (“last-in, first-out”). 

- To add an item to the top of the stack, use `append()`. 
- To retrieve an item from the top of the stack, use `pop()` without an explicit index. 
 
For example:

```
>>>stack = [3, 4, 5]
>>>stack.append(6)
>>>stack.append(7)
stack
[3, 4, 5, 6, 7]
stack.pop()
7
>>> stack
[3, 4, 5, 6]
>>> stack.pop()
6
>>> stack.pop()
5
>>> stack
[3, 4]
```

### Queue

To implement a queue, use `collections.deque` which was designed to have fast appends and pops from both ends.

- `append()`
- `popleft()`

```
>>> from collections import deque
>>> queue = deque(["Eric", "John", "Michael"])
>>> queue.append("Terry")           # Terry arrives
>>> queue.append("Graham")          # Graham arrives
>>> queue.popleft()                 # The first to arrive now leaves
'Eric'
>>> queue.popleft()                 # The second to arrive now leaves
'John'
>>> queue                           # Remaining queue in order of arrival
deque(['Michael', 'Terry', 'Graham'])
```

deque to list
`
list(collections.deque(1,2,3))
`
