# Minimum Spanning Tree

Minimum Spanning Tree (MST) คือต้นไม้แผ่ทั่วของกราฟ ที่มีผลรวมของน้ำหนักของเส้นเชื่อมน้อยที่สุดเท่าที่เป็นไปได้ หลัก ๆ แล้วมี algorithm ที่ใช้แก้ 2 อย่างคือ Kruskal's Algorithm และ Prim's Algorithm

## Kruskal's Algorithm

หลักการคร่าว ๆ ดังนี้
- ตอนแรก เราจะมีกราฟว่าง ๆ ที่มีแต่ node แต่ไม่มี edge
- sort edge เรียงตามน้ำหนักจากน้อยไปมาก
- ค่อย ๆ ทดลองเพิ่มทีละ edge โดยจะเพิ่มได้ก็ต่อเมื่อเพิ่มแล้วทำให้ไม่เกิด cycle
- ถ้าเพิ่มได้ก็เพิ่มเลย แต่ถ้าเพิ่มไม่ได้ก็ข้าม edge นั้นไป
- ทำไปเรื่อย ๆ จนกว่าครบทุก edge

ปัญหาจะอยู่ที่การเช็คว่าการเพิ่ม edge `(u, v)` จะทำให้เกิด cycle หรือไม่ วิธีการเช็คหลัก ๆ แล้วมีสามวิธี
- เช็คว่า node `u` และ `v` อยู่ component เดียวกันหรือไม่โดยการ DFS - ถ้าพบว่าอยู่ component เดียวกัน เราไม่ควรเพิ่ม edge `(u, v)` เพราะจะทำให้เกิด cycle
- ทดลองเพิ่มไปเลย แล้วใช้ DFS เช็ค cycle ในกราฟ ถ้าพบว่ามี cycle ก็ค่อยลบ edge `(u, v)` ทิ้ง
- ใช้ Data Structure ที่ใช้ว่า Union-find Disjoint set (DSU) เพื่อที่จะเช็คได้อย่างรวดเร็วว่าอยู่ใน component เดียวกันหรือไม่

โดยปกติแล้ว เวลา implement Kruskal's เราจะใช้ DSU เพราะวิธีอื่นจะเสียเวลาในการทำ Graph Traversal เป็นอย่างมาก

สำหรับรายละเอียดของ DSU ให้อ่านในหัวข้ออื่น ๆ ตอนนี้ทราบแค่ว่า DSU ทำได้สองอย่างคือ

1. `merge(u, v)` การรวม component ของ node `u` กับ `v` เข้าด้วยกัน (เพิ่ม edge `(u, v)`)
2. `find(u)` หาว่า node `u` อยู่ component ใด (เราเทียบ node `u` กับ node `v` - ถ้าอยู่ component เดียวกัน ห้ามเพิ่ม edge)

โค้ดของ Kruskal's Algorithm เป็นดังนี้

```cpp
// disjoint set functions
int find(int u);
int merge(int u, int v);

// สมมุติว่าเก็บกราฟเป็น edge list
struct Edge {
    int u, v, w;
    bool operator<(const Edge &rhs) const {
        return w < rhs.w;
        // ฟังก์ชันสำหรับ compare Edge (compare ตาม weight)
    }
};
vector<Edge> edges;

// kruskal's algorithm
sort(edges.begin(), edges.end());
int sum = 0;
for (auto edge : edges) { // พิจารณาทีละ edge เรียงจากน้อยไปมาก
    if (find(edge.u) == find(edge.v))
        continue; // ถ้าอยู่ component เดียวกัน ห้ามเพิ่ม edge
    // ถ้าไม่ได้อยู่ component เดียวกัน เพิ่ม edge ได้
    sum += edge.w;
    merge(edge.u, edge.v);
}
printf("%d\n", sum);
```

## Prim's Algorithm

หลักการทำงานคร่าว ๆ ดังนี้
- เราจะเริ่มต้นจาก node ใดก็ได้ node หนึ่งก่อน
- เลือก node ที่อยู่ชิดกับ node นี้ ที่เชื่อมโดย edge ที่สั้นที่สุด แล้วแผ่ไปยัง node นั้น
- ตอนนี้มี 2 node ละ ใน 2 node นี้ก็ให้พิจารณา node รอบ ๆ แล้วเลือก node ที่เราแผ่ไปได้สั้นที่สุด เป็น 3 node
- ทำเรื่อย ๆ จนกว่าจะแผ่ครบทั้งกราฟ

(รู้สึกว่าอธิบายยาก แนะนำให้ดู https://visualgo.net/en/mst ดีกว่า)

ในการโค้ด เราจะใช้ `dist` array โดยนิยามให้ `dist[u]` คือ edge ที่สั้นที่สุดที่สามารถเชื่อมจาก node ที่ถูกแผ่มาถึงแล้ว มาถึง node `u` แล้วใช้ priority queue เพื่อเลือกแผ่ไปยัง node `u` ที่มีค่า `dist[u]` น้อยที่สุด

สังเกตว่าลักษณะคล้ายกับ Dijkstra's Algorithm มาก แค่เปลี่ยนนิยาม `dist[u]` จากเส้นทางสั้นสุดเป็น edge สุดท้ายที่สั้นที่สุดเฉย ๆ

จุดที่แตกต่างในโค้ด
- จากเดิม สูตรปรับ `dist[v] = dist[u]+w` เราใช้เป็น `dist[v] = w` เฉย ๆ (การเปรียบเทียบก็เช่นกัน)
- เราจะเก็บ sum ของ dist node ที่เลือกไว้ด้านนอกตลอด

```cpp

using pii = pair<int, int>;

vector<int> dist(n+1, INF);
vector<bool> visited(n+1, false);

priority_queue<pii, vector<pii>, greater<pii>> Q;
dist[start] = 0;
Q.push({dist[start], start});

int sum = 0;
while (!Q.empty()) {

    int u = Q.top().second, d = Q.top().first;
    Q.pop();
    if (visited[u])
        continue;
    visited[u] = true;
    sum += dist[u]; // <-- เพิ่ม edge เข้า MST

    for (auto vw : G[u]) {
        int v = vw.first;
        int w = vw.second;
        if (!visited[v] && w < dist[v]) { // ปรับ edge weight ให้น้อยลง
            dist[v] = w;
            Q.push({dist[v], v}) // ถ้าปรับได้ก็ยัดใส่ queue
        }
    }
}

```

เช่นเดียวกับ Dijkstra's Algorithm - Prim's Algorithm จะทำงานใน `O(m + n log n)`

สำหรับ Complete Graph แนะนำให้ใช้ Prim's Algorithm แบบดัดแปลง แทนที่จะใช้ Priority Queue เราจะใช้การลูปหา node ที่ `dist` น้อยสุดแทน จะทำให้ time complexity เป็น `O(n^2)`

## ปัญหาประยุกต์ MST

โจทย์บางข้อ ไม่ได้ถามหา MST ตรง ๆ แต่ต้องประยุกต์ใช้ MST จึงจะหาคำตอบได้ ยกตัวอย่างเช่นโจทย์ข้อนี้

> กำหนดกราฟไม่มีทิศทางที่มีน้ำหนักให้ ให้หาว่า หากต้องการเดินทางจาก node `u` ไปยัง node `v` จะต้องเสียพลังงานน้อยสุดเท่าใด โดยพลังงานจะพิจารณาจาก **น้ำหนักของ edge ที่หนักที่สุดที่ต้องเดินผ่าน** (ส่วน edge อื่น ๆ ไม่สน)

สังเกตว่านี่ไม่ใช่โจทย์ shortest path เพราะเราสนใจเพียงแค่ edge ที่หนักที่สุดเท่านั้น โจทย์ข้อนี้สามารถแก้ได้โดยการดัดแปลง Kruskal's Algorithm ดังนี้

- เราจะ sort edge จากน้อยไปมาก แล้วค่อย ๆ เพิ่มทีละ edge เช่นเดิม
- ให้หยุด algorithm ทันทีที่มีเส้นทางจาก `u` ไป `v` แล้วตอบ edge weight สุดท้ายที่เพิ่ม (ซึ่งเป็น edge ที่หนักที่สุด)

สังเกตว่า ทันทีที่มีเส้นทางจาก `u` ไป `v` นั่นคือ เราพบคำตอบที่ดีที่สุดแล้ว เพราะตลอด algorithm เราพิจารณา edge ที่น้อยที่สุดที่เป็นไปได้ตลอด

นอกจากนี้ อัลกอริทึมสำหรับหา Minimum Spanning Tree สามารถนำมาใช้หา **Maximum** Spanning Tree ได้ เพียงแค่ปรับการ sort เรียงจากมากไปน้อยเท่านั้น

Credits: [toi14-tutorial](https://github.com/aquablitz11/toi14-tutorial) by aquablitz11