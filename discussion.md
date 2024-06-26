# Discussion and Reflection


The bulk of this project consists of a collection of five
questions. You are to answer these questions after spending some
amount of time looking over the code together to gather answers for
your questions. Try to seriously dig into the project together--it is
of course understood that you may not grasp every detail, but put
forth serious effort to spend several hours reading and discussing the
code, along with anything you have taken from it.

Questions will largely be graded on completion and maturity, but the
instructors do reserve the right to take off for technical
inaccuracies (i.e., if you say something factually wrong).

Each of these (six, five main followed by last) questions should take
roughly at least a paragraph or two. Try to aim for between 1-500
words per question. You may divide up the work, but each of you should
collectively read and agree to each other's answers.

[ Question 1 ] 

For this task, you will three new .irv programs. These are
`ir-virtual?` programs in a pseudo-assembly format. Try to compile the
program. Here, you should briefly explain the purpose of ir-virtual,
especially how it is different than x86: what are the pros and cons of
using ir-virtual as a representation? You can get the compiler to to
compile ir-virtual files like so: 

racket compiler.rkt -v test-programs/sum1.irv 

(Also pass in -m for Mac)

add.irv
double.irv
sub.irv

The purpose of ir-virtual is to give a brief view of how the programs look like before assembly.
Pros of using it is that it is easy to understand.
Cons of using it is that it is it can not deal with large amount of programming.

[ Question 2 ] 

For this task, you will write three new .ifa programs. Your programs
must be correct, in the sense that they are valid. There are a set of
starter programs in the test-programs directory now. Your job is to
create three new `.ifa` programs and compile and run each of them. It
is very much possible that the compiler will be broken: part of your
exercise is identifying if you can find any possible bugs in the
compiler.

For each of your exercises, write here what the input and output was
from the compiler. Read through each of the phases, by passing in the
`-v` flag to the compiler. For at least one of the programs, explain
carefully the relevance of each of the intermediate representations.

For this question, please add your `.ifa` programs either (a) here or
(b) to the repo and write where they are in this file.

For the first.ifa program, if first uses 3 let to store value to three different variables.
Then it use it to compute the final answer to x1258 then return it at last.
First.ifa:
Input:(* 3 (+ 2 5))
Output:
Input source tree in IfArith:
'(* 3 (+ 2 5))
ifarith-tiny:
'(* 3 (+ 2 5))
'(* 3 (+ 2 5))
3
'(+ 2 5)
2
5
anf:
'(let ((x1254 3))
   (let ((x1255 2))
     (let ((x1256 5))
       (let ((x1257 (+ x1255 x1256))) (let ((x1258 (* x1254 x1257))) x1258)))))
ir-virtual:
'(((label lab1259) (mov-lit x1254 3))
  ((label lab1260) (mov-lit x1255 2))
  ((label lab1261) (mov-lit x1256 5))
  ((label lab1262) (mov-reg x1257 x1255))
  (add x1257 x1256)
  ((label lab1263) (mov-reg x1258 x1254))
  (imul x1258 x1257)
  (return x1258))
x86:
section .data
	int_format db "%ld",10,0


	global _main
	extern _printf
section .text


_start:	call _main
	mov rax, 60
	xor rdi, rdi
	syscall


_main:	push rbp
	mov rbp, rsp
	sub rsp, 80
	mov esi, 3
	mov [rbp-40], esi
	mov esi, 2
	mov [rbp-32], esi
	mov esi, 5
	mov [rbp-24], esi
	mov esi, [rbp-32]
	mov [rbp-16], esi
	mov edi, [rbp-24]
	mov eax, [rbp-16]
	add eax, edi
	mov [rbp-16], eax
	mov esi, [rbp-40]
	mov [rbp-8], esi
	mov edi, [rbp-16]
	mov eax, [rbp-8]
	imul eax, edi
	mov [rbp-8], eax
	mov rax, [rbp-8]
	jmp finish_up
finish_up:	add rsp, 80
	leave 
	ret 

Second.ifa:
Input:16
Output:
Input source tree in IfArith:
16
ifarith-tiny:
16
16
anf:
'(let ((x1254 16)) x1254)
ir-virtual:
'(((label lab1255) (mov-lit x1254 16)) (return x1254))
x86:
section .data
	int_format db "%ld",10,0


	global _main
	extern _printf
section .text


_start:	call _main
	mov rax, 60
	xor rdi, rdi
	syscall


_main:	push rbp
	mov rbp, rsp
	sub rsp, 16
	mov esi, 16
	mov [rbp-8], esi
	mov rax, [rbp-8]
	jmp finish_up
finish_up:	add rsp, 16
	leave 
	ret 

Thirt.ifa:
Input:((let* ([a 3] [b 4]))(* a b))
Output:
Input source tree in IfArith:
'(if 1 (print (+ 1 2)) (print (+ 3 4)))
ifarith-tiny:
'(if 1 (print (+ 1 2)) (print (+ 3 4)))
'(if 1 (print (+ 1 2)) (print (+ 3 4)))
1
'(print (+ 1 2))
'(+ 1 2)
1
2
'(print (+ 3 4))
'(+ 3 4)
3
4
anf:
'(let ((x1254 1))
   (if x1254
     (let ((x1255 1))
       (let ((x1256 2)) (let ((x1257 (+ x1255 x1256))) (print x1257))))
     (let ((x1258 3))
       (let ((x1259 4)) (let ((x1260 (+ x1258 x1259))) (print x1260))))))
ir-virtual:
'(((label lab1261) (mov-lit x1254 1))
  ((label lab1262) (mov-lit zero1271 0))
  (cmp x1254 zero1271)
  (jz lab1263)
  (jmp lab1267)
  ((label lab1263) (mov-lit x1255 1))
  ((label lab1264) (mov-lit x1256 2))
  ((label lab1265) (mov-reg x1257 x1255))
  (add x1257 x1256)
  ((label lab1266) (print x1257))
  (return 0)
  ((label lab1267) (mov-lit x1258 3))
  ((label lab1268) (mov-lit x1259 4))
  ((label lab1269) (mov-reg x1260 x1258))
  (add x1260 x1259)
  ((label lab1270) (print x1260))
  (return 0))
x86:
section .data
	int_format db "%ld",10,0


	global _main
	extern _printf
section .text


_start:	call _main
	mov rax, 60
	xor rdi, rdi
	syscall


_main:	push rbp
	mov rbp, rsp
	sub rsp, 160
	mov esi, 1
	mov [rbp-48], esi
	mov esi, 0
	mov [rbp-8], esi
	mov edi, [rbp-8]
	mov eax, [rbp-48]
	cmp eax, edi
	mov [rbp-48], eax
	jz lab1263
	jmp lab1267
lab1263:	mov esi, 1
	mov [rbp-16], esi
	mov esi, 2
	mov [rbp-40], esi
	mov esi, [rbp-16]
	mov [rbp-32], esi
	mov edi, [rbp-40]
	mov eax, [rbp-32]
	add eax, edi
	mov [rbp-32], eax
	mov esi, [rbp-32]
	lea rdi, [rel int_format]
	mov eax, 0
	call _printf
	mov rax, 0
	jmp finish_up
lab1267:	mov esi, 3
	mov [rbp-24], esi
	mov esi, 4
	mov [rbp-80], esi
	mov esi, [rbp-24]
	mov [rbp-72], esi
	mov edi, [rbp-80]
	mov eax, [rbp-72]
	add eax, edi
	mov [rbp-72], eax
	mov esi, [rbp-72]
	lea rdi, [rel int_format]
	mov eax, 0
	call _printf
	mov rax, 0
	jmp finish_up
finish_up:	add rsp, 160
	leave 
	ret 

[ Question 3 ] 

Describe each of the passes of the compiler in a slight degree of
detail, using specific examples to discuss what each pass does. The
compiler is designed in series of layers, with each higher-level IR
desugaring to a lower-level IR until ultimately arriving at x86-64
assembler. Do you think there are any redundant passes? Do you think
there could be more?

In answering this question, you must use specific examples that you
got from running the compiler and generating an output.

I think there is no redundant passes. Every stage has its own use in practical.
The reason why it may look redundant is that we are not really passing big programs
into it. We are just passing very simple functions into it. So eavey step looks 
almost the same.

[ Question 4 ] 

This is a larger project, compared to our previous projects. This
project uses a large combination of idioms: tail recursion, folds,
etc.. Discuss a few programming idioms that you can identify in the
project that we discussed in class this semester. There is no specific
definition of what an idiom is: think carefully about whether you see
any pattern in this code that resonates with you from earlier in the
semester.

After reading thorugh the project, I think pattern matching is one of the 
most used idioms throught out the semester. Like we always did in previous 
project, we solve it cases by case.

[ Question 5 ] 

In this question, you will play the role of bug finder. I would like
you to be creative, adversarial, and exploratory. Spend an hour or two
looking throughout the code and try to break it. Try to see if you can
identify a buggy program: a program that should work, but does
not. This could either be that the compiler crashes, or it could be
that it produces code which will not assemble. Last, even if the code
assembles and links, its behavior could be incorrect.

To answer this question, I want you to summarize your discussion,
experiences, and findings by adversarily breaking the compiler. If
there is something you think should work (but does not), feel free to
ask me.

Your team will receive a small bonus for being the first team to
report a unique bug (unique determined by me).

Input source tree in IfArith:
'((let* ((a 3) (b 4))) (* a b))
match: no matching clause for '((let* ((a 3) (b 4))) (* a b))

[ High Level Reflection ] 

In roughly 100-500 words, write a summary of your findings in working
on this project: what did you learn, what did you find interesting,
what did you find challenging? As you progress in your career, it will
be increasingly important to have technical conversations about the
nuts and bolts of code, try to use this experience as a way to think
about how you would approach doing group code critique. What would you
do differently next time, what did you learn?

I think every time we learn a new compiler, it is really important that we
do not use the stereotype in our mind. We should learn like a baby, like we
know nothing. We should have a whole new understanding to different language.
I think this is also important when we start working in companies. We should 
fully understant how things work before diving into them. It can really save
us a lot of time on doing meaning less work.

