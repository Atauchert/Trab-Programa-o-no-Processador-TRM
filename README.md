# Trab-Programa-o-no-Processador-TRM
# Enunciado: 
Cada dupla deverá elaborar e desenvolver programas para resolver os problemas
colocados abaixo na linguagem de montagem do processador TRM. Para validar o trabalho deverão
executar o código de montagem no simulador do processador TRM, conforme trabalhado em aula. Para
cada problema, os alunos deverão:
1. Escrever um parágrafo explicando o funcionamento do código em questão. Este parágrafo deve
ser adicionado a um arquivo README.md em seu repositório;
2. Salvar uma imagem ou print da tela do simulador, mostrando o estado da memória e
registradores após a execução do programa. As imagens deverão constar logo abaixo dos
parágrafos, também no arquivo README.md;
3. Salvar o código do programa em um arquivo separado dos demais.
# ATENÇÃO:
Seu repositório deverá conter apenas um arquivo README.md com todas as descrições
dos programas e prints de simulação, além de um arquivo para cada programa desenvolvido.

# Problema 1: 
Desenvolva um programa que escreva na memória os N primeiros números inteiros
ímpares maiores que zero. Seu programa deverá ser N da memória e funcionar para qualquer valor de
N. Os valores deverão ser escritos a partir do endereço 0x20 e aparecerem contiguamente na memória.

main

    add v0,zr,1
	add v2,zr

loop

	stw v0,v2,0x20
	add v2,v2,2
	add v0,v0,2
	add v1,v1,2
	beq zr,zr,loop

hlt
	
# Problema 2: 
Desenvolva um programa capaz de gerar os N primeiros números da sequência de
Fibonacci. Escreva cada número gerado na memória, a partir do endereço 0x30.

main

	add v0,zr	        ;0
	add v1,zr,1	        ;1
	add v2,zr	        ;n da sequencia
	add v3,zr	        ;indice do index
	add v5,zr	        ;(i)
	add v6,zr,24	    ;(n)
	sub v4,v6,2	        ;(N) = n-2
	stw v0,zr,0x40	    ;carrega o primeiro numero da sequencia
	stw v1,zr,0x42	    ;carrega o segundo numero

loop0

	ldw v2,v3,0x40	    ;carrega na v2 um dos anteriores
	add v2,v1	        ;soma o anterior que foi adicionado na v2 com o valor da v1
	stw v2,v3,0x44	    ;guarda o proximo numero da sequencia na memoria
	ldw v0,v3,0x42	    ;carrega o v1 no v0
	ldw v1,v3,0x44	    ;carrega o v2 no v1
	add v3,v3,2	        ;indice de navegação da memoria
	add v5,v5,1	        ;indice do loop0
	blt v5,v4,loop0	    ;volta para o loop0


# Problema 3: 
Escreva um programa para encontrar o maior número inteiro positivo escrito em memória
entre os endereços 0x40 e 0x80. O número encontrado deverá ser escrito na posição 0x90.

main

    add v0,zr,1
    add v1,zr,0
    add v2,zr,0x40
    add v3,zr,0x82

loop

    ldw v5,v2,0
    blt v5,v0,continuo
    add v0,v5,0

continuo

    add v2,v2,2
    beq v2,v3,feito
    beq zr,zr,loop

feito

    stw v0,zr,0x90
    hlt

# Problema 4: 
Escreva um programa que lê todos os valores escritos em memória entre as posições 0x40
e 0x80 e os escreve a partir da posição 0x90, somando 1 aos valores pares para que se tornem ímpares.
Assim, todos os 64 primeiros valores na memória a partir de 0x90 devem ser ímpares.

main
    
    add   v2, zr, 0x40       
    add   v3, zr, 0x80       
    add   v4, zr, 0x90       
    add   v0, zr, var_a
    stw v0,zr,0x50

loop
    
    ldw   v5, v2, 0          
    and   v6, v5, 1          
    beq   v6, zr, par        

impar

    stw   v5, v4, 0         
    add   v2, v2, 2       
    add   v4, v4, 2          
    beq   v2, v3, fim      
    beq   zr, zr, loop       

par

    add   v5, v5, 1           
    beq   zr, zr, impar      

var_a

    123

fim

    hlt

# Problema 5: 
Escreva um programa que procura entre os endereços 0x60 e 0x70 por dois números cuja
soma seja igual a 10. Caso este números existam, a posição deles deverá ser escrita nos endereços 0x80 e 0x90.


# Problema 6: 
Escreva um programa que inverte a ordem dos valores compreendidos entre os endereços
0x40 e 0x60, isto é, os valores deverão aparecer "de trás pra frente" na memória. Você não deverá ler
ou escrever em endereços de memória fora do intervalo entre 0x40 e 0x60.

main

    add v0, zr, 0x40      
    add v1, zr, 0x60      
    add v2, zr, var_a
    stw v2, zr, 0x52
    add v3, zr, var_b
    stw v3, zr, 0x4e

loop

    bge v0, v1, fim   
    ldw a0, v0, 0      
    ldw a1, v1, 0       
    stw a1, v0, 0       
    stw a0, v1, 0        
    add v0, v0, 2
    sub v1, v1, 2
    beq zr, zr, loop      

var_a

    123

var_b

    456

fim

    hlt

# Problema 7: 
Assuma uma matriz MxN, onde M está escrita no endereço 0x40, N está escrito no
endereço 0x42 e os valores da matriz iniciam no endereço de memória 0x44. Escreva um programa que
escreve os elementos da diagonal principal a partir do endereço 0x100. Seu programa deve funcionar
para qualquer matriz de tamanho MxN < 64.

# Problema 8: 
Escreva um programa que escreve seu nome completo na saída do simulador. Se estiver
trabalhando com uma dupla, escreva o nome de ambos os alunos. Exemplos de escrita na saída do
simulador podem ser encontrados na pasta exemplo.

main

	add a0,zr,text		; a0 = &text
loop

	ldb v0,a0		; v0 = mem[a0]
	stw v0,zr,0xf006	; print char
	beq v0,zr,end		; if v0 == 0, goto end
	add a0,a0,1		; &text++
	beq zr,zr,loop		; goto loop

end

	add v0,zr,10		; print a line feed char
	stw v0,zr,0xf006
	hlt

text	

	"Alexandre, Gabriel, Gabriel, Gianlluca"

# Problema 9: 
Escreva um programa que identifique se uma palavra é palíndrome. O número de
caracteres na palavra deve ser lido do endereço 0x60 da memória, enquanto a palavra deve iniciar no
endereço 0x70.

# Problema 10: 
Escreva um programa que conta o número de cada vogal em uma palavra. O número de
letras A, E, I, O e U devem ser escritos, respectivamente, nos endereços 0x40, 0x44, 0x48, 0x50 e
0x52. A palavra deve ser lida a partir do endereço 0x60. A quantidade de letras na palavra deve ser lida do endereço 0x58
