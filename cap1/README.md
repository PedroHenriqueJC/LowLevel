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

	Status: Em progresso
