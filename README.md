# CPU de 8 Bits - Projeto de Arquitetura de Computadores

Este documento descreve o funcionamento e a organização da CPU de 8 bits desenvolvida no simulador Digital. A arquitetura processa instruções de 8 bits, sendo divididas em 4 bits para operação (OpCode) e 4 bits para endereçamento de memória.

## Unidade de Processamento (Datapath)

### Program Counter (PC)
O Program Counter é o registrador responsável por armazenar o endereço da próxima instrução a ser buscada na memória. Ele funciona de forma sequencial, incrementando seu valor a cada ciclo de busca. Para o funcionamento correto nesta arquitetura, o pino de direção (dir) deve estar aterrado (valor 0), garantindo a contagem ascendente.

### Memory Address Register (MAR)
O MAR é o componente que faz a interface direta com o barramento de endereços da memória. Ele retém o endereço que a ROM ou RAM deve acessar no momento. Durante o ciclo de busca, ele recebe o valor do PC; durante o ciclo de execução, ele recebe o endereço de operando vindo do Instruction Register.

### Memória ROM
A memória de apenas leitura armazena o programa a ser executado. Ela recebe o endereço do MAR e disponibiliza o dado correspondente para o barramento de dados. No projeto atual, ela está configurada para 8 bits de largura, tratando tanto instruções quanto dados imediatos.

### Instruction Register (IR)
O IR captura a instrução vinda da memória no final do ciclo de busca. Ele utiliza um divisor (Splitter) para separar o byte recebido: os 4 bits superiores definem a operação para a Unidade de Controlo, enquanto os 4 bits inferiores definem o endereço do dado na memória.

### Acumulador (AC) e ALU
O Acumulador é o registrador principal de trabalho da CPU. Ele armazena o resultado de operações aritméticas e lógicas. A ALU (Unidade Lógica e Aritmética) processa os dados vindos da memória em conjunto com o valor já armazenado no Acumulador, realizando operações como a soma.

### Multiplier-Quotient Register (MQ)
O registrador MQ é um registrador auxiliar de 8 bits utilizado para estender as capacidades de processamento da CPU. Ele atua como um armazenamento temporário adicional para dados que saem do barramento ou da ALU, sendo fundamental em operações de multiplicação e divisão que resultam em valores superiores a 8 bits ou que necessitam de manipulação de operandos secundários sem afetar o Acumulador principal.

## Unidade de Controlo (Control Unit)

### Step Counter (Contador de Passos)
Este componente gerencia os micro-passos de cada instrução. Ele conta de 0 a 7, permitindo que a CPU saiba exatamente em qual fase da execução está (busca, decodificação ou execução). Ele pode ser reiniciado prematuramente pelo sinal FIM.

### EEPROM de Controlo
Atua como a lógica combinatória central. Com base na instrução presente no IR e no passo atual do Step Counter, a EEPROM ativa os sinais de controle necessários, incluindo a habilitação do registrador MQ através de sinais específicos de carga (MQ-IN) ou leitura (MQ-OUT).

## Componentes Auxiliares e Integração

### Bit Extender (Extensor de Bits)
O Extensor de Bits é fundamental para a compatibilidade entre barramentos. Ele recebe a morada de 4 bits vinda do IR e a expande para 8 bits, preenchendo os espaços vazios com zeros. Isso evita que o MAR receba valores flutuantes ou incorretos.

### Clock
O sinal de clock sincroniza todas as transições de estado da CPU. Cada transição de nível lógico baixo para alto faz com que os registradores (PC, MAR, IR, AC e MQ) capturem os dados presentes em suas entradas, permitindo o avanço do processamento.

## Fluxo de Operação

O ciclo inicia-se com o PC enviando o endereço para o MAR. A instrução é lida da ROM e carregada no IR. A Unidade de Controle decodifica a instrução e direciona o fluxo de dados: dependendo do OpCode, os dados podem ser movidos entre a memória, o Acumulador e o registrador MQ. Ao final de cada instrução, o sistema retorna ao passo inicial para buscar a próxima instrução.

## Dificuldades

Tive muita dificuldade com essa atividade. Havia começado uma CPU com uma lógica, não estava conseguindo resolver o erro. Resolvi refazer considerando algumas recomendações de colegas. Acredito que entreguei algo bem feito mas não vou conseguir gravar um vídeo explicando a atividade.

Agradeço!
