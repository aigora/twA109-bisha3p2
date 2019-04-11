# Sistema de riego

Nuestro proyecto se basa en crear un sistema de riego automático que trabaje en función de la humedad del suelo y la temperatura ambiente, cuyos valores se introducirían a traves del ordenador. 

## Integrantes del equipo 

Daniel Sánchez García - danielsanchezg
Sara Rodríguez Fernández - sararodriguezfernandez
Marcos Lozano Rodríguez - marcoslozanorodriguez

## Objetivos del trabajo

Aprender a utilizar el lenguaje C++ para capacitar un sistema de riego.
Ser capaz de conectar un arduino junto a sensores al ordenador con el que controlar las variables de la temperatura y la humedad.
Poder mostrar una representación de la programación teórica que recibimos en clase en la vida real,viendo las acciones que toma el programa en un terreno bajo un sistema de riego que se efectua bajo unos estados y condiciones determinadas

## Sensores
Sensor de temperatura y humedad relativa en el aire DHT11

## Funciones utilizadas
Una función dentro del menú principal para elegir la temperatura mínima para que regar sea necesario.
int elegir_temperatura_minima (void)
{
	int temperaturaminima;
	printf("Introduzca un valor para la temperatura minima:\n");
	scanf("%d",&temperaturaminima);
	return temperaturaminima;
	  
}
Una función dentro del menú principal para elegir la humedad mínima para que regar sea necesario.
{
	int humedadminima;
	printf("Introduzca un valor para la humedad minima:\n");
	scanf("%d",&humedadminima);
	return humedadminima;
	  
}


Una función que reciba los valores de temperatura y humedad de los sensores para determinar si es necesario regar.
int es_necesario_regar (float, int);
int es_necesario_regar (float temperatura, int humedad)
{
	int regar=1;
	if (temperatura<temperaturaminima || humedad<humedadminima)
	regar=0;
	return regar;
	  
}
