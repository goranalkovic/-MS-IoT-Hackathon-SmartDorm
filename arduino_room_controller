#include"TimerOne.h"

// pins

const int lSensorPin = A1;
int tSensorPin = A3;
const int btnLights = 4;
const int mag1 = 2;
const int mag2 = 3;

const int dLed1Pin = 8;
const int dLed2Pin = 10;
const int dLed3Pin = 9;
const int dLed4Pin = 12;

// vars

double temp;

int reading;

int b1C = 0;
int b1S = 0;
int b1LS = 0;

int b2C = 0;
int b2S = 0;
int b2LS = 0;

int b3C = 0;
int b3S = 0;
int b3LS = 0;

int mag1val = 0;
int mag2val = 0;

int blindsLevel;
int blindsLow = 760;
int blindsHigh = 220;

int blindsActive = LOW;

//

void setup(){

  Serial.begin(9600);

  Timer1.initialize(100000);
  Timer1.attachInterrupt(devicesSensor);

  // debug led self-test

  Serial.println("\n[INFO] Press and hold the button to calibrate the light sensor.");
  
  int btn = digitalRead(13);
  if(btn == HIGH) {  
  
  blindsHigh = calibrateLight(lSensorPin);
  blindsLow = calibrateLowLight(lSensorPin);
  }

  delay(1000);

  digitalWrite(dLed1Pin, HIGH);
  digitalWrite(dLed2Pin, HIGH);
  digitalWrite(dLed3Pin, HIGH);
  digitalWrite(dLed4Pin, HIGH);
  delay(1000);
  digitalWrite(dLed1Pin, LOW);
  digitalWrite(dLed2Pin, LOW);
  digitalWrite(dLed3Pin, LOW);
  digitalWrite(dLed4Pin, LOW);
  
  Serial.println("LOADED!");

}

void loop(){
  float temp = getTemperature(tSensorPin);
  Serial.print("Temperature: "); Serial.print(temp); Serial.print(" C\n");

  blindsActive = operateBlinds(blindsLow,lSensorPin);
  if(blindsActive == HIGH) digitalWrite(dLed2Pin, HIGH);
  if(blindsActive == LOW) digitalWrite(dLed2Pin, LOW);
}

////

float getTemperature(int pin){
  int i; float sum;
  

    for (i = 0; i <= 6; i++){
      float temp = analogRead(pin);
      temp = (5.0 * temp * 100.0) / 1024.0;
      sum += temp ;
    delay(1000);
  }
  sum = sum / i;

  return sum;
}

////

void devicesSensor(){
  
  static int LastRead;
  if(LastRead && (0 == digitalRead(mag1))){
    Serial.println("Door opened!");
  }
  LastRead = digitalRead(mag1);

    static int LastRead2;
  if(LastRead2 && (0 == digitalRead(mag2))){
    Serial.println("Window opened!");
  }
  LastRead2 = digitalRead(mag2);
 

  b1LS = b1S;

     b1S = digitalRead(btnLights);
  if(b1S != b1LS){
    if(b1S == HIGH){
      b1C++;
    }
    delay(50);
  }

  if(b1C % 2 == 0 && (0 != b1S)) {
  digitalWrite(dLed1Pin, LOW); 
  Serial.println("Lights off");
  delay(100);
    }
    
  if(b1C % 2 != 0 && (0 != b1LS)){
  digitalWrite(dLed1Pin, HIGH);
  Serial.println("Lights ON");
  delay(100);
  }
  
}

////////////

int calibrateLight(int pin){
  int i, sumHigh, valHigh = 0;
  Serial.println("---- LIGHT SENSOR CALIBRATION ----");
  Serial.println("\nWhen you see the message, point the light sensor to a WELL LIT area\nHold it for 4 seconds until you see the message.");
  delay(2500);
  digitalWrite(dLed1Pin,HIGH);
   Serial.println("Calibrating WELL LIT AREA now!");
  for (i = 0; i <= 17; i++){
    valHigh = analogRead(pin);
    Serial.print("  Current reading: "); Serial.print(valHigh); Serial.println();
    sumHigh += valHigh;
    delay(250);
  }
  sumHigh = sumHigh / i;
  Serial.print("Calibration for the HIGH level: "); Serial.print(sumHigh); Serial.print(". Proceeding...\n\n");
  digitalWrite(dLed1Pin, LOW);

    return sumHigh;
}

int calibrateLowLight(int pin){
  int  j, sumLow, valLow = 0;
    Serial.println("Calibration for the HIGH level DONE! Starting calibration for LOW level...");
    Serial.println("\nWhen you see the message, point the light sensor to a DARK area\nHold it for 4 seconds until you see the message.");
  delay(2500);
  digitalWrite(dLed1Pin,HIGH);
   Serial.println("Calibrating DARK now!");
  for (j = 0; j <= 17; j++){
    valLow = analogRead(pin);
    Serial.print("  Current reading: "); Serial.print(valLow); Serial.println();
    sumLow += valLow;
    delay(250);
  }
  sumLow = sumLow / j;
    Serial.print("Calibration for the LOW level: "); Serial.print(sumLow); Serial.print(".\n\n");
    digitalWrite(dLed1Pin, LOW);

    return sumLow;
}

///////////

int operateBlinds(int border, int pin){
  int val = analogRead(pin);
  //Serial.println(val);
  int active;
  static int avg = border - (border * 0.3);

  if(active == LOW & val < avg){
        active = HIGH;
        Serial.println("Blinds going down");
    }
    
  if(active == HIGH & val > avg){
        active = LOW;
        Serial.println("Blinds going up");
    }

 delay(5000);
 return active;

}
