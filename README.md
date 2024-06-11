# Sprint Edge Computing 01

# Controle de Motor com Arduino e Potenciômetro

## Link para o simulador:
https://www.tinkercad.com/things/lCBqMpOEObr-sprint-edge-01?sharecode=5zCs_j3GMQ3i6v7E0-F2AfI-rP3lcan5eW0UBrxjgww

## Link para o vídeo:


## Descrição do Projeto

Este projeto utiliza um Arduino, um driver de motor L293D e um potenciômetro para controlar a direção e a velocidade de um motor DC. O usuário pode controlar o motor através da porta serial, definindo a direção de rotação e ligando/desligando o motor. A velocidade do motor é ajustada girando o potenciômetro.

## Como Funciona

O motor é controlado pelo Arduino através do driver de motor L293D, que permite controlar a direção e a velocidade do motor utilizando sinais PWM. O potenciômetro é utilizado para ajustar a velocidade do motor, convertendo a leitura analógica para um valor de PWM. O usuário pode enviar comandos via porta serial para definir a direção de rotação ou desligar o motor.

## Componentes Necessários

- 1 x Arduino Uno
- 1 x Driver de motor L293D
- 1 x Motor DC
- 1 x Potenciômetro 
- 1 x Bateria 9V
- 1 x Protoboard
- Jumpers

## Montagem do Circuito

1. **Conectar o Potenciômetro:**
   - Pino 1 (GND) do potenciômetro ao barramento de GND na protoboard.
   - Pino 2 (Wiper) do potenciômetro ao pino A1 do Arduino.
   - Pino 3 (VCC) do potenciômetro ao barramento de 5V na protoboard.

2. **Conectar o Motor e o L293D:**
   - O pino 3 do L293D ao pino 3 do Arduino (PWM).
   - O pino 2 do L293D ao pino 6 do Arduino.
   - O pino 7 do L293D ao pino 5 do Arduino.
   - Pinos 4, 5, 12 e 13 do L293D ao GND da protoboard.
   - Pinos 1, 9 e 16 do L293D ao 5V da protoboard.
   - Conectar os terminais do motor aos pinos 3 e 6 do L293D.
     
1. **Conectar a Bateria de 9V:**
   - Conectar o positivo da bateria ao pino 8 do L293D.
   - Conectar o negativo da bateria ao barramento de GND na protoboard.

## Código

Aqui está o código completo utilizado no projeto:

```cpp
// Configurações do motor e pinos
int PinoVelocidade = 3; // Pino PWM para controlar a velocidade do motor
int input1 = 6; // Pino de controle 1 do L293D
int input2 = 5; // Pino de controle 2 do L293D

// Pino do potenciômetro
int pinoPotenciometro = A1; // Pino analógico para ler o valor do potenciômetro

void setup() {
    // Iniciando a comunicação Serial para interagir com o usuário
    Serial.begin(9600);
    Serial.println("Controle do motor:");
    Serial.println("Digite 1 Esquerda"); // Instruções para o usuário
    Serial.println("Digite 2 Direita");  // Instruções para o usuário
    Serial.println("Digite 0 Desligar"); // Instruções para o usuário

    // Configurando os pinos como saída
    pinMode(PinoVelocidade, OUTPUT);
    pinMode(input1, OUTPUT);
    pinMode(input2, OUTPUT);
}

void loop() {
    int operacao; // Variável para armazenar a operação desejada pelo usuário
    
    // Leitura do valor do potenciômetro
    int valorPot = analogRead(pinoPotenciometro); // Lê o valor analógico do potenciômetro (0-1023)
    int pwm = map(valorPot, 0, 1023, 0, 255); // Mapeia o valor lido para a faixa de PWM (0-255)
    analogWrite(PinoVelocidade, pwm); // Define a velocidade do motor com base no valor do PWM

    // Verifica se há algum valor disponível na Serial para leitura
    if (Serial.available() > 0) {
        // Lê a operação inserida pelo usuário e converte para inteiro
        operacao = Serial.parseInt();
        
        // Verifica qual operação foi inserida e realiza a ação correspondente
        if (operacao == 1) {
            // Gira o motor para a esquerda
            digitalWrite(input1, HIGH);
            digitalWrite(input2, LOW);
            Serial.println("Motor girando para esquerda <<<");
        } else if (operacao == 2) {
            // Gira o motor para a direita
            digitalWrite(input1, LOW);
            digitalWrite(input2, HIGH);
            Serial.println("Motor girando para direita >>>");
        } else if (operacao == 0) {
            // Desliga o motor
            digitalWrite(input1, LOW);
            digitalWrite(input2, LOW);
            Serial.println("Motor parado [ ]");
        } else {
            // Se a operação inserida for inválida, exibe uma mensagem de erro
            Serial.println("Opcao Invalida!");
            Serial.println("Digite 0, 1 ou 2");
        }
        delay(50); // Pequeno atraso para estabilidade
    }
}
```
## Imagem do projeto
![Sprint-Edge-01](https://github.com/aarthurbf/sprint01edgecomputing/assets/161460625/e50bd7c7-a3b7-4d80-8a7e-8e6d8b794aa4)

## Conclusão
Este projeto demonstra como controlar um motor DC usando Arduino e um potenciômetro. Ele fornece uma base sólida para projetos mais complexos que envolvem controle de motores. Se enfrentar problemas, verifique todas as conexões e certifique-se de que todos os componentes estão funcionando corretamente.
