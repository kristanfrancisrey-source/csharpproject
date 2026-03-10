int red = 9;
int blue = 10;
int green = 11;

void setup() {
  pinMode(red, OUTPUT);
  pinMode(blue, OUTPUT);
  pinMode(green, OUTPUT);
}

void loop() {
  // Red light
  digitalWrite(red, HIGH);
  delay(500);
  digitalWrite(red, LOW);

  // Blue light
  digitalWrite(blue, HIGH);
  delay(500);
  digitalWrite(blue, LOW)

  // Green light
  digitalWrite(green, HIGH);
  delay(500);
  digitalWrite(green, LOW);
}
