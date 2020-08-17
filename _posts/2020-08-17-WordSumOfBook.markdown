---
layout: post
title:  "Word Sum in a book is Odd Or Even"
date:   2020-08-17 15:42:25 +0100
categories: [algorithm, DFS, Union-Find]
---

# Question:

Assuming there is a book. You need to design and implement an API that takes three input:x, y, and v. The [x,y] is a range of book pages, and v indicats the sum of words of these pages is even(0) or odd. This API return true if the input is valid, and false otherwise.

Example: 

[1,10] 0 # return True, because we don't have any input yet
[5,10] 0 # return True, because we can not calculate [5,10] from previous inputs
[1,5,1] # False, [1,5] should be even from previous inputs

# Solution

This question is quite similar to the [evaluate division problem](https://leetcode.com/problems/evaluate-division/). We can convert it to a graph search problem and use DFS/Union-Find.

```python

even = 0
odd = 1

class DSU(object):
    def __init__(self):
        self.U = {}
    def find(self,x):
        if self.U[x][0] != x:
            px,vx = self.find(self.U[x][0]) # x-self self.U[x][0]->px: vx
            self.U[x] = (px, vx + self.U[x][1])
        return self.U[x]
    def insert(self,x,y,v):
        if x not in self.U and y not in self.U:
            self.U[x] = (y, v)
            self.U[y] = (y, 0)
        elif x not in self.U:
            self.U[x] = (y, v)
        elif y not in self.U:
            self.U[y] = (x, -1 * v)
        else:
            px,vx = self.find(x) #x -> px:vx
            py,vy = self.find(y) # y -> py:vy # x - y:v
            self.U[px] = (py, py+v - vx)
    def search(self,x,y,v):
        if x in self.U and y in self.U:
            px,vx = self.find(x) #x -> p:vx
            py,vy = self.find(y) # y -> p:vy # x - y:v
            if px != py:# undef
                self.insert(x,y,v)
                return True
            else:
                return (vy-vx) % 2 == v
        else:
            self.insert(x,y,v)
            return True
dsu = DSU()

inps = [
(1,10,0),
(5,10,0),
(1,5,0),
        ]

for inp in inps:
    print(inp,dsu.search(inp[0],inp[1],inp[2]))

```
