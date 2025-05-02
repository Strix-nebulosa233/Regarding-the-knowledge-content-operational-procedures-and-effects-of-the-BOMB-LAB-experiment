# Regarding-the-knowledge-content-operational-procedures-and-effects-of-the-BOMB-LAB-experiment
## Student Helping Student Program: If you're struggling with coursework in computer systems or other assignments that require reverse engineering techniques, you might find this personal report and reflection I submitted helpful.
BOMB LAB is a well-known CTF (Capture The Flag) reverse engineering challenge program. There are already a large number of tutorials available online. However, most of these tutorials either simply tell you what steps to take or provide only general discussions.<br>
If you merely follow the existing tutorials, you may find it challenging to perform well in both theoretical and practical assessments within the course. Here, I will use Dr. Evil's Insidious Bomb, Version 1.1 as an example to provide a detailed guide for those who are unfamiliar with the process.<br>
Additionally, it is worth noting that the document "23331128" is the original assignment file given to me by my teacher, which is my student ID. If your teacher has also assigned you the same exact homework, please note that the answers I obtained after completing my experiment and the answers you should obtain for your homework are likely to be different. Therefore, directly copying my conclusions is not advisable.<br>
First, observe the code you have, which calls from phase_0 to phase_6(Of course, in some versions, it calls from phase_1 to phase_6, reducing one issue). After each phase succeeds, the program outputs different prompts, and the phase numbers increase sequentially and clearly. This indicates that there are seven issues waiting to be addressed. Finally, based on the hints(/* Wow, they got it!  But isn't something... missing?  Perhaps. * something they overlooked?  Mua ha ha ha ha! */), it can be guessed that this experiment has a hidden question, which can be temporarily referred to as Question 8.<br>
Next, let's go through this experiment step by step.<br>

### Analysis before the experiment
Although the BOMB file declares two files: support.h and phases.h, in practice, the actual problem may only provide an executable file named bomb, and bomb.c may be an incomplete framework file (for example, without support.h and phases.h). In such cases, there is no need to compile the source code; instead, the bomb should be disassembled through reverse engineering. So, when the compilation fails, don't panic and don't doubt that there's something wrong with the problem; it's a normal occurrence.<br>
Next, install the debugging tools first. In the terminal, run the following code to download the tool. <br>
```
sudo apt-get update
sudo apt-get install -y gdb binutils
# Install Debugging Tools
sudo apt-get install -y wget
# Optional, for downloading files
```
When setting debugging breakpoints, unexpected additional breakpoints were found(See the image below for details on "Set debugging breakpoint."). So the 'strings bomb' command was used to investigate.<br>
<div align="center">
  <img src="./Set debugging breakpoint.png" alt="Set debugging breakpoint">
  <br>
  <small>Set debugging breakpoint</small>
</div>
