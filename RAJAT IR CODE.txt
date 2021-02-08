#include <LiquidCrystal.h>
#include<string.h>
#include <boarddefs.h>
#include <IRremote.h>
#include <IRremoteInt.h>
#include <ir_Lego_PF_BitStreamEncoder.h>
LiquidCrystal lcd(8, 9, 10, 11, 12, 13);
#define bulb1 3
#define bulb2 4
#define bulb3 5

int RECV_PIN = 2;


int itsON[] = {1,1,1,1};

#define code1 16724175 
#define code2 16718055 
#define code3 16743045 

IRrecv irrecv(RECV_PIN); 
decode_results results;
char temp;
char str[10];
char i=0;
void setup() 
{
irrecv.enableIRIn();
lcd.begin(16, 2);
Serial.begin(9600);
pinMode(bulb1, OUTPUT);
pinMode(bulb2, OUTPUT);
pinMode(bulb3, OUTPUT);
digitalWrite(bulb1, HIGH);
digitalWrite(bulb2, HIGH);
digitalWrite(bulb3, HIGH);
lcd.print("MICROCONTROLLERS ");
lcd.setCursor(0,1); 
lcd.print(" LAB ");
delay(2000);
lcd.clear();
lcd.print("HOME AUTOMATION ");
lcd.setCursor(0,1);
lcd.print(" USING IR ");
delay(2000);
lcd.clear();
lcd.print("1. Bulb 1");
lcd.setCursor(0,1);
lcd.print("2. Bulb 2");
delay(2000);
lcd.clear();
lcd.print("3. Bulb 3");
delay(2000);
lcd.clear();
lcd.print("Bulb 1 2 3 ");
lcd.setCursor(0,1);
lcd.print(" OFF OFF OFF");
}
void loop() {
lcd.setCursor(0,0);
lcd.print("Bulb 1 2 3 ");
if (irrecv.decode(&results)) {
unsigned int value = results.value;
switch(value) {
case code1:
if(itsON[1] == 1) { 
digitalWrite(bulb1, LOW); 
itsON[1] = 0;
lcd.setCursor(4,1); 
lcd.print("ON ");
}
else { 
digitalWrite(bulb1, HIGH);
itsON[1] = 1;
lcd.setCursor(4,1); 
lcd.print("OFF"); 
}
break; 
case code2:
if(itsON[2] == 1) {
digitalWrite(bulb2, LOW);
itsON[2] = 0;
lcd.setCursor(8,1); 
lcd.print("ON "); 
} else {
digitalWrite(bulb2, HIGH);
itsON[2] = 1;
lcd.setCursor(8,1); 
lcd.print("OFF"); 
}
break;
case code3:
if(itsON[3] == 1) {
digitalWrite(bulb3, LOW);
itsON[3] = 0;
lcd.setCursor(12,1); 
lcd.print("ON ");
} else {
digitalWrite(bulb3, HIGH);
itsON[3] = 1;
lcd.setCursor(12,1); 
lcd.print("OFF "); 
}
break; 
}
Serial.println(value); // you can comment this line
irrecv.resume(); // Receive the next value
}
}