# Arduino-Codigo-No-videntes
Proyecto Arduino para personas no Videntes 
Basado en detectar obstáculos 

const int trigPins[3] = {13, 11, 9};   // Trig: Frente, Izquierda, Derecha
const int echoPins[3] = {12, 10, 8};   // Echo: Frente, Izquierda, Derecha

// Buzzer
const int buzzer = 3;  
const int buzzerDos = 6;  // Pin del zumbador
void setup() {
  Serial.begin(9600);

  for (int i = 0; i < 3; i++) {
    pinMode(trigPins[i], OUTPUT);
    pinMode(echoPins[i], INPUT);
  }

  pinMode(buzzer, OUTPUT);
  pinMode(buzzerDos, OUTPUT);
}

void loop() {
  int distancias[3];

  for (int i = 0; i < 3; i++) {
    digitalWrite(trigPins[i], LOW);
    delayMicroseconds(2);
    digitalWrite(trigPins[i], HIGH);
    delayMicroseconds(10);
    digitalWrite(trigPins[i], LOW);

    long tiempo = pulseIn(echoPins[i], HIGH, 30000);
    distancias[i] = tiempo * 0.034 / 2;
    delay(50);
  }

  Serial.print("Frente: ");
  Serial.print(distancias[0]);
  Serial.print("cm | Izquierda: ");
  Serial.print(distancias[1]);
  Serial.print("cm | Derecha: ");
  Serial.println(distancias[2]);

  if (distancias[0] > 0 && distancias[0] <= 30) {
    //tone(buzzer, 800); delay(100); noTone(buzzer);  // Frente
    //tone(buzzerDos, 800); delay(100); noTone(buzzerDos);
    analogWrite (3,10); //suena buzz derecha
    analogWrite (6,10); //suena buzz derecha
    delay(200);

    analogWrite (3,0); //OFF buzz derecha
    analogWrite (6,0); //OFF buzz derecha
    delay(100);
   
  }
  if (distancias[1] > 0 && distancias[1] <= 30) {
    //tone(buzzerDos, 800); delay(100); noTone(buzzerDos);  // Izquierda
   
    analogWrite (6,10); //suena buzz izquierda
    delay(200);

    analogWrite (3,0); //OFF buzz derecha
    analogWrite (6,0); //OFF buzz derecha
    delay(100);
  }
  if (distancias[2] > 0 && distancias[2] <= 30) {
    //tone(buzzer, 600); delay(100); noTone(buzzer);  // Derecha
    analogWrite (3,10); //suena buzz derecha
   
    delay(200);

    analogWrite (3,0); 
    analogWrite (6,0);
    delay(100);
  }
  else {
    noTone(buzzer);  // Silencio
  }

  delay(100);
}
