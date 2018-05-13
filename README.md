# HackatonTalaIlegal
Sistema de sensores PIR (movimiento) que al activarse enciendan una luz y activen una alarma.
nombre del proyecto: Lignum
nombre de integrantes: Gabriela Villatoro, Dedzly Mejía, María José Gudiel, Andrea Reyes.

Programación:

int calibrationTime = 30;        
long unsigned int lowIn;         
long unsigned int pause = 5000;  
boolean lockLow = true;
boolean takeLowTime;  
int pirPin = 7;    
int ledPin = 8;

const int pinBuzzer=3;
const int pinBuzzer2=4;

void setup() {
  
  Serial.begin(9600);
  pinMode(pirPin, INPUT);
  pinMode(ledPin, OUTPUT);
  digitalWrite(pirPin, LOW);
  Serial.print("calibrating sensor ");
    for(int i = 0; i < calibrationTime; i++){
      Serial.print(".");
      delay(1000);
      }
    Serial.println(" done");
    Serial.println("SENSOR ACTIVE");
    delay(50);

  }

void loop() {
  
  if(digitalRead(pirPin) == HIGH){
       digitalWrite(ledPin, HIGH);
         tone(pinBuzzer, 5000);
         delay(0);
          tone(pinBuzzer2, 5000);
          delay(0);
         noTone(pinBuzzer);
  delay(0);
   noTone(pinBuzzer2);
  delay(0);
    tone(pinBuzzer, 1000, 5000);
    tone(pinBuzzer2, 1000, 5000);
       if(lockLow){  
         lockLow = false;            
         Serial.println("---");
         Serial.print("motion detected at ");
         Serial.print(millis()/1000);
         Serial.println(" sec"); 
         delay(50);
         }         
         takeLowTime = true;
       }
  if(digitalRead(pirPin) == LOW){       
       digitalWrite(ledPin, LOW);  
  if(takeLowTime){
        lowIn = millis();          
        takeLowTime = false;       
        }
  if(!lockLow && millis() - lowIn > pause){  
            lockLow = true;                        
           Serial.print("motion ended at ");     
           Serial.print((millis() - pause)/1000);
           Serial.println(" sec");
           delay(50);
           }
      } 
}
