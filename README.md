# ultrasonic-distance-alert-lcd
Ultrasəs məsafə sensoru ilə LCD və xəbərdarlıq sistemi
# Ultrasəs Məsafə Ölçmə və Xəbərdarlıq Sistemi

Bu layihədə **HC-SR04 ultrasəs sensoru** ilə məsafə ölçülür və nəticə həm **LCD ekran** üzərində göstərilir, həm də **LED-lər və buzzer** ilə xəbərdarlıq verilir.

##  İstifadə olunan komponentlər:
- Arduino Uno
- HC-SR04 Sensor
- LCD I2C 16x2
- Yaşıl, Sarı, Qırmızı LED
- Buzzer
- 3x 220 ohm rezistor
- Jumper kabellər

##  Funksiya:
- **Məsafə > 100 sm** → Yaşıl LED
- **100 <= Məsafə ≤ 300 sm** → Sarı LED
- **Məsafə > 300 sm** → Qırmızı LED + Buzzer siqnalı
- Bütün məsafə məlumatı LCD-də göstərilir (sm ilə)

## Tinkercad Simulyasiya Linki:
https://www.tinkercad.com/things/7D4SlqF7Mds-dazzling-elzing-hillar

## Arduino Code
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

#define trigPin 4
#define echoPin 3

#define greenLED 5
#define yellowLED 6
#define redLED 7
#define buzzer 2

LiquidCrystal_I2C lcd(0x27, 16, 2); // Əgər 0x27 işləmirsə, 0x3F və ya 0x20 yoxla

void setup() {
  Serial.begin(9600);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  pinMode(greenLED, OUTPUT);
  pinMode(yellowLED, OUTPUT);
  pinMode(redLED, OUTPUT);
  pinMode(buzzer, OUTPUT);

  lcd.init();
  lcd.backlight();
  lcd.setCursor(0, 0);
  lcd.print("Messefe Olculur");
  delay(1000);
}

void loop() {
  long duration;
  float distance;

  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2;

  Serial.print("Messefe: ");
  Serial.print(distance);
  Serial.println(" cm");

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Messefe:");
  lcd.setCursor(8, 0);
  lcd.print(distance);
  lcd.print("cm");

  if (distance < 100) {
    digitalWrite(greenLED, HIGH);
    digitalWrite(yellowLED, LOW);
    digitalWrite(redLED, LOW);
    noTone(2);
  } else if (distance  >=100  && distance <= 300) {
    digitalWrite(greenLED, LOW);
    digitalWrite(yellowLED, HIGH);
    digitalWrite(redLED, LOW);
    noTone(2);
  } else {
    digitalWrite(greenLED, LOW);
    digitalWrite(yellowLED, LOW);
    digitalWrite(redLED, HIGH);
    tone(2,500);
  }

  delay(500);
}


