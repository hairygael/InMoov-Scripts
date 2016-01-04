//This is based on the MiniBot project, MiniBlue.pde from Zagros Robotics, Inc
//Modified by Elizabeth Greene (Elizabeth.a.Greene@gmail.com) 30 August 2013 for the inMoov Prosthetics project
//Modified by Gaël Langevin (hairygael@gmail.com) 13/11/2014 for the inMoov Prosthetics project
//Todo: set PWM pulse on pins 2 to 11


#include <MyoController.h>
#define DOUBLETAP_PIN 12
MyoController myo = MyoController();

int r_motor_Rotthumb_n = 2;  //Arduino pin to motor Controller Ain1
int r_motor_Rotthumb_p = 3;  //Arduino pin tomotor Controller Ain2

int r_motor_Thumb_n = 4;  //Arduino pin to motor Controller Bin1
int r_motor_Thumb_p = 5;  //Arduino pin to motor Controller Bin2

int r_motor_Index_n = 6;  //Arduino pin to motor Controller Ain1
int r_motor_Index_p = 7;  //Arduino pin to motor Controller Ain2

int r_motor_Majeure_n = 8;  //Arduino pin to motor Controller Bin1
int r_motor_Majeure_p = 9;  //Arduino pin to motor Controller Bin2

int r_motor_Pinky_n = 10;  //Arduino pin to motor Controller Ain1
int r_motor_Pinky_p = 11;  //Arduino pin to motor Controller Ain2

int incomingByte = 0; // for incoming serial data

//This value sets the maximum speed of the motor.  It's a value between 1 and 255.
//64 is 1/4 speed, 128 is 1/2 speed, 255 is full speed
int maxSpeed = 250;
int halfSpeed = 150;
int minSpeed = 128;

//These are the values you determined to be the limit for the minimum and maximum finger positions.

int potLimitLowRotthumb = 50;  //RotThumb extend
int potLimitHighRotthumb = 800;  //RotThumb retract
int potLimitLowThumb = 125;  //Thumb extend
int potLimitHighThumb = 400;  //Thumb retract
int potLimitLowIndex = 0;  //Index extend
int potLimitHighIndex = 620;  //Index retract
int potLimitLowMajeure = 130;  //Majeure extend
int potLimitHighMajeure = 550;  //Majeure retract
int potLimitLowPinky = 170;  //Pinky extend
int potLimitHighPinky = 400;  //Pinky retract

int speedDesired;

unsigned long safeDelay = (5 * 1000);

unsigned long lastMillis;

int rotthumbPot;
int rotthumbDesired;
int deltaRotthumb;
int rotthumbPos;

int thumbPot;
int thumbDesired;
int deltaThumb;
int thumbPos;

int indexPot;
int indexDesired;
int deltaIndex;
int indexPos;

int majeurePot;
int majeureDesired;
int deltaMajeure;
int majeurePos;

int pinkyPot;
int pinkyDesired;
int deltaPinky;
int pinkyPos;




void setup()
{
    pinMode(DOUBLETAP_PIN, OUTPUT);
    pinMode(13, OUTPUT);
    
    pinMode(r_motor_Rotthumb_n, OUTPUT);  //Set control pins to be outputs
    pinMode(r_motor_Rotthumb_p, OUTPUT);
    
    pinMode(r_motor_Thumb_n, OUTPUT);  //Set control pins to be outputs
    pinMode(r_motor_Thumb_p, OUTPUT);
    
    pinMode(r_motor_Index_n, OUTPUT);  //Set control pins to be outputs
    pinMode(r_motor_Index_p, OUTPUT);
    
    pinMode(r_motor_Majeure_n, OUTPUT);  //Set control pins to be outputs
    pinMode(r_motor_Majeure_p, OUTPUT);
    
    pinMode(r_motor_Pinky_n, OUTPUT);  //Set control pins to be outputs
    pinMode(r_motor_Pinky_p, OUTPUT);
    
    stopMotors();
    
    Serial.begin(115200);
    myo.initMyo();

    //Serial.println("c = close hand ");
    //Serial.println("o = open hand ");
    //Serial.println("g = intermediate position of hand ");
    
    
   

}

void loop()
{  
  rotthumbPot = analogRead(A2);
  thumbPot = analogRead(A3);
  indexPot = analogRead(A4);
  majeurePot = analogRead(A5);
  pinkyPot = analogRead(A6);
  
  actualPosition(rotthumbPot,thumbPot,indexPot,majeurePot,pinkyPot);
    
  myo.updatePose();
  if (Serial.available() > 0) {
            // read the incoming byte:
            incomingByte = Serial.read();

  switch (incomingByte,myo.getCurrentPose()) {
  case rest:
                   digitalWrite(DOUBLETAP_PIN,LOW);
                   incomingByte='*';
                   
                   

      
                 
                        
        break;  
        
        case fist:
      moveHand(90,0,90,90,90, maxSpeed);

      //Serial.println("Closing");
                        incomingByte='*';
                        safeDelay = (5 * 150);
                        lastMillis = millis();
        break;
        
        case fingersSpread:
      moveHand(0,0,0,0,0, maxSpeed);

      //Serial.println("Opening");
      incomingByte='*';
                        safeDelay = (5 * 150);
                        lastMillis = millis();
        break;
        
        case waveIn:
      moveHand(90,25,40,0,0, maxSpeed);

      //Serial.println("Pinch Fingers");
      incomingByte='*';
                        safeDelay = (5 * 150);
                        lastMillis = millis();
        break;
        
        case waveOut:
      moveHand(30,50,0,60,160, maxSpeed);

      //Serial.println("Typing Finger");
      incomingByte='*';
                        safeDelay = (5 * 150);
                        lastMillis = millis();
        break;
        
        case doubleTap:
                        digitalWrite(DOUBLETAP_PIN,HIGH);
        break;}
        delay(100);
       
}

if ((millis()-lastMillis) < safeDelay){
 check();
 delay(1);
 digitalWrite(13,LOW);}
 else {
   stopMotors();
   digitalWrite(13,HIGH);
   delay(1);
 }
    


}

void actualPosition(int rotthumbPot, int thumPot, int indexPot, int majeurePot, int pinkyPot){
  rotthumbPos = map(rotthumbPot,potLimitLowRotthumb,potLimitHighRotthumb,0,90);
  thumbPos = map(thumbPot,potLimitLowThumb,potLimitHighThumb,0,90);
  indexPos = map(indexPot,potLimitLowIndex,potLimitHighIndex,0,90);
  majeurePos = map(majeurePot,potLimitLowMajeure,potLimitHighMajeure,0,90);
  pinkyPos = map(pinkyPot,potLimitLowPinky,potLimitHighPinky,0,90);
}
  
void moveHand(int rotthumb, int thumb, int index, int majeure, int pinky, int val){
  
       rotthumbDesired = rotthumb;
       thumbDesired = thumb;
       indexDesired = index;
       majeureDesired = majeure;
       pinkyDesired = pinky;
       speedDesired = val;
     }
     
void stopMotors(){
     digitalWrite(r_motor_Rotthumb_n, LOW);  //set both motors off for start-up
     digitalWrite(r_motor_Rotthumb_p, LOW);
     digitalWrite(r_motor_Thumb_n, LOW);  //set both motors off for start-up
     digitalWrite(r_motor_Thumb_p, LOW);
     digitalWrite(r_motor_Index_n, LOW);  //set both motors off for start-up
     digitalWrite(r_motor_Index_p, LOW);
     digitalWrite(r_motor_Majeure_n, LOW);  //set both motors off for start-up
     digitalWrite(r_motor_Majeure_p, LOW);
     digitalWrite(r_motor_Pinky_n, LOW);  //set both motors off for start-up
     digitalWrite(r_motor_Pinky_p, LOW);
}
     
     
       
       
void check(){
    deltaRotthumb = (rotthumbDesired - rotthumbPos);
    if (abs(deltaRotthumb) > 10) { 
          if (deltaRotthumb >0){
            analogWrite(r_motor_Rotthumb_n,speedDesired);
            digitalWrite(r_motor_Rotthumb_p, LOW);
            }
          
          else {
            analogWrite(r_motor_Rotthumb_p,speedDesired);
            digitalWrite(r_motor_Rotthumb_n, LOW);
            }}
     else { 
       digitalWrite(r_motor_Rotthumb_n, LOW);
       digitalWrite(r_motor_Rotthumb_p, LOW);
       }
       
    deltaThumb = (thumbDesired - thumbPos);
    if (abs(deltaThumb) > 10) { 
          if (deltaThumb >0){
            analogWrite(r_motor_Thumb_n,speedDesired);
            digitalWrite(r_motor_Thumb_p, LOW);
            }
          
          else {
            analogWrite(r_motor_Thumb_p,speedDesired);
            digitalWrite(r_motor_Thumb_n, LOW);
            }}
     else { 
       digitalWrite(r_motor_Thumb_n, LOW);
       digitalWrite(r_motor_Thumb_p, LOW);
       }
       
    deltaIndex = (indexDesired - indexPos);
    if (abs(deltaIndex) > 10) { 
          if (deltaIndex >0){
            analogWrite(r_motor_Index_n,speedDesired);
            digitalWrite(r_motor_Index_p, LOW);
            }
          
          else {
            analogWrite(r_motor_Index_p,speedDesired);
            digitalWrite(r_motor_Index_n, LOW);
            }}
     else { 
       digitalWrite(r_motor_Index_n, LOW);
       digitalWrite(r_motor_Index_p, LOW);
       }
       
       deltaMajeure = (majeureDesired - majeurePos);
    if (abs(deltaMajeure) > 10) { 
          if (deltaMajeure >0){
            analogWrite(r_motor_Majeure_n,speedDesired);
            digitalWrite(r_motor_Majeure_p, LOW);
            }
          
          else {
            analogWrite(r_motor_Majeure_p,speedDesired);
            digitalWrite(r_motor_Majeure_n, LOW);
            }}
     else { 
       digitalWrite(r_motor_Majeure_n, LOW);
       digitalWrite(r_motor_Majeure_p, LOW);
       }
       
        deltaPinky = (pinkyDesired - pinkyPos);
    if (abs(deltaPinky) > 10) { 
          if (deltaPinky>0){
            analogWrite(r_motor_Pinky_n,speedDesired);
            digitalWrite(r_motor_Pinky_p, LOW);
            }
          
          else {
            analogWrite(r_motor_Pinky_p,speedDesired);
            digitalWrite(r_motor_Pinky_n, LOW);
            }}
     else { 
       digitalWrite(r_motor_Pinky_n, LOW);
       digitalWrite(r_motor_Pinky_p, LOW);
       }
     }               
