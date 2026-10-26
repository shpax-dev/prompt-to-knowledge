# Segmented Least Squares

Given points (x₁, y₁), …, (xₙ, yₙ) with x₁ < x₂ < … < xₙ, a single line y = ax + b has squared error ∑ (yᵢ − axᵢ − b)². Fitting one line to data that bends is a poor fit. Fitting a separate line to every pair of points is overfitting.

**Problem:** partition the points, by x, into contiguous segments. Minimize the total error of the best line in each segment, plus a fixed cost C per segment. C trades accuracy against the number of segments.

## The DP

**Definitions:** e(i, j) is the minimum squared error of one line through points i..j.

**Matrix:** a 1-D array s, where s[j] is the optimal cost for points 1..j.

**Bellman equation:** the last segment starts at some point i.

```text
s[0] = 0
s[j] = min over 1 ≤ i ≤ j of ( e(i, j) + C + s[i − 1] )
```

**Solution and order:** the answer is s[n]. Fill j = 1..n.

**Runtime:**

- Precomputing every e(i, j) takes O(n³) as the lecture presents it.
- The table has O(n) cells, and cell j costs O(j), so filling it takes O(n²).

**Reconstruction:** store the minimizing i for each j, then follow those indices back from n to recover the segments.

**Pitfall:** C is added once *per segment*. That is what stops the DP from using n − 1 tiny segments with zero error.

See [[review/dynamic-programming/dynamic-programming-method|the DP method]].

Source: `Lecture 10_ Dynamic Programming In Depth Examples and Applications.pdf`, PDF pp. 222–237. The lecture marks this topic with an asterisk in its navigation bar.
