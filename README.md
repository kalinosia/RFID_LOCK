# RFID LOCK WITH SERVO
My first project witch arduino from my university if technology.  These were practical classes on microcontrollers. Was very fun. In home I do document box with lock.
I had no idea about programing. Code C++ from Internet was sort of magical spell to me, but when I looked at it longer, I understood almost everything and I could add more from myself. So, it's easy but is sentimental (very good vibes).

## Connecions
<img src="RFID.jpg">

## ARDUINO
```
/**
* Read a card using a mfrc522 reader on your SPI interface
* Pin layout should be as follows (on Arduino Uno):
* MOSI: Pin 11 / ICSP-4
* MISO: Pin 12 / ICSP-1
* SCK: Pin 13 / ISCP-3
* SS: Pin 10
* RST: Pin 9
*
* 
*/

#include <SPI.h>
#include <RFID.h>
#include <Servo.h> 

#define SS_PIN 10
#define RST_PIN 9

RFID rfid(SS_PIN, RST_PIN); 
Servo myservo; 
int pos = 0;
int buzzPin = 3;

// Setup variables:
    int serNum0;
    int serNum1;
    int serNum2;
    int serNum3;
    int serNum4;

int caly;
void setup()
{ 
  Serial.begin(9600);
  delay(2000);
  SPI.begin(); 
  rfid.init();
  myservo.attach(7); /*serwo pin7*/
}

void loop()
{
    
    if (rfid.isCard()) {
        if (rfid.readCardSerial()) {
            if (rfid.serNum[0] != serNum0
                && rfid.serNum[1] != serNum1
                && rfid.serNum[2] != serNum2
                && rfid.serNum[3] != serNum3
                && rfid.serNum[4] != serNum4
            ) {
                /* With a new cardnumber, show it. */
                Serial.println(" ");
                Serial.println("Card found");
                serNum0 = rfid.serNum[0];
                serNum1 = rfid.serNum[1];
                serNum2 = rfid.serNum[2];
                serNum3 = rfid.serNum[3];
                serNum4 = rfid.serNum[4];
               
                //Serial.println(" ");
                Serial.println("Cardnumber:");
                Serial.print("Dec: ");
    Serial.print(rfid.serNum[0],DEC);
                Serial.print(", ");
    Serial.print(rfid.serNum[1],DEC);
                Serial.print(", ");
    Serial.print(rfid.serNum[2],DEC);
                Serial.print(", ");
    Serial.print(rfid.serNum[3],DEC);
                Serial.print(", ");
    Serial.print(rfid.serNum[4],DEC);
                Serial.println(" ");
                        
                Serial.print("Hex: ");
    Serial.print(rfid.serNum[0],HEX);
                Serial.print(", ");
    Serial.print(rfid.serNum[1],HEX);
                Serial.print(", ");
    Serial.print(rfid.serNum[2],HEX);
                Serial.print(", ");
    Serial.print(rfid.serNum[3],HEX);
                Serial.print(", ");
    Serial.print(rfid.serNum[4],HEX);
                Serial.println(" ");
                caly = serNum0 + serNum1 + serNum2 + serNum3 + serNum4   ;
      Serial.print(caly);          
             }
            else {
               /* If we have the same ID, just write a dot. */
              Serial.print(".");
             Serial.print(" ");
             }
             /*here is easy way- just sum of numbers*/
             /*I haveone cart so it's esy easy*/
           /*  if (caly == 464 /*|| caly == 111*/)*/
             /*alternaty             wny kod*/
             
             if(serNum==147 && serNum1==7 && serNum2==145 && serNum3 ==85 && serNum4==80)
             
{
             //buzzer
             analogWrite(3,10);
             delay(500);
             analogWrite(3,0);
             Serial.print("Karta zgodna ");

             if (myservo.read()==0){
              myservo.write(90);
              Serial.print(" Zamknięte ");
             } else {
              myservo.write(0);
              Serial.print(" Otwarte ");
             }
             }
             else {
              analogWrite(3,80);
              delay(500);
              analogWrite(3,0);
              Serial.print(" Karta niezgodna ");
               }
          }
    }
    
    rfid.halt();
delay(3000);
}
```
It's very simple way to detect good rfid, probably I change it to right way.

## Source

Script is based on the script of Miguel Balboa. 
* New cardnumber is printed when card has changed. Only a dot is printed
* if card is the same.
* @version 0.1
* @author Henri de Jong
* @since 06-01-2013
