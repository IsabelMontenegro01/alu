# Documentação dos Circuitos

## Somador de 8 bits

O somador de 8 bits foi construído a partir da conexão em série de oito somadores completos de 1 bit (Full Adders). Cada somador é responsável por uma posição do número binário.

O funcionamento ocorre da seguinte forma:

* Cada Full Adder recebe três entradas: A, B e o carry de entrada (Cin)
* Ele produz duas saídas: soma (S) e carry de saída (Cout)
* O Cout de um bit é conectado ao Cin do próximo bit (encadeamento)

As portas lógicas utilizadas em cada somador completo são:

* XOR: utilizada para calcular a soma dos bits
* AND: utilizada para identificar quando há geração de carry
* OR: utilizada para combinar os sinais de carry

A soma final é obtida pela propagação do carry ao longo dos 8 bits.

---

## Subtrator de 8 bits

O subtrator de 8 bits foi implementado a partir do somador de 8 bits, utilizando a técnica de complemento de dois.

A subtração é realizada com base na seguinte equivalência:

A - B = A + (~B) + 1

Para isso:

* Cada bit de B é invertido utilizando portas NOT
* O carry de entrada (Cin) do primeiro bit é fixado em 1
* Os demais bits seguem o encadeamento normal do carry

Dessa forma, o circuito de subtração reutiliza completamente a estrutura do somador, alterando apenas as entradas.

---

## Multiplicador de 8 bits

O multiplicador de 8 bits foi implementado utilizando a técnica de soma de produtos parciais.

O funcionamento ocorre da seguinte forma:

* Cada bit do multiplicador é combinado com os bits do multiplicando através de portas AND, gerando produtos parciais
* Esses produtos são deslocados de acordo com a posição do bit
* Os resultados são somados utilizando meio somadores (Half Adders) e somadores completos (Full Adders)

As portas e componentes utilizados são:

* AND: para gerar os produtos parciais
* Half Adder: para somas simples sem carry de entrada
* Full Adder: para somas que envolvem carry

O resultado final é obtido pela soma de todos os produtos parciais, podendo atingir até 16 bits.

---

## Módulo de Shift Lógico (Barramento de Deslocamento)

O circuito de **Shift Lógico** realiza o deslocamento de bits para a esquerda ou direita no operando **AC**. Ele utiliza uma malha de seleção bidirecional para determinar a posição final de cada bit.

### Lógica de Funcionamento

* **Controle de Direção:** O pino *Direção* atua como seletor mestre. Quando em nível alto (**1**), habilita o deslocamento em um sentido; quando invertido por uma porta NOT, habilita o sentido oposto.
* **Seleção por Estágio:** Para cada bit de saída, um conjunto de portas **AND** e **OR** define a origem do dado:
    * Captura o bit vizinho da esquerda para shift à direita[cite: 75, 83].
    * Captura o bit vizinho da direita para shift à esquerda[cite: 64, 72].
* **Preenchimento com Zero:** Como padrão de operação lógica, as posições que ficam vazias nas extremidades (MSB ou LSB) são automaticamente preenchidas com o valor **0**.
* **Descarte:** Os bits deslocados para fora do barramento de 8 bits são descartados.



---
