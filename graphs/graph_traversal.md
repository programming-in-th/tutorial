# Graph Traversal

หลัก ๆ ก็มี Depth-first Search กับ Breadth-first Search นอกจากนี้ ยังต้องนำมาประยุกต์เป็นอีกด้วย ยกตัวอย่างการประยุกต์ดังนี้

## ใช้ DFS หา cycle ใน undirected graph

หลักการคร่าว ๆ คือ เราจะ DFS ไปเรื่อย ๆ แล้วจด node ที่เคยเจอไว้ ถ้าเดินทางไป ๆ มา ๆ วนกลับมา node เดิมที่เคย visit แล้วก็ถือว่าเจอ cycle
ต้องระวังไว้สองอย่างหลัก ๆ คือ 1) ต้องเก็บ parent ไว้ตลอด เพราะเวลา dfs เราอาจจะเผลอเดินจาก node `u` ไป `v` แล้วเดินจาก `v` กลับมา `u` ซ้ำผ่าน edge เดิม และ 2) กราฟอาจจะ disconnect ออกจากกันได้ ดังนั้น ต้องลูปทดลองให้ครบ มั่นใจว่าทดลองทุก node แล้ว

```cpp
bool visited[N]; // เริ่มมาเซตทุกอย่างเป็น false

// dfs จะ return true ถ้าเจอ cycle, false ถ้าไม่เจอ cycle
// p คือ parent, ต้องเก็บไว้ด้วย กันเดิน edge เดิมซ้ำ
bool dfs(int u, int p) {
    if (visited[u]) // เจอ cycle แล้ว
        return true;
    visited[u] = true;
    for (auto v : G[u]) {
        if (v == p) // skip ข้าม edge parent ไป
            continue;
        if (dfs(v, u)) // ถ้า dfs ไปแล้วพบว่าเจอ cycle ก็ให้ return true ตาม
            return true;
    }
    return false; // ถ้าทำจนหมดแล้วยังไม่เจอ ก็ตอบ false
}

// ใน main:
bool found_cycle = false;
for (int u = 1; u <= n; ++u) {
    if (!visited[u]) {
        if (dfs(u)) {
            found_cycle = true;
            break;
        }
    }
}
// ต้องลูปทดลอง node ที่ยังไม่เคยไป เพราะกราฟอาจจะแยกจากกันได้
```

## ใช้ DFS หา cycle ใน directed graph

สำหรับ Directed Graph จะต่างออกไป เราใช้ `visited` เฉย ๆ ไม่ได้แล้ว แต่จะใช้ `color` แทน โดยแทนสถานะดังนี้
- **สีขาว (color 0)** แทน node ที่ยังไม่ได้เริ่ม visit แม้แต่นิดเดียว
- **สีเทา (color 1)** แทน node ที่ visit แล้ว และกำลังอยู่ระหว่างการ DFS ตัวลูก ๆ อยู่
- **สีดำ (color 2)** แทน node ที่ visit แล้ว และจัดการ DFS ตัวลูก ๆ ครบหมดแล้ว

ตอนแรกทุก node เป็นสีขาว และเมื่อ DFS ตอนแรกเราจะเซต node นั้นเป็นสีเทา แล้วทำการ DFS ตัวที่อยู่ติดกันทั้งหมด เมื่อทำครบแล้วจึงเซต node นี้เป็นสีดำ ถ้าเผลอเดินทางไป node ใดที่เป็นสีเทาอยู่ซ้ำ ถือว่าเจอ cycle

เหตุผลที่ต้องใช้สี เพราะต้องกันกรณีเคสแบบนี้: `2 --> 1 --> 3` สังเกตว่าเคสนี้ ถ้าเราใช้ `visited` array แล้วเริ่ม DFS จาก node 1 ก่อน จะทำให้ `visited[1] = visited[3] = true` และเมื่อเรามาลอง DFS node 2 ต่อ เราจะมาเจอ node 1 ซึ่งเคย visit แล้ว ทำให้อัลกอริทึมนึกว่ามี cycle ทั้ง ๆ ที่มันไม่มี

นั่นคือ การที่จะเจอ cycle ได้นั้น ต้องเจอระหว่าง DFS ในรอบเดียวกันเท่านั้น ไม่ใช่เจอ node ที่เคย visited เพราะการ DFS รอบอื่น

```cpp
int color[N]; // เริ่มมาเป็น 0 หมด
bool dfs(int u) {
    if (color[u] == 1) // เจอ cycle
        return true;
    if (color[u] == 2) // เจอ node ที่เคยผ่านในรอบอื่นแล้ว
        return false;
    color[u] = 1; // ตอนเริ่มทำ เซตเป็นสีเทา
    for (auto v : G[u]) {
        if (dfs(v)) // ถ้า dfs แล้วเจอ cycle ก็ return true เลย
            return true;
    }
    color[u] = 2; // พอทำเสร็จแล้วก็เซตเป็นสีดำ
}

// main: คล้าย ๆ กับด้านบน
```

## ใช้ DFS เพื่อนับจำนวน component ใน undirected graph

ทำคล้าย ๆ 1.2.1 แต่เราจะไม่ return อะไรทั้งสิ้น แค่ DFS เพื่อมาร์คว่าจุดไหนเคยไปแล้วเฉย ๆ ทำไปเรื่อย ๆ แล้วพบว่า เราต้องเริ่ม DFS ทั้งหมดกี่ครั้งจึงจะมาร์คครบทั้งกราฟ

```cpp
bool visited[N];
void dfs(int u) {
    if (visited[u])
        return;
    visited[u] = true;
    for (auto v : G[u])
        dfs(v);
}

// main:
int component = 0;
for (int u = 1; u <= n; ++u) {
    if (!visited[u]) {
        ++component;
        dfs(u); // dfs เพื่อมาร์คทุก node ใน component ว่าเคยเจอแล้ว
    }
}
```

นอกจากใช้นับจำนวน component แล้ว แทนที่จะใช้เป็น visited เฉย ๆ ก็อาจจะเก็บเลข component ไว้ด้วยก็ได้ (component แรกเป็นเลข 1, ถัดมาเป็นเลข 2 ...) ทำให้เราสามารถตอบได้อย่างรวดเร็วว่า node 2 node อยู่ component เดียวกันหรือไม่ (ถ้าตัวเลขเหมือนกันก็คือ component เดียวกัน)

## ใช้ BFS หาเส้นทางสั้นสุดใน unweighted graph

เนื่องจากว่า Breadth-first Search จะมีลักษณะคล้าย ๆ การกระจายเป็นชั้น ๆ ออกไป ถ้าทุกเส้นเชื่อมในกราฟน้ำหนักเท่ากันหมด เราสามารถใช้ BFS ในการหาเส้นทางสั้นสุดจาก node นึงออกไปหาอีก node นึงได้

```cpp
int level[N];

int shortest_path(int start, int target) {
    // fill ทุกช่องเป็น -1 เปรียบเสมือน visited[u] = false
    fill(level, level+N, -1);

    queue<int> Q;
    level[start] = 0;
    Q.push(start);

    while (!Q.empty()) {
        int u = Q.front();
        Q.pop();
        if (u == target) // เจอคำตอบแล้ว
            return level[u];
        for (auto v : G[u]) {
            if (level[v] == -1) { // กระจายไปยัง node รอบ ๆ ที่ยังไม่เคยไป
                level[v] = level[u]+1;
                Q.push(v);
            }
        }
    }

    return -1; // ถ้าทำมาถึงตรงนี้แล้ว แปลว่าไม่มีเส้นทางจาก start ไปถึง target 
}
```

## ใช้ DFS เพื่อเช็คความเป็น bipartite graph

กราฟจะเป็น Bipartite Graph ก็ต่อเมื่อเราสามารถระบายสี node แต่ละ node ด้วย 1 ใน 2 สี โดยที่ แต่ละ node ที่มีเส้นเชื่อมกันต้องมีสีต่างกัน

สามารถเช็คได้ง่าย ๆ โดยการ DFS ทดลองระบายสี โดยเราจะระบายสีสลับกันไปเรื่อย ๆ แต่ถ้าวนกลับมา node เดิมที่เคยระบายแล้วพบว่าสีที่ต้องการระบาย กับสีที่ระบายลงไปแล้วขัดแย้งกัน จะถือว่ากราฟดังกล่าวไม่เป็น Bipartite Graph

```cpp
int color[N]; // 0 = ยังไม่ได้ลงสี, 1 = ลงสีแรก, 2 = ลงสีที่สอง, ตอนแรกเป็น 0 หมด

// dfs, ต้องการระบายสี node u เป็นสี c
// return true ถ้ายัง*อาจจะ*เป็น bipartite graph อยู่, false ถ้าเป็นไม่ได้แน่นอน
bool dfs(int u, int c) {
    if (color[u] != 0) { // ถ้าเคยระบายสีแล้ว
        // สีต้องตรงกัน ถ้าไม่ตรงกันจะต้อง return false
        return (color[u] == c);
    }
    color[u] = c; // ระบาย
    for (auto v : G[u]) {
        // ระบาย node รอบ ๆ แต่ว่ากลับสี
        if (!dfs(v, c == 1 ? 2 : 1)) // ถ้าเละ ก็ให้ return false
            return false;
    }
    return true; // ถ้ารอดมาได้ ก็ยังมีสิทธิ์เป็น bipartite graph
}

// main:
bool isbipartite = true;
for (int u = 1; u <= n; ++u) {
    if (color[u] == 0) {
        if (!dfs(u, 1)) { // ถ้าลองระบาย (เริ่มจากสี 1) ใน component นั้นแล้วเละ ก็ถือว่าไม่เป็น bipartite graph
            isbipartite = false;
            break;
        }
    }
}
// กราฟอาจจะ disconnect กัน, ต้องลองเติมสีให้ครบทุก component
```

Credits: [toi14-tutorial](https://github.com/aquablitz11/toi14-tutorial) by aquablitz11
