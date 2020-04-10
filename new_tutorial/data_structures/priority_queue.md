# Priority Queue

Priority Queue เป็น Abstract Data Type ที่รองรับ operation 2 อย่างได้แก่

-   `push(x)` นำข้อมูล `x` ใส่เข้าไปใน Priority Queue
-   `pop()` นำข้อมูลตัวที่มี priority สูงสุดออกจาก Priority Queue

สังเกตว่า Priority Queue จะมีการจัดลำดับความสำคัญให้กับข้อมูล โดยข้อมูลที่สำคัญที่สุดจะถูกดึงออกมาก่อน เช่น หากเก็บจำนวนใน Min Priority Queue ข้อมูลที่มีค่าน้อยที่สุดจะถูกดึงออกมาก่อน เป็นต้น

โดยปกติแล้วจะ implement Priority Queue โดยใช้โครงสร้างข้อมูล Binary Heap ถึงอย่างไรก็ตาม ใน STL มี `priority_queue` เตรียมไว้ให้ใช้แล้ว สามารถใช้ได้เลย

ในที่นี้ ขอไม่กล่าวถึงรายละเอียดการทำงานของ Binary Heap

## STL `priority_queue`

`priority_queue` ของ STL ปกติจะทำงานโดยให้ความสำคัญกับตัวเลขที่มีค่ามากที่สุดก่อน นั่นคือทำงานเป็น Max Priority Queue ฟังก์ชันที่มีให้ใช้งานได้แก่ `push` (เพิ่มข้อมูลเข้าไป), `top` (ดูข้อมูลตัวที่มีค่ามากสุด), `pop` (นำข้อมูลตัวบนสุดออก)

หากต้องการใช้ Min Priority Queue เราอาจจะปรับวิธีการเปรียบเทียบข้อมูลได้ เช่น หากจะเก็บข้อมูลจำนวนเต็ม ตามปกติแล้ว Max Priority Queue จะใช้ `less<int>` ในการเปรียบเทียบ แต่หากต้องการใช้ Min Priority Queue เราจะต้องใช้ `greater<int>` ในการเปรียบเทียบ ดังตัวอย่างด้านล่าง

```cpp
priority_queue<int> maxheap;
maxheap.push(3);
maxheap.push(5);
maxheap.push(1);
printf("%d\n", maxheap.top()); // got 5
maxheap.pop();
printf("%d\n", maxheap.top()); // got 3
maxheap.pop();

priority_queue<int, vector<int>, greater<int>> minheap;
minheap.push(3);
minheap.push(5);
minheap.push(1);
printf("%d\n", minheap.top()); // got 1
minheap.pop();
printf("%d\n", minheap.top()); // got 3
minheap.pop();
```

ทั้งนี้ ให้ศึกษาข้อมูลฟังก์ชันอื่น ๆ เพิ่มเติมจาก reference

Credits: aquablitz11
