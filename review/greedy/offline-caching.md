# Offline Caching (Farthest-in-Future)

A cache holds k pages from a larger universe U. Requests arrive for pages. A request for a page not in the cache is a **fault**, and serving it may require evicting a page. The goal is to minimize faults. In the **offline** version, the whole request sequence is known in advance.

## Farthest-in-Future (FF)

On a fault with a full cache, evict the page whose *next* request is farthest in the future. A page that is never requested again counts as infinitely far away.

**Derived trace:** U = {a, b, c}, k = 2, requests a, b, c, b, c, a, b.

| Request | Cache before | Result |
| :--- | :--- | :--- |
| a | {} | fault (cold) |
| b | {a} | fault (cold) |
| c | {a, b} | fault: a is next needed at position 6, b at position 4, so evict a |
| b | {b, c} | hit |
| c | {b, c} | hit |
| a | {b, c} | fault: b is needed again, c never is, so evict c |
| b | {a, b} | hit |

That is 4 faults in total, 2 of them cold (the first time each page is loaded). State whether you count cold faults.

## Optimality by exchange

**Theorem:** let S be any schedule that agrees with FF on the first j requests. Then there is a schedule S′ that agrees with FF on the first j + 1 requests and has no more faults than S.

**Proof sketch:**

- Both caches are identical after j steps, because they made the same evictions.
- At step j + 1, if no eviction is needed, or both evict the same page, set S′ = S.
- Otherwise S evicts f while FF evicts e. Build S′ to follow FF at step j + 1, so its cache differs from S's only in holding f instead of e. Then let S′ copy S until they must differ, at some request x:
  - If S evicts e, S′ evicts f, and the caches match again.
  - If S evicts some g ≠ e to bring in f, S′ evicts g and brings in e.
  - S cannot need e before f: FF evicted e precisely because f is requested sooner.
- In each case S′ makes no more faults than S, and afterwards the caches agree.

Start from an optimal S* and apply the theorem for j = 1, 2, …, n. The result is FF, with no more faults than S*. So FF is optimal.

**Pitfall:** FF needs the future, so it is a benchmark rather than a practical online policy. Online policies such as LRU are not covered in these slides.

The same exchange pattern appears in [[review/greedy/greedy-proof-techniques|greedy proof techniques]].

Source: `Lecture 8_ Greedy Algorithms Hard Level Problems.pdf`, PDF pp. 2–36.
