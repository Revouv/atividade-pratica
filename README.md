# Atividade Prática — Meu Primeiro Circuito no Tinkercad.
<img width="1141" height="793" alt="tinkercad-jogo-da-memoria jpg" src="https://github.com/user-attachments/assets/59037cc3-795f-42ae-b8c2-29176f5ad8a8" />

> *Fig. 1 — Circuito montado no Tinkercad.*

## Jogo da Memória com Arduino:
---

## 1. Nome do projeto
**Jogo da Memória com Arduino (Genius)**
Veja também o projeto no Github:
🔗 [Jogo da Memória com Arduino — GitHub](https://github.com/Revouv/atividade-pratica.git)


## 2. Link do projeto no Tinkercad
🔗 [Jogo da Memória com Arduino — Tinkercad](https://www.tinkercad.com/things/7XOxxUSO116-jogo-da-memoria-com-arduino?sharecode=vn7mSqJRMt3UM_O_AFWQkAWHZ3bv7oOwFPOeOOk8kqU)

## 3. Print do projeto
Montagem do circuito no Tinkercad: Arduino Uno, 4 LEDs com resistores no cátodo, 4 push buttons e buzzer sobre protoboard.
Imagem anexada no começo do arquivo READ.ME.

## 4. Código utilizado

🔗 [Jogo da Memória com Arduino — Código-fonte](arduino_code.cpp)

## 5. Respostas às perguntas propostas:

### 1. Qual é a ideia do seu projeto?:
Experimentar com a construção de circuitos com Arduino e recriar o jogo eletrônico de memória "Genius". O sistema gera uma sequência aleatória via função: `random()`, progressivamente maior de LEDs e o jogador deve repetir a sequência pressionando cada um dos botões  correspondentes.

### 2. Qual problema ou situação ele tenta resolver?:
É um circuito didático, que pode ser aplicado em contextos pedagógicos como instrumento de ensino ou em aplicações reais de conceitos de sistemas embarcados, como entrada e saída digital, geração de som por `tone()`, uso de arrays para armazenar estado, controle de fluxo e temporização e etc., construindo, como resultado, um jogo interativo funcional. Para outros contextos, a randomização de valores e o armazenamento de respostas na variável `sequencia[]` pode ser usado em casos reais, como as ferramentas de autenticação via gerador de senhas numéricas.

### 3. Quais componentes você utilizou?:
Os mesmos definidos no tutorial de referência:

> 1 Arduino UNO R3;
> 1 Protoboard (830 pontos);
> 4 LEDs de 5 mm (vermelho, verde, azul e amarelo);
> 4 Resistores (ligados ao catodo de cada LED, para limitar a corrente);
> 4 Push buttons (chave táctil), sem resistor externo. Usa-se o pull-up interno do Arduino;
> 1 Buzzer, para o retorno sonoro de cada LED e do game over;
> Jumpers (cabos macho/macho) para todas as conexões.

### 4. Qual é a função de cada componente?:
| Componente | Função |
|---|---|
| **Arduino UNO** | Controlador do projeto. Executa o código, lê os botões e comanda LEDs e o buzzer através das entradas. |
| **Protoboard** | Base de montagem do circuito sem necessidade de solda. |
| **LEDs** | Retorno visual de qual item da sequência está sendo mostrado ou foi pressionado. |
| **Resistores** | Limitam a corrente que passa pelos LEDs, evitando que queimem. |
| **Push buttons** | Entrada do jogador. Cada um representa um item da sequência (uma "cor"). |
| **Buzzer** | Gera o som (`tone()`) associado a cada LED, e o efeito sonoro de game over. |
| **Jumpers** | Ligação elétrica entre os pinos do Arduino e os componentes na protoboard. |

### 5. Como o Arduino participa do projeto?
O Arduino é o responsável por toda a lógica do jogo. Ele configura os pinos dos LEDs e do buzzer como saída e os pinos dos botões como entrada com pull-up interno (`INPUT_PULLUP`). Por isso não há resistor físico nos botões. A cada rodada, ele sorteia um novo item com `random()`, armazena a sequência completa em um array, reproduz essa sequência acendendo os LEDs e tocando os sons correspondentes, e depois fica lendo os botões para conferir, passo a passo, se o jogador reproduziu a sequência corretamente. Também é o Arduino que controla toda a temporização do jogo (`delay()`) e decide quando ocorre o `Game Over`.

### 6. Como o seu código funciona?
O programa gira em torno de um array global `sequencia[]`, que guarda os itens sorteados, e de duas listas de pinos (`pinosLeds[]` e `pinosBotoes[]`) que associam cada índice (0 a 3) a um pino físico. A cada passagem do `loop()`:
1. `proximaRodada()` sorteia um novo índice e o adiciona à sequência;
2. `reproduzirSequencia()` percorre e reproduz toda a sequência acumulada até então, acendendo o LED e tocando o tom de cada item;
3. `aguardarJogador()` bloqueia a execução em `aguardarJogada()`, que varre os 4 botões até detectar um pressionado (lido como `LOW`, por causa do pull-up), e compara esse índice com o próximo item esperado da sequência.

Se o jogador errar, `gameOver()` dispara um efeito sonoro/visual de derrota e ativa a flag `perdeu_o_jogo`, que reinicia as variáveis do jogo na próxima passagem do `loop()`.

### 7. O que você pesquisou para conseguir construir o projeto?:
Utilizei como base principal o tutorial *"Jogo da Memória (Genius) - Arduino Jogo #03"* do site *Squids Arduino*, de onde vieram o esquema de montagem e o código-fonte. Também assisti ao vídeo indicado pelo professor e consultei o repositório do GitHub de *CaetanoNav* como uma segunda referência de implementação do mesmo jogo no Tinkercad, para comparar abordagens.

### 8. Qual foi a principal dificuldade encontrada?
Tive dificuldade de entender a lógica de cabos de ligação e polaridade dos LEDs. Quando apliquei o código, não entendi o porquê da posição dos LEDs na placa de ensaio, mesmo que não correspondesse à posição definida no código, retornava exatamente o que o circuito deveria retornar. A metodologia em si não foi a maior dificuldade, aplicá-la passo a passo me fez criar um circuito funcional sem maiores problemas. O que me foi custoso foi entender a metodologia quando aplicada no contexto do meu projeto.

### 9. O que você faria para melhorar seu projeto?
Entre as melhorias possíveis estão: adicionar um placar ou contador de pontos, por exemplo em um display LCD (como sugerido no "Desafio 65" do próprio tutorial de referência); aumentar a dificuldade progressivamente, reduzindo o delay entre os itens da sequência conforme o jogo avança; adicionar um efeito sonoro de vitória distinto do efeito de derrota; salvar o recorde de rodadas em EEPROM para persistir entre execuções; e implementar um debounce mais robusto na leitura dos botões.

## 6. Fontes utilizadas na pesquisa

- **Tutorial principal (montagem e código-fonte):** [Jogo da Memória (Genius) - arduino jogo #03 ~ Squids Arduino](https://www.squids.com.br/arduino/projetos-arduino/jogos/252-jogo-da-memoria-genius-arduino-jogo-03)
- **Vídeo indicado pelo professor:** <https://youtu.be/WY0NRpp4eog?si=HWprj9oMfFCWqSX9>
- **Referência adicional consultada (outra implementação do mesmo jogo):** [CaetanoNav/Jogo-da-Memoria-Arduino (GitHub)](https://github.com/CaetanoNav/Jogo-da-Memoria-Arduino)
