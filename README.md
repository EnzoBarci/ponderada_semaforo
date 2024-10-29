# Documentação do Projeto: Semáforo com Arduino UNO

## Introdução

Este documento descreve o desenvolvimento de um projeto de semáforo utilizando Arduino UNO, LEDs e um buzzer. O objetivo foi criar uma simulação simples de um semáforo, alternando entre as luzes vermelha, amarela e verde com uma lógica de temporização específica. A estrutura física do semáforo foi montada em uma protoboard, e a programação do Arduino foi utilizada para controlar as transições das luzes e o alerta sonoro.

## Materiais Utilizados

- 1 Arduino UNO
- 1 Protoboard
- LEDs (vermelho, amarelo e verde)
- Resistores para cada LED
- Fios de conexão
- 1 Buzzer

## Estrutura do Projeto

### Parte 1: Montagem Física

Primeiro, montei a estrutura física do semáforo utilizando LEDs e resistores em uma protoboard. A montagem seguiu os seguintes passos:

1. **Conexão dos LEDs**: Coloquei os LEDs na protoboard, cada um representando uma cor do semáforo – vermelho, amarelo e verde.
2. **Resistores**: Conectei resistores em cada LED para garantir que eles não recebessem mais corrente do que o necessário.
3. **Fiação e Organização**: Organizei os fios de forma clara, para evitar confusões durante a programação e para facilitar a visualização do circuito. 
4. **Buzzer**: Decidi incluir um buzzer para emitir um som durante a fase de "verde adicional", indicando o tempo extra para pedestres terminarem a travessia.

### Parte 2: Programação e Lógica do Semáforo

Em seguida, programei o comportamento do semáforo para alternar entre as fases vermelho, amarelo e verde, de acordo com a lógica abaixo:

- **Vermelho**: Acende por 6 segundos.
- **Amarelo**: Acende por 2 segundos.
- **Verde**: Acende por 2 segundos.
- **Verde com Alerta Sonoro**: Mantém o LED verde aceso por mais 2 segundos enquanto o buzzer emite um som para indicar o tempo adicional para pedestres.
- **Amarelo**: Acende novamente por 2 segundos para completar o ciclo.

Esse ciclo é repetido continuamente, formando um loop que simula o funcionamento de um semáforo real.

#### Passos para a Programação

1. **Definição dos Pinos**: Inicialmente, defini no código os pinos correspondentes a cada LED e ao buzzer.
2. **Controle das Temporizações**: Utilizei a função `delay()` para definir o tempo de cada cor, seguindo a sequência descrita.
3. **Loop Infinito**: Implementei um loop `while(true)` para garantir que o semáforo continue funcionando continuamente, repetindo o ciclo de cores indefinidamente.
4. **Buzzer**: Adicionei um comando para ativar o buzzer durante os últimos 2 segundos do sinal verde, simulando um aviso sonoro de tempo extra para pedestres.

### Teste e Ajustes

Após carregar o código no Arduino, realizei testes para garantir que as transições entre as luzes ocorressem nos tempos corretos. Também testei o buzzer durante o tempo adicional do verde para confirmar que ele emitisse o alerta no momento adequado.

## Resultados

O semáforo funcionou conforme planejado, alternando entre as cores vermelho, amarelo e verde de acordo com a lógica programada. O buzzer também foi acionado no momento correto, indicando o tempo extra de forma audível.

## Conclusão

Esse projeto foi uma excelente oportunidade para aplicar conhecimentos sobre eletrônica básica e programação no Arduino. Além de entender melhor como funcionam os componentes eletrônicos, o projeto permitiu explorar conceitos de temporização e controle de alertas sonoros.

# Código Utilizado:

```
// Definindo os pinos dos LEDs e do buzzer
const int vermelho = 10;
const int amarelo = 9;
const int verde = 8;
const int buzzer = 7; // Pino do buzzer

// Variáveis para o controle do semáforo
unsigned long tempoAnterior = 0;
int etapa = 0;

void setup() {
  // Configurando os pinos como saída
  pinMode(vermelho, OUTPUT);
  pinMode(amarelo, OUTPUT);
  pinMode(verde, OUTPUT);
  pinMode(buzzer, OUTPUT); // Configura o pino do buzzer

  // Garantindo que todos os LEDs e o buzzer estejam apagados no início
  digitalWrite(vermelho, LOW);
  digitalWrite(amarelo, LOW);
  digitalWrite(verde, LOW);
  digitalWrite(buzzer, LOW);
}

void loop() {
  unsigned long tempoAtual = millis();

  switch (etapa) {
    case 0:
      // Liga vermelho
      digitalWrite(vermelho, HIGH);
      digitalWrite(amarelo, LOW);
      digitalWrite(verde, LOW);
      digitalWrite(buzzer, LOW); // Buzzer desligado
      tempoAnterior = tempoAtual;
      etapa = 1;
      break;
    case 1:
      if (tempoAtual - tempoAnterior >= 6000) {
        // Após 6 segundos, passa para amarelo
        digitalWrite(vermelho, LOW);
        digitalWrite(amarelo, HIGH);
        digitalWrite(buzzer, LOW); // Buzzer desligado
        tempoAnterior = tempoAtual;
        etapa = 2;
      }
      break;
    case 2:
      if (tempoAtual - tempoAnterior >= 2000) {
        // Após 2 segundos, passa para verde
        digitalWrite(amarelo, LOW);
        digitalWrite(verde, HIGH);
        digitalWrite(buzzer, HIGH,); // Buzzer ligado durante o verde
        tempoAnterior = tempoAtual;
        etapa = 3;
      }
      break;
    case 3:
      if (tempoAtual - tempoAnterior >= 4000) {
        // Após 4 segundos, volta para amarelo
        digitalWrite(verde, LOW);
        digitalWrite(amarelo, HIGH);
        digitalWrite(buzzer, LOW); // Buzzer desligado
        tempoAnterior = tempoAtual;
        etapa = 4;
      }
      break;
    case 4:
      if (tempoAtual - tempoAnterior >= 2000) {
        // Após 2 segundos, reinicia o ciclo
        digitalWrite(amarelo, LOW);
        etapa = 0;
      }
      break;
  }
}

```


# Template Avaliação Pares

### Avaliador: Gabriela Silva

| Critério                                                                                                 | Contempla (Pontos) | Contempla Parcialmente (Pontos) | Não Contempla (Pontos) | Observações do Avaliador |
|---------------------------------------------------------------------------------------------------------|--------------------|----------------------------------|--------------------------|---------------------------|
| Montagem física com cores corretas, boa disposição dos fios e uso adequado de resistores                | Até 3              | Até 1,5                            | 0                        |        OK                   |
| Temporização adequada conforme tempos medidos com auxílio de algum instrumento externo                  | Até 3              | Até 1,5                          | 0                        |       OK                    |
| Código implementa corretamente as fases do semáforo e estrutura do código (variáveis representativas e comentários) | Até 3              | Até 1,5                          | 0                        |              OK             |
| Extra: Implmeentou um componente de liga/desliga no semáforo e/ou usou ponteiros no código | Até 1              |  Até 0,5                         | 0                        |              COLOCOU PARA TOCAR UM BUZZER QUANDO O SEMAFORO FICAVA VERDE             |
|  |                                                             |  | |**Pontuação Total: 10**|
