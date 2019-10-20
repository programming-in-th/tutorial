# Topics List

Strings

- Hashing
  - Guide to polynomial hashing: [Link](https://www.mii.lt/olympiads_in_informatics/pdf/INFOL119.pdf)
  - Simple polynomial hashing guidelines: [Link](https://codeforces.com/blog/entry/52697)
  - Why picking $2^{64}$ as a modulo is a bad idea: [Link](https://codeforces.com/blog/entry/4898)
- KMP
  - TopCoder explanation: scroll down to KMP section at [Link](https://www.topcoder.com/community/competitive-programming/tutorials/introduction-to-string-searching-algorithms/)
  - Johnny Ho's explanation: scroll down to his answer at [Link](https://www.quora.com/Why-is-the-time-complexity-of-the-Knuth-Morris-Pratt-algorithm-O-n)
- Suffix arrays
  - A suffix array is a lexicographically sorted array of all of a string's suffixes. In order to save space, we usually only store the first index of the suffix rather than the actual string representation of the suffix.
  - LCP stands for the longest common prefix of two strings, which indicates the maximum integer $k$ where the first $k$ characters of both strings are the same.
  - Full explanation and implementation of Suffix Array can be found below
- Z Algorithm: [Link](https://www.hackerearth.com/practice/algorithms/string-algorithm/z-algorithm/tutorial/)

# Suffix Array Construction with Prefix Doubling

When the array of strings is composed of only suffixes, a very beneficial property manifests. Assuming the suffix array is already sorted by the first $2^i$ cells, we need to further sort it by the **next** (not last) $2^i$ cells. However, the order of the next $2^i$ cells is already known from our previous work because it is, itself, a suffix. Therefore, we can keep on doubling the prefix and sort it by the order of the last $2^i$ cells.

Implementation:

```c
// pos = position of suffix i in suffix array
// tmp = cumulative sum of inequalities
int gap;
for(int i = 0; i < n; i++) sa[i] = i, pos[i] = s[i];
auto cmp = [&] (int i, int j) { // i, j are positions in the actual string
  if(pos[i] != pos[j]) return pos[i] < pos[j];
  i += gap;
  j += gap;
  return ((i < n && j < n) ? pos[i] < pos[j] : i > j);
  // i > j because guaranteed that the one out of bounds will be empty from right after the gap is added (since positions denote START of the suffix), therefore simply index comparison is OK
}
for(gap = 1;; gap <<= 1) {
  sort(sa, sa+n, cmp);
  for(int i = 1; i < n; i++) tmp[i] = tmp[i-1] + cmp(sa[i-1], sa[i]);
  for(int i = 0; i < n; i++) pos[sa[i]] = tmp[i];
  if(tmp[n - 1] == n - 1) break; // if every adjacent index is not equal (sorted completely)
}
```

# Kasai's algorithm

Observation: given two suffixes with positions in the **suffix array** $i$, $i+1$ = $sa[i]$, $sa[i+1]$ and $LCP(sa[i], sa[i+1]) > 0$, if we remove the first character of both $sa[i]$ and $sa[i+1]$, then the new $LCP(sa[j], sa[j+1]) \geq LCP(sa[i], sa[i+1]) - 1$

We can spawn $sa[j]$, since removal of the first character also gives a suffix, therefore it can be found in the suffix array.

In the algorithm, we will iterate **positions in the string** (not SA!) to guarantee that we remove the first character in each step, and move on to another suffix (from $sa[i]$ to $sa[j]$). The whole reason behind having to remove the first character is to guarantee $O(n)$ time. Since the new LCP ≥ old LCP - 1, all we need to do is increase the LCP until it is no longer possible.

Implementation:

```c
// pos is the position of suffix i in the suffix array
for(int i = 0; i < n; i++, k = (k ? k-1 : 0)) {
  if(pos[i] == n-1) {
    k = 0;
    continue;
  }
  int j = sa[pos[i] + 1]
  while(min(i + k, j + k) < n && s[i + k] == s[j + k]) ++k;
  lcp[pos[i]] = k; // in implementation, lcp is defined on positions in SA rather than strings themselves
}
```

The running time is $O(n)$ because $k$ is decreased only $n$ times, and $k$ can only be incremented up to $n$, therefore total number of operations $\leq 2n$
