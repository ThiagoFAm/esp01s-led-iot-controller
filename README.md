# 💡 ESP01S MQTT LED Controller

Sistema IoT para controle de LED utilizando **ESP01S (ESP8266)**,
**Node-RED** e **MQTT**, com suporte a modo manual e automático baseado
em horários.

------------------------------------------------------------------------

## 🧠 Visão Geral

Este projeto permite controlar um LED remotamente através de um
dashboard web, com dois modos de operação:

-   🔧 **Manual** → controle direto pelo usuário
-   🤖 **Automático** → acionamento baseado em horários definidos

A comunicação entre o servidor e o dispositivo é feita via protocolo
MQTT.

------------------------------------------------------------------------

## 🏗️ Arquitetura

    [Dashboard Node-RED]
            ↓
    [Node-RED (lógica)]
            ↓ MQTT
    [Broker MQTT]
            ↓
    [ESP01S]
            ↓
    [LED]

------------------------------------------------------------------------

## 📡 Comunicação MQTT

### 📤 Tópico

    esp32/led

### 📥 Payload

  Valor   Ação
  ------- -------------
  true    Liga LED
  false   Desliga LED

⚠️ **Observação:**\
O ESP01S utiliza lógica invertida: - `led.off()` → Liga - `led.on()` →
Desliga

------------------------------------------------------------------------

## ⚙️ Funcionalidades

### 🔘 Modo Automático

-   Ativado via dashboard
-   Controla o LED com base no horário

#### Períodos configurados:

-   🌅 **Manhã:** 05:00 → 07:30
-   🌙 **Noite:** 18:00 → 23:59

------------------------------------------------------------------------

### 🎛️ Modo Manual

-   Controle direto do LED
-   Desativado automaticamente quando o modo automático está ativo

------------------------------------------------------------------------

### 🔄 Sincronização de Estado

-   O estado do LED é sincronizado com o dashboard
-   Evita inconsistência entre interface e dispositivo

------------------------------------------------------------------------

### 🚫 Prevenção de Flood MQTT

-   O sistema evita envio repetido de comandos iguais
-   Reduz tráfego e melhora estabilidade

------------------------------------------------------------------------

## 📟 Código do ESP01S

### 📶 Conexão Wi-Fi

``` python
wlan.connect(SSID, PASSWORD)
```

------------------------------------------------------------------------

### 📡 Conexão MQTT

``` python
client = MQTTClient("esp01s_led", MQTT_BROKER)
client.set_callback(switch_led)
client.connect()
client.subscribe(MQTT_TOPIC)
```

------------------------------------------------------------------------

### 🔁 Loop principal

``` python
while True:
    client.wait_msg()
```

------------------------------------------------------------------------

### 💡 Controle do LED

``` python
if msg == b"true":
    led.off()
elif msg == b"false":
    led.on()
```

------------------------------------------------------------------------

## ⚠️ Pontos Importantes

### 🔴 Lógica invertida do LED

O comportamento do LED é invertido por característica do ESP01S.

------------------------------------------------------------------------

### 🔴 Endereço do Broker MQTT

-   Deve ser o IP do servidor MQTT
-   Não utilizar `localhost` no ESP

------------------------------------------------------------------------

### 🔴 Persistência de estado

-   Variáveis são armazenadas no `flow context` do Node-RED
-   Podem ser perdidas em reinicializações (sem persistência
    configurada)

------------------------------------------------------------------------

### 🔴 Reconexão (Próxima feature)

-   O ESP não possui reconexão automática implementada
-   Pode travar caso perca conexão Wi-Fi ou MQTT

------------------------------------------------------------------------

### 🔴 Segurança

-   Credenciais não devem ser expostas no código público
-   Recomenda-se:
    -   autenticação no broker MQTT
    -   uso de HTTPS/VPN para acesso remoto

------------------------------------------------------------------------

## 🚀 Possíveis Features

-   🔐 Autenticação MQTT
-   🔄 Reconexão automática no ESP
-   📦 Suporte a múltiplos dispositivos
-   ☁️ Integração com serviços em nuvem
-   📱 Integração com a siri

------------------------------------------------------------------------

## 🧩 Tecnologias Utilizadas

-   MicroPython (ESP8266)
-   MQTT (Mosquitto)
-   Node-RED
-   Raspberry Pi (servidor)

------------------------------------------------------------------------

## 📌 Objetivo

Este projeto serve como base para sistemas IoT escaláveis, podendo
evoluir para:

-   automação residencial completa
-   integração com assistentes virtuais
-   aplicações comerciais

------------------------------------------------------------------------

