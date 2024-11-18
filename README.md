# objetos_inteligentes

# Projeto de Automação IoT com ESP32 e Relé de Estado Sólido

Este projeto utiliza um **ESP32**, um **sensor DHT22** (temperatura e umidade) e um **relé de estado sólido** para controlar dispositivos com base nas condições ambientais. Ele envia os dados coletados para a nuvem usando o protocolo **MQTT** e realiza ações automatizadas com base em análises preditivas simples (baseadas na temperatura). 

A aplicação foi projetada para ilustrar como sistemas de automação baseados em IoT podem ser usados para otimizar processos e responder a condições críticas de forma eficiente.

## Recursos do Projeto

- **Microcontrolador**: ESP32
- **Sensor de Temperatura e Umidade**: DHT22
- **Relé de Estado Sólido**: Para controle de dispositivos elétricos
- **Protocolo de Comunicação**: MQTT
- **Plataforma de Nuvem**: Integração com plataformas de IoT para visualização e análise de dados
- **Lógica de Controle**: Ação automatizada com base na temperatura
- **Bibliotecas Utilizadas**:
  - `DHT.h` para o sensor de temperatura e umidade DHT22
  - `PubSubClient.h` para comunicação MQTT
  - `WiFi.h` para conexão Wi-Fi

## Objetivos do Projeto

- **Monitoramento de Temperatura e Umidade**: Coletar dados ambientais usando o sensor DHT22.
- **Controle Automático de Dispositivos**: Usar um relé de estado sólido para controlar dispositivos, desativando-os automaticamente em situações de risco (temperatura crítica).
- **Envio de Dados para a Nuvem**: Enviar dados em tempo real para uma plataforma IoT usando o protocolo MQTT.
- **Análise Preditiva Simples**: Monitorar condições críticas e realizar ações preventivas com base em padrões de temperatura.


## Pré-requisitos

Para compilar e executar o código, você precisará dos seguintes itens:

- **Hardware**:
  - ESP32 (qualquer modelo com Wi-Fi)
  - Sensor DHT22
  - Relé de Estado Sólido (SSR)
  - Cabos jumper para conexões
  - Fonte de alimentação para o ESP32

- **Software**:
  - Arduino IDE (ou PlatformIO)
  - Bibliotecas: `DHT`, `PubSubClient` e `WiFi`

## Instalação

### 1. Instalar o Arduino IDE

Se você ainda não possui o Arduino IDE, baixe e instale a versão mais recente:
- [Baixar o Arduino IDE](https://www.arduino.cc/en/software)

### 2. Instalar o ESP32 na IDE Arduino

- Vá para **Arquivo > Preferências**.
- Adicione o link do gerenciador de placas no campo "URLs adicionais para gerenciadores de placas" com o seguinte link:

  https://dl.espressif.com/dl/package_esp32_index.json

- Em seguida, vá para **Ferramentas > Placa > Gerenciador de Placas** e busque por **ESP32**.
- Instale a versão mais recente da placa ESP32.

### 3. Instalar Bibliotecas Necessárias

Na Arduino IDE, vá para **Programa > Incluir Biblioteca > Gerenciar Bibliotecas**. Procure e instale as bibliotecas necessárias:

- `DHT sensor library`
- `PubSubClient`
- `WiFi`

### 4. Conectar o Hardware

- **Sensor DHT22**: Conecte o pino de dados (do DHT22) ao pino **4** do ESP32.
- **Relé de Estado Sólido (SSR)**: Conecte o pino de controle do relé ao pino **15** do ESP32.
- Conecte o **VCC** do DHT22 ao **3V3** do ESP32 e o **GND** ao **GND**.

### 5. Configurar as Credenciais do Wi-Fi

No código fonte (`main.cpp`), substitua as variáveis `ssid` e `password` com suas credenciais de rede Wi-Fi:

```cpp
const char* ssid = "SEU_SSID";
const char* password = "SUA_SENHA";

6. Configurar o Broker MQTT
Substitua o mqtt_server com o endereço do broker MQTT que você deseja usar. Você pode utilizar um broker público, como o broker.mqtt-dashboard.com, ou configurar o seu próprio broker.

cpp
Copiar código

  const char* mqtt_server = "broker.mqtt-dashboard.com";  // Substitua pelo seu broker
7. Carregar o Código para o ESP32
Conecte o ESP32 ao seu computador e selecione a porta correta na IDE. Clique em Carregar para transferir o código para o dispositivo.

Como Usar
Conectar ao Wi-Fi: Após carregar o código, o ESP32 tentará se conectar à sua rede Wi-Fi.
Leitura de Dados: O sensor DHT22 coletará dados de temperatura e umidade a cada 10 segundos.
Envio de Dados: Os dados serão enviados para a nuvem via MQTT nos tópicos:
home/sensor/temperature
home/sensor/humidity
Controle do Relé: O sistema monitora a temperatura, e se ela ultrapassar 27°C, o relé de estado sólido será desligado automaticamente, cortando a energia para proteger o dispositivo controlado.
Monitoramento em Tempo Real: Você pode visualizar os dados e o estado do relé na plataforma de nuvem configurada para receber as mensagens MQTT.
Testes
Teste de Leitura do Sensor DHT22
Objetivo: Verificar se o ESP32 coleta corretamente os dados de temperatura e umidade.
Procedimento: O sensor foi colocado em diferentes ambientes com variações de temperatura e umidade, e os dados foram lidos e exibidos no monitor serial.
Teste de Envio de Dados via MQTT
Objetivo: Confirmar que os dados coletados são enviados corretamente para a nuvem via MQTT.
Procedimento: O ESP32 foi configurado para enviar dados para o broker MQTT, e a plataforma IoT foi utilizada para verificar se os dados foram recebidos.
Teste de Resposta Automática
Objetivo: Testar a resposta automática do sistema com base na previsão de temperatura.
Procedimento: A temperatura foi aumentada gradualmente e o sistema acionou o relé para desativar o dispositivo quando a temperatura ultrapassou 27°C.
Contribuições
Sinta-se à vontade para melhorar ou expandir este projeto! Algumas possíveis melhorias incluem:

Melhorias na IA Preditiva: Integração de modelos mais sofisticados para previsão de condições críticas com aprendizado de máquina.
Interface Web: Criação de uma interface web para monitoramento e controle do sistema.
Alertas e Notificações: Adicionar notificações automáticas (por exemplo, via e-mail ou SMS) quando o sistema detectar condições críticas.
Licença
Este projeto é de código aberto, licenciado sob a MIT License. Você pode modificar, distribuir e utilizar o código conforme sua necessidade.
