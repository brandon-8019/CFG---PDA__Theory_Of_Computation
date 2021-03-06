# DFA-CFG-PDA

## 1) grammar_2_pda.py
> A program written in **python** to convert a **Context Free Grammar** to a **Push Down Automata**.
#### Input Pattern ( from some 'file.txt' in 'cfg_sample_files' folder )
```
S:aSa|bA
A:aS|bB
B:bB|b
```
> S - Start State <br>
> A, B - Non-Terminals <br>
> a, b - Terminals <br>
#### Steps to Solve
> Read line by line from the input file and do the following steps for each line - 
* Separate the left and right-hand side.
* Isolate all the productions on the right-hand side.
* Now, say the l.h.s is X and r.h.s. productions are y1, y2,..., yn, then, the corresponding transition function will be - 
```
δ(q0, ε, X) -> {(q0, y1), (q0, y2), ..., (q0, yn)}
```
* In each production, there may be **Terminal** variables, detecting them will not be difficult, because they are written in **lower-case alphabets**, where the Non-terminals are not.
* After all the conversions are over, one more transition function will be created for each Terminal **a**, in the form - 
```
δ(q0, a, a) -> ε
```
* Productions have been stored in a **Python Dictionary** as a key-value pair, where key is l.h.s. of the production and the value contains all the r.h.s productions.
* Converted **pda** will be stored in a text file in **After_cfg_to_pda_sample_files** folder, from where it can be retrieved for further use.
#### Output
```
python3 grammar_2_pda.py
```
> In the above example, a and b are terminals. So, the converted **pda** will look like - 
```
δ(q,ε,S) -> {(q,aAA)}
δ(q,ε,A) -> {(q,aS), (q,bS), (q,a)}
δ(q,a,a) -> {(q,ε)}
δ(q,b,b) -> {(q,ε)}
```
> <hr>
## 2) pda_2_grammar.py
> A program written in **python** to convert a **Push Down Automata** to a **Context Free Grammar**.
#### Input Pattern ( from some 'file.txt' in 'pda_sample_files' folder )
```
q0,q1
Z,X
q0,Z
δ(q0,1,Z)=(q0,XZ)
δ(q0,1,X)=(q0,XX)
δ(q0,0,X)=(q1,X)
δ(q0,ε,Z)=(q0,ε)
δ(q1,1,X)=(q1,ε)
δ(q1,0,Z)=(q0,Z)
```
> q0, q1 - Valid States <br>
> Z, X - Stack Symbols <br>
> Z - Stack Base Symbol <br>
> q0 - Start State <br>
> 0, 1 - Terminals <br>
#### Points to Note
* If there are **k** states and **m** stack symbols, then the corresponding **CFG** will have **mk<sup>2</sup>+1** states including **S**, the Start State of the CFG. Rest of the states will look like - 
```
[q Y q'], where q,q' - Combination of all valid States, and Y - all the Stack Symbols.
```
* And, all the **mk<sup>2</sup>** states will be mapped into **unique** **Non-Terminals** for the corresponding CFG.
#### Steps to Solve
* Extract the Valid States, Stack Symbols, and Terminal variables from the first three lines.
> Next, for the rest of the lines ( **Transition Function** ), perform the following steps - 
* If it is a **non-erasing** production ( producing something except **epsilon** ), in the form - 
```
δ(q',a,Y) = (q",X1X2X3...Xn), where q', q" are states; a is Terminal; and Y,X1,X2,...,Xn are Stack-symbols.
```
* Then it will lead into a number of productions, in the following pattern - 
```
[q' Y q] -> a[q" X1 q1][q1 X2 q2][q2 X3 q3]...[qn Xn q]; where q,q1,q2,...,qn can be any valid states.
```
> For this, we might have to create a **n-nested for-loop ( by recursion )** to generate all the productions.
* If the production is a **erasing** one, i.e., in the form -
```
δ(q',a,Y)=(q",ε)
```
* Then, it will lead to the following production - 
```
[q' Y q"] -> a
```
* Now, there will be Start State Production for the CFG, in the form - 
```
S -> [q0 Z q]; where q0 - Start State of the pda, Z - Base Stack-symbol, and q - any Valid State of the pda.
```
* To store all these productions, **Python Dictionary ( Key-value pair )** has been used.
* After generating all possible productions, all the states enclosed in **[ ]** brackets will be mapped into there **unique non-terminal symbols**.
* Next and final job is to cluster those productions in a well-defined form. Productions generated from same state will be together (will have same key in the dictionary).
### Output
```
python3 pda_2_grammar.py
```
> In the above input, **2x2<sup>2</sup>+1 = 9** states will be there; and say they are mapped into **A, B, C,..., H, and S**. Then the output will be - 
```
S:A|B
A:1CA|1DE|ε
B:1CB|1DF
C:1CC
D:1CD|1DH|0H
E:0A
F:0B
H:1
```
> Converted **CFG** will stored in a text file in **After_pda_to_cfg_sample_files** folder, from where it can be restored for further use.
## 3) dfa_2_mindfa.cpp
> A program written in **c++** to convert a **DFA** to a **Min-State DFA**.
```
Algorithm used : Table-Filling Algorithm
```
#### Steps to Solve
* Create a table for all pairs of states (Qi, Qj). For this a matrix of size (NxN) is created, where N is number of states. Initially all are marked false.
* Consider every state pair **(Qi,Qj)**, where **Qi ∈ F** and **Qj ∉ F** ( F : Final State ) and mark them as true **( Distinguishable )** .
* If there is an unmarked pair (Qi, Qj), mark it if the pair {δ (Qi, A), δ (Qj, A)} is marked for some input alphabet and keep repeating this process until anymore states can't be marked.
* After that, combine all the unmarked pairs (Qi,Qj) and treat them as indistinguishable. They are treated as a single state in the reduced DFA.
> when (Qi,Qj) is indistinguishable and (Qj,Qk), (Qk,Qj) are also indistinguishable; then, a single state consisting of (Qi,Qj,Qk) is formed in the minimal DFA.
* For all distinguishable states, we allocate a state in the minimal DFA and finally display it.
