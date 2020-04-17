## 1.4 Code Commenting

บางครั้ง เราต้องการจะเขียนคำอธิบายโค้ด (**comment**) กำกับไว้ในไฟล์ สามารถทำได้สองรูปแบบด้วยกัน
1. **Single line comment** ทำได้โดยเขียนเครื่องหมาย `//` แล้วตามด้วยข้อความที่ต้องการ
2. **Multiple line comment** จะเริ่มต้นด้วยเครื่องหมาย `/*` แล้วจบด้วยเครื่องหมาย `*/` โดยข้อความที่อยู่ระหว่างสองเครื่องหมายนี้จะถือว่าเป็น comment ทั้งหมด

**ตัวอย่าง**
```cpp
/*
hello.c, written by aquablitz11
This program is used as a part of programming.in.th tutorial.
*/
#include <stdio.h> // allow printf to be used

int main(void)
{
    printf("hello, world!"); // show the word "hello, world"
    // commented out code will NOT be executed:
    // printf("hello, world!");
    // printf("hello, world!");
    // printf("hello, world!");
    return 0;
}
```

ผลลัพธ์
```
hello, world!
```