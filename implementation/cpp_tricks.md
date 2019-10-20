สำหรับบทความนี้ จะพูดถึงในเรื่องของเทคนิต่างๆในภาษา C++ โดยใช้ C++11 standard ที่ทำให้การเขียนโปรแกรมสะดวกยิ่งขึ้น โดยเฉพาะอย่างยิ่งในสนามแข่งขัน

# #include <bits/stdc++.h>

ในการเขียนโปรแกรมภาษา C++ นั้นมี Library ช่วยมากมาย
ในการ include library ทั้งหมดที่ใช้บ่อยๆ (เช่น set,vector,algoritm) สามารถใช้ `#include <bits/stdc++.h>` ได้แทนที่จะใส่ include แต่ละอย่างที่ต้องใช้ต่างหาก การทำเช่นนี้ทำให้เราสามารถเขียนโปรแกรมได้รวดเร็วยิ่งขึ้น เนื่องจากไม่ต้องเสียเวลาและพลังสมองในการตรวจสอบว่าไม่มีการใช้ library ที่ไม่ได้ include มา ซึ่งสำคัญเป็นอย่างยิ่งในสนามแข่งขันเนื่องจากเวลาและพลังสมองเป็นสิ่งล้ำค่า

# Standard input/output optimization

ถ้าหากใช้ภาษา C++ เราจะสามารถรับข้อมูลนำเข้าได้สองแบบ แบบแรกคือการใช้ `scanf/printf` และแบบที่สองคือการใช้ `cin/cout` โดยทั่วไปแล้ว การใช้ `scanf/printf` จะเร็วกว่า เนื่องจากไม่มีการ flush buffer และการ tie เข้าสู่ stdio แต่ `cin/cout` พิมพ์ง่ายกว่า ไม่จำเป็นต้องเป็นห่วงเรื่อง type และส่งข้อมูลเข้าเป็น C++ string ได้โดยตรง ดังนั้นเราอาจอยากใช้ `cin/cout` ในบางครั้ง เพื่อทำให้ `cin/cout` เร็วขึ้น เราจะเพิ่มสองบรรทัดนี้ในด้านบนสุดของฟังก์ชัน `int main()`:

```cpp
ios::sync_with_stdio(false);
cin.tie(0);
```

มีข้อระวังอย่างหนึ่งคือ ถ้าหากทำการ optimize เช่นนี้แล้ว จะใช้ `scanf/printf` และ `cin/cout` ในโปรแกรมเดียวกันไม่ได้ เนื่องจาก `ios::sync_with_stdio(false);` จะทำให้ลำดับการนำข้อมูลเข้าและออกสลับสับสนและจะทำให้คำตอบผิด

ใน C++11 ความเร็วของการ optimize เช่นนี้เทียบเท่ากับ `scanf/printf` ธรรมดาเลยทีเดียว แต่ใน C++98 อาจจะยังช้าอยู่บ้าง

# เลข 64-bit

หนึ่งในสาเหตุที่ทำให้โปรแกรมรันแล้วได้คำตอบผิดคือ overflow ปกติแล้ว ตัวแปรถูกสร้างขึ้นโดยการจองเนื้อที่ใน RAM ที่จำกัด ถ้าหากใส่ข้อมูลเข้าไปในตัวแปรที่ใหญ่เกินกว่าเนื้อที่ที่จองไว้ จะทำให้เกิด overflow ซึ่งจะทำให้ข้อมูลในตัวแปรกลายเป็นค่าขยะ

ประเภทของ overflow ที่เรามักเจอมากที่สุดในการสนามแข่งคือ integer overflow ปกติแล้ว integer สามารถเก็บจำนวนเต็มที่มีค่าระหว่าง $-2^{31}$ ถึง $2^{31} - 1$ เท่านั้น ซึ่งจะเท่ากับประมาณ $-2*10^9$ ถึง $2*10^9$ ถ้าต้องการเก็บจำนวนเต็มที่มีค่าสัมบูรณ์ใหญกว่านี้ จะต้องใช้ long long ซึ่งสามารถก็บค่าได้ระหว่าง $-2^{63}$ ถึง $2^{63} - 1$ ซึ่งจะเท่ากับประมาณ $-9*10^{18}$ ถึง $9*10^{18}$ การจะใช้ long long แทน int สามารถทำได้โดยเปลี่ยน data type นำหน้า เช่น `int n = 2;` จะกลายเป็น `long long n = 2;`

การรับค่า int โดยใช้ scanf จะใช้ `scanf("%d", n)` แต่ถ้าหากใช้ long long จะต้องใช้ `scanf("%lld", n)` ถ้าหากใช้ `scanf("%d", n)` จะไม่สามารถรับค่าเป็น long long ได้

อีกอย่างที่ควรระวังคือการคูณเลขใหญ่ สมมติเรามีตัวแปร `x, y` ที่เป็น int ทั้งคู่ซึ่ง `x * y` เกินขีดจำกัดของ int (เช่น `x = y = 2000000000`) และเราต้องการ `x * y / z` ซึ่งสามารถใส่ใน int ได้ `x * y / z` นั้นจะไม่ใช่ค่าที่ต้องการ เนื่องจากการหาค่าของนิพจน์ทำจากซ้ายไปขวาและ `x * y` นั้นเกินขีดจำกัดของ int ไปแล้ว ดังนั้นข้าที่นำไปหารกับ `z` จะเป็นค่าขยะ เราสามารถแก้ปัญหานี้ได้โดยการนำ `1ll` (ซึ่งเป็นประเภท long long อยู่แล้ว) มาคูณด้านหน้า เลยกลายเป็น `1ll * x * y / z` ทั้งนี้จะบอกให้ compiler บังคับใช้ตั้งแต่ต้น จึงทำให้ได้คำตอบที่ถูกต้อง

# foreach

ใน C++11 เราสามารถใช้ loop แบบ foreach เพื่อ iterate (ไล่) C++ collection ต่างๆเช่น vector, set, map เป็นต้น ปกติแล้วการ iterate collection (ในที่นี้จะใช้ `vector<int>` เป็นตัวอย่าง) ใน C++98 จะใช้

```cpp
for(vector<int>::iterator it = vec.begin(); it != vec.end(); ++it)
```

แต่ foreach loop ก็ทำงานคล้ายๆกัน ยกตัวอย่างเช่น

```cpp
vector<int> vec = {1, 2, 3, 4, 5};
for(int v: vec) {
    cout << v << ' ';
}
```

Output:

```
1 2 3 4 5
```

ถ้าหากต้องการเปลี่ยนค่าใน collection เราสามารถใช้ reference ได้อีกด้วย ยกตัวอย่างเช่น

```cpp
vector<int> vec = {1, 2, 3, 4, 5};
for(int &v: vec) {
    cout << v << ' ';
    v = 10;
}
for(int v: vec) {
    cout << v << ' ';
}
```

Output:

```
1 2 3 4 5
10 10 10 10 10
```

# emplace

การใส่ข้อมูลใน collection นั้น ถ้าเป็น pair, tuple หรือ struct ที่มี constructor สามารถใช้ emplace function ได้เพื่อการเขียนที่ง่ายขึ้น ยกตัวอย่างเช่น

```cpp
vector<pair<int, int>> vec;
vec.emplace_back(1, 2);

set<pair<int, int>> S;
S.emplace(1, 2);
```

# auto type

ข้อมูลที่รู้ type อยู่แล้วสามารถใช้ type auto ได้ใน C++11 ยกตัวอย่างเช่น

```cpp
int n = 5;
auto m = n;

vector<int> vec = {1, 2};
for(auto x : vec) cout << x << endl;
```

# Set/Map erase

สมมติ มี set<int> S = {1, 2, 3, 4, 5}; และต้องการลบเลข 3 และตัวหลังมันสามารถทำได้ดังนี้

```cpp
auto it = S.erase(S.find(3));
cout << *it << endl;
```

Output:

```
4
```

คำตอบเป็นเช่นนี้เนื่องจากค่าที่คืนจาก erase คือ iterator ที่ชี้ไปหาค่าต่อไปใน S จากค่าที่กำลังจะถูกลบออก

# Priority Queue Custom Comparators

บางครั้ง เราอาจต้องการใช้ priority_queue แต่ต้องการใช้กับ struct ที่เราประกาศเอง ในที่นี้ เราต้องเขียน custom comparator เพื่อบอก priority_queue ว่าเราจะเปรียบเทียบแต่ละค่าใน priority_queue อย่างไร

```cpp
struct pq {
    int x, y;
    pq(int x, int y) : x(x), y(y) {}
    friend bool operator<(const pq &a, const pq &b) {
        return a.x < b.x;
    }
};

priority_queue<pq> Q;
```

ในอีกหลายกรณี เราอาจจะต้องการ minimum priority_queue แทนที่จะเป็น maximum priority_queue ของ type บางอย่างที่มี comparator อยู่แล้ว (เช่น int หรือ pair<double, double> ซึ่งสามารถทำได้ดังต่อไปนี้

```cpp
priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>>
```

# Lambda functions

หนึ่งใน feature ใหม่ของ C++11 คือ lambda function ซึ่งคือ function ไร้ชื่อและสามารถเก็บในตัวแปรได้ ปกติในสนามแข่ง เราจะใช้ lambda function เพื่อเป็น comparator เมื่อต้องการเรียงข้อมูล ยกตัวอย่างเช่น

```cpp
vector<int> vec = {1, 5, 2, 4, 3};
auto cmp = [](const int &a, const int &b) { return a > b; };
sort(vec.begin(), vec.end(), cmp);
```

# Builtin functions

ใน compiler GCC จะมี function บางอย่างที่สามารถใช้ได้ และมีประโยชน์สำหรับการแข่งขัน

1. `__builtin_gcd(a, b)` รับจำนวนเต็มมาสองค่า a และ b (จะเป็น int หรือ long long ก็ได้) และจะคืนค่า $\gcd(a,b)$ มา
2. `__builtin_clz(x)` จะคืนค่าเท่ากับจำนวน leading zero ใน binary representation ของ x (x ต้องเป็น int) เราสามารถใช้ function นี้หา log ฐานสองได้จากคำสั่ง `31 - __builtin_clz(x)`
3. `__builtin_clzll(x)` เหมือน `__builtin_clz(x)` แต่ x ต้องเป็น long long การหา log ฐานสองให้ใช้ `63 - __builtin_clzll(x)`
4. `__builtin_popcount(x)` จะคืนค่าเท่ากับจำนวน bit ที่เป็น 1 ใน binary representation ของ x (x ต้องเป็น int)
5. `__builtin_popcountll(x)` เหมือน `__builtin_popcount(x)` แต่ x ต้องเป็น long long
