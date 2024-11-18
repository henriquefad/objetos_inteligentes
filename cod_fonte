#include <WiFi.h>
#include <PubSubClient.h>
#include <DHT.h>

// Definir pinos
#define DHTPIN 4          // Pino onde o DHT22 está conectado
#define RELAY_PIN 15      // Pino onde o relé de estado sólido está conectado

// Definir parâmetros do DHT22
#define DHTTYPE DHT22

DHT dht(DHTPIN, DHTTYPE);  // Inicializa o sensor DHT22

// Definir credenciais do Wi-Fi
const char* ssid = "SEU_SSID";
const char* password = "SUA_SENHA";

// Definir broker MQTT
const char* mqtt_server = "broker.mqtt-dashboard.com"; // Exemplo de broker MQTT público

WiFiClient espClient;
PubSubClient client(espClient);

// Variáveis de temperatura e umidade
float temperature = 0.0;
float humidity = 0.0;

// Função de conexão com Wi-Fi
void setup_wifi() {
  delay(10);
  // Conectar à rede Wi-Fi
  Serial.print("Conectando-se a ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("Wi-Fi conectado");
  Serial.print("Endereço IP: ");
  Serial.println(WiFi.localIP());
}

// Função de reconexão MQTT
void reconnect() {
  while (!client.connected()) {
    Serial.print("Tentando conexão MQTT...");
    if (client.connect("ESP32Client")) {
      Serial.println("Conectado");
    } else {
      Serial.print("Falha na conexão. Código de erro: ");
      Serial.print(client.state());
      delay(5000);
    }
  }
}

// Função para configurar o relé de estado sólido
void controlRelay(bool state) {
  if (state) {
    digitalWrite(RELAY_PIN, HIGH);  // Aciona o relé (Liga o dispositivo)
    Serial.println("Relé acionado (Dispositivo ligado)");
  } else {
    digitalWrite(RELAY_PIN, LOW);   // Desliga o relé (Desliga o dispositivo)
    Serial.println("Relé desacionado (Dispositivo desligado)");
  }
}

// Função de leitura do sensor DHT22 e envio de dados via MQTT
void readAndSendSensorData() {
  humidity = dht.readHumidity();
  temperature = dht.readTemperature();

  // Verifica se a leitura falhou
  if (isnan(humidity) || isnan(temperature)) {
    Serial.println("Falha na leitura do sensor DHT22");
    return;
  }

  Serial.print("Temperatura: ");
  Serial.print(temperature);
  Serial.print("°C  Umidade: ");
  Serial.print(humidity);
  Serial.println("%");

  // Envia os dados para a nuvem via MQTT
  String tempStr = String(temperature);
  String humStr = String(humidity);
  client.publish("home/sensor/temperature", tempStr.c_str());
  client.publish("home/sensor/humidity", humStr.c_str());

  // Implementa a lógica preditiva (Simples: se a temperatura exceder um valor crítico, aciona o relé)
  if (temperature > 27.0) {  // Temperatura crítica de 27°C
    controlRelay(false);  // Desliga o dispositivo (aciona o relé)
  } else {
    controlRelay(true);   // Mantém o dispositivo ligado
  }
}

void setup() {
  // Inicialização Serial
  Serial.begin(115200);
  
  // Inicializa o sensor DHT22
  dht.begin();
  
  // Configura o pino do relé como saída
  pinMode(RELAY_PIN, OUTPUT);
  
  // Conecta ao Wi-Fi
  setup_wifi();
  
  // Configura o cliente MQTT
  client.setServer(mqtt_server, 1883);
}

void loop() {
  // Reconecta ao broker MQTT, se necessário
  if (!client.connected()) {
    reconnect();
  }
  client.loop();  // Mantém a conexão MQTT viva

  // Coleta os dados do sensor e envia via MQTT
  readAndSendSensorData();

  // Aguarda 10 segundos antes de coletar novamente
  delay(10000);  
}
