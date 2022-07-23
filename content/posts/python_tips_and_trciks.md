---
title: "Python_tips_and_trciks"
date: 2022-07-23T14:25:00+02:00
---


# Some tips and tricks regarding Python
## For chaining in pandas tutorial visit
[chaining: a good way to work in pandas](https://github.com/sudo-umar/python/blob/main/practice.ipynb)
## zip 

Through this keyword we can zip/or pair wise group two lists together. The lists can be of different size but it will only map one to one, so the remaining elements of one list will be left behind.
```python
a = [1,2,3]
b = [3,5,6,9]
for x,y in zip(a,b):
	print(x,y)
print(list(zip(a,b)))
```

The output will look something like this:
1 3
2 5 
3 6

[(1,3),(2,5),(3,6)]

## args, kwargs

read as arguments and keyword arguments. Args is a tuple and kwargs is just a dictionary.

```python
def someFunction(s,*args,**kwargs):
	print(s)
	if args:
		print(args)
	if kwargs:
		print(kwargs)
someFunction('without args and kwargs')
someFunction('with args and kwargs',1,2,3,'yellow',key1 = 'hello', key2 = 'whatup')
```



The output of fist fucntion call would simply be 
==without arsgs and kwargs==

But the output of the second fucntion call would be 
==with args and kwargs==
==(1,2,3,'yellow')
{'key1':'hello','key2':'whatup'}==

You don't have to worry about the parameters being passed into the function. It is super useful for wrapping the funciton.

## Generators

Just remember they do not hold all the values in memory. So, it makes sense that whatever they are they should be extremely useful in handling large datasets.

There are two ways to create generators:
1. Generator functions:

```python
def  squareElementsNormal(arr):
	output = []
	for i in arr:
		output.append(i*i)
	return output
squareElementsNormal([1,2,3])

# output : [1,4,6]

def squareElementsGenerator(arr):
	for i in arr:
		yield i*i

gen = squarElementsGenerator([1,2,3])
print(gen)
# output: GeneratorObject
print(next(gen))
# output: 1
print(next(gen))
# output: 2
print(list(gen))
```

The difference between two functions is that the first function exits at return call but the second function maintains the state. For example, when we call next() it will fetech the first value from the function and when we are going to call next() again it will ask the function to give it the second value which is maintained by the funciton. 

2. Second way to create them is through  generator comprehension/expression just like list comprehension
```python
gen = (x*x for x in arr)
```
