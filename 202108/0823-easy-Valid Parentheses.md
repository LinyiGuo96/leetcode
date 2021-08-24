**20. Valid Parentheses**

Given a string `s` containing just the characters `(`, `)`, `{`, `}`, `[` and `]`, determine if the input string is valid.

And input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

**Example 1**

> Input: s = "{}"  
> output: true

**Example 2**

> Input: s = "(){}\[]"  
> output: true

**Example 3**

> Input: s = "{\[}]"  
> output: false

**Example 4**

> Input: s = "{\[]}"  
> output: true

**Constraints** 
- `1 <= s.length <= 10^4`
-  `s` consists of parentheses only `()[]{}` 


**Solutions**

```python
class Solution:
    def isValid(self, s: str) -> bool:
    # return a boolean
        stack = []
        dict = {"]":"[", "}":"{", ")":"("}
        for char in s:
            if char in dict.values():
                stack.append(char)
            elif char in dict.keys():
                if stack == [] or dict[char] != stack.pop():
                    return False
            else:
                return False
        return stack == []
```

**Notes**

1. dict[key] = value or dict = {key1: value1, key2: value2, ...}
2. `list.pop(index)` **removes** the item at the given index from the list and **returns** the removed item. 
    - If the index is null, then the default index is `-1`, which is the last element
    - If the index passed to the method is not in range, it throws IndexError: pop index out of range exception

This method is called `stack` method, which is widely used in Java.

Here is a [introduction](https://www.liaoxuefeng.com/wiki/1252599548343744/1265121668997888) to `stack` method, but written in chinese. Just remember the `LIFO` (last in first out) principle.






