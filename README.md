# sound_sensor.ino
const int soundDigital = 7;   // D0 من الحساس
const int soundAnalog  = A0;  // A0 من الحساس
const int ledPin       = 8;   // LED عبر مقاومة 220Ω

bool ledState = false;
bool lastSoundState = LOW;

void setup() {
  pinMode(soundDigital, INPUT);
  pinMode(ledPin, OUTPUT);

  Serial.begin(9600);
}

void loop() {
  int level = analogRead(soundAnalog);
  Serial.println(level);

  bool soundState = digitalRead(soundDigital);

  // نكتشف بداية الصوت فقط
  if (soundState == HIGH && lastSoundState == LOW) {
    ledState = !ledState;              // يعكس حالة الـLED
    digitalWrite(ledPin, ledState);

    delay(100); // يمنع احتساب نفس الصوت عدة مرات
  }

  lastSoundState = soundState;
}