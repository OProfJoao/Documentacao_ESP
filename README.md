# Documentação Básica - ESP32 para Iniciantes

Esta documentação tem como objetivo apresentar os passos iniciais para quem deseja começar a programar com a placa ESP32. Serão abordados desde a instalação do ambiente de desenvolvimento até a criação de um projeto simples, além de dicas para solucionar problemas comuns na configuração da placa.

---

## 1. Introdução

O ESP32 é um microcontrolador de baixo custo e alta performance, amplamente utilizado em projetos de IoT (Internet das Coisas) e automação. Ele possui Wi-Fi e Bluetooth integrados, o que o torna ideal para aplicações conectadas.

---

## 2. Pré-requisitos

Antes de iniciar, certifique-se de ter os seguintes itens:

- Placa ESP32 (diversos modelos estão disponíveis, como ESP32-DevKitC ou NodeMCU-32S)
- Cabo USB para conectar a placa ao computador
- Computador com sistema operacional Windows, macOS ou Linux
- Acesso à internet para baixar softwares e bibliotecas

---

## 3. Instalação do Ambiente de Desenvolvimento

### 3.1. Instalando a IDE Arduino

1. Acesse o [site oficial do Arduino](https://www.arduino.cc/en/software) e baixe a IDE correspondente ao seu sistema operacional.
2. Siga as instruções de instalação para seu sistema.

### 3.2. Configurando a Placa ESP32 na IDE Arduino

1. Abra a IDE do Arduino.
2. Vá até **Arquivo > Preferências**.
3. No campo **URLs adicionais para Gerenciadores de Placas**, adicione a seguinte URL: https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
4. Clique em **OK** para salvar as preferências.
5. Vá em **Ferramentas > Placa > Gerenciador de Placas...** e pesquise por "esp32".
6. Selecione o pacote **esp32 by Espressif Systems** e clique em **Instalar**.
7. Após a instalação, selecione a placa adequada em **Ferramentas > Placa** (ex.: ESP32 Dev Module).

---

## 4. Primeiro Projeto: Blink

Vamos criar um projeto simples que faz um LED piscar, conhecido como “Blink”.

### 4.1. Código Exemplo

Crie um novo sketch na IDE Arduino e copie o código abaixo:

```cpp
// Define o pino do LED (geralmente o LED embutido está no pino 2)
const int ledPin = 2;

void setup() {
// Configura o pino do LED como saída
pinMode(ledPin, OUTPUT);
}

void loop() {
// Liga o LED
digitalWrite(ledPin, HIGH);
delay(1000); // Espera 1 segundo
// Desliga o LED
digitalWrite(ledPin, LOW);
delay(1000); // Espera 1 segundo
}
```
### 4.2. Carregando o Código

1. Conecte o ESP32 ao computador via cabo USB.
2. Selecione a porta correta em Ferramentas > Porta.
3. Clique no botão Upload na IDE Arduino para compilar e enviar o código para a placa.
4. O LED deve começar a piscar, indicando que o programa foi executado corretamente.

---

## 5. Problemas Comuns e Soluções

Durante a configuração do ESP32, pode ser necessário resolver alguns problemas relacionados ao driver do chip conversor serial presente na placa. Muitas vezes, o problema está relacionado ao chip utilizado, que pode ser CH341 ou CP210x.

### 5.1. Identificando o Chip Conversor Serial

- Inspeção Visual: Verifique na placa de desenvolvimento se há alguma indicação ou marcação que identifique o chip. Geralmente, as placas mais comuns utilizam:

  - CH341: Normalmente apresenta a inscrição "CH341".

  - CP210x: Fabricado pela Silicon Labs, pode vir identificado como "CP2102", "CP2104", etc.

- Especificações da Placa: Consulte a documentação ou o site do fabricante da sua placa para confirmar qual chip está sendo utilizado.

### 5.2. Instalando os Drivers Recomendados

- **Para CH341:**
  - Acesse o [site oficial da WCH](http://www.wch.cn/download/CH341SER_EXE.html) para baixar o driver.
  - Siga as instruções de instalação fornecidas pelo site.

- **Para CP210x:**
  - Visite o [site da Silicon Labs](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers) para baixar o driver CP210x.
  - Instale o driver conforme as orientações do site.

### 5.3. Dicas Adicionais

- Após a instalação do driver, reinicie o computador, se necessário.
- Verifique se a porta COM (no Windows) ou o dispositivo serial (no macOS/Linux) está sendo reconhecido corretamente.
- Caso o problema persista, consulte fóruns e comunidades online para encontrar soluções específicas para o seu modelo de placa.

---

## 6. Uso para IoT

O ESP32 é amplamente utilizado em aplicações de Internet das Coisas (IoT), permitindo a conexão com serviços de nuvem e brokers MQTT como HiveMQ e Mosquitto. A seguir, um exemplo básico de como utilizar o ESP32 para se conectar a um broker MQTT usando a biblioteca **PubSubClient**.

### 6.1. Pré-requisitos para IoT

- **Wi-Fi:** Configure sua rede Wi-Fi.
- **Broker MQTT:** Utilize um broker público como HiveMQ ou instale o [Mosquitto](https://mosquitto.org/) localmente.
- **Biblioteca MQTT:** Instale a biblioteca **PubSubClient** na IDE Arduino (Menu: **Sketch > Incluir Biblioteca > Gerenciar Bibliotecas…** e procure por "PubSubClient").

### 6.2. Código Exemplo para Conexão MQTT

```cpp
#include <WiFi.h>
#include <PubSubClient.h>

// Configurações da rede Wi-Fi
const char* ssid = "SUA_SSID";
const char* password = "SUA_SENHA";

// Configurações do broker MQTT (exemplo com HiveMQ)
const char* mqtt_server = "broker.hivemq.com";

WiFiClient espClient;
PubSubClient client(espClient);

void setup() {
  Serial.begin(115200);
  setup_wifi();
  client.setServer(mqtt_server, 1883);
}

void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Conectando-se a ");
  Serial.println(ssid);

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("WiFi conectado");
  Serial.print("Endereço de IP: ");
  Serial.println(WiFi.localIP());
}

void reconnect() {
  // Loop até reconectar
  while (!client.connected()) {
    Serial.print("Tentando conexão MQTT...");
    if (client.connect("ESP32Client")) {
      Serial.println("conectado");
      // Inscreva-se em um tópico, se necessário
      client.subscribe("seu/topico");
    } else {
      Serial.print("falha, rc=");
      Serial.print(client.state());
      Serial.println(" tentando novamente em 5 segundos");
      delay(5000);
    }
  }
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();
}
```
### 6.3 Considerações

- **Segurança:** Para projetos em progução, utilize conexões seguras (TLS/SSL) e autenticação no broker MQTT.
- **Testes:** Teste sua conexão e ajuste os parâmetros conforme necessário. Utilize ferramentas como MQTT.fx ou MQTT Explorer para monitorar os tópicos.

---

## 7. Dicas Acidionais para Desenvolvimento

- **Bibliotecas:** Explore bibliotecas adicionais que podem facilitar o desenvolvimento, como as para conexão Wi-Fi e MQTT, entre outras.
- **Documentação:** Consulte a [documentação oficial do ESP32](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/) para informações mais detalhadas.
- **Comunidade:** Participe de fóruns e comunidades online para trocar experiências e tirar dúvidas.

---

## 8. Recursos e Referências

- [Site Oficial do ESP32](https://www.espressif.com/en/products/socs/esp32)
- [Documentação do ESP-IDF (SDK oficial)](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/)
- [Tutorial Arduino para ESP32](https://randomnerdtutorials.com/getting-started-with-esp32/)
- [Driver CH341 (WCH)](http://www.wch.cn/download/CH341SER_EXE.html)
- [Driver CP210x (Silicon Labs)](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers)
- [Broker MQTT HiveMQ](https://console.hivemq.cloud/)
- [Broker MQTT Mosquitto](https://test.mosquitto.org/)

---

Esta documentação serve como ponto de partida. Conforme você for avançando, explore mais exemplos, bibliotecas e recursos que podem ajudar a desenvolver projetos cada vez mais complexos com o ESP32. Caso enfrente outros problemas, a comunidade e os recursos oficiais serão aliados importantes para solucionar dúvidas e desafios.

**Boa sorte e bons projetos!**
