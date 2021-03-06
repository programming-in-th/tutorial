## 1.5.3 ตัวแปรค่าคงที่ (Constant variable)

"ตัวแปรค่าคงที่ (Constant variable)" คือ ตัวแปรที่สามารถกำหนดค่าได้เพียงครั้งเดียว หลังจากนั้น จะไม่สามารถเปลี่ยนแปลงค่าที่ตัวแปรนั้นเก็บไว้ได้อีก

โดยปกติแล้วเราจะใช้ตัวแปรค่าคงที่สำหรับเก็บค่าคงที่ทางคณิตศาสตร์ หรือค่าคงที่เฉพาะสำหรับโปรแกรมนั้น ๆ (เช่น ขนาดข้อมูลมากที่สุดที่รับได้ เป็นต้น)

ในภาษา C การประกาศตัวแปรค่าคงที่สามารถทำได้ดังนี้
```cpp
#include <stdio.h>
#define MAX_NUMBER 10 // <-- this line
int main(void)
{
    printf("The maximum value is %d.", MAX_NUMBER);
}
```

สังเกตว่าการประกาศตัวแปรค่าคงที่นั้นต่างจากตัวแปรทั่วไป โดยมีรูปแบบคือ
```cpp
#define ชื่อตัวแปร ค่าที่ต้องการ
```
และไม่มีเครื่องหมาย semicolon

---

**โจทย์**: จงเขียนโปรแกรมเพื่อคำนวณพื้นที่ของวงกลมจากรัศมีที่กำหนดให้
ให้สร้างตัวแปรจำนวนจริงชือว่า `radius` เพื่อเก็บรัศมี โดยให้กำหนดให้ `radius = 14`
สร้างตัวแปรค่าคงที่ `PI` เพื่อใช้ในการคำนวณ โดย `PI = 3.1415926`
แสดงผลคำตอบด้วยทศนิยม 2 ตำแหน่ง
โปรแกรมควรได้ผลลัพธ์ดังต่อไปนี้
```
Radius: 14
Area of Circle: 615.75
```

---

จากตัวอย่างโค้ดดังต่อไปนี้ จงทายว่าผลลัพธ์คืออะไร แล้วลองรันโปรแกรมดู
```cpp
#include <stdio.h>
#define NUMBER 3+2
int main(void)
{
    int x = NUMBER*5;
    printf("%d\n", x);
    return 0;
}
```
ผลลัพธ์ควรจะออกมาเป็น `5x5 = 25` แต่เรากลับได้ `13` เป็นคำตอบซะงั้น เกิดอะไรขึ้น?

---

ความจริงแล้ว ตัวแปรค่าคงที่ไม่ใช่ตัวแปรที่เก็บค่าจริง ๆ แต่เป็นการ copy-paste โค้ดเท่านั้น เพราะฉะนั้น บรรทัด
```cpp
int x = NUMBER*5;
จึงเปรียบเสมือน
int x = 3+2*5;
```
ซึ่งส่งผลให้ `x` มีค่าเท่ากับ `3+10 = 13` นั่นเอง

เพราะฉะนั้น เพื่อป้องกันปัญหานี้ หากมีการใช้ตัวดำเนินการบวก ลบ คูณ หาร ฯลฯ ในตัวแปรค่าคงที่ ควรใส่วงเล็บไว้ ดังนี้
```cpp
#define NUMBER (3+2)
```
เพื่อให้การคำนวณค่า `int x = (3+2)*5;` ออกมาดังที่ตั้งใจไว้

---

ในภาษา C++ เรามีตัวแปรค่าคงที่ของจริงให้ใช้ สามารถประกาศเหมือนตัวแปรทั่วไปได้ แต่เติมคำว่า `const` ไว้ข้างหน้า ดังนี้

```cpp
#include <stdio.h>
int main()
{
    const int NUMBER = 3+2;
    int x = NUMBER*5;
    printf("%d\n", x);
    return 0;
}
```
ทั้งนี้ ไฟล์ Source Code จะต้องเซฟเป็นนามสกุล `.cpp` จึงจะ compile ตามแบบของภาษา C++ ได้
