# simple-arduino-alarm-system

#include<EEPROM.h>
const int buzzer = 12;
const int RED = 2;
const int BLUE = 3;
const int GREEN = 4;
const int button = 7;
const int trigpin = 8;
const int echopin = 9;
const int laserpin = 13;
const int soundsensor = A1;
const int ledpin = 11; 
const int lightsensor = A0;
const int buzzer2 = 6;
long duration;
int distance;
int curbutton = 0,lastbutton = 0;
int scount = 0;
int ldistance = 0;
int soundarray[] = {HIGH,LOW,HIGH,LOW,HIGH,HIGH,LOW,LOW};
void setup()
{
  // put your setup code here, to run once:

pinMode(trigpin,OUTPUT);
pinMode(echopin,INPUT);
pinMode(button,INPUT_PULLUP);
pinMode(buzzer,OUTPUT);
pinMode(RED,OUTPUT);
pinMode(GREEN,OUTPUT);
pinMode(BLUE,OUTPUT);
pinMode(laserpin,OUTPUT);
pinMode(soundsensor,INPUT);
pinMode(ledpin,OUTPUT);
pinMode(lightsensor,INPUT);
pinMode(buzzer2,OUTPUT);
EEPROM.get(0,scount);
Serial.begin(9600);

}

void loop() 
{
  // put your main code here, to run repeatedly:
digitalWrite(RED,LOW);
digitalWrite(GREEN,HIGH);
digitalWrite(BLUE,LOW);

//sound sensortest
scount = analogRead(soundsensor);



curbutton = digitalRead(button);

if(curbutton != lastbutton )
{
  
if(curbutton == HIGH)
{
  EEPROM.put(0,curbutton);
  EEPROM.put(1,lastbutton);
digitalWrite(RED,HIGH);
digitalWrite(GREEN,LOW);
digitalWrite(BLUE,LOW);

digitalWrite(trigpin,LOW);
delay(2);
digitalWrite(trigpin,HIGH);
delay(10);
digitalWrite(trigpin,LOW);

duration = pulseIn(echopin,HIGH);
distance = duration *0.034/2;

Serial.println("Distance : ");
delay(200);
Serial.println(distance);

digitalWrite(laserpin,HIGH);
delay(200);
digitalWrite(laserpin,HIGH);

if(distance < 64)
{
  tone(buzzer,HIGH);
  digitalWrite(ledpin,HIGH);
  tone(buzzer2,HIGH);
  delay(2);
  tone(buzzer,LOW);
  digitalWrite(ledpin,LOW);
  tone(buzzer2,LOW);
  delay(2);
  tone(buzzer,HIGH);
  digitalWrite(ledpin,HIGH);
  tone(buzzer2,HIGH);
  delay(2);
  tone(buzzer,LOW);
  digitalWrite(ledpin,LOW);
  tone(buzzer2,LOW);
  if(scount > 530)
  scount++;
}
else
{
  noTone(buzzer);
noTone(buzzer2);
}

Serial.println("light sensor : ");
delay(200);
ldistance = analogRead(lightsensor);
Serial.println(analogRead(lightsensor));
//ldistance = 

if(ldistance < 1004)
{
  tone(buzzer,HIGH);
  digitalWrite(ledpin,HIGH);
  tone(buzzer,HIGH);
  delay(2);
  tone(buzzer,LOW);
  digitalWrite(ledpin,LOW);
  tone(buzzer2,LOW);
  delay(2);
  tone(buzzer,HIGH);
  digitalWrite(ledpin,HIGH);
  tone(buzzer2,HIGH);
  delay(2);
  tone(buzzer,LOW);
  digitalWrite(ledpin,LOW); 
  tone(buzzer2,LOW);
  if(scount>530)
  scount++;
}
else
{noTone(buzzer);
noTone(buzzer2);}
}


if(scount < 10)
{
  for(int i =0;i<=7;i++)
  {
  tone(buzzer,soundarray[i]);
  tone(buzzer2,soundarray[i]);
  }
  
  lastbutton = curbutton;
  scount = 0;
EEPROM.put(0,scount);
}
}
}
