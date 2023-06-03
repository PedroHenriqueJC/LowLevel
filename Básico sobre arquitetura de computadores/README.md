# Capítulo 1: Básico sobre arquiteturas de computadores

## Arquitetura do núcleo

	### Modelo de computação:

		Para conseguirmos construir um algoritmo é necessário ter anteriormente uma definição de um conjunto de ações básicas.

		O modelo de computação é um conjunto de operações básicas e os custos respectivos.

		Esse custo é relacionado a complexidade dos algoritmos.

		Esses modelos de computação são máquinas abstratas, ou seja, os modelos descrevem um computador hipotético.

	### Arquitetura de von Neumann:
	
		Um dos primeiros modelos a serem propostos.

		Arquitetura de um computador descreve sua funcionalidade, organização e implementação.

		Duas vantegens inicias era a robustes e facilidade de programar.

		Ele é inicialmente constituído de um processador e um banco de memória, conectados por um barramento(bus).
			Uma CPU(Central processing unit) que executa instruções.

			As instruções são buscadas na memória por uma unidade de controle.

			A ALU(Arithmetic logic unit)executa os processos necessários.

		(Ver imagem "arquitetura de Von Neumann")

		A memória desse modelo so armazena bits(0 ou 1)

		A memória armazena instruções e dados

		A memória está organizada em células, atualmente cada célula tem um byte.

		Os programas são constituidos de instruções que são buscadas uma após a outra a não ser que tenha uma instrução jump

		A linguagem assembly que será abordada posteriormente facilita a programação de códigos de máquinas mais simples por permitir que o programador não tenha que memorizar a codificação binária.

		*Uma arquitetura nem sempre define um conjunto exato de instruções, de modo diferente de um modelo de computação*
## Evolução

	### Desvantagens da arquitetura von Neumann

		Não é interativa.


		Não é apropriada a multitarefa.

		Não é realmente seguro porque não faz distinção do nível do usuário.

		O desempenho da memória criaria um gargalo em relação a CPU.

	### Arquitetura Intel 64

		Projetada pela intel, é suportada por processadores mais modernos.

		Será abordado o uso da CPU no modo longo.

	### Extensões da arquitetura

		Registradores:
			Memórias colocada diretamente na CPU, acessos muito rápido.

		Pilha de Hardware:
			Igual a estrutura de dado pilha, usada principalmente para processamento e armazenamento de variáveis

		Interrupções:
			Permite alterar a ordem de execução dos programas devido eventos externos.

			Ocorre tanto por sinais externos como por exceções(divisões por zero, instrução inválida, etc...)

		Anéis de proteção:
			Cada anel está ligado a um nível e define as instruções permitidas.

			Existem 4 aneis(00,01,10,11), os anéis 00 e 11 são utilizados atualmente.

			00 indica um conjunto privilegiado e 11 o conjunto de usuário.

		Memória virtual:
			Abstração da memória física que ajuda a distribuí-la mais igualmente.

		(Ver imagem "extensões modernas")

		*Para consulta de instruções busque o guia completo da intel*

## Registradores

	Criado para que os processadores não precisem acessar a memória RAM a todo momento.

	São poucos mais muito rápidos e geralmente os programas estão constantemente utilizando ele.

	(Registradores são feitos baseados em transistores e a memória RAM em condensadores)

	Programas são escritos para terem uma propriedade em particular, chamada de localidade de referência:
		Localidade temporal: acesso a memórias próximos em tempo

		Localidade espacial: endereços próximos em espaço, X e o próximo seria X-16 ou X-28.

	Programas típicos geralmente salvam os dados nos registradores e trabalham com eles, depois salvam os dados na memória.

	Geralmente os dados salvos não são utilizados, se precisar ser utilizado perderia desempenho

	### Registradores de propósito geral

		São registradores que podem ser utilizados para propósitos distintos.

		Possuem 64 bits e são nomeados de r0 a r15. De 0 a 7 eles recebem nomes diferentes.

		(Ver imagem registradores propósito geral)

		Em geral não se deve utilizar os registradores rsp e rbp, eles podem corromper a pilha e o stack frame.
		Porém podemos utilizar para operações

		É possível endereçar somente uma parte do registrador, sendo:
			32 bits menos significativos - d(double word)
			16 bits menos significativos - w(word)
			8 bits menos significativos - b(byte)

		É usado com o nome dele e o sufixo, r7b(por exemplo, que indica o byte menos significativo do registrador r7).

		Também pode-se endereçar utilizando nomes alternativo.

		(Ver imagem decomposição rax)

		O padrão de rax, rbx, rcx e rdx são iguais(so muda a letra do meio).

		Para o byte menos significativo em rsi e rdi são sil e dil para rsp e rbp são spl e bpl.

		(Ver imagem decomposição r4-r7)

	### Outros registradores

		São registradores em geral especiais e modificados somente pelo sistema.

		Um exemplo é o registrador rip que armazena o endereço da próxima instrução.(Alterado com o jmp)

		E o rflags que reflete o estado atual do programa, armazena o resultado da ultima instrução aritmética, se foi negativo ou até mesmo se ouve um overflow. Suas partes menores são a eflags(32 bits) e a flags(16bits)

		(Ver questão 1)

		Registradores xmm0 a xmm15 tem 128 bits e trabalham com pontos flutuantes(float)

	### Registradores de sistema

		Usados especificamente pelo sistema operacional, isolam frameworks do resto do sistema

		São inacessíveis(pelo menos inalteráveis) fora do modo privilegiado(00)

		Exemplos:
			cr0 e cr4 -> armazenam flags relacionadas a diferentes modos do processador e memória virtual

			cr2 e cr3 -> suporte para memória virtual

			cr8(tpr) -> ajuste nas interrupções

			efer -> controla o modo do processaodr

			idtr -> endereço da tabela de interrupções

			gdtr e ldtr -> endereço de descritores

			cs,ds,ss,es,gs,fs -> registradores de segmento. Oferecem mecanismos de segmentação legados.

## Anéis de proteção

	Cada anel está ligado a um nível de privilégio. Cada instrução é ligada a um nível e não é acessível a outros níveis.

	No intel 64 geralmente é utilizado somente 2 mas tem no total 4. Os utilizados são o 0 e o 3.

	O 0 é o mais privilegiado e o 3 menos privilegiado. 

	No modo longo os o número do anél de proteção é armazenado nos 2 bits menos significativos de cs e ss. E so são alterado por uma interrupção ou syscall

## Pilha de hardware

	Pilha é uma estrutura de dado LIFO com instruções push e pop.

	A pilha é na verdade uma simulação e não uma memória em si utilizada para a pilha.

	O registrador rsp armazena o endereço do elemento que ta no topo da pilha.

	push:
		Conforme o tamanho do argumento o valor de rsp será decrementado

		Um argumento é armazenado na memória começando no endereço obtido do rsp modificado

	pop:
		O elemento que está no topo da pilha é copiado para o registrador/memória

		Rsp é incrementado com o tamanho do argumento.

	Conveniente para implementar chamadas de função.

	(Ver imagem pilha)

	A pilha nunca está vazia, pode ser que tenha lixo dentro dela. Logo sempre pode utilizar um pop

	A pilha cresce em direção ao endereço 0(por isso o push decrementa)

	Todos os elementos são considerados inteiro com sinais

## Questões:
	
	1) Oque significam as flags CF,AF,ZF,OF,SF? Qual a diferença entre OF e CF?
		CF -> Carry flag
		AF -> Auxiliary carry flag
		ZF -> Zero flag
		OF -> Overflow flag
		SF -> Sign flag

		CF indica quando o resultado de uma operação foi feita pela ALU
		OF indica quando houve um overflow em uma operação

		https://en.wikipedia.org/wiki/FLAGS_register

	2) Quais são os princípios essenciais da arquitetura de von Neumann?
		Memória armazena somente bits
		Memória armazena dados e instruções
		Memória organizada em células
		Um programa consiste em instruções

	3) O que são registradores?
		São células de memórias implementadas diretamente no chip da CPU, para que possam ser acessadas pelo barramento de sistema e tenham um melhor desempenho.

	4) O que é pilha de hardware?
		É uma estrutura de dados que permite o comando pop e push(e outros também). O topo é apontado pelo rsp.
		É uma simulação de um espaço de memória.

	5) O que são interrupções?
		São controle de violações na sequência de execução de um programa. Eles permitem a alteração na ordem de execução de programas baseados em eventos externos e internos.

	6) Quais são os principais problemas que as extensões modernas do modelo de von Neumann tentam resolver?
		Nada é possível sem acessar a memória lenta.
		Pouca interatividade.
		Péssimo multitasking
		Baixo isolamento de programas

	7) Quais são os principais registradores de propósito geral do intel 64?
		r0 a r15. Sendo do r0 a r7 nomes especiais.

	8) Qual o propósito do ponteiro de pilha?
		rsp armazena o endereço do último elemento da pilha(topo). Ele executa instruções como call, pop e push.

	9) A pilha pode estar vazia?
		Não, a pilha sempre terá alguma coisa, mesmo que essa coisa seja um lixo.

	10) É possível contar os elementos de uma pilha?
		Não, nunca saberemos qual a quantidade de elementos de uma pilha. Mesmo que executamos pop infinitamente ela nunca ficará vazia e o valor de rsp incrementaria.