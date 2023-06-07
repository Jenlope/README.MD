# README.MD
Jennifer Lopez, Jathaniel Dischinger
An LC-3 program that displays the minimum, maximum and average grade of 5 test scores and display the letter grade associated with the test scores.
.ORIG x3000 

LEA R0, PROMPT 
TRAP x22 
LEA R1, SCORES 
LD R2, COUNT 
READ_LOOP 
TRAP x23 
STR R0, 0(R1) 
ADD R1, R1, #1 
ADD R2, R2, #-1
BRp READ_LOOP 
LEA R1, SCORES 
LD R2, COUNT 
JSR MIN_MAX_AVG 

LEA R0, RESULTS 
TRAP x22 
LD R0, MIN 
TRAP x21 
LD R0, MAX 
TRAP x21 
LD R0, AVG 
TRAP x21 

TRAP x25 

MIN_MAX_AVG
ST R7, SAVE_R7 
LD R3, COUNT 
LD R4, 0(R1)
ST R4, MIN 
ST R4, MAX 
AND R5, R5, #0 

CALC_LOOP
ADD R5, R5, R4 
ADD R1, R1, #1 
LD R4, 0(R1) 
ADD R3, R3, #-1 
BRz DONE_CALC 
CMP R4, MIN 
BRn UPDATE_MIN 

BRp UPDATE_MAX 
BRnzp CALC_LOOP 
UPDATE_MIN
ST R4, MIN 
BRnzp CALC_LOOP 
UPDATE_MAX
ST R4, MAX 
BRnzp CALC_LOOP 
DONE_CALC
LD R3, COUNT 
DIV R5, R5, R3 
ST R5, AVG 
LD R7, SAVE_R7 
RET 
PROMPT .STRINGZ "Enter 5 test scores: " 
RESULTS .STRINGZ "Min: %d, Max: %d, Avg: %d\n" 
SCORES #5 
COUNT .FILL #5 
MIN .FILL #0 
MAX .FILL #0 
AVG .FILL #0 
SAVE_R7 .FILL #0 
.END
