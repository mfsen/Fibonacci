@ STM32F107 - Assembly template
@ Turns on GPIOD Pin 1

@ Start with enabling thumb 32 mode since Cortex-M3 do not work with arm mode
@ Unified syntax is used to enable good of the both words...
@ Make sure to run arm-none-eabi-objdump.exe -d prj1.elf to check if
@ the assembler used proper insturctions. (Like ADDS)

	.thumb
	.syntax unified

	.equ     STACKINIT,   0x20008000
	.equ     DELAY,       1500000



@ Register Addresses
@ You can find the base addresses for all the peripherals from Memory Map section
@ RM0008 on page 49. Then the offsets can be found on their relevant sections.
@ As shown in GPIOD_ODR register


	.equ     RCC_APB2ENR,   0x40021018      @ enable clock
	.equ     GPIOD_CRL,     0x40011400      @ PORTD control register low
	.equ     GPIOD_ODR,     0x4001140C      @ PORTD output data (Page 172 from RM0008)
	.equ     GPIOC_IDR,     0x40011008      @ PORTC input data (Page 172 from RM0008)
	.equ     GPIOC_CRL,     0x40011000      @ PortC control register low


.section .text
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@ Vectors
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@ Vector table start
	.word    STACKINIT
	.word    _start + 1

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@ Main code starts from here
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

_start:
	@ Enable GPIOD Peripheral Clock (Page 145 from RM0008)
	LDR R6, = RCC_APB2ENR  @ Load peripheral clock enable regiser
	LDR R5, [R6]           @ Read its content
	ORR R5, 0x00000030     @ Set Bit 5 to enable GPIOD clock
	STR R5, [R6]           @ Store back the result in Perihperal clock enable register


	@ Make GIOOD Pin1 as output pin (Page 170 from RM0008)
	LDR R6, = GPIOD_CRL    @ Load GPIOD control register low  (Pin1 is in CRL register)
	LDR R5, = 0x22222222   @ Set MODE bits to 10 and CNF bits to 00, thus 0x00000020
	STR R5, [R6]           @ Store back the result in GPIOD control register low


	LDR R6, = GPIOD_ODR
	LDR R5, = 0x00000002   @ Set mode off pins to output
	STR R5, [R6]           @ Store back the result in GPIOD output
	BL button


	LDR R1,= #0				@adder
	LDR R4,= #1				@result
	LDR R2,= #20				@counter
	B loop

button:
	@ Set GIOOC Pin1 to 1 (Page 172 from RM0008)
	LDR R6, = GPIOC_IDR    @ Load GPIOC input data register
	LDR R8, [R6]
	LDR R2, = 0x00000001 @ to reset the  other values
	AND R8, R2

	CMP R8, #1 				@if R8 = 1 go to loop
	BNE button
	LDR r8,=#0
	BX LR

loop:
@calculation part



	ADDS R7,R1,R4
	MOV R1,R4
	MOV R4,R7


	b separating










separating:

	subs R2, #1
	CMP r2, #0
	Beq _start

	LDR r12,=#1000
	UDIV r10,r7,r12
	push {r10}
	mul r10,r12
	subs r7,r7,r10
	LDR r12,=#100
	UDIV r10,r7,r12
	push {r10}
	mul r10,r12
	subs r7,r10
	LDR r12,=#10
	UDIV r10,r7,r12
	push {r10}
	mul r10,r12
	subs r7,r10
	push {r7}

	pop {r0}
	pop {r6}
	pop {r5}
	pop {r7}

	push {r0}
	push {r6}
	push {r5}
	push {r7}

	ldr r12,=#5
  b se

se:


	subs r12,#1
	CMP r12, #0
	BEQ loop

	LDR R3,= #1100000 @ delay for 3 sec
	Bl delaythree

	pop {r7}

	CMP R7, #0
	BEQ se

	CMP R7, #0
	BEQ zero
	CMP R7, #1
	BEQ one
	CMP R7, #2
	BEQ two
	CMP R7, #3
	BEQ three
	CMP R7, #4
	BEQ four
	CMP R7, #5
	BEQ five
	CMP R7, #6
	BEQ six
	CMP R7, #7
	BEQ seven
	CMP R7, #8
	BEQ eight
	CMP R7, #9
	BEQ nine


zero:
ldr R7, = #0
 ldr r0,=#6
B morse

one:
LDR R7, = #1
 ldr r0,=#6
B morse

two:
LDR R7, = #3
 ldr r0,=#6
B morse

three:
ldr R7, = #7
 ldr r0,=#6
B morse

four:
ldr R7, = #15
 ldr r0,=#6
B morse

five:
ldr R7, = #31
 ldr r0,=#6
B morse

six:
ldr R7, = #30
 ldr r0,=#6
B morse

seven:
ldr R7, = #28
 ldr r0,=#6
B morse

eight:
ldr R7, = #24
 ldr r0,=#6
B morse

nine:
ldr R7, = #16
 ldr r0,=#6
B morse

delaythree:

LDR R6, = GPIOD_ODR
LDR R5, = 0x00000000   @ Set MODE bits to 10 and CNF bits to 00, thus 0x00000020
STR R5, [R6]
subs R3, #1
BNE  delaythree
BX lr


morse:

	LDR R6, = GPIOD_ODR
	LDR R5, = 0x00000000   @ Set MODE bits to 10 and CNF bits to 00, thus 0x00000020
	STR R5, [R6]
	ldr r3,= DELAY
	bl delays

	LDR R3,= 0x00000001      @to get last bit
	AND R11, R3, R7
	LDR R6,= #2
	UDIV R7,R7,R6

	subs r0,#1
	cmp r0,#0
	Beq se

	LDR R3,= #900000   @ delay for 1 sec
	cmp r11,#0
	Bne delayone

	LDR R3,= #2500000 @ delay for 3 sec
	B delaytwo


delayone:

	LDR R6, = GPIOD_ODR
	LDR R5, = 0x00000001   @ Set MODE bits to 10 and CNF bits to 00, thus 0x00000020
	STR R5, [R6]
	subs R3, #1
	BNE  delayone
	B morse

delaytwo:
	LDR R6, = GPIOD_ODR
	LDR R5, = 0x00000001   @ Set MODE bits to 10 and CNF bits to 00, thus 0x00000020
	STR R5, [R6]
	subs R3, #1
	BNE  delaytwo
	B morse

delays:

	subs R3, #1
	BNE  delays
	BX LR
