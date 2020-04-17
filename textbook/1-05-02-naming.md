## 1.5. กฎการตั้งชื่อตัวแปร (Variable Naming)

ภาษา C มีกฎเกณฑ์ในการตั้งชื่อตัวแปร ดังต่อไปนี้

- ต้องประกอบขึ้นจากตัวอักษรภาษาอังกฤษ ตัวเลข และเครื่องหมาย `_` (Underscore) เท่านั้น
- อักขระตัวแรกจะต้องเป็นตัวอักษรภาษาอังกฤษ หรือเครื่องหมาย `_` เท่านั้น (ห้ามเป็นตัวเลข)
- ตัวพิมพ์ใหญ่ และตัวพิมพ์เล็กถือเป็นคนละตัวกัน เช่น `Salary` และ `SALARY` เป็นชื่อที่แตกต่างกัน
- มีความยาวไม่เกิน 31 อักขระ
- ชื่อจะต้องไม่ซ้ำกับคำสงวน (Reserved word)

| | | | |
|--|--|--|--|
auto | double | int | struct
break | else | long | switch
case | enum | register | typedef
char | extern | return | union
const | float | short | unsigned
continue | for | signed | void
default | goto | sizeof | volatile
do | if | static | while

(ไม่จำเป็นต้องจำคำสงวนก็ได้ คำพวกนี้เป็นคำที่ปรากฏในส่วนอื่น ๆ ของการเขียนโปรแกรม เดี๋ยวเรียนไปเรื่อย ๆ ก็จะจำได้เองว่ามีคำไหนบ้าง)
หากพยายามตั้งชื่อตัวแปรผิดจากกฏที่กำหนด จะไม่สามารถ compile ได้

นอกจากนี้ ยังมีหลักเกณฑ์การตั้งชื่อตัวแปรที่เข้าใจโดยทั่วไป ดังนี้
- ไม่ควรตั้งชื่อตัวแปรเป็นตัวพิมพ์ใหญ่ทั้งหมด ปกติแล้วตัวพิมพ์ใหญ่ทั้งหมดจะใช้แทนค่าคงที่ (มีในบทถัดไป)
- หากชื่อตัวแปรประกอบจากคำหลายคำ ให้ใช้ _ ขั้น เช่น student_info_nickname หรือใช้ตัวพิมพ์ใหญ่ขึ้นต้นแต่ละคำแทน เช่น studentInfoNickName (เรียกว่าวิธีตั้งชื่อแบบนี้ว่า camel case)
- ควรตั้งชื่อให้สื่อความหมายว่าตัวแปรนั้น ๆ เก็บข้อมูลอะไรอยู่ เช่น student_id เราสามารถทราบได้ทันทีว่าเป็นข้อมูล ID ของนักเรียน แต่หากตั้งชื่อแค่ว่า data เราไม่สามารถทราบได้ว่าเป็นข้อมูลอะไร หากมาอ่านในภายหลังจะทำความเข้าใจได้ลำบาก

---

**โจทย์**: (True/False) ชื่อตัวแปรใดต่อไปนี้ สามารถนำไปใช้ได้ตามกฏของภาษา C
```
customerName
ID
address1
%available
auto
Int
3rdSubject
_sys_one
address_2
number5
#5
```