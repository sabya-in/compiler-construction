# mytoyc

Toy programming language for the compiler construction course. Original code is taken from the tutorial by L. Segal at http://gnuu.org/2009/09/18/writing-your-own-toy-compiler. Original code at github: https://github.com/lsegal/my_toy_compiler.

# Programmierpraktikum lab
1. Professor: Peter Faber

# Extending the toy compiler to enable void functions
1. Student: Sabyasachi Mondal
2. Author: Sabyasachi.mondal@stud.th-deg.de

###   Guide to implementaion ###

We first analyze exactly how the int function works it's similar to our Void except the return rule which returns void and the function type.

We first introduce changes to the tokens add two tokens "Void" and "returnvoid" . These form our lexical construct of the function.

To handle our newly introduced tokens we add two seperate grammar rules to handle the two tokens till common rules for the statements (statement will be same for both void and int).

Once the grammar rules are handled we must tell our compiler what to do once they encounter "returnvoid" or "Void" that fits our grammar.

To do just that we make changes to codegen.cpp where we introduce seperate handler function one for Void and another for returnvoid (they are exactly similar to their int counterpart except what they do). The codegen creates the nodes which are to be passed to LLVM backend.

returnvoid recieves statement nodes but simply ignores them and return a nullpointer which is of void return type.

The Void function declaration uses the same base class or Node as int, but the difference is that we create a different function type.

We can obtain a different void function type in same way as integer by using the LLVM api to get Void type (see: https://llvm.org/doxygen/classllvm_1_1Type.html#a6e20e76960d952de088354cbcd14c3ab , and also the course notes).

The format of my custom void implementaion in my compiler is :

void <Func_name>(<Integer_arguments>){\
	<block_statements>;\
	returnvoid <any_arbitrary_int_value>;\
}

##### NOTE: The compiler's lexicon is not like C we need to provide a return type with any values even for Void but it is ignored and returns void like C

All multiline changes are within opening and closing comments :  /*Sabyasachi.mondal@stud.th-deg.de*/
and single line changes have single comment (same comment) on right hand side.

See sample output for the example.txt file: https://mygit.th-deg.de/sm11312/compiler-design/-/raw/seperate-return-types/Finally_My_Compiler_Running!.png

### Instructions to Run ###

1. Clone repository from with "git clone https://mygit.th-deg.de/sm11312/compiler-design"
or Clone using "git clone --branch seperate-return-types https://mygit.th-deg.de/sm11312/compiler-design"
2. Within cloned directory open terminal
3. command: "make" or "make" after "make clean"
4. Edit example.txt file putting in the code you wish to run or use the one already present in the file
5. Next run command : cat example.txt | ./mytoyc
6. You should be able to get an output showing detailed execution steps and LLVM codes being churned out

For example check sample output below (You can also check the image inside repository for sample output) :
#### Most importantly we will notice in the final 3 lines of output the following ####
##### For 8 as input to function in default example.txt file (it conatins two function one int one void for comparison)
##### 1. Void has internal printi-function which returns 43
##### 2. Void returns nothing 0 always
##### 3. Int returns 43 and extern printi-function adds 2 to that making output as 45

________________________________________________________________
Check example.txt for a runnable format of Input file
The corresponding sample LLVM output is shown below:
________________________________________________________________
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
