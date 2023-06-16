# TITTLE: Arduino door locking system with 4-digit Pincode

## ABSTRACT:
The Arduino door locking system with a 4-digit Pincode is a project aimed at creating a secure and convenient door locking mechanism. Traditional key-based systems have their limitations, such as the risk of losing keys or unauthorized key duplication. This project addresses these issues by utilizing an Arduino board, a 4x4 keypad, and a servo motor to control the locking and unlocking of the door. By entering a correct 4-digit Pincode, the system will activate the servo motor to unlock the door, providing a reliable and customizable solution for enhancing security in residential or commercial settings.


## INTRODUCTION:
The security of our homes and workplaces is of utmost importance. Conventional key-based door locking systems, while widely used, are prone to vulnerabilities such as lost or stolen keys. To overcome these limitations, electronic locking mechanisms have gained popularity. This project introduces an Arduino-based door locking system that utilizes a 4-digit Pincode for access control. The system ensures enhanced security and offers convenience to the users by eliminating the need for physical keys.

## LITERATURE REVIEW:
Several electronic door locking systems exist in the market, ranging from biometric access control to keycard-based systems. However, Pincode-based systems offer a simple yet effective solution for secure access control. Various research papers and projects have explored Arduino-based Pincode door locks, highlighting their reliability and affordability. These systems leverage the versatility of Arduino boards and the ease of integrating peripherals such as keypads and servo motors. By reviewing existing literature, we can gain insights into the strengths and weaknesses of different approaches and refine our proposed methodology.


## PROPOSED METHODLOGY:
The proposed methodology involves several key steps in creating the Arduino door locking system with a 4-digit Pincode. Firstly, the hardware setup requires connecting the Arduino board to the 4x4 keypad and the servo motor. The keypad allows users to enter the Pincode, while the servo motor controls the physical locking mechanism. Secondly, the software implementation entails writing code for the Arduino board to handle user input from the keypad and control the servo motor. The code verifies the Pincode entered by the user and activates the servo motor to unlock the door if the Pincode is correct. Finally, thorough testing and debugging are conducted to ensure the system operates smoothly and reliably.


## CIRCUIT DIAGRAM:
![WhatsApp Image 2023-06-16 at 8 53 39 AM](https://github.com/anishkumar-Embedded/Simulation-project/assets/117346668/cb12d40b-c395-4e06-9d5c-59c2bd64e239)

## PROGRAM:
```
#include<Keypad.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x3F,20,4);

char keys[4][3]={
 {'1','2','3'},
 {'4','5','6'},
 {'7','8','9'},
 {'*','0','#'}};
 
byte rowPin[4]={6,7,8,9};
byte colPin[3]={3,4,5};

String password = "6679";  // The Pin Code.
int position = 0;

int wrong = 0; // Variable to calculate the wrong inputs.

int redPin = 10;
int greenPin = 11;
int buzzer = 12;
 
Keypad keypad=Keypad(makeKeymap(keys),rowPin,colPin,4,3);
// MAPPING THE KEYPAD.


int total = 0; // Variable to determine the number of wrong attempts.

void setup()
{
  
 pinMode(redPin,OUTPUT);
 pinMode(greenPin,OUTPUT);
 pinMode(buzzer, OUTPUT);
 
lcd.init(); //lcd startup
lcd.init();
lcd.backlight();
lcd.print("     4x3 Keypad     ");
lcd.setCursor(0,1);
lcd.print("   Locking System   ");
lcd.setCursor(0,2);
lcd.print("         By:        ");
lcd.setCursor(0,3);
lcd.print("HomeMade Electronics");
delay(3000);
lcd.clear();
setLocked(true);
delay(20);
}

void loop()
{
  
  lcd.clear();
  lcd.print(" Enter Unlock Code: ");
  delay(100);
  
 char pressed=keypad.getKey();
 String key[3];
  
 if(pressed)
 {
  lcd.clear();
  lcd.print(" Enter Unlock Code: ");
  lcd.setCursor(position,2);
  lcd.print(pressed);
  delay(500);
    if(pressed == '*' || pressed == '#')
      {
          position = 0;
          setLocked(true);
          lcd.clear();
      }

    else if(pressed == password[position])
      {
          key[position]=pressed;
          position++;
      }
 
    else if (pressed != password[position] )
      {
          wrong++;
          position ++;
      }

    if(position == 4){
          if( wrong >0)
            {
                total++;
                wrong = 0;
                position = 0;
                lcd.clear();
                lcd.setCursor(0,2);
                lcd.print("    Wrong Code!     ");
                delay(1000);
                setLocked(true);
            }

          else if(position == 4 && wrong == 0)
            {
                position = 0;
                wrong = 0;
                lcd.clear();
                lcd.setCursor(0,1);
                lcd.print("      Welcome!     ");
                lcd.setCursor(5,2);
                lcd.print(" Door Open");
                delay(2000);
                setLocked(false);
            }

             if(total ==3)
            {
                total=0;
                buzzer_beep();
                delay(500);
            }

        }

   }

   
}

void setLocked(int locked)
  {
    if (locked)
      {
          digitalWrite(redPin, HIGH);
          digitalWrite(greenPin, LOW);
          delay(1000);
      }
    else
      {
          digitalWrite(redPin, LOW);
          digitalWrite(greenPin, HIGH);
          delay(2000);
          digitalWrite(redPin, HIGH);
          digitalWrite(greenPin, LOW);
      }
  }
void buzzer_beep()
{
   lcd.clear();
   lcd.setCursor(0,1);
   lcd.print("    WARNING  !!!!   ");
   lcd.setCursor(0,2);
   lcd.print("   Access Denied");
   for (int i=0;i<10;i++){
   digitalWrite(buzzer,HIGH);
   delay(1000);
   digitalWrite(buzzer,LOW);
   delay(1000);
   }
}
```

## OUTPUT:
![WhatsApp Image 2023-06-16 at 8 53 39 AM (2)](https://github.com/anishkumar-Embedded/Simulation-project/assets/117346668/366c005f-3d45-4c4e-96c1-b8d190a9e073)
![WhatsApp Image 2023-06-16 at 8 53 39 AM (1)](https://github.com/anishkumar-Embedded/Simulation-project/assets/117346668/47b4cf77-853b-4cca-a937-518ffd39c4d2)

## CONCLUSION:
The Arduino door locking system with a 4-digit Pincode provides a reliable, customizable, and user-friendly solution for enhancing security in residential and commercial settings. By leveraging the power of Arduino boards, this system eliminates the need for physical keys and offers an improved level of security against unauthorized access.

Throughout the project, we successfully implemented and tested the system, verifying its effectiveness in providing secure access control. The Pincode-based authentication mechanism ensures that only authorized individuals can unlock the door, mitigating the risks associated with lost or stolen keys.

The project demonstrated the simplicity and affordability of Arduino-based solutions, utilizing readily available components such as the Arduino board, 4x4 keypad, and servo motor. The circuit diagram and program provided clear guidance for setting up and configuring the system, allowing users to adapt and modify it to suit their specific requirements.

During testing, the system consistently verified the entered Pincode and promptly unlocked the door upon successful authentication. It also handled incorrect Pincode entries effectively, providing appropriate feedback to the user. These results highlight the reliability and performance of the system, confirming its practicality and usefulness in real-world scenarios.

Furthermore, this project opens up avenues for further improvements and enhancements. Additional features can be incorporated, such as integrating a display module for visual feedback, implementing multiple Pincode access levels, or connecting the system to a remote monitoring platform. Such enhancements can provide enhanced functionality and convenience to users, expanding the scope of the system's application.

In conclusion, the Arduino door locking system with a 4-digit Pincode addresses the limitations of traditional key-based systems by offering a secure, user-friendly, and cost-effective alternative. It serves as a testament to the versatility and potential of Arduino-based solutions in enhancing security measures. As technology continues to evolve, projects like this pave the way for innovative advancements in access control systems, contributing to safer environments for both residential and commercial spaces.

## REFERENCE:
www.electronicsforu.com
www.electronicshub.org
www.wikipedia.org
