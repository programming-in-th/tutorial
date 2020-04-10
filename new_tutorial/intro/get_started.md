การที่เราจะรันโปรแกรมที่เขียนด้วยภาษา C++ ได้นั้นเราต้องมีสิ่งที่เรียกว่า compiler เสียก่อน ซึ่ง compiler จะทำหน้าที่ในการแปลงโค้ดที่เราเขียนไปเป็น binary file ทำให้ OS เราสามารถรันได้นั่นเอง

Compiler นั้นมีหลายตัวให้เราใช้ แต่ตัวที่ฮิตที่สุดน่าจะหนีไม่พ้น [GCC](https://gcc.gnu.org/) หรือ GNU Compiler Collection นั่นเอง (Default ใน macOS ใช้ clang) ซึ่งเราจะมาดูวิธีการติดตั้ง GCC กันทั้งใน macOS และ Windows

# การติดตั้งใน macOS

ขั้นแรก เราต้องทำการติดตั้ง `Xcode Command Line Tools` เสียก่อน โดยเราต้องทำการเปิด `Terminal.app` ขึ้นมา และพิมพ์คำสั่ง

```bash
xcode-select --install
```

หลังจากรันคำสั่งข้างต้นเสร็จแล้ว ให้เราทำการยอมรับ LICENSE ของ Xcode

```bash
sudo xcodebuild -license accept
```

ต่อไปจะเป็นการติดตั้ง package manager ให้สำหรับเครื่องเรา โดยตัวที่เลือกใช้คือ [Homebrew](https://brew.sh/) ด้วยการรันคำสั่ง

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

หลังจากนั้นเราจึงสามารถติดตั้ง GCC ด้วยคำสั่ง

```bash
brew install gcc
```

พอขั้นตอนการติดตั้งเรียบร้อยแล้ว เราสามารถทดสอบได้ด้วยการรัน

```bash
g++-9 --version
```

หากเราติดตั้งสำเร็จ จะมี output ขึ้นมาที่ Terminal ของเราคล้ายกับตัวอย่างนี้

```bash
g++-9 (Homebrew GCC 9.2.0_1) 9.2.0
Copyright (C) 2019 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

ถือว่าเป็นอันสำเร็จในการติดตั้ง GCC ใน macOS

# การติดตั้งใน Windows (ทางเลือกที่ 1)

ทางเลือกแรก เราสามารถติดตั้ง GCC ด้วย [MinGW](http://www.mingw.org/) ที่ย่อมาจาก Minimalist GNU for Windows

โดยเราต้องทำการเข้าไปที่หน้า [Download](https://osdn.net/projects/mingw/releases/) ของ MinGW หลังจากนั้นทำการ Download file ที่ชื่อว่า `mingw-get-setup.exe`

![MinGW Website](get_started_img/MinGW.png)

พอเราทำการติดตั้งตามตัวช่วยติดตั้งไปเรื่อย ๆ ก็จะเป็นอันเสร็จสิ้นการติดตั้ง MinGW ลงใน `C:\MinGW` แต่เรายังไม่สามารถใช้งานได้ในตอนนี้ เราต้องทำการเพิ่ม `bin` ของ MinGW ลงใน path เสียก่อน

เราสามารถเพิ่ม Path ได้โดยเข้าไปใน Control Panel และทำการ search ในช่องว่า `Edit environment variables for your account`

![Environment Variable 1](get_started_img/path1.png)

หลังจากนั้นกด Edit ที่ PATH แล้วทำการใส่ `C:\MinGW\bin` ลงไปโดยการกดปุ่ม New และพิม `C:\MinGW\bin` ลงไปในช่อง

![New Path ENV](get_started_img/path2.png)

หลังจากนั้นเราทำการเปิด Command Prompt ขึ้นมา ก็จะพบว่าเราสามารถใช้ g++ ได้แล้ว โดยสามารถทดสอบด้วยคำสั่ง

```bash
g++ --version
```

หากมี output แบบนี้ออกมา ก็ถือว่าการติดตั้งสำเร็จแล้ว

```bash
g++ (MinGW.org GCC-8.2.0-5) 8.2.0
Copyright (C) 2018 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

# การติดตั้งใน Windows (ทางเลือกที่ 2)

เราสามารถใช้ Linux ใน Windows ได้โดยการใช้ Windows Subsystem for Linux ซึ่งสามารถเปิดใช้งานได้ด้วยวิธีตามลิงก์[นี้](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

## การติดตั้งใน Ubuntu

เราต้องทำการเพิ่ม PPA ลงในเครื่องเราก่อน ไม่เช่นนั้นเราจะได้ GCC version เก่ามาใช้งานแทน ซึ่งเราสามารถเพิ่มได้ด้วยคำสั่ง

```bash
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
```

หลังจากนั้นเราจึงสามารถติดตั้ง GCC ด้วยคำสั่ง

```bash
sudo apt install gcc-9 g++-9
```

พอขั้นตอนการติดตั้งเรียบร้อยแล้ว เราสามารถทดสอบได้ด้วยการรัน

```bash
g++-9 --version
```

หากสำเร็จก็จะขึ้น output คล้ายกับตัวอย่างด้านล่าง

```bash
g++-9 (Ubuntu 9.1.0-2ubuntu2~18.04) 9.1.0
Copyright (C) 2019 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

**_หมายเหตุ_**
ณ เวลาที่เขียนบทความ version ปัจจุบันของ Homebrew GCC คือ `9.2` ของ MinGW GCC คือ `8.2` และ Ubuntu Toolchain PPA คือ `9.1`
