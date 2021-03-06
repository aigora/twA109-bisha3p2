# Sistema de riego

Nuestro proyecto se basa en crear un sistema de riego automático que trabaje en función de la humedad del suelo y la temperatura ambiente, en función de unos valores establecidos en arduino, de modo que si el sistema se ha puesto en funcionamiento el ordenador mostrará la hora, la fecha, y los valores de temperatura y humedad. 

## Integrantes del equipo 

Daniel Sánchez García - danielsanchezg
Sara Rodríguez Fernández - sararodriguezfernandez
Marcos Lozano Rodríguez - marcoslozanorodriguez

## Objetivos del trabajo

Aprender a utilizar el lenguaje C++ para capacitar un sistema de riego.
Ser capaz de conectar un arduino junto a sensores al ordenador con el que controlar las variables de la temperatura y la humedad.
Poder mostrar una representación de la programación teórica que recibimos en clase en la vida real,viendo las acciones que toma el programa en un terreno bajo un sistema de riego que se efectua bajo unos estados y condiciones determinadas.
## Funcionamiento

Mediante el sensor de humedad y temperatura recogemos los valores de temperatura y humedad, de tal forma que si la temperatura está entre 20 y 30 grados Celsius y el porcentaje de humedad se encuentra entre el 20% y el 40%, el riego se activará durante un tiempo, en este caso para simular el riego se utilizará un LED. El ordenador recibe estos datos y muestra la hora, la fecha y los valores registrados cuando se ha activado el riego.

## Sensores
Sensor de temperatura y humedad relativa en el aire DHT22.

## Funciones utilizadas
Función para devolver la hora: struct tm* time(time_t)
Función para la fecha: size_t strftime(char *,size_t,char *,struct tm *)
Función para la conexión ordenador-arduino: void autoConnect(SerialPort *arduino,char*)



## Código en arduino
#include "DHT.h"
#define DHTPIN 2    
#define DHTTYPE DHT22   

const int ledPIN = 8;
const float maxh = 40.00;
const float minh = 20.00;
const float maxt = 30.00;
const float mint = 20.00;

DHT dht(DHTPIN, DHTTYPE);

void setup()
{
  Serial.begin(9600);//iniciar puerto serie
  
  pinMode(ledPIN , OUTPUT);  //definir pin como salida
  dht.begin();
 
}

void loop() 
{
  
  // Leer porcentaje de humedad
  float h = dht.readHumidity();
  // Leer temperatura en Celsius
  float t = dht.readTemperature();
  // Esperar un minuto entre medidas
  
  if (isnan(h) || isnan(t))
  {
    Serial.println(F("Error en la lectura del sensor"));
    return;
  }
  
  
  float hic = dht.computeHeatIndex(t, h, false);

  Serial.print(F("Humedad: "));
  Serial.print(h);
  Serial.print(F("%  Temperatura: "));
  Serial.print(t);
  Serial.print(F(" Grados C"));

  Serial.print(hic);
  Serial.println(F(" Grados C "));

  if (t < maxt && t > mint && h < maxh && h > minh)
  {
  digitalWrite(ledPIN , HIGH);   // poner el Pin en HIGH
  delay(2000);                   // esperar un segundo
  digitalWrite(ledPIN , LOW);    // poner el Pin en LOW
  delay(1000); // esperar un segundo
  }
// Tiempo entre medidas
  delay(5000);
}
## Código en Dev c++
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>
#include "SerialPort.h"
#define MAX_DATA_LENGTH 255
// Funciones prototipo
void autoConnect(SerialPort *arduino,char*);
int main(void)

{
	
 //Arduino SerialPort object
 SerialPort *arduino;
 // Puerto serie en el que está Arduino
 char* portName = "\\\\.\\COM7";
 // Buffer para datos procedentes de Arduino
 char incomingData[MAX_DATA_LENGTH];

 // Crear estructura de datos del puerto serie
 arduino = (SerialPort *)malloc(sizeof(SerialPort));
 // Apertura del puerto serie
 Crear_Conexion(arduino,portName);
 autoConnect(arduino,incomingData);
}
void autoConnect(SerialPort *arduino,char *incomingData)
{
 int readResult;
  time_t t;
  struct tm *tm;
  char fecha[100];

// Espera la conexión con Arduino
while (!isConnected(arduino))
{
Sleep(100);
Crear_Conexion(arduino,arduino->portName);
}
 //Comprueba si arduino está connectado
if (isConnected(arduino))
{
printf ("Conectado con Arduino en el puerto %s\n",arduino->portName);
printf ("\n");
}
 // Bucle de la aplicación
while (isConnected(arduino))
{
 readResult = readSerialPort(arduino,incomingData, MAX_DATA_LENGTH);
if (readResult!=0)
{
  t=time(NULL);
  tm=localtime(&t);
  strftime(fecha, 100, "%d/%m/%Y", tm);
  printf ("El sistema ejecuto el riego el %s ", fecha);
  printf ("a las %d:%d:%d\n",tm->tm_hour,tm->tm_min,tm->tm_sec);
  printf("%s \n", incomingData,readResult);
 Sleep(10);
}
}
if (!isConnected(arduino))
 printf ("Se ha perdido la conexion con Arduino\n");
}
## Nota
Ya que a la hora de guardar el código en esta plataforma se come los saltos de línea adjuntamos fotos de los códigos
