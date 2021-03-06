**223. Rectangle Area (medium)**

Given the coordinates of two **rectilinear** rectangles in a 2D plane, return the total area covered by the two rectangles.

The first rectangle is defined by its **bottom-left** corner `(ax1, ay1)` and its **top-right** corner `(ax2, ay2)`.

The second rectangle is defined by its **bottom-left** corner `(bx1, by1)` and its **top-right** corner `(bx2, by2)`.

![image](https://user-images.githubusercontent.com/51500878/137381557-d7077718-23b3-4cec-8606-f6785e6f7fe7.png)

![image](https://user-images.githubusercontent.com/51500878/137381574-02b5af59-d255-4382-8f1c-9ef77d950661.png)


**Solution**

```python
class Solution:
    def computeArea(self, ax1: int, ay1: int, ax2: int, ay2: int, bx1: int, by1: int, bx2: int, by2: int) -> int:
        x = min(ax2, bx2) - max(ax1, bx1)
        y = min(ay2, by2) - max(ay1, by1)
        
        if x < 0 or y < 0:
            return (ax2-ax1) * (ay2-ay1) + (bx2-bx1) * (by2-by1)
        return (ax2-ax1) * (ay2-ay1) + (bx2-bx1) * (by2-by1) - x*y
```

```python
# A concise version

def computeArea(self, A, B, C, D, E, F, G, H):
    overlap = max(min(C,G)-max(A,E), 0)*max(min(D,H)-max(B,F), 0)
    return (A-C)*(B-D) + (E-G)*(F-H) - overlap
```    

**Note**

- IF two rectangles have no overlapped area, then their length and width (`x, y`) of the overlapped area should be negative


**224. Basic Calculator (hard)**

Given a string s representing a valid expression, implement a basic calculator to evaluate it, and return the result of the evaluation.

Note: You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as eval().

![image](https://user-images.githubusercontent.com/51500878/137387657-22bcb5ee-2ffd-47b0-8016-62a1d5bea35c.png)

![image](https://user-images.githubusercontent.com/51500878/137387699-4e27bfc2-4004-4b7b-92ef-f64559723be6.png)

**Solution**

```python


```

**Note**









