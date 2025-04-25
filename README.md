Sistema de segurança IoT com sensores de movimento.
Objetivo do Projeto:
Este projeto tem como objetivo desenvolver um sistema de detecção de movimento utilizando um microcontrolador ESP32 e um sensor PIR (Passive Infrared Sensor). Sempre que um movimento for detectado, o sistema enviará uma notificação em tempo real por meio de um aplicativo ou serviço de mensagens, como Telegram ou uma notificação push via Wi-Fi. A aplicação pode ser usada em sistemas de segurança residenciais ou comerciais, monitoramento automatizado e automação residencial.
Justificativa:
Em um cenário cada vez mais conectado, os sistemas ciberfísicos desempenham um papel fundamental na integração do mundo físico com o digital. Este projeto demonstra a aplicação prática desse conceito ao monitorar o ambiente físico (movimento) e reagir digitalmente (enviando notificações em tempo real). Além disso, o uso de um microcontrolador ESP32 com conectividade Wi-Fi evidencia o potencial da IoT (Internet das Coisas) em soluções de segurança acessíveis, escaláveis e eficientes.
Tecnologias Utilizadas
Hardware:
ESP32 (microcontrolador com Wi-Fi integrado)
Sensor de Movimento PIR
Jumpers e protoboard
Software/Bibliotecas:
Arduino IDE (para desenvolvimento e upload de código)
Biblioteca WiFi.h (para conexão do ESP32 à rede)
Biblioteca para comunicação com o serviço de notificação
Protocolos de Comunicação:
Wi-Fi (IEEE 802.11)
HTTP ou HTTPS
Arquitetura Geral do Sistema:
 Sensor de                ESP32                       Serviço de Notificação
 Movimento PIR    ----->  (Processamento     --->     (ex: Telegram, Push   
 (Detecta presença)       e envio via Wi-Fi)          Notifications, etc.) 
Fluxo:
O sensor PIR detecta um movimento.
O ESP32 processa essa entrada e conecta-se à rede Wi-Fi.
O ESP32 envia uma notificação para o serviço configurado.
O usuário recebe a notificação em seu dispositivo.
Cronograma de Execução:
Semana	Atividade
1	Definição do escopo e aquisição dos componentes
2	Configuração inicial do ESP32 e testes com sensor PIR
3	Implementação da lógica de detecção e notificação
4	Integração com o serviço de notificação (Telegram/API)
5	Testes em campo e ajustes
6	Documentação final e apresentação do projeto
