# Arduino Code for Syringe Pump
[Return Home](/SyringePumpDemo/index.html)
It's worth noting that this is code I wrote, and thus differs from the tutorial code in several ways. It allows for microstepping without speed loss through step width modulation, for one, and allows the user to set manual and routine loops easily.

This works for the Uno and Mega. Paste it into the IDE to get started.

```
// defines pins numbers
const int stepPin = 3; 
const int dirPin = 2; 
const float microstep = 16; // This alters steps per rotation and pulse width to maintain speed. No decision tree currently. 
const int stepsperrotation = 200*microstep;
const int pwm = 1000/microstep;
const float inchperspin = 3.0;
float incomingByte;
int setMode;
 
void setup() {
  // Sets the two pins as Outputs
  pinMode(stepPin,OUTPUT); 
  pinMode(dirPin,OUTPUT);
  Serial.begin(9600); // opens serial port, sets data rate to 9600 bps
  Serial.print("Would you like to run in manual mode or set up a routine?\n");
  Serial.print("1. Manual\n");
  Serial.print("2. Routine\n");
  digitalWrite(10,HIGH);
  digitalWrite(9,HIGH);
  digitalWrite(8,HIGH);
  while(Serial.available() < 1){
    delay(100); 
  }      
  setMode = Serial.readString().toInt();  
}

void spinches(float inches){
  spin(inches/inchperspin);
}

// Takes number of rotations and turns that many. Sign of rotations variable determines direction.
void spin(float rotations) {
  int pulses = abs(rotations*stepsperrotation);
  if(rotations > 0){
    digitalWrite(dirPin,HIGH); // Enables the motor to move in a particular direction
  } else {
    digitalWrite(dirPin,LOW); // Enables the motor to move in a particular direction
  }
  for(int x = 0; x < pulses; x++) {
    digitalWrite(stepPin,HIGH); 
    delayMicroseconds(pwm); 
    digitalWrite(stepPin,LOW); 
    delayMicroseconds(pwm); 
  }
}

void manual() {
    Serial.print("Manual control: How many rotations should we spin?\n");
    while(Serial.available() < 1){
      delay(100); 
    }      
    incomingByte = Serial.readString().toFloat();
    Serial.print("We received:");
    Serial.print(incomingByte);
    Serial.print("\n");
    spin(incomingByte);
}

float routine(){
  Serial.print("Routine control: How many cycles (1 spin + 1 delay) are we going for?\n");
  while(Serial.available() < 1){
    delay(100); 
  }      
  int cycles = Serial.readString().toInt();
  float spincycle[cycles];
  float delaycycle[cycles];
  float input;
  for(int i = 0; i < cycles; i++){
    Serial.print("Spins in the cycle?\n");
    while(Serial.available() < 1){
      delay(100); 
    }      
    spincycle[i] = Serial.readString().toFloat();
    Serial.print("Delay? (seconds)\n");
    while(Serial.available() < 1){
      delay(100); 
    }      
    delaycycle[i] = Serial.readString().toFloat()*1000;
  }
  int runagain = 1;
  while(runagain == 1){
    for(int i = 0; i < (sizeof(spincycle) / sizeof(spincycle[0])); i++){
      spin(spincycle[i]);
      delay(delaycycle[i]);
    }
    Serial.print("Routine control: Run this routine again? 1. Yes. 2. No.\n");
    while(Serial.available() < 1){
      delay(100); 
    }      
    runagain = Serial.readString().toInt();
  }
}

int runcycle(float* spincycle, float* delaycycle){
  for(int i = 0; i < (sizeof(spincycle) / sizeof(spincycle[0])); i++){
    spin(spincycle[i]);
    delay(delaycycle[i]);
  }
  Serial.print("Routine control: Run this routine again? 1. Yes. 2. No.\n");
  while(Serial.available() < 1){
    delay(100); 
  }      
  int incoming = Serial.readString().toInt();
  return incoming;
}



void loop() {
  if(setMode == 1){
    manual();
  } else {
    routine();
  }  
}
```
