// Stoplight 1 Pins
#define RED_PIN_1 2
#define YELLOW_PIN_1 3
#define GREEN_PIN_1 4

// Stoplight 2 Pins
#define RED_PIN_2 5
#define YELLOW_PIN_2 6
#define GREEN_PIN_2 7

// Sensor 1 Pins
#define TRIG_PIN_1 8
#define ECHO_PIN_1 9

// Sensor 2 Pins
#define TRIG_PIN_2 10
#define ECHO_PIN_2 11 

float tau = 2.0;          
float Ts = 0.1;          
float alpha;             

float filteredDistance1 = 0;
float previousFiltered1 = 0;
float filteredDistance2 = 0;
float previousFiltered2 = 0;

int minGreen = 3000;      
int maxGreen = 10000;     

void setup() {
  Serial.begin(9600);
  
  pinMode(TRIG_PIN_1, OUTPUT);
  pinMode(ECHO_PIN_1, INPUT);
  
  pinMode(TRIG_PIN_2, OUTPUT);
  pinMode(ECHO_PIN_2, INPUT);
  
  pinMode(RED_PIN_1, OUTPUT);
  pinMode(YELLOW_PIN_1, OUTPUT);
  pinMode(GREEN_PIN_1, OUTPUT);
  
  pinMode(RED_PIN_2, OUTPUT);
  pinMode(YELLOW_PIN_2, OUTPUT);
  pinMode(GREEN_PIN_2, OUTPUT);
  
  alpha = Ts / (tau + Ts);
}

float readDistance(int trigPin, int echoPin) {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  long duration = pulseIn(echoPin, HIGH);
  float distance = duration * 0.034 / 2;
  return distance; 
}

void loop() {
  // --- Sensor 1 (Stoplight 1) Data ---
  float distance1 = readDistance(TRIG_PIN_1, ECHO_PIN_1);
  filteredDistance1 = previousFiltered1 + alpha * (distance1 - previousFiltered1);
  previousFiltered1 = filteredDistance1;
  float density1 = 100 - filteredDistance1; 
  if (density1 < 0) density1 = 0;
  if (density1 > 100) density1 = 100;
  int greenTime1 = map(density1, 0, 100, minGreen, maxGreen);

  // --- Sensor 2 (Stoplight 2) Data ---
  float distance2 = readDistance(TRIG_PIN_2, ECHO_PIN_2);
  filteredDistance2 = previousFiltered2 + alpha * (distance2 - previousFiltered2);
  previousFiltered2 = filteredDistance2;
  float density2 = 100 - filteredDistance2; 
  if (density2 < 0) density2 = 0;
  if (density2 > 100) density2 = 100;
  int greenTime2 = map(density2, 0, 100, minGreen, maxGreen);

  Serial.print("Light 1 Green Time: ");
  Serial.print(greenTime1);
  Serial.print(" | Light 2 Green Time: ");
  Serial.println(greenTime2);

  // Phase 1: Stoplight 1 is GREEN, Stoplight 2 is RED
  digitalWrite(RED_PIN_1, LOW);
  digitalWrite(YELLOW_PIN_1, LOW);
  digitalWrite(GREEN_PIN_1, HIGH);
  
  digitalWrite(RED_PIN_2, HIGH);
  digitalWrite(YELLOW_PIN_2, LOW);
  digitalWrite(GREEN_PIN_2, LOW);
  
  delay(greenTime1);

  // Phase 2: Stoplight 1 is YELLOW, Stoplight 2 is RED
  digitalWrite(GREEN_PIN_1, LOW);
  digitalWrite(YELLOW_PIN_1, HIGH);
  
  delay(2000);

  // Phase 3: Stoplight 1 is RED, Stoplight 2 is GREEN
  digitalWrite(YELLOW_PIN_1, LOW);
  digitalWrite(RED_PIN_1, HIGH);
  
  digitalWrite(RED_PIN_2, LOW);
  digitalWrite(GREEN_PIN_2, HIGH);
  
  delay(greenTime2);
  
  // Phase 4: Stoplight 1 is RED, Stoplight 2 is YELLOW
  digitalWrite(GREEN_PIN_2, LOW);
  digitalWrite(YELLOW_PIN_2, HIGH);
  
  delay(2000);
}
