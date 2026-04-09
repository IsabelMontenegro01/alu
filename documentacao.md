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

# Documentação dos Circuitos — Parte 2

## Circuitos Otimizados

Durante o desenvolvimento do projeto, alguns circuitos foram revisados e otimizados com o objetivo de melhorar a organização, reutilização e funcionamento dentro da CPU.

* **Somador:** foi refeito utilizando um componente modular de 1 bit já pronto (`somador.dig`), evitando a repetição manual de oito estruturas iguais.
* **Subtrator:** também foi refeito seguindo a mesma lógica modular do somador, aproveitando melhor a reutilização de componentes.
* **Shift:** inicialmente havia apenas um circuito para ambas direções, porém isso dificultou a integração na ALU e na CPU. Por isso, foi separado em dois módulos: `shift_dir.dig` e `shift_esq.dig`, facilitando o controle e a implementação.
* **Multiplicador:** foi refeito mantendo a mesma lógica de funcionamento, porém utilizando **túneis**, o que reduziu a repetição desnecessária de conexões e melhorou a legibilidade do circuito.

---

## Operações Acrescentadas Tardiamente

### Divisor de 8 bits

O divisor de 8 bits foi implementado utilizando o método de **subtração restaurativa**, uma técnica clássica de divisão binária.

### Funcionamento

O processo ocorre de forma iterativa e pode ser resumido nos seguintes passos:

1. O dividendo é carregado em um registrador (ou barramento principal).
2. O divisor é alinhado à esquerda em relação ao dividendo.
3. A cada etapa:

   * É realizada uma subtração entre o valor atual e o divisor.
   * Se o resultado for **positivo ou zero**, o bit do quociente recebe **1**.
   * Se o resultado for **negativo**, o valor original é **restaurado** (por isso o nome “restaurativo”) e o bit do quociente recebe **0**.
4. O divisor é deslocado para a direita a cada iteração.
5. O processo continua até que todos os bits tenham sido processados.

### Componentes utilizados

* Subtrator (reaproveitando o circuito já desenvolvido)
* Registradores (implícitos na estrutura do barramento)
* Shift (para alinhamento do divisor)
* Lógica de controle (para decisão entre restaurar ou manter o resultado)

Ao final:

* O resultado da divisão gera o **quociente**
* O valor restante corresponde ao **resto**

---

### ALU (Unidade Lógica e Aritmética)

A ALU foi projetada como o bloco central da CPU, integrando todas as operações desenvolvidas anteriormente em um único circuito.

### Operações suportadas

A ALU é capaz de executar:

* Soma
* Subtração
* Multiplicação
* Divisão
* Shift à esquerda
* Shift à direita

### Estrutura e Funcionamento

Cada operação é implementada como um módulo independente dentro da ALU. As entradas (operandos) são compartilhadas entre todos os módulos, e cada um produz sua saída simultaneamente.

Para selecionar qual operação será utilizada, foi empregado um **seletor (multiplexador)**.

### Papel do seletor (MUX)

O seletor é responsável por:

* Receber todas as saídas das operações (somador, subtrator, multiplicador, divisor e shifts)
* Escolher **apenas uma saída** com base em sinais de controle (opcode)
* Encaminhar essa saída para o barramento principal da CPU

Ou seja, mesmo que todas as operações sejam calculadas internamente, apenas o resultado da operação selecionada é utilizado.

### Integração dos componentes

* Os operandos A e B são distribuídos para todos os blocos
* O sinal de controle define a operação ativa
* O multiplexador seleciona o resultado correto
* O resultado final é enviado para o acumulador (AC) ou barramento de saída

### Observações

* O uso de módulos separados facilitou a manutenção e testes individuais
* A utilização de túneis e componentes reutilizáveis melhorou significativamente a organização do circuito
* A separação dos shifts em dois módulos simplificou o controle dentro da ALU

---
