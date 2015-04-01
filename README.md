/* 
RPM-counter
RPM counter for the linear increasing voltage motor with distance sensor. 
created 25 March 2015
by Mustafa Fatih Bayram
 */
 

int SENSOR = 2 ; // define the Hall magnetic sensor interface
int motor = 5;

#define echoPin 7 // Echo Pin
#define trigPin 8 // Trigger Pin

long duration, distance;
long prevtime = 0;
unsigned long time;
unsigned long int rpm=0;
volatile int state = LOW;
int a=0;

void setup ()
{
  Serial.begin(9600);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(motor,OUTPUT);
  attachInterrupt( 0, blik, CHANGE);
  
  
}
void loop ()
{
 digitalWrite(trigPin, LOW); 
 delayMicroseconds(2); 

 digitalWrite(trigPin, HIGH);
 delayMicroseconds(10); 
 
 digitalWrite(trigPin, LOW);
 duration = pulseIn(echoPin, HIGH);
 
 //Calculate the distance (in cm) based on the speed of sound.
 distance = duration/58.2;
if(distance<=10)
       {
            analogWrite(motor,25*distance);delay(50); 
       }   
      else  analogWrite(motor,0);
            
}

void blik()
{
   
  if (digitalRead(SENSOR) == HIGH) 
  {
    state = !state;
    a=a+1;
      if(a%10 == 0)
    {
      rpm=600000/(millis()-prevtime);
      Serial.println(rpm);
      prevtime=millis();   
    }
   
  }
    
}
   
