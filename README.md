# Regarding-the-knowledge-content-operational-procedures-and-effects-of-the-BOMB-LAB-experiment
## Student Helping Student Program: If you're struggling with coursework in computer systems or other assignments that require reverse engineering techniques, you might find this personal report and reflection I submitted helpful.
BOMB LAB is a well-known CTF (Capture The Flag) reverse engineering challenge program. There are already a large number of tutorials available online. However, most of these tutorials either simply tell you what steps to take or provide only general discussions.<br>
If you merely follow the existing tutorials, you may find it challenging to perform well in both theoretical and practical assessments within the course. Here, I will use Dr. Evil's Insidious Bomb, Version 1.1 as an example to provide a detailed guide for those who are unfamiliar with the process.<br>
Additionally, it is worth noting that the document "23331128" is the original assignment file given to me by my teacher, which is my student ID. If your teacher has also assigned you the same exact homework, please note that the answers I obtained after completing my experiment and the answers you should obtain for your homework are likely to be different. Therefore, directly copying my conclusions is not advisable.<br>
First, observe the code you have, which calls from phase_0 to phase_6(Of course, in some versions, it calls from phase_1 to phase_6, reducing one issue). After each phase succeeds, the program outputs different prompts, and the phase numbers increase sequentially and clearly. This indicates that there are seven issues waiting to be addressed. Finally, based on the hints(/* Wow, they got it!  But isn't something... missing?  Perhaps. * something they overlooked?  Mua ha ha ha ha! */), it can be guessed that this experiment has a hidden question, which can be temporarily referred to as Question 8.<br>
Next, let's go through this experiment step by step.<br>
Before the experiment begins, place an image of Theresa here. If you feel tired during the experiment, you can look back at this image; perhaps she can bring you passion and strength. <br>
<div align="center">
  <img src="./have a break.jpg" alt="Bathing Theresa: A Vision of Serene Repose">
  <br>
  <small>Bathing Theresa: A Vision of Serene Repose</small>
</div>

### Analysis before the experiment
Since this tutorial aims to help students who haven't attended a single class get started quickly and learn how to operate, ultimately understanding the principles, I will repeatedly mention many things that you might assume don't need further explanation.<br>
Although the BOMB file declares two files: support.h and phases.h, in practice, the actual problem may only provide an executable file named bomb, and bomb.c may be an incomplete framework file (for example, without support.h and phases.h). In such cases, there is no need to compile the source code; instead, the bomb should be disassembled through reverse engineering. So, when the compilation fails, don't panic and don't doubt that there's something wrong with the problem; it's a normal occurrence.<br>
To prevent some students from being completely clueless, here's an additional tip: before debugging or performing any operations on the source code, make sure the computer knows which source code file you're working with. So, first, use the 'cd' command to navigate to the folder containing the source code file, and then proceed with other operations.<br>
Next, install the debugging tools first. In the terminal, run the following code to download the tool. <br>
```
sudo apt-get update
sudo apt-get install -y gdb binutils
# Install Debugging Tools
sudo apt-get install -y wget
# Optional, for downloading files
```
Additionally, make sure you have the necessary permissions for the file; you can use the following command to check:<br>
```
ls -l bomb
```
Next, the following command can be used to grant permissions
```
chmod +x bomb
```
Next, start using the GNU Debugger (GDB) to debug the 'bomb' file.

_When debugging the 'bomb' program using GNU Debugger (GDB), the process fundamentally involves analyzing compiled binary executable files rather than directly interacting with source code. To begin, ensure the program has been properly compiled with debugging symbols retained (typically via compiler flags like -g), as GDB relies on these symbols to map machine instructions to source-level constructs. Upon launching GDB, the debugger loads the executable's symbol table and memory layout, enabling breakpoint placement, register inspection, and stack trace analysis at the assembly or high-level language level. Critical challenges arise when debugging incomplete codebases: if the binary references unobtainable dependencies (e.g., missing header files or libraries), GDB cannot resolve symbolic information for affected components, limiting analysis to available code segments. In such cases, debugging integrity requires either acquiring/reconstructing missing dependencies or focusing exclusively on executable portions with complete symbol information.<br>_
```
gdb ./bomb
```
When setting debugging breakpoints, unexpected additional breakpoints were found(See the image below for details on "Set debugging breakpoint."). So the 'strings bomb' command was used to investigate.<br>
<div align="center">
  <img src="./Set debugging breakpoint.jpg" alt="Set debugging breakpoint">
  <br>
  <small>Set debugging breakpoint</small>
</div>
After detection, the following important information was obtained (with some deletions).<br>

```
Welcome to my fiendish little bomb. You have 7 phases with
which to blow yourself up. Have a nice day!
Well done! You seem to have warmed up!
Phase 1 defused. How about the next one?
That's number 2.  Keep going!
Halfway there!
So you got that one.  Try this one.
Good work!  On to the next...
23331128
rI5aKdLsFXC49WeoGLmfXjdz3pLtpcIH7KQthE2N4l0IWrDboJZREuUMssagIvDRHHiNSw39g5Ym
Bell Labs used the a.out format.
%d %d
Wow! You've defused the secret stage!
So you think you can stop the bomb with ctrl-c, do you?
Well...
OK. :-)
Invalid phase%s
%d %d %d %d %d %d
Error: Premature EOF on stdin
GRADE_BOMB
Error: Input line too long
BOOM!!!
The bomb has blown up.
%d %d %s
CbSbho
Curses, you've found the secret phase!
But finding it and solving it are quite different...
Congratulations! You've defused the bomb!
```

Obviously, by observing these, this problem does have hidden questions beyond the 0-6 questions.<br>

### The general solution method for this type of problem.
The core of such issues lies in controlling the execution flow by deploying breakpoints at critical verification points in the program, combined with static disassembly and dynamic debugging techniques to deeply analyze program behavior. Specifically, breakpoints are first set at critical nodes such as the entry of input validation functions and before string comparison operations. Then, using the debugger's disassembly function, the machine code and corresponding assembly instructions at the current execution position are obtained, with a focus on immediate operands in comparison instructions, memory address references, and conditional jump logic. During this process, monitoring the state of registers and examining memory data during dynamic execution allows capturing the specific numerical or string characteristics expected by the program when validating valid inputs. For example, using the `info registers` command to track the values of registers used in comparison instructions, or using the `x/wx` command to check preset validation values stored in memory. The essence of this analytical method lies in interrupting the execution flow in a controlled manner and sampling the state to reverse-engineer the program's input validation rules. In practical scenarios, the expected answer may exist in the form of hard-coded values in the immediate operands of comparison instructions, be stored at specific offset addresses in the data segment, or even be dynamically generated through encryption algorithms.<br>
This lengthy content may seem tedious, but it's the most concise and clear expression I can imagine. In brief, it involves: 1. Analyzing the assembly language content to infer the program's intention. 2. Setting reasonable breakpoints to pause the program's response. 3. Analyzing the software, hardware, and memory at this time to obtain the content we need. The second and third points complement each other, with the breakpoint setting in the second point serving the purpose of the third point.<br>
### Phase_0
