# PROJECT1-SEC-1-AVENGERS 
## Member
| StudentID | Name | Responsibilities | Percentage
|-----------|------|------------|--------
| 64130500003 | Korapin Jongsriwattanaporn | Give knowledge and advice on writing functions to friends. and write checkAnswer, restart and setTimeout function, background track, JSON Writter| 20%
| 64130500010 | Jutamas Sirijarupongsa | Sound management function, design heart and reduce heart by using Vue directives| 20%
| 64130500039 | Thanchanok Pornchaisri | Writing function about hint function, design interface of Page True, False and TryAgain| 20%
| 64130500044 | Nanthawan Prangratsmee | Writing functions about hint function, design Hint button, writting ChangePages function| 20%
|	64130500058 | Patsakorn Jakklom | Writing functions Question, design Page GameOver and design interface How to play| 20%

## Features
1. Game play :
   - หน้าแรกของเราหรือ Home page จะประกอบด้วยปุ่ม play และ How to play
   - หลังจากกด play จะไปสู่หน้า question ซึ่งประกอบด้วยพลังชีวิตของผุ้เล่น(Heart), ปุ่มเริ่มใหม่/จบเกม, นับคะแนน(score), คำใบ้(Hint) และปุ่มเลิอกเปิด/ปิดเพลงขวาล่าง
   - ผู้เล่นต้องตอบคำถามกวนๆของเราทั้งหมด 10 ข้อถ้าผิดเกิน 3 ครั้งจะ Game over
   - เมื่อผู้เล่นชนะเกม(ตอบถูกครบ 10 ข้อและผิดไม่เกิน 3 ครั้ง)จะไปหน้าจบเกม(End game) และแสดงคะแนนที่ผู้เล่นได้
2. How to play : เราจะใช้เป็น pop-up เพราะจะได้ไม่เสียเวลาเยอะ
3. music : เรามีเพลงประกอบที่สามารถเลือกเปิด/ปิดได้
4. Hint : เรามีคำใบ้ที่ใช้ได้ 3 คำถาม
5. Score : นับคะแนนเมื่อผู้เล่นตอบถูก
6. End game : ปุ่มจบเกมทันทีแม้จะตอบไม่ครบ 10 ข้อ สำหรับผู้เล่นที่พลังชีวิตจะหมดและกลัวตอบผิด หรือผู้ที่อยากจบเกม
7. Restart : ปุ่มเริ่มเกมใหม่สำหรับผู้ที่อยากย้อนไปเล่นใหม่อีกรอบ

## Manual
1. หลังจากผู้เล่นอ่าน How to play และเข้าใจวิธีการเล่นแล้ว
2. ก็ใส่คำตอบเพื่อไปคำถามข้อถัดไป หากตอบผิดจะเสียพลังชีวิต 1 ดวงถ้าตอบถูก score จะเพิ่มขึ้น 1
3. หากตอบครบ 10 ข้อและหัวใจยังไม่หมดคุณก็จะชนะเกมแต่ถ้าตอบผิดจนหัวใจหมดคุณก็จะ Game over
4. มีความสุขและเพลิดเพลินกับการเล่น

## Demo
https://www.youtube.com/watch?v=UYt_ZNMUXb4

## Inspiration
ได้แรงบันดาลใจมาจากหนังสือคำถามกวนๆในร้่านหนังสือที่เคยเห็นตั้งแต่สมัยเด็กๆ
![image](https://github.com/NanthawnPI/INT203_JS/assets/156407674/ccd20bfb-20b4-471f-8199-ee9cf897f3b1)
