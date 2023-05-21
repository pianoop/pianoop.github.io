---
title:  "[시스템프로그래밍] week10 ~ 12"
excerpt: "공부 정리 "

categories:
  - system
tags:
  - [assembly]

toc: true
toc_sticky: true
 
date: 2023-05-21
last_modified_at: 2023-05-21
---
# 시프 10 ~ 12주차 정리   
## conditional Loop Instructions
Loopz Loope Loopnz Loopne  
-> ecx--, ecx > 0 and ZF = 1 or 0 을 검사  
  
### IF문을 assembly로 작성

### Conditinoal structuress
- short circuit evaluation 

if: and, or, .. loop 사용  
while: jmp 사용  
- Table driven selection: 구조가 복잡할 경우 사용
    - 장점: case가 많을 경우에도 간단하게 작성 가능, 테이블 조건 관련된 내용을 런타임에 수정 가능
    - 단점: initial overhead가 발생

### FSM
DFA/NFA와 유사  

### Conditional Control Flow Directives
- IF, ELSE, ELSEIF, and ENDIF directives
```asm
.IF condition1
    statements
[.ELSEIF cond2
    state2]
[.ELSE
    state3]
.ENDIF
```
- <, >, ==, !=, ...  

와 같은 편의 기능들이 있음, but 공부의 목적을 잊지 말 것!(학습중 사용 X)  
<br>  

## shift and Rotate instructions
1. Logical shift(>>)연산의 MSB에는 0이, LSB는 CF로 이동함.
1. Arithmetic shift는 Logical shift와 동일하나 MSB의 값은 이전의 MSB를 복사해옴. (부호 유지)

***
1. SHL: << (unsigned)
SHL reg/mem, shift 상수(8bit)
1. SHR: >>  
1. SAL: SHL과 동일 (signed)
1. SAR: SHR과 같으나 MSB에 이전의 MSB값이 복사됨
1. ROL: MSB->LSB, CF (상위비트와 하위 비트를 swap하려할 떄 사용)
1. ROR: LSB->MSB, CF
1. RCL: MSB->CF, CF->LSB (SHR 1 한 값을 복구 가능)
1. RCR: LSB->CF, CF->MSB  
1. SHLD: LSB는 src의 MSB부터 가져옴  
SHLD dst, src, cnt(src는 register 여야함)  
1. SHRD: MSB는 src의 LSB부터 가져옴  

## Shift and Rotate Applications
1. Shifting multiple Doublewords  
SHL/SHR을 가장 높은/낮은 byte에서 한 후 RCL 1,RCR 1을 byte단위로 내려/올라 가면서 진행해줌 (carry bit이용)  
1. Binary Multiplication  
2의 n승이 아니어도 2^n +* 꼴로 나타내면 shift와 +연산으로 곱셈 연산이 가능하다.
1. Displaying Binary Bits  
1비트씩 확인, 30h, 31h 출력  
1. Extracting File Date Fields  
day: 5bit, 00011111과 and  
month: 4bit, 5bit shift후 00001111 and  
year: 7bit shift 1  
 
<br>  

##  Multiplication and Division Instructions
1. MUL: reg/mem8(16, 32)에 AL(AX, EAX) reg에 저장된 값을 곱하고 AX(DX:AX, EDX:EAX)에 값이 저장된다.  
결과 값에서 하위 reg를 넘어가는 결과가 나오는 경우에 Carry, Overflow flag가 setting 된다.  
1. IMUL  
    1. Single-Operand Formats: MUL과 동일, C, OF flag가 set되지 않은 경우 하위 reg의 msb로 상위 reg가 채워져있다고 계산
    1. Tow-Operand Formats: reg16, reg/mem16/imm16(8 허용) (16->32도 가능), 계산 결과가 첫 Operand에 저장됨, 연산 후 메모리를 넘어가는 부분은 truncate됨.  
    1. Tree-Operand Forats: reg, reg/mem16, imm16(16->32 가능)  
상수 피연산자 X, 시험에 나올 수 있음, 중요!  

1. Measuring Program Execution Times  
1. DIV: reg/mem8(16, 32)  
![img](12_1.png)  
1. IDIV: ..., 나머지의 부호는 dividend와 같다.    
    - CBW, CWD, CDQ로 al, ax, eax를 보고 ah, dx, edx를 채워줌  
    - DIV -> Flag의 값 변경에 대한 정의가 따로 되어있지 않음.  
    - DIV, IDIV는 연산시 Overflow가 가능! -> 프로그램이 비정상 종료됨.  
1. Implementing Arithmetic Expressions
-> C++ 수식을 asm코드로 변환하는 예제들.  
<br>  
## Extended addition and Subtraction
***

1. ADC: carry 값까지 더해줌!  
reg, reg/mem/imm  
mem, reg/imm
1. Implementing Arithmetic Expressions: ADC를 이용하여 4, 16, 64외의 bit 연산 예제  
1. SBB: carry를 같이 빼줌!
