# Documentação da CPU de 8 bits

## Visão Geral

A CPU foi projetada com base na arquitetura **SAP (Simple As Possible)**, que organiza o funcionamento do processador de forma simples e didática.

Ela executa instruções seguindo o ciclo:

1. Busca (Fetch)
2. Decodificação (Decode)
3. Execução (Execute)

O controle das operações é realizado por uma unidade de controle baseada em estados.

---

## Arquitetura SAP

A arquitetura SAP divide a CPU em três partes principais:

* Unidade de Controle → define o que será executado
* Caminho de Dados → por onde os dados trafegam
* Memória → armazenamento de instruções e dados

O funcionamento ocorre por meio de sinais de controle que coordenam:

* Transferência entre registradores
* Operações da ALU
* Acesso à memória

---

## Contador de 4 bits (Program Counter)

O contador de 4 bits atua como **Program Counter (PC)**, responsável por indicar o endereço da próxima instrução.

### Funcionamento

* Armazena o endereço atual da memória
* Incrementa automaticamente a cada ciclo de busca
* Pode ser resetado para reiniciar a execução

### Importância

Ele garante a execução sequencial das instruções, controlando o fluxo do programa.

---

## Contador de Anel (Ring Counter)

O contador de anel é responsável por gerar os **estados de controle da CPU**.

### Funcionamento

* Possui um único bit em nível alto que circula entre os estágios
* Cada posição representa um estado da execução (T0, T1, T2, ...)

Exemplo:

* T0 → busca da instrução
* T1 → carregamento
* T2 → execução

### Papel no sistema

* Controla o tempo das operações
* Sincroniza os sinais de controle
* Define em qual etapa do ciclo a CPU está

---

## Integração da ALU

A ALU desenvolvida anteriormente foi integrada como o bloco responsável pelas operações aritméticas e lógicas da CPU.

### Funcionamento na CPU

* Recebe dados dos registradores (ex: AC e outro operando)
* Executa a operação selecionada
* Retorna o resultado para o acumulador

### Controle

A operação da ALU é definida pelos sinais de controle gerados pela unidade de controle, de acordo com a instrução atual.

---

## Fluxo de Execução

O funcionamento da CPU segue o ciclo básico:

### 1. Busca (Fetch)

* O PC fornece o endereço
* A instrução é carregada da memória

### 2. Decodificação (Decode)

* A instrução é interpretada
* Sinais de controle são definidos

### 3. Execução (Execute)

* A operação é realizada:
  * Pode envolver a ALU
  * Pode envolver movimentação de dados

---

## Observações do Projeto

* A utilização da arquitetura SAP facilitou a organização do sistema
* O uso do contador de anel simplificou o controle por estados
* A ALU modular permitiu integração direta com a CPU
* A separação entre controle e dados melhorou a clareza do circuito

---