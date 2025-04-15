#Easy 
## Problem Description
You are given an image represented by an `m x n` grid of integers `image`, where `image[i][j]` represents the pixel value of the image. You are also given three integers `sr`, `sc`, and `color`. Your task is to perform a **flood fill** on the image starting from the pixel `image[sr][sc]`.

To perform a **flood fill**:
1. Begin with the starting pixel and change its color to `color`.
2. Perform the same process for each pixel that is **directly adjacent** (pixels that share a side with the original pixel, either horizontally or vertically) and shares the **same color** as the starting pixel.
3. Keep **repeating** this process by checking neighboring pixels of the _updated_ pixels and modifying their color if it matches the original color of the starting pixel.
4. The process **stops** when there are **no more** adjacent pixels of the original color to update.

Return the **modified** image after performing the flood fill.

### Examples
#### 1
> **Input:** `image = [[1,1,1],[1,1,0],[1,0,1]], sr = 1, sc = 1, color = 2`
> **Output:** `[[2,2,2],[2,2,0],[2,0,1]]`
> **Explanation:** 
> ![](https://assets.leetcode.com/uploads/2021/06/01/flood1-grid.jpg)
> From the center of the image with position `(sr, sc) = (1, 1)` (i.e., the red pixel), all pixels connected by a path of the same color as the starting pixel (i.e., the blue pixels) are colored with the new color.
> Note the bottom corner is **not** colored 2, because it is not horizontally or vertically connected to the starting pixel.
#### 2
> **Input:** `[[0,0,0],[0,0,0]], sr = 0, sc = 0, color = 0`
> **Output:** `[[0,0,0],[0,0,0]]`
> **Explanation:** 
> The starting pixel is already colored with 0, which is the same as the target color. Therefore, no changes are made to the image.

### Constraints
- `m == image.length`
- `n == image[i].length`
- `1 <= m, n <= 50`
- `0 <= image[i][j], color < 2^16`
- `0 <= sr < m`
- `0 <= sc < n`

### Follow-up
None.

## Hints
> [!HINT]- Hint 1
> Write a recursive function that paints the pixel if it's the correct color, then recurses on neighboring pixels.

## Topics
> [!WARNING]- 查看相關主題可能會影響你的思考、思路。建議先構思好一種解法之後再點開來查看。
> - [[Array]]
> - [[Depth-First Search]]
> - [[Breadth-First Search]]
> - [[Matrix]]

---
## 我的答案
### Attempt 1
~~~~tabs

tab: Python
```python
class Solution(object):
    def floodFill(self, image, sr, sc, color):
        if color == image[sr][sc]:
            return image

        self.DFS(image, sr, sc, color, image[sr][sc])
        return image

    
    def DFS(self, img, sr, sc, color, old_color):
        img[sr][sc] = color

        if sr - 1 >= 0 and img[sr-1][sc] == old_color:
            self.DFS(img, sr-1, sc, color, old_color)
        if sc - 1 >= 0 and img[sr][sc-1] == old_color:
            self.DFS(img, sr, sc-1, color, old_color)
        if sr + 1 < len(img) and img[sr+1][sc] == old_color:
            self.DFS(img, sr+1, sc, color, old_color)
        if sc + 1 < len(img[0]) and img[sr][sc+1] == old_color:
            self.DFS(img, sr, sc+1, color, old_color)
```
~~~~

#### Result
Accepted
- Runtime: $0$ ms, beats <font color="#76923c">100.00%</font>
- Memory: 12.63 MB, beats <font color="#e5b9b7">42.66%</font>


---
## 解題思路
這是一題經典的難易度簡單的 DFS / BFS 應用題目。
由於此題的出發點僅有一個，且不需計算經過時間，因此直接使用 DFS 即可。當然，使用 BFS 來完成這提也是可行的。若此題改為出發點有多個，使用能夠平行處理的 BFS是比較好的做法。