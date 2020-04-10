# Topological Sorting

สำหรับกราฟระบุทิศทางที่ไม่มี cycle (Directed Acyclic Graph หรือ DAG) เราสามารถหมายเลข node เป็นลำดับได้ โดยที่ สำหรับทุก edge `(u, v)` node `u` จะอยู่ก่อน `v` ในลำดับเสมอ (กราฟหนึ่งอาจจะสร้าง Topological Sorting ได้หลายลำดับ)

เพื่อให้เข้าใจง่ายขึ้น เราอาจจะมองว่ากราฟเป็นตัวระบุลำดับการทำงานว่า เราต้องทำงานอะไรก่อนหลัง (edge `(u, v)` คือต้องทำงานชิ้นที่ `u` ก่อนจึงอนุญาตให้ทำงานชิ้นที่ `v`) ยกตัวอย่าง หากกราฟมี 4 node และมี edge `(1, 4)`, `(3, 2)`, `(3, 1)` นั่นหมายความว่า
- เราต้องทำงาน `1` ก่อนงาน `4`
- เราต้องทำงาน `3` ก่อนงาน `2`
- เราต้องทำงาน `3` ก่อนงาน `1`

ลำดับการทำงานที่เป็นไปได้ เช่น `3, 1, 2, 4` สังเกตว่าลำดับนี้ไม่ขัดกับเงื่อนไขที่กำหนดให้ ดังนั้น ลำดับนี้ถือว่าเป็น Topological Sorting (อาจจะย่อว่า Topo-sort) ของกราฟดังกล่าว

สังเกตว่า ถ้ากราฟมี cycle เราจะไม่สามารถหาลำดับได้ เช่น หากกราฟมี edge `(1, 2)` และ edge `(2, 1)` เราต้องทำงาน `1` ก่อนงาน `2` แต่เราต้องทำงาน `2` ก่อนงาน `1` เช่นกัน ซึ่งขัดแย้งกัน

การหา Topological Sorting สามารถทำได้ดังนี้
- หา in-degree ของแต่ละ node, in-degree แสดงถึงจำนวนงานที่ต้องทำก่อนที่จะทำงานดังกล่าวได้ (node ที่มี in-degree = 0 ก็คือ เราสามารถทำงานนั้นได้ตั้งแต่แรก)
- นำ node ที่มี in-degree = 0 ใส่คิวพิจารณา
- นำ node นึงในคิวมาใส่ลำดับ topo-sort แล้วลด in-degree ของ node รอบ ๆ ไป 1 (เปรียบเสมือนว่าทำงานนั้นเสร็จแล้ว)
- ถ้าลด in-degree ของ node ใดแล้วกลายเป็น 0 นั่นแปลว่างานนั้นเราสามารถทำได้แล้ว ก็ให้นำ node นั้นใส่คิวพิจารณาด้วย
- ทำไปเรื่อย ๆ จนกว่าจะนำทุก node ใส่ในลำดับ topo-sort ครบ

ถ้าเรานำ node ใส่ sequence ได้ไม่ครบทุก node แปลว่ากราฟดังกล่าวมี cycle ซึ่งทำให้เราไม่สามารถหา topological sorting ได้

```cpp
int indeg[N];
// สำหรับทุก edge (u, v) ให้ ++indeg[v]

queue<int> Q;
for (int u = 1; u <= n; ++u) {
    if (indeg[u] == 0)
        Q.push(u);
}

vector<int> seq; // sequence
while (!Q.empty()) {
    int u = Q.front();
    Q.pop();
    seq.push_back(u); // นำใส่ topo-sort
    for (auto v : G[u]) {
        --indeg[v]; // ลด in-degree node อื่น ๆ
        if (indeg[v] == 0) // ถ้าเหลือ 0 ก็ใส่คิว
            Q.push(v);
    }
}

// print sequence
// if seq.size() != n: fail
```

อัลกอริทึมนี้เรียกว่า Kahn's Algorithm โดยทำงานได้ใน `O(n+m)` 

นอกจากนี้ เราสามารถใช้ DFS ในการหา topo-sort ได้ สามารถศึกษาได้ที่ https://www.geeksforgeeks.org/topological-sorting/

Credits: [toi14-tutorial](https://github.com/aquablitz11/toi14-tutorial) by aquablitz11