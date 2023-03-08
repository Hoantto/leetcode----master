### 回溯
使用回溯的方法，生成排列序列后，再`slicing`出最后的结果。但这样做会超出时间的限制。

**代码**
*python version*
```
class Solution:
    def getPermutation(self, n: int, k: int) -> str:
        return ["".join(map(str, x)) for x in itertools.permutations(range(1, n + 1))][k - 1]
```

### 逆康拓排序

**代码**
*python version*
```
class Solution:
    def getPermutation(self, n: int, k: int) -> str:
        fac = [1]
        for i in range(1, n):
            fac += [fac[-1] * i]
        
        output = []
        mod = k - 1
        nums = [x for x in range(1, n + 1)]
        for mul in fac[::-1]:
            div, mod = divmod(mod, mul)
            output += [str(nums.pop(div))]
        return "".join(output)

```


**参考**
- [using  str.join() function and map() function](https://www.geeksforgeeks.org/python-program-to-convert-a-tuple-to-a-string/)
- [itertools — Functions creating iterators for efficient looping](https://docs.python.org/3/library/itertools.html)
- [字节尿性，康托展开求第K个排列！](https://blog.51cto.com/u_15076236/2609722)
- [ython List/Array Methods](https://www.w3schools.com/python/python_ref_list.asp)
- [Python divmod() Function](https://www.w3schools.com/python/ref_func_divmod.asp)