+++
date = "2011-06-26T23:25:56+08:00"
title = "手机九点密码锁的可能性有多少种？"
categories = ["algorithm"]
tags = ["algorighm", "interview"]

+++

```
neighbor = [ [], # 0 for null 
             [2,4,5,6,8], # node1
             [1,3,4,5,6,7,9],
             [2,4,5,6,8],
             [1,2,3,4,7,8,9],
             [1,2,3,4,6,7,8,9], # node 5
             [1,2,3,5,7,8,9],
             [2,4,5,6,7],
             [1,3,4,5,6,7,9],
             [2,4,5,6,8] ] # node 9

def step(trace, current):
    nblist = neighbor[current]
    avail = list(set(nblist) - set(trace))
    c = 0
    for i in avail:
        newtrace = trace + [i]
        c = c + 1 + step(newtrace, i)
    return c
    
def main():
    return step([1], 1) * 4 + step([2], 2) * 4 + step([5], 5)
    
if __name__ == '__main__':
    print main()
```
