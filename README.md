# Práctica ESP32 con encendido de led a través de un botón usando Node Red de Diego Llampallas
Este repositorio muestra como podemos programar una ESP32 para encender un led simulando el encendido de un motor con un relay por medio de node red.


## Introducción
A través de la página https://wokwi.com/  se puede hacer simulaciones de programas con arduino y sensores.
### Descripción

La ```Esp32``` la utilizamos en un entorno de adquision de datos, lo cual en esta practica encenderemos un led simulando que es un encendido de un motor usando la página web creada por node red; Cabe aclarar que esta practica se usara un simulador llamado [WOKWI](https://https://wokwi.com/).


## Material Necesario

Para realizar esta practica se usaran los siguientes elementos:

- [WOKWI](https://https://wokwi.com/)
- Tarjeta ESP 32
- Relay
- [NODE RED](http://localhost:1880/)


## Instrucciones

### Requisitos previos

Para poder usar este repositorio necesitas entrar a la plataforma [WOKWI](https://https://wokwi.com/).

### Instrucciones de preparación de entorno 
1. Una vez dentro de wokwi seleccionar la tarjeta ESP32

![](https://github.com/DiegoLlampallas/Practica-DHT22/blob/main/6.png?raw=true)

2. Abrir la terminal de programación y colocar la siguente programación:

## Programación

```
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "Wokwi-GUEST";
const char* password = "";
const char* mqttServer = "52.29.234.128";
const int mqttPort = 1883;
const char* mqttUser = "DiegoAlberto";
const char* mqttPassword = "1234";
const char* mqttTopic = "DiegoRelay";

WiFiClient espClient;
PubSubClient client(espClient);

int ledPin = 13; // Pin del LED

void setup() {
  pinMode(ledPin, OUTPUT);
  Serial.begin(115200);
  setup_wifi();
  client.setServer(mqttServer, mqttPort);
  client.setCallback(callback);
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();
}

void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Conectando a: ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("Conectado a la red WiFi");
}

void reconnect() {
  while (!client.connected()) {
    Serial.print("Intentando conexión MQTT...");
    if (client.connect("ESP32Client", mqttUser, mqttPassword)) {
      Serial.println("Conectado");
      client.subscribe(mqttTopic);
    } else {
      Serial.print("Error de conexión, rc=");
      Serial.print(client.state());
      Serial.println(" Intentando de nuevo en 5 segundos");
      delay(5000);
    }
  }
}

void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Mensaje recibido: [");
  Serial.print(topic);
  Serial.print("] ");
  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println();

  if (strcmp(topic, mqttTopic) == 0) {
    if ((char)payload[0] == '1') {
      digitalWrite(ledPin, HIGH);
    } else {
      digitalWrite(ledPin, LOW);
    }
  }
}

```


## Partes
1. ![](https://github.com/DiegoLlampallas/ACTIVARLED/blob/main/30.png?raw=true)


## Librerias
![](https://github.com/DiegoLlampallas/ACTIVARLED/blob/main/31.png?raw=true)

## Conexión

![](https://github.com/DiegoLlampallas/ACTIVARLED/blob/main/32.png?raw=true)

# Configuración y conexión de node red

## Conexión de node red

 1. Colocar bloque ```switch```.

![](https://github.com/DiegoLlampallas/ACTIVARLED/blob/main/33.png?raw=true)

 2. Colocar bloque ```mqqtt out```.

![](https://github.com/DiegoLlampallas/ACTIVARLED/blob/main/34.png?raw=true)


 3. Conectamos todos los componentes como se muestra en la imagen:

![](https://github.com/DiegoLlampallas/ACTIVARLED/blob/main/35.png?raw=true)

Tras conectar todo se pasa a la configuración.

## Configuración de node red

1. Vamos a la esquina superior derecha y en la "flechita que apunta  hacia abajo seleccionamos y buscamos "Dashboard".

![](https://github.com/DiegoLlampallas/DHT22NODERED/blob/main/21.png?raw=true)

2. Le damos de"+ tab".

![](https://github.com/DiegoLlampallas/DHT22NODERED/blob/main/22.png?raw=true)

3. Le damos en "+ group".

![](https://github.com/DiegoLlampallas/DHT22NODERED/blob/main/23.png?raw=true)

4. Le damos en "edit" al "tab" creado.

![](https://github.com/DiegoLlampallas/DHT22NODERED/blob/main/24.png?raw=true)

5. Llenamos los datos con la información tal y como aparece en la siguiente imagen:

![](https://github.com/DiegoLlampallas/ACTIVARLED/blob/main/36.png?raw=true)

6. Le damos en "edit" al "grou 1" creado.

![](https://github.com/DiegoLlampallas/DHT22NODERED/blob/main/26.png?raw=true)

7. Llenamos los datos con la información tal y como aparece en la siguiente imagen:

![](https://github.com/DiegoLlampallas/ACTIVARLED/blob/main/37.png?raw=true)


Tras completar todo esto vamos a ir seleccionando y dando doble click a cada bloque y llenarlo con la información necesaria, en este caso de izquierda a derecha:

1. Edit "switch" y completar los datos mostrados en la imagen:

![](https://github.com/DiegoLlampallas/ACTIVARLED/blob/main/38.png?raw=true)

2. Edit "mqtt" y completar los datos mostrados en la imagen:

![](https://github.com/DiegoLlampallas/ACTIVARLED/blob/main/39.png?raw=true)



Tras completar todo esto puede empezar la simulación de operación tanto en "wokwi" como en el "node red" como se mostrará acontinuación: 

### Instrucciónes de operación

1. Iniciar simulador "wokwi": 


2. Una vez que conecte debe mostrará el acceso:

![](https://github.com/DiegoLlampallas/ACTIVARLED/blob/main/40.png?raw=true)
 
3. Tras esto podemos dar en iniciar en "node red" como se muestra en la siguiente imagen:

![](https://github.com/DiegoLlampallas/DHT22NODERED/blob/main/30.png?raw=true)

3. Colocar la distancia y humedad dando *doble click* al sensor **ultrasónico** 

4. Para acceder a la información mandada por el "ESP32" hay dos opciones para acceder a la página web creada:

Opción 1:

1. Escribiendo el código "localhost/1880/ic" en la imagen en una nueva página limpia:

![](https://github.com/DiegoLlampallas/DHT22NODERED/blob/main/31.png?raw=true)

Opción 2:

2. Dando click en la esquina superior derecha por donde esta dashboard:

![](https://github.com/DiegoLlampallas/DHT22NODERED/blob/main/32.png?raw=true)

## Resultados

Cuando haya funcionado, verás los valores dentro del monitor serial en "wokwi" y en la página creada.

## Funcionamiento

1. Funcionamiento tanto en "wokwi" como en la página web creada:

LED APAGADO:

![](https://github.com/DiegoLlampallas/ACTIVARLED/blob/main/41.png?raw=true)

LED ENCENDIDO:

![](https://github.com/DiegoLlampallas/ACTIVARLED/blob/main/42.png?raw=true)

## Evidencias

[Página](https://wokwi.com/projects/367802604636969985) 


# Créditos

Desarrollado por Ing. Diego Alberto Llampallas Vega

- [GitHub](https://github.com/DiegoLlampallas)