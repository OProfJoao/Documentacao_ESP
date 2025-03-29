# Documentação Básica - ESP32 para Iniciantes

Esta documentação tem como objetivo apresentar os passos iniciais para quem deseja começar a programar com a placa ESP32. Serão abordados desde a instalação do ambiente de desenvolvimento até a criação de um projeto simples, além de dicas para solucionar problemas comuns na configuração da placa.

---

## 🚀 Introdução

O ESP32 é um microcontrolador de baixo custo e alta performance, amplamente utilizado em projetos de IoT (Internet das Coisas) e automação. Ele possui Wi-Fi e Bluetooth integrados, o que o torna ideal para aplicações conectadas.

---

## 📋 Pré-requisitos

Antes de iniciar, certifique-se de ter os seguintes itens:

- Placa ESP32 (diversos modelos estão disponíveis, como ESP32-DevKitC ou NodeMCU-32S)
- Cabo USB para conectar a placa ao computador
- Computador com sistema operacional Windows, macOS ou Linux
- Acesso à internet para baixar softwares e bibliotecas

---

## 🛠 Instalação do Ambiente de Desenvolvimento

### 1. Instalando o VS Code e o PlatformIO

1. Baixe e instale o [Visual Studio Code](https://code.visualstudio.com/download).
2. Abra o VS Code e acesse a aba de Extensões (Ctrl + Shift + X).
3. Pesquise por PlatformIO IDE e clique em Instalar.
4. Após a instalação, reinicie o VS Code.

### 2. Criando um Novo Projeto no PlatformIO

1. No VS Code, clique no ícone do PlatformIO (Alienígena 👽) na barra lateral.
2. Selecione _New Project_.
3. Defina um nome para o projeto.
4. Escolha a placa correspondente ao seu ESP32 (ex.: Denky32 (WROOM32)).
5. Em Framework, selecione Arduino.
6. Escolha a pasta onde deseja salvar o projeto e clique em _Finish_.
7. Aguarde a instalação das dependências.

---

## 💡 Primeiro Projeto: Blink

Vamos criar um projeto simples que faz um LED piscar, conhecido como “Blink”.

### 1. Código Exemplo

Abra o arquivo `src/main.cpp` e copie o código abaixo:

```cpp
#include <Arduino.h>

const byte pinoLED = 2; //Pino do LED pode mudar para cada modelo de placa

void setup() {
  Serial.begin(115200);
  pinMode(pinoLED,OUTPUT);  //Define o pino como saída
}

void loop() {
  digitalWrite(pinoLED,HIGH);
  delay(1000);
  digitalWrite(pinoLED,LOW);
  delay(1000);
}
```
### 2. Carregando o Código

1. Conecte o ESP32 ao computador via cabo USB.
2. Clique em _Upload_ (seta para direita ➡️) na barra de ferramentas inferior.
3. Aguarde a compilação e envio do código para a placa.
4. O LED deve começar a piscar, indicando que o programa foi executado corretamente.

---

### 🖥️ Utilizando o Monitor Serial

O Monitor Serial é uma ferramenta útil para depuração de código e comunicação com o ESP32.

- Como acessar o Monitor Serial:
  
  No VSCode, utilizando o framework PlatformIO, o Monitor serial fica na barra de ferramentas inferior, com o símbolo de um plug de tomada🔌 (PlatformIO: Serial Monitor)


  Se o ESP32 estiver enviando dados, eles aparecerão no Terminal.

  Exemplo de Código para Teste

  Adicione o seguinte código ao seu programa para testar a comunicação serial:
``` cpp
void setup() {
  Serial.begin(115200);
  Serial.println("ESP32 pronto!");
}

void loop() {
  Serial.println("Rodando...");
  delay(2000);
}
```
  Se tudo estiver correto, a mensagem "Rodando..." aparecerá no Monitor Serial a cada 2 segundos.

---

## 🛑 Problemas Comuns e Soluções

Durante a configuração do ESP32, pode ser necessário resolver alguns problemas relacionados ao driver do chip conversor serial presente na placa. Muitas vezes, o problema está relacionado ao chip utilizado, que pode ser CH341 ou CP210x.

### 1. Identificando o Chip Conversor Serial

- Inspeção Visual: Verifique na placa de desenvolvimento se há alguma indicação ou marcação que identifique o chip. Geralmente, as placas mais comuns utilizam:

  - CH341: Normalmente apresenta a inscrição "CH341".

  - CP210x: Fabricado pela Silicon Labs, pode vir identificado como "CP2102", "CP2104", etc.

- Especificações da Placa: Consulte a documentação ou o site do fabricante da sua placa para confirmar qual chip está sendo utilizado.

### 2. Instalando os Drivers Recomendados

- **Para CH341:**
  - Acesse o [site oficial da WCH](http://www.wch.cn/download/CH341SER_EXE.html) para baixar o driver.
  - Siga as instruções de instalação fornecidas pelo site.

- **Para CP210x:**
  - Visite o [site da Silicon Labs](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers) para baixar o driver CP210x.
  - Instale o driver conforme as orientações do site.

### 3. Dicas Adicionais

- Após a instalação do driver, reinicie o computador, se necessário.
- Verifique se a porta COM (no Windows) ou o dispositivo serial (no macOS/Linux) está sendo reconhecido corretamente.
- Caso o problema persista, consulte fóruns e comunidades online para encontrar soluções específicas para o seu modelo de placa.

---

## 🌐 Uso para IoT

O ESP32 é amplamente utilizado em aplicações de Internet das Coisas (IoT), permitindo a conexão com serviços de nuvem e brokers MQTT como HiveMQ e Mosquitto. A seguir, um exemplo básico de como utilizar o ESP32 para se conectar a um broker MQTT usando a biblioteca **PubSubClient**.

Este [Repositório](https://github.com/OProfJoao/ESP_Bare_Minimum) contém um passo a passo, de como implementar um cliente MQTT básico utilizando o ESP32. 

---

## 🛠 Dicas Adicionais para Desenvolvimento

- **Bibliotecas:** Explore bibliotecas adicionais que podem facilitar o desenvolvimento, como as para conexão Wi-Fi e MQTT, entre outras.
- **Documentação:** Consulte a [documentação oficial do ESP32](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/) para informações mais detalhadas.
- **Comunidade:** Participe de fóruns e comunidades online para trocar experiências e tirar dúvidas.

---

## 📚 Recursos e Referências

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
