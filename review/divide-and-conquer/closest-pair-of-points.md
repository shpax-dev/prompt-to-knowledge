# Closest Pair of Points

Given n points in the plane, find the pair with the smallest Euclidean distance. Brute force checks every pair in O(n²).

## One dimension

Sort the points in O(n log n), then scan adjacent pairs in O(n). The closest pair must be adjacent in sorted order.

## Two dimensions: divide and conquer

1. **Divide:** keep two lists, Pₓ sorted by x and P_y sorted by y. Split Pₓ at the median into a left half Q and a right half R, separated by a vertical line L. The lists Qₓ, Q_y, Rₓ, R_y can be built in O(n) without re-sorting.
2. **Conquer:** recursively find the closest pair in Q and the closest pair in R. Let δ be the smaller of the two distances.
3. **Combine:** the only pair that could beat δ has one point on each side.
   - **Claim:** such a pair must have both points within δ of L. Call those points S, kept sorted by y.
   - **Lemma:** if s, s′ ∈ S with d(s, s′) < δ, then they are within 15 positions of each other in S_y.
   - *Proof idea:* cover the strip with δ/2 × δ/2 squares. Each square holds at most one point, since two points in one square would be closer than δ while on the same side. Points 16 positions apart are separated by at least 3 rows of squares, which is a distance of at least 3δ/2.
   - So, for each point in S_y, compare it with the next 15 points only. That is O(n) work.

## Runtime

Presorting takes O(n log n). After that there are 2 recursive calls of size n/2, plus O(n) work to split and check the strip:

```text
T(n) ≤ 2T(n/2) + cn = O(n log n)
```

**Correctness:** by induction on the number of points, using the strip claim and the 15-position lemma.

**Pitfall:** sorting by y inside every recursive call adds a log factor, giving O(n log² n). The O(n log n) bound depends on passing the presorted lists down.

Compare the same recurrence in [[review/divide-and-conquer/merge-sort-and-the-paradigm|merge sort]].

Source: `Lecture 5_ Divide and Conquer Strategies Part B.pdf`, PDF pp. 60–114. The geometry figures on PDF pp. 84–102 are diagrams.
