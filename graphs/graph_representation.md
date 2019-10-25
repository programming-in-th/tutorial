# Graph Representation

## Adjacency Matrix

ถ้ารับ edge `(u, v)` มาก็แค่เซตให้ `Matrix[u][v] = 1` (ถ้าเป็น undirected graph ก็ `Matrix[v][u] = 1` ด้วย)

## Adjacency List

สำหรับ Adjacency List ให้ใช้ array of vectors เอา แบบนี้

```cpp
vector<int> G[num_of_nodes];
// เพิ่ม edge (u,v) => G[u].push_back(v);
```

เวลาต้องการหาทุก node ที่อยู่รอบ ๆ node u ใด ๆ สามารถใช้การลูปแบบนี้ได้
```cpp
for (auto v : G[u]) {
    // อยากทำอะไรก็เชิญ
}
```

ถ้าเป็นกราฟที่มีน้ำหนัก แนะนำให้ใช้เป็น pair<int, int> เอา โดยที่ตัวแรกเก็บ node ปลายทาง ส่วนตัวที่สองเก็บน้ำหนัก แบบนี้

```cpp
using pii = pair<int, int>; // หรือจะ typedef ก็แล้วแต่สะดวก
vector<pii> G[num_of_nodes];
// เพิ่ม edge (u,v,w) => G[u].push_back({v, w});
// หรือจะใช้เป็น G[u].emplace_back(v, w); ก็ได้
```

## Edge List

ถ้ารับ edge `(u, v)` มา ก็ยัดเป็น `pair<int, int>` ใส่ `vector` ไว้ได้เลย หรือถ้ากราฟมีน้ำหนัก อาจจะประกาศ `struct` ขึ้นมาก่อน หรือใช้ `pair<pair<int, int>, int>` ก็ได้

Credits: [toi14-tutorial](https://github.com/aquablitz11/toi14-tutorial) by aquablitz11