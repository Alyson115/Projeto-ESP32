TDE 01
1. Definição do Projeto
Controle de iluminação via sensores e monitoramento web.
2.Apresentação da Proposta
•	Objetivo do  projeto – Desenvolver um sistema ciberfísico para automação de iluminação que utiliza sensores de presença e luminosidade e permitir o monitoramento e controle remoto por meio de uma interface web.

•	Justificativa – O projeto é relevante por promover eficiência energética, reduzir custos com eletricidade. Além disso, permite explorar tecnologias como IoT e comunicação em rede, altamente demandadas no mercado atual.


•	Tecnologias utilizadas –

 Hardware:

o	ESP32 (microcontrolador com Wi-Fi integrado)
o	Sensor de Movimento PIR
o	Jumpers e protoboard
o	Fonte de alimentação ou bateria (caso necessário)
Software/Bibliotecas:
o	Arduino IDE (para desenvolvimento e upload de código)
o	Biblioteca WiFi.h (para conexão do ESP32 à rede)
o	Biblioteca para comunicação com o serviço de notificação (ex: UniversalTelegramBot.h para Telegram ou HTTPClient.h para APIs)
Protocolos de Comunicação:
o	Wi-Fi (IEEE 802.11)
o	HTTP ou HTTPS (para envio de notificações via APIs)



•	Arquitetura geral do sistema – 
![image](https://github.com/user-attachments/assets/ceb9da68-1757-45d8-bc9c-80b6fb69c1ec)

•	Cronograma de execução –
Semana	Tarefa
1	Levantamento de requisitos e definição dos componentes
2	Montagem e testes com sensores e controle de lâmpadas
3	Desenvolvimento da interface web
4	Integração entre sensores, controle e interface web
5	Testes de funcionamento e ajustes
6	Documentação e preparação da apresentação final



