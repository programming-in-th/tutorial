# Bridges and Articulation Points

กำหนดกราฟที่เชื่อมต่อกัน (Connected Graph) มาให้ เราจะเรียก node `u` ว่าเป็น Articulation Point (หรือ Cut Vertex) ก็ต่อเมื่อ การตัด node `u` ออก จะทำให้กราฟขาดออกจากกัน (ไม่ connected)

เราจะเรียก edge `(u, v)` ว่าเป็น Bridge ก็ต่อเมื่อ การตัด edge `(u, v)` ออก จะทำให้กราฟขาดออกจากกัน (ไม่ connected)

หากโจทย์ต้องการให้หา Bridge หรือ Articulation Point วิธีที่ง่ายที่สุดวิธีหนึ่งคือการทดลองตัดทุก node/edge แล้วทำการ DFS เพื่อหาว่ากราฟยังคงมี component เดียวอยู๋หรือไม่

วิธีนี้ หากใช้หา Articulation Point จะใช้เวลา `O(V(V+E))` หากใช้หา Bridge จะใช้เวลา `O(E(V+E))` ซึ่งใช้ไม่ได้ หากกราฟมีขนาดใหญ่

เราสามารถใช้ Tarjan's Algorithm หา Articulation Point หรือ Bridge ได้ใน `O(V+E)` ดังนี้

## นิยามเกี่ยวกับ edge ใน Depth-first Search

ในการทำ DFS สังเกตว่า เมื่ออยู่ที่ node `u` แล้วเราพิจารณา node `v` ซึ่งอยู่ติดกับ node `u` และไม่ใช่ parent ในการ DFS ของ node `u` จะมีอยู่สองความเป็นไปได้
1) ยังไม่เคย visit node `v` (ต้อง recursive ต่อไป)
2) เคย visit node `v` อยู่แล้ว (นั่นคือ เจอ cycle, return)

เราจะเรียก edge `(u, v)` ว่าเป็น tree edge หากตรงตามเงื่อนไขที่ 1 แต่หากตรงตามเงื่อนไขที่ 2 เราจะเรียกว่าเป็น back edge

หากวาดกราฟโดยใช้เฉพาะ tree edge สังเกตว่าเราจะได้ spanning tree ออกมา ส่วน back edge เป็นเหมือนส่วนเสริมที่ทำให้เกิด cycle ต่าง ๆ ในกราฟ

## นิยาม Discovery Time และ Lowlink Value

ระหว่างทำ DFS เราจะเก็บตัวแปรเพื่อนับลำดับ node ที่เจอ โดยทุกครั้งที่เจอ node ใหม่ เราจะเพิ่มค่า counter ดังกล่าว แล้วเก็บค่าไว้ใน `disc[u]` (`u` คือ node ใหม่ที่พิจารณาอยู่)

ต่อมา ขอนิยามให้ `low[v]` หมายถึง ค่า `disc` ที่น้อยที่สุดที่เป็นไปได้ หากเดินทางลงไปใน subtree บน DFS tree ของ node `v` แล้วเดินทางผ่าน back edge ไม่เกิน 1 เส้น

เมื่อพิจารณา node `u` อยู่ ตอนแรกเราจะกำหนดให้ `low[u] = disc[u]` แล้วเมื่อพิจารณา node รอบ ๆ node `u` - สมมุติว่าพิจารณา node `v` อยู่ (`v` ไม่ใช่ parent ของ node `u` ในการ DFS)

ถ้า `(u, v)` เป็น tree edge (นั่นคือยังไม่เคย visit node `v`) ให้ทำการ DFS ไปยัง node `v` จนเสร็จสิ้น แล้วกำหนดให้ `low[u] = min(low[u], low[v])` (เพราะ `low[v]` จะเก็บค่าน้อยสุดที่เป็นไปได้ หากเราเดินทางผ่าน `(u, v)` ลงไป ก็จะเดินทางไปถึงค่าดังกล่าวได้เช่นกัน นำมากำหนดเป็นค่า `low[u]` ได้)

ถ้า `(u, v)` เป็น back edge (นั่นคือ เคย visit node `v` แล้ว) ไม่จำเป็นต้อง DFS ซ้ำแล้ว แต่ให้กำหนด `low[u] = min(low[u], disc[v])` - สังเกตว่าในทีนี้เราไม่สามารถใช้ `low[v]` ในการปรับค่าได้แล้ว เพราะเราเดินทางผ่าน `(u, v)` ซึ่งเป็น back edge ไปแล้วเส้นหนึ่ง ตามนิยาม จะใช้ back edge ต่ออีกเส้นหนึ่งไม่ได้

## คุณสมบัติของ Articulation Point

สำหรับ node `u` ใด ๆ หากพิจารณา tree edge `(u, v)` แล้วพบว่า `low[v] >= disc[u]` นั่นคือ การเดินทางลงไป node `v` เราไม่สามารถหา back edge กลับขึ้นมาเหนือ node `u` ได้ จะสรุปได้ว่า node `u` เป็น articulation point

หากตัด node `u` ทิ้ง จะทำให้กราฟขาดออกจากกันแน่นอน เพราะ node ตั้งแต่ `v` ลงไป ไม่มีทางกลับขึ้นมาเหนือ node `u` ได้ นั่นคือ ไม่สามารถติดต่อกับส่วนอื่น ๆ ของกราฟได้

อนึ่ง มีข้อยกเว้น กรณีที่ node `u` เป็น root ของการ DFS จะใช้เงื่อนไขดังกล่าวไม่ได้ เพราะยังไงก็ไม่มี node ไหนเชื่อมขึ้นไปเหนือ root ได้อีกแล้ว เราจะใช้เงื่อนไขจำนวน node ลูกแทน หากมีจำนวน node ลูกซึ่งเชื่อมต่อกันด้วย tree edge มากกว่า 1 node จะถือว่า node `u` เป็น articulation point

## คุณสมบัติของ Bridge

สำหรับ tree edge `(u, v)` ใด ๆ หากพบว่า `low[v] > disc[u]` นั่นแปลว่า ทุก node ตั้งแต่ `v` ลงไป ไม่มี back edge กลับขึ้นมาเหนือเส้น `(u, v)` ได้ จะสรุปได้ว่าเส้น `(u, v)` เป็น Bridge

## โค้ดของ Tarjan's Algorithm

```cpp
// adjacency list
vector<int> G[N];

bool visited[N];
int disc[N], low[N];

set<int> ap; // answer: articulation points
set<pii> bridge; // answer: bridges

int counter = 0;
void tarjan(int u, int p) { // p = parent of u
    visited[u] = true;
    low[u] = disc[u] = ++counter;
    int child = 0;
    for (auto v : G[u]) {
        if (!visited[v]) {
            ++child;
            tarjan(v, u);
            low[u] = min(low[u], low[v]);
            // articulation point
            // ตามเงื่อนไขด้านบน (ในที่นี้ ให้ root มี parent เป็น 0)
            if ((p != 0 && low[v] >= disc[u]) || (p == 0 && child > 1))
                ap.insert(u);
            // bridge
            if (low[v] > disc[u])
                bridge.insert(pii(u, v));
        } else if (v != p) {
            low[u] = min(low[u], disc[v]);
        }
    }
}
```

อนึ่ง เราไม่จำเป็นต้องใช้ `visited` array ก็ได้ เพราะตอนแรก `disc` ทุกช่องทีย่ังไม่เคย visit จะมีค่าเท่ากับ 0 - ดังนั้นเช็คค่าจาก `dist` เอาก็ได้

Credits: [toi14-tutorial](https://github.com/aquablitz11/toi14-tutorial) by aquablitz11
