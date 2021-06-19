# mytoyc

Toy programming language for the compiler construction course. Original code is taken from the tutorial by L. Segal at http://gnuu.org/2009/09/18/writing-your-own-toy-compiler. Original code at github: https://github.com/lsegal/my_toy_compiler.

# Programmierpraktikum lab
# Extending the toy compiler to enable void functions
1. Prof: Peter Faber
2. Stud: Sabyasachi Mondal
3. Auth: Sabyasachi.mondal@stud.th-deg.de

###   Guide to implementaion nad running ###

A separate data type void is implemented

The format of my custom void implementaion in my compiler is :

void <Func_name>(<Integer_arguments>){\
	<block_statements>;\
	returnvoid <any_arbitrary_int_value>;\
}

All multiline changes are within opening and closing comments :  /(*)Sabyasachi.mondal@stud.th-deg.de(*)/ 
and single line changes have single comment (same comment) on right hand side.

### Instructions to Run ###

1. Clone repository from : 
2. Within cloned directory open terminal
3. command: "make" or "make" after "make clean"
4. Edit example.txt file putting in the code you wish to run or use the one already present in the file
5. Next run command : cat example.txt | ./mytoyc
6. You should be able to get an output showing detailed execution steps and LLVM codes being churned out

For example check sample output below (You can also check the image inside repository for sample output) :
##### Most importantly we will notice in the final 3 lines of output the following
For 8 as input in default example.txt file
1. Void has internal printi which returns 43
2. Void returns nothing 0 always
3. Int returns 43 and extern printi adds 2 to that making output as 45

_______________________________________________________
Check example.txt for a runnable format of Input file
The corresponding LLVM output is shown below:
_______________________________________________________
sm11312@e214pc76:~/git/mytoyc-00$ cat example.txt | ./mytoyc
Generating code...\
Creating block\
Generating code for 18NExternDeclaration\
Generating code for 24NFunctionDeclarationVoid\
Creating function: do_math_void\
Creating variable declaration a\
Creating block\
Generating code for 20NVariableDeclaration\
Creating variable declaration x\
Creating assignment for x\
Creating comparison operation 2\
Creating integer: 5\
Creating identifier reference: a\
Generating code for 20NExpressionStatement\
Generating code for PK11NExpression\
Creating comparison operation 0\
Creating integer: 3\
Creating identifier reference: x\
Creating method call: printi\
Generating code for 20NReturnStatementVoid\
Generating return code for PK11NExpression\
Creating comparison operation 0\
Creating integer: 3\
Creating identifier reference: x\
Generating code for 20NFunctionDeclaration\
Creating function: do_math_int\
Creating variable declaration a\
Creating block\
Generating code for 20NVariableDeclaration\
Creating variable declaration x\
Creating assignment for x\
Creating comparison operation 2\
Creating integer: 5\
Creating identifier reference: a\
Generating code for 16NReturnStatement\
Generating return code for PK11NExpression\
Creating comparison operation 0\
Creating integer: 3\
Creating identifier reference: x\
Generating code for 20NExpressionStatement\
Generating code for PK11NExpression\
Creating comparison operation 0\
Creating integer: 2\
Creating integer: 8\
Creating method call: do_math_void\
Creating method call: printi\
Generating code for 20NExpressionStatement\
Generating code for PK11NExpression\
Creating comparison operation 0\
Creating integer: 2\
Creating integer: 8\
Creating method call: do_math_int\
Creating method call: printi\
Code is generated.\
; ModuleID = 'main'\
source_filename = "main"\

define i32 @main() {\
entry:\
  call void @do_math_void(i32 8)\
  add void <badref>, i32 2\
  %0 = call i32 @printi(void <badref>)\
  %1 = call i32 @do_math_int(i32 8)\
  %2 = add i32 %1, 2\
  %3 = call i32 @printi(i32 %2)\
  ret void\
}

declare i32 @printi(i32)

define internal void @do_math_void(i32 %a1) {\
entry:\
  %a = alloca i32\
  store i32 %a1, i32* %a\
  %x = alloca i32\
  %0 = load i32, i32* %a\
  %1 = mul i32 %0, 5\
  store i32 %1, i32* %x\
  %2 = load i32, i32* %x\
  %3 = add i32 %2, 3\
  %4 = call i32 @printi(i32 %3)\
  %5 = load i32, i32* %x\
  %6 = add i32 %5, 3\
  ret void\
}

define internal i32 @do_math_int(i32 %a1) {\
entry:\
  %a = alloca i32\
  store i32 %a1, i32* %a\
  %x = alloca i32\
  %0 = load i32, i32* %a\
  %1 = mul i32 %0, 5\
  store i32 %1, i32* %x\
  %2 = load i32, i32* %x\
  %3 = add i32 %2, 3\
  ret i32 %3\
}\
Running code...\
43\
0\
45\
Code was run.\
sm11312@e214pc76:~/git/mytoyc-00$ 
