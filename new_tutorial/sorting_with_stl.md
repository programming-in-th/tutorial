# Sorting with STL

ในการทำโจทย์บางข้อ เราอาจจะต้องเรียงข้อมูลใน array เพื่อแสดงผล หรือเพื่อให้ง่ายต่อการประมวลผล แต่ว่าถ้าจะให้เรียงข้อมูลโดยเขียน Bubble Sort/Insertion Sort/Selection Sort เองก็เสียเวลา เราสามารถใช้ฟังก์ชันที่ภาษา C++ มีให้ได้เลย

ในภาษา C ไม่มีให้ใช้ (เอาจริง ๆ ก็มีแหละ แต่ใช้ยาก) คนที่ใช้ภาษา C อยู่ควรเปลี่ยนมาใช้ภาษา C++ (ไม่จำเป็นต้องเปลี่ยนมาใช้ cin, cout ก็ได้ printf, scanf ยังใช้ได้อยู่)

ก่อนที่จะใช้ เราต้อง

```cpp
#include <algorithm>
using namespace std;
```

หลังจากนั้น หากเราต้องการจะเรียงข้อมูลใน array ใด ๆ ให้ใช้คำสั่งในรูปแบบนี้

```cpp
sort(array+begin, array+end);
```

begin คือตำแหน่งที่จะเริ่มเรียง end คือตำแหน่งเกินตำแหน่งสุดท้าย ที่ให้เรียง

เช่น หากต้องการจะเรียงค่าของ num[0], num[1], ..., num[9] ต้องใช้คำสั่ง

```cpp
sort(num+0, num+10); // ไม่เขียน +0 ก็ได้
```

เราต้องการจะเรียงถึงตำแหน่ง 9 เท่านั้น แต่ตอนใช้คำสั่ง sort เราจะต้องเขียนถึง 10 หากต้องการจะเรียงข้อมูลใน vector ทั้งหมด สามารถทำได้โดย

```cpp
sort(vec.begin(), vec.end());
```

แต่หากต้องการจะเรียงแค่จากตัวที่ a ถึง b สามารถใช้แบบนี้ได้

```cpp
sort(vec.begin() + a, vec.begin() + b + 1);
```

ฟังก์ชัน sort ใน ภาษา C++ ใช้วิธี Introspective Sort ในการเรียงข้อมูล ซึ่งเป็นการผสมผสานระหว่าง Quick Sort กับ Heap Sort ทำให้สามารถจัดเรียงข้อมูลได้ในเวลา O(n log n) ซึ่งดีกว่า Bubble/Insertion/Selection Sort ที่ใช้เวลา O(n^2) (การ Sort ทั้งสองแบบที่กล่าวมา จะกล่าวถึงในค่าย 2)

Credits: aquablitz11
