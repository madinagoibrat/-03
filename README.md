#include "DHT.h"
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

#define DHTTYPE DHT11

int pinDHT11 = 8;
int pinSoilMoisture = A0;
int pinPhotoresistor = A2;
int redLedPin = 13;    // Пин для красного светодиода
int greenLedPin = 12;  // Пин для зеленого светодиода

DHT dht(pinDHT11, DHTTYPE);
LiquidCrystal_I2C lcd(0x27, 20, 4);  // Адрес 0x27 для дисплея 2004 I2C

void setup()
{
  Serial.begin(9600);
  dht.begin();

  pinMode(redLedPin, OUTPUT);
  pinMode(greenLedPin, OUTPUT);

  lcd.begin(20, 4);
}

void loop()
{
  float h = dht.readHumidity();
  float t = dht.readTemperature();

  lcd.clear();  // Очистка дисплея перед выводом новых данных
  lcd.setCursor(0, 0);
  lcd.print("Hum= ");
  lcd.print(h);
  lcd.print(" %");

  lcd.setCursor(0, 1);
  lcd.print("Temp= ");
  lcd.print(t);
  lcd.print(" C");

  // Управление светодиодами на основе показаний влажности
  if (h < 25.0)
  {
    digitalWrite(redLedPin, HIGH);   // Красный светодиод включен
    digitalWrite(greenLedPin, LOW);  // Зеленый светодиод выключен
  }
  else
  {
    digitalWrite(redLedPin, LOW);    // Красный светодиод выключен
    digitalWrite(greenLedPin, HIGH); // Зеленый светодиод включен
  }

  lcd.setCursor(0, 2);
  lcd.print("Soil6= ");
  lcd.print(analogRead(pinSoilMoisture));

  lcd.setCursor(0, 3);
  lcd.print("Light= ");
  lcd.print(analogRead(pinPhotoresistor));

  delay(500);
}
