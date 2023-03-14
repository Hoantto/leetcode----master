### 栈
首先将给定的字符串`path`根据`/`分割成一个由若干字符串组成的列表，记为`names`，其中包含以下几种情况：
- 空字符串。例如当出现若干个连续的`/`，就会分割出空字符
- 一个点`.`
- 两个点`..`
- 只包含英文字母、数字或`_`的目录名

对于「空字符串」以及「一个点」，我们实际上无需对它们进行处理，因为「空字符串」没有任何含义，而「一个点」表示当前目录本身，我们无需切换目录。

对于「两个点」或者「目录名」，我们则可以用一个栈来维护路径中的每一个目录名。当我们遇到「两个点」时，需要将目录切换到上一级，因此只要栈不为空，我们就弹出栈顶的目录。当我们遇到「目录名」时，就把它放入栈。

**代码**
*python version*
```
class Solution:
    def simplifyPath(self, path: str) -> str:
        names = path.split("/")
        stack = []
        for name in names:
            if name == "..":
                if len(stack) != 0:
                    stack.pop()
            elif name and name != ".":
                stack += [name]
        return "/" + "/".join(stack)
```

**参考**
- [Join all items in a tuple into a string, using a hash character as separator](https://www.w3schools.com/python/ref_string_join.asp)
