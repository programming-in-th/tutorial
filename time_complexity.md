อัลกอริทึมที่มีประสิทธิภาพเป็นส่วนที่สำคัญในการแข่งขันเขียนโปรแกรม โดยทั่วไปเป็นเรื่องที่ง่ายที่จะออกแบบอัลกอริทึม ที่แก้ปัญหาโดยที่ใช้เวลามาก แต่ความจริงเราต้องการอัลกอริทึมที่สามารถแก้ปัญหาได้รวดเร็ว ถ้าอัลกอริทึมนั้นช้าจะได้คะแนนเพียงบางส่วนเท่านั้น

Time complexity ของ อัลกอริทึมคือเวลาที่ใช้ในการรันสำหรับบาง input ไอเดียคือสร้าง function ที่รับขนาดของ input การคำนวณ time complexity เราสามารถรู้ได้ว่าอัลกอริทึมนั้นเร็วแค่ไหนโดยที่ยังไม่ได้เขียน

# กฏในการคำนวณ

time complexity ของอัลกอริทึม นิยามโดย $O(...)$ โดยที่ $...$ แสดงถึงบางฟังก์ชัน โดยทั่วไป ตัวแปร $n$ คือขนาดของ input ตัวอย่างเช่น ถ้า input คือ array ตัวเลข, $n$ จะเป็นขนาดของ array และถ้า input คือ string, $n$ จะเป็นความยาวของ string

## Loops

โดยทั่วไปอัลกอริทึมจะทำงานได้ช้าก็ต่อเมื่ออัลกอริทึมนั้นมีหลาย loop

ถ้ามี $k$ nested loops, time complexity จะเป็น $O(n^k)$

ตัวอย่างเช่น time complexity ของโด้ดต่อไปนี้คือ $O(n)$:

```cpp
for(int i = 1; i <= n; ++i) {
  // code
}
```

และ time complexity ของโค้ดต่อไปนี้คือ $O(n^2)$:

```cpp
for(int i = 1; i <= n; ++i) {
  for(int j = 1; j <= n; ++j) {
    // code
  }
}
```

## Order of magnitude

time complexity ไม่ได้บอกเวลาที่แน่ชัดในการรัน แต่บอกแค่เวลาคร่าวๆ ในตัวอย่างถัดไป โค้ดจะรันโดยใช้เวลา $3n$, $n+5$, $\left\lfloor\frac{n}{2}\right\rfloor$ แต่ time complexity ยังเป็น $O(n)$

```cpp
for(int i = 1; i <= 3*n; ++i) {
  // code
}
```

```cpp
for(int i = 1; i <= n+5; ++i) {
  // code
}
```

```cpp
for(int i = 1; i <= n/2; ++i) {
  // code
}
```

ตัวอย่างถัดไป time complexity เป็น $O(n^2)$

```cpp
for(int i = 1; i <= n; ++i) {
  for(int j = i+1; j <= n; ++j) {
    // code
  }
}
```

## Phases

ถ้าอัลกอริทึมมีหลาย phase, time complexity รวมจะเป็น phase ของ time complexity ที่ใหญ่ที่สุด

ตัวอย่างเช่น โค้ดด้านล่างมี 3 phases ประกอบด้วย time complexity $O(n)$, $O(n^2)$ และ $O(n)$ time complexity รวมจะเป็น $O(n^2)$

```cpp
for(int i = 1; i <= n; ++i) {
  // code
}
for(int i = 1; i <= n; ++i) {
  for(int j = 1; j <= n; ++j) {
    // code
  }
}
for(int i = 1; i <= n; ++i) {
  // code
}
```

## Several variables

ในบางครั้ง time complexity มีหลายตัวแปร ในกรณีนี้ time complexity สามารถมีหลายตัวแปรได้

ตัวอย่างเช่น time complexity ของโค้ดนี้เป็น $O(nm)$:

```cpp
for(int i = 1; i <= n; ++i) {
  for(int j = 1; j <= m; ++j) {
    // code
  }
}
```

## Recursion

time complexity ของ recursive function ขึ้นอยู่กับจำนวนครั้งที่ function ได้ถูกเรียก และ time complexity ของการเรียกครั้งเดียว, time complexity รวมคือผลคูณของทั้งสองอย่างนี้

ตัวอย่างเช่น

```cpp
void f(int n) {
  if(n == 1) return;
  f(n - 1);
}
```

ในการเรียก $f(n)$ จะเรียกทั้งหมด $n$ ครั้ง และ time complexity ในการเรียกแต่ละครั้งคือ $O(1)$, time complexity รวมคือ $O(n)$

ตัวอย่างอีกอัน คือ

```cpp
void g(int n) {
  if(n == 1) return;
  g(n - 1);
  g(n - 1);
}
```

ในกรณีนี้ function ได้เรียก function เดิมอีก 2 รอบยกเว้นกรณี $n = 1$ เราจะแสดงตารางในการเรียก function

| เรียก function | จำนวนครั้งที่เรียก |
| -------------- | ------------------ |
| $g(n)$         | 1                  |
| $g(n-1)$       | 2                  |
| $g(n-2)$       | 4                  |
| $\cdots$       | $\cdots$           |
| $g(1)$         | $2^{n-1}$          |

จากตาราง time complexity คือ $1+2+4+\cdots+2^{n-1} = 2^n-1 = O(2^n)$

# Complexity classes

ด้านล่างคือ time complexity ที่พบเจอได้บ่อยในการเขียนอัลกอริทึม

- $O(1)$ คือ constant-time algorithm ที่ไม่ขึ้นอยู่กับขนาดของ input

- $O(\log n)$ คือ logarithmic algorithm ที่แบ่งครึ่งขนาดของ input ในแต่ละครั้ง เวลาในการรันของอัลกอริทึมเป็น logarithmic เพราะว่า $log_2 n$ เท่ากับจำนวนครั้งที่ $n$ ที่หารด้วย 2 จนได้ 1.

- $O(\sqrt{n})$ คือ square root algorithm จะช้ากว่า $O(\log n)$ แต่เร็วกว่า $O(n)$ สมบัติพิเศษของ square root คือ $\sqrt{n} = \frac{n}{\sqrt{n}}$

- $O(n)$ คือ linear algorithm จะรันเท่ากับขนาดของ input

- $O(n \log n)$ time complexity นี้จะใช้ใน sort ข้อมูลนำเข้า เพราะว่า time complexity ของ sorting algorithm คือ $O(n \log n)$ อีกอย่างที่เป็นไปได้คือ algorthm ใช้ data structure ที่แต่ละ operation ใช้เวลา $O(\log n)$

- $O(n^2)$ คือ quadratic algorithm ที่ประกอบด้วย nested loop 2 ชั้น เป็นไปได้ที่รับคู่อันดับของ input ใน $O(n^2)$

- $O(n^3)$ คือ quadratic algorithm ที่ประกอบด้วย nested loop 3 ชั้น เป็นไปได้ที่รับสามอันดับของ input ใน $O(n^3)$

- $O(2^n)$ เป็น time complexity ที่ใช้ในการรันทุก subset ของ input ตัวอย่างเช่น subset ของ ${1, 2, 3}$ คือ $\emptyset$, $\{1\}$, $\{2\}$, $\{3\}$, $\{1,2\}$, $\{1,3\}$, $\{2,3\}$, $\{1,2,3\}$.

- $O(n!)$ เป็น time complexity ที่ใช้ในการรันทุก permutation ของ input ตัวอย่างเช่น permutation ของ ${1, 2, 3}$ คือ $(1,2,3)$, $(1,3,2)$, $(2,1,3)$, $(2,3,1)$, $(3,1,2)$, $(3,2,1)$.

อัลกอริทึมที่เป็น polynomial time algorithm จะรันอย่างมาก $O(n^k)$ โดยที่ $k$ เป็นค่าคงที่

# Estimating efficiency

ในการคำนวน time complexity ของอัลกอริทึม นั้นสามารถใช้ได้โดยที่ 1 วินาที จะรันได้ประมาณ 100,000,000 คำสั่ง

ตัวอย่างเช่นโจทย์ข้อนี้ให้เวลา 1 วินาทีโดยที่ input size $n = 10^5$ ถ้า time complexity เป็น $O(n^2)$ อัลกอริทึม จะใช้ประมาณ​ $(10^5)^2 = 10^{10}$ operation ต้องใช้เวลารันอย่างน้อย 10 วินาทีดังนั้น อัลกอริทึมนี้ช้าไปสำหรับแก้ไขปัญหานี้

ตารางข้างล่างแสดงถึง time complexity ที่เหมาะสมในการรันโปรแกรม 1 วินาที สำหรับแต่ละขนาดของ input

| Input size   | Required Time Complexity |
| ------------ | ------------------------ |
| $n \le 10$   | $O(n!)$                  |
| $n \le 20$   | $O(2^n)$                 |
| $n \le 500$  | $O(n^3)$                 |
| $n \le 5000$ | $O(n^2)$                 |
| $n \le 10^6$ | $O(n \log n)$ or $O(n)$  |
| $n$ is large | $O(1)$ or $O(\log n)$    |

ตัวอย่างเช่น ขนาดของ input คือ $n = 10^5$ สามารถใช้อัลกอริทึมในการรันเป็น $O(n)$ หรือ $O(n \log n)$ ข้อมูลนี้ทำให้การออกแบบอัลกอริทึมง่ายขึ้น เนื่องจากทำให้คุณสามารถรู้ได้ว่าอัลกอริทึมไหนประสิทธิภาพต่ำที่สุด

แต่เรายังจำเป็นที่ต้องรุ้ไว้เสมอว่า time complexity เป็นแค่การประมาณ เนื่องมันซ่อนตัวแปรบางอย่างไว้ เช่นอัลกอริทึมที่รันในเวลา $O(n)$ อาจจะทำ $n/2$ หรือ $5n$ ครั้งก็เป็นได้ ซึ่งเป็นสิ่งที่ส่งผลกับเวลาในการรันโปรแกรมจริง ๆ

Attribution: [Competitive Programmer's Handbook by Antti Laaksonen](https://github.com/pllk/cphb/), licensed under [CC BY-NC-SA 3.0](https://creativecommons.org/licenses/by-nc-sa/3.0/)
