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
Next, start using the GNU Debugger (GDB) to debug the 'bomb' file.
<span style="color: #6a737d; font-style: italic; font-size: 0.9em">
When debugging the 'bomb' program using GNU Debugger (GDB), the process fundamentally involves analyzing compiled binary executable files rather than directly interacting with source code. To begin, ensure the program has been properly compiled with debugging symbols retained (typically via compiler flags like -g), as GDB relies on these symbols to map machine instructions to source-level constructs. Upon launching GDB, the debugger loads the executable's symbol table and memory layout, enabling breakpoint placement, register inspection, and stack trace analysis at the assembly or high-level language level. Critical challenges arise when debugging incomplete codebases: if the binary references unobtainable dependencies (e.g., missing header files or libraries), GDB cannot resolve symbolic information for affected components, limiting analysis to available code segments. In such cases, debugging integrity requires either acquiring/reconstructing missing dependencies or focusing exclusively on executable portions with complete symbol information.<br>
</span>

When setting debugging breakpoints, unexpected additional breakpoints were found(See the image below for details on "Set debugging breakpoint."). So the 'strings bomb' command was used to investigate.<br>
<div align="center">
  <img src="./Set debugging breakpoint.jpg" alt="Set debugging breakpoint">
  <br>
  <small>Set debugging breakpoint</small>
</div>
