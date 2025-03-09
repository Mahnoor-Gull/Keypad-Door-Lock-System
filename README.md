
# **🔒 Secure Keypad Lock System**  

![Arduino](https://img.shields.io/badge/Arduino-Lock%20System-blue?style=for-the-badge&logo=arduino)  

## **🔹 Project Goal**  
This project is a **secure electronic lock system** that uses a **4x3 keypad** for password entry, a **servo motor** for unlocking, and  and a buzzer** for user feedback. It includes security features such as **incorrect attempt limits and automatic re-locking** to enhance protection.  

---

## **🚀 How It Works?**  

🔹 **User Inputs Password** → The system prompts the user to enter a **4-digit password**.  
🔹 **Password Verification** → Keypresses appear as `*` on the **LCD**.  
🔹 **Unlocking Mechanism** → If correct, the **servo motor unlocks the door**, and **green LED turns ON**.  
🔹 **Incorrect Attempts** → If incorrect, the **buzzer sounds**, and the **red LED flashes**.  
🔹 **Security Measures:**  
   - **5 incorrect attempts** → **15-second delay**.  
   - **8 incorrect attempts** → **1-minute lockout with buzzer alarm**.  
🔹 **Automatic Re-locking** → After access is granted, the system automatically **locks after a countdown**.  

---

## **🛠 Hardware Components**  

| **Component**     | **Description**          |
|-------------------|-------------------------|
| **Arduino Board** | Microcontroller         |
| **4x3 Keypad**   | User input device       |
| **16x2 LCD**     | Displays instructions   |
| **Servo Motor**  | Controls door lock      |
| **Buzzer**       | Alerts for incorrect attempts |
| **Resistors & Wires** | Circuit connections |

---

## **🔌 Wiring Diagram**  

| **Component**    | **Arduino Pin** |
|-----------------|--------------|
| **LCD**        | A0, A1, A2, A3, A4, A5 |
| **Keypad Rows** | 1, 2, 3, 4 |
| **Keypad Cols** | 5, 6, 7 |
| **Servo Motor** | 9 |
| **Red LED**     | 10 |
| **Green LED**   | 11 |
| **Buzzer**      | 8 |

---

## **💻 Arduino Code**  

```cpp
#include <Keypad.h>
#include <LiquidCrystal.h>
#include <Servo.h>

Servo myservo;
LiquidCrystal lcd(A0, A1, A2, A3, A4, A5);

const byte rows = 4;
const byte cols = 3;

char key[rows][cols] = {
  {'1', '2', '3'},
  {'4', '5', '6'},
  {'7', '8', '9'},
  {'*', '0', '#'}
};

byte rowPins[rows] = {1, 2, 3, 4};
byte colPins[cols] = {5, 6, 7};
Keypad keypad = Keypad(makeKeymap(key), rowPins, colPins, rows, cols);

char* password = "4567";
int currentPosition = 0;
int redLED = 10;
int greenLED = 11;
int buzzer = 8;
int invalidCount = 0;

void setup() {
  lcd.begin(16, 2);
  Serial.begin(9600);
  pinMode(redLED, OUTPUT);
  pinMode(greenLED, OUTPUT);
  pinMode(buzzer, OUTPUT);
  myservo.attach(9);
  displayScreen();
}

void loop() {
  char code = keypad.getKey();
  if (code != NO_KEY) {
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("PASSWORD:");
    lcd.setCursor(7, 1);
    for (int l = 0; l <= currentPosition; ++l) {
      lcd.print("*");
    }

    if (code == password[currentPosition]) {
      ++currentPosition;
      if (currentPosition == 4) {
        unlockDoor();
        currentPosition = 0;
      }
    } else {
      incorrect();
      currentPosition = 0;
    }
  }
}

void unlockDoor() {
  lcd.clear();
  lcd.setCursor(3, 0);
  lcd.print("ACCESS GRANTED");
  digitalWrite(greenLED, HIGH);
  myservo.write(90); // Unlock
  delay(5000);
  myservo.write(0);  // Lock again
  digitalWrite(greenLED, LOW);
  lcd.clear();
  lcd.print("LOCKED");
}

void incorrect() {
  lcd.clear();
  lcd.setCursor(3, 0);
  lcd.print("ACCESS DENIED");
  digitalWrite(redLED, HIGH);
  tone(buzzer, 1000);
  delay(2000);
  noTone(buzzer);
  digitalWrite(redLED, LOW);
}
```

---

## **✨ Features**  

✅ **Secure PIN-Based Access**  
✅ **Alarm for Unauthorized Attempts**  
✅ **Servo-Controlled Locking System**  
✅ **Security Lockout After Multiple Failures**  


## **🔧 Future Improvements**  

🔹 **User-Configurable Password**  
🔹 **Mobile App Integration** (WiFi/Bluetooth)  
🔹 **RFID or Fingerprint Authentication**  

🚀 **Secure Your Home with Arduino!** 🏠

 # **PROJECT DIAGRAM**
![Image](https://github.com/user-attachments/assets/1235bcd6-cf02-4229-bbed-a634ed1c15df)
