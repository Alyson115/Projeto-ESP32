TDE 01
Integrantes: Alyson Ferreira, Matheus Pereira
1. Definição do Projeto
  Controle de presença via sensor e monitoramento web.
2.Apresentação da Proposta
  •	Objetivo do  projeto – Desenvolver um sistema ciberfísico que utiliza sensor de presença e permite o monitoramento e controle remoto por meio de uma interface web.

•	Justificativa – O projeto é relevante por explorar tecnologias como IoT e comunicação em rede, altamente demandadas no mercado atual.


•	Tecnologias utilizadas –

   Hardware: 
   o	ESP32 (microcontrolador com Wi-Fi integrado)
    o	Sensor de Movimento PIR
    o	Jumpers e protoboard
    o	Fonte de alimentação
    
   Software/Bibliotecas:
    o	Arduino IDE (para desenvolvimento e upload de código)
    o	Biblioteca WiFi (para conexão do ESP32 à rede)
    o	Biblioteca para comunicação com o serviço de notificação
    Protocolos de Comunicação:
    o	Wi-Fi (IEEE 802.11)
    o	HTTP ou HTTPS (para envio de notificações via APIs)



•	Arquitetura geral do sistema – 
![image](https://github.com/user-attachments/assets/6c20faab-e77b-4d4a-8cab-aa8699769db1)


•	Cronograma de execução –

 
  Semana	Tarefa
  1	Levantamento de requisitos e definição dos componentes
  2	Montagem e testes com sensor
  3	Desenvolvimento da interface web
  4	Integração entre sensor e o ESP32, controle e interface web
  5	Testes de funcionamento e ajustes
  6	Documentação e preparação da apresentação final


Durante a simulação, utilizamos o sensor PIR para simular a detecção de movimento. Ao pressioná-lo, os dados foram exibidos em tempo real no Monitor Serial, mostrando valores de leitura da LDR e mensagens indicando a detecção ou não de movimento.


TDE 02
Integrantes: Alyson Ferreira, Matheus Pereira

Especificação do Sistema

Descrição Geral:

O projeto consiste em um sistema ciberfísico que utiliza um ESP32 e um sensor PIR para detectar movimento. O ESP32 se conecta a uma rede Wi-Fi e fornece uma interface web que permite monitorar em tempo real se há ou não presença detectada no ambiente. Para garantir eficiência, o sistema foi programado com millis() em vez de delay(), e entra em modo deep sleep após 30 segundos sem detecção de movimento.

Principais Aplicações
  •	Sistemas de segurança residencial
  •	Monitoramento de Ambientes Comerciais
  •	Aplicações IoT de Baixo Consumo
  •	Automação de Energia


Implementação
  Técnicas Aplicadas:
  1.	Uso de millis() para evitar
  travamentos e permitir multitarefa;
  2.	Criação de um servidor web
  embarcado no ESP32 com HTML
  dinâmico;
  3.	Implementação de deep sleep para
  reduzir o consumo quando o
  ambiente estiver inativo.


Funcionamento:
1.	O sensor PIR detecta movimento;
2.	O ESP32 atualiza a interface web
com o estado atual;
3.	Após 30 segundos sem movimento,
o ESP32 entra em deep sleep;
4.	Ao detectar novo movimento, o PIR
acorda o ESP32 automaticamente.

Código fonte:
#include <WiFi.h>               // Biblioteca para conexão Wi-Fi
#include <WebServer.h>          // Biblioteca para criar um servidor web local
#include "esp_sleep.h"          // Biblioteca para funções de economia de energia (sleep)

// Configurações de rede Wi-Fi
const char* ssid = "esp32";         // Nome da rede Wi-Fi (SSID)
const char* password = "12345678";  // Senha da rede Wi-Fi

WebServer server(80); // Cria um servidor web na porta 80 (HTTP padrão)

// Pino do sensor PIR (detecção de movimento)
const int pirPin = 12;

// Variável que indica se houve movimento
bool movimento = false;

// Controle de tempo para leituras do PIR
unsigned long ultimoCheck = 0;
const unsigned long intervalo = 200;  // Intervalo entre leituras do PIR em ms

// Controle de tempo para entrar em sleep
unsigned long tempoUltimoMovimento = 0;
const unsigned long tempoSemMovimento = 30000; // 30 segundos sem movimento → deep sleep

// Função que gera a página HTML mostrada ao acessar o ESP32 via navegador
void handleRoot() {
  String html = "<!DOCTYPE html><html><head><meta charset='UTF-8'>";
  html += "<meta http-equiv='refresh' content='2'>";  // Atualiza automaticamente a cada 2 segundos
  html += "<style>body{font-family:Arial;text-align:center;}</style>";
  html += "</head><body>";
  html += "<font size = '10'>";
  html += "<h2>Monitoramento PIR via ESP32</h2>";

  // Exibe "Movimento Detectado" em vermelho ou "Sem Movimento" em verde
  html += movimento ? "<h3 style='color:red;'>Movimento Detectado!</h3>" : "<h3 style='color:green;'>Sem Movimento</h3>";
  
  html += "<p>Atualize manualmente ou aguarde 2 segundos.</p>";
  html += "</font>";
  html += "</body></html>";

  server.send(200, "text/html", html); // Envia a resposta ao navegador
}

// Função que entra em modo de deep sleep após tempo sem movimento
void entrarEmSleep() {
  Serial.println("Sem movimento por 30 segundos. Entrando em Deep Sleep...");
  delay(1000); // Tempo para visualizar a mensagem no monitor serial

  // Configura o ESP32 para acordar com sinal HIGH no pino do PIR
  esp_sleep_enable_ext0_wakeup((gpio_num_t)pirPin, 1);

  // Inicia o deep sleep
  esp_deep_sleep_start();
}

void setup() {
  Serial.begin(115200); // Inicializa a comunicação serial para debug
  delay(1000); // Garante que a serial esteja pronta

  // Verifica se o ESP acordou por causa do sensor PIR
  if (esp_sleep_get_wakeup_cause() == ESP_SLEEP_WAKEUP_EXT0) {
    Serial.println("Acordou com movimento do sensor PIR!");
  }

  pinMode(pirPin, INPUT); // Configura o pino do PIR como entrada

  // Conecta ao Wi-Fi
  WiFi.begin(ssid, password);
  Serial.print("Conectando ao Wi-Fi");
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);  // Espera 500ms e tenta novamente
    Serial.print(".");
  }

  // Exibe o IP local após conexão bem-sucedida
  Serial.println("\nConectado! IP:");
  Serial.println(WiFi.localIP());

  // Define rota raiz para o servidor web
  server.on("/", handleRoot);
  server.begin();
  Serial.println("Servidor web iniciado.");

  tempoUltimoMovimento = millis(); // Inicia o contador de tempo desde o último movimento
}

void loop() {
  unsigned long agora = millis(); // Captura o tempo atual

  // Faz leituras do PIR em intervalos de tempo definidos
  if (agora - ultimoCheck >= intervalo) {
    ultimoCheck = agora;

    bool leituraPIR = digitalRead(pirPin); // Lê o estado do PIR

    if (leituraPIR) { // Se detectar movimento
      if (!movimento) {
        Serial.println("Movimento detectado.");
      }
      movimento = true;
      tempoUltimoMovimento = agora; // Atualiza o tempo do último movimento
    } else { // Se não houver movimento
      if (movimento && (agora - tempoUltimoMovimento > 2000)) { // Espera 2s antes de declarar "sem movimento"
        Serial.println("Sem movimento.");
        movimento = false;
      }
    }
  }

  // Se não houver movimento por 30 segundos, entra em deep sleep
  if (!movimento && (millis() - tempoUltimoMovimento >= tempoSemMovimento)) {
    entrarEmSleep();
  }

  server.handleClient(); // Mantém o servidor respondendo às requisições
}
