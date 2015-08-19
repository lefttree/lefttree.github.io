---
layout: post
title: Python Mutable Default Arguments
cover: cover.jpg
category: Python
tags: [python]
---

Reference [Python Guide](http://docs.python-guide.org/en/latest/writing/gotchas/)

The treament of mutable default arguments in function definitions are tricky when you first encounter them.

I have encountered a problem when I write recursive calls

```python
def helper(self, curSet, index, target):                                                                                              
        if target == 0:
            #need to create a copy here everytime find a result
            result = curSet[:]
            self.resultList.append(result)
        for i in range(index, len(self.candidates)):
            if self.candidates[i] > target:
                break
            else:
                curSet.append(self.candidates[i])
                self.helper(curSet, i, target - self.candidates[i])
                curSet.pop()
```

In the above code, everytime target == 0, I need to create a local copy of
curSet, cause the curSet is used for all calls. It would be [] at the end.

Explain:
Python’s default arguments are evaluated **once** when the function is  `defined`, **not** each time the function is `called` (like it is in say, Ruby). 
This means that if you use a `mutable default argument` and mutate it, you `will` and have mutated that object for all future calls to the function as well.

####Similar Example

```python
def append_to(element, to=[]):
    to.append(element)
    return to
```

```python
my_list = append_to(12)
print my_list

my_other_list = append_to(42)
print my_other_list
```

result would be 

```
[12]
[12, 42]
```

####What You Should Do Instead

Create a new object each time the function is called, by using a default arg to signal that no argument was provided (None is often a good choice).

```python
def append_to(element, to=None):
    if to is None:
        to = []
    to.append(element)
    return to
```

Also see[Late Binding Closures](http://docs.python-guide.org/en/latest/writing/gotchas/#late-binding-closures)