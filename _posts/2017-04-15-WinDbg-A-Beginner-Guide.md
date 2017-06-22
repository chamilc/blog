### WinDbg : A Windows Debugging Tool, Beginner Guide 

![debugging][img1]
Debugging a bug is always challenging and interesting, but the above comic is way too much
to reproduce the bug. I know, your are laughing, all credits goes to awesome [XKCD][link5].
OK, get back to serious business. 

[Windbg][link1] is an awesome, multipurpose debugger for [Microsoft Windows][link2],
 distributed on the web by Microsoft. It is very useful tool to debug user applications,
 drivers and third party software modules. It is always handy whenever there is a need to
 analyse field issues reported by customer. In ordinary cases, we just request the customer
 to send the mini dump files (*.dmp) files generated during the software crash and 
 analyse it with matching binaries to find the root of the problem. 
 
 Following guidelines will give a guide to jump start with windbg tool. 
 
Step 1: First [download][link3] the latest Windbg tool from Microsoft's distribution.  

Step 2: Download the DotNet/MFC/VisualC++ symbol files from [SymSrv][link4]. These symbol files are important for kernel debugging as well as user mode application debugging.		 

Step 3: Get the Symbol (Private and Public) file for target application  

>Start using the WinDbg Tool.

>1. Open the WinDbg 
>2. Open the Crash Dump file generated during the software crash.
>3. Add Symbol Paths
	*	***.Sympath+ D:\\MS\\Symbols***  		-- Symbol file path for Microsoft windows and DotNet
	*	***.Sympath+ D:\\MyApp\\Symbols\\***     -- Symbol file path for target application
>
> 4. Load Symbols
	* ***!sym noisy***           - adds extra logging to debugger window
	* ***.reload /f***          - reloads the symbol files again
	* ***lm***                  - list out the loaded modules 
	
> 5. Analyse the Crash Dump contents
	* ***!analyze -v***          - do the details analysis of crash dump produce crash report 
	                       with details.
	                       
if the !analyse output is not enough, still can do more deeper level debugging, I found
following list commands are very useful. 

>*  ***!Analyze -v***  - get details debug information

>* ***dv***           - get the local variable information, private symbols needed

>* ***dt varName***   - get details of local a local variable 

>* ***k***     -  *** Stack trace for last set context - .thread/.cxr resets it

>* ****~Kn***  - List all the stacks for all the threads

> * ***kb***    - List stack of current thread

> * ***kn***    - List the stack of current thread

> * ***.frame frameNo*** - Specify current frame #

> * ***.f+***  - set current stack frame to caller of current frame

> * ***.f-***  - set current stack frame to callee of current frame

> * ***kp***   - display all parameters to a function call

> * ***kl***   - hide source line information good when logging a stack trace in case
				 notes

> * ***kb***   - display first three possible parameters on the stack

> * ***kp***   - display all parameters to a function call

> * ***kv***  - display calling convention used

> * ***n***    - n suffix that outputs frame of reference numbers ( ex. kpn )

> * ***~*k***  - Dump stacks for all threads

> * ***dd esp*** - display memory x86-architecture

> * ***dd***     - display memory x86 or x64 architecture
 
> * ***r***      - Dump All the registers
 
> * ***!uniqstack*** - Display all the threads

> * ***~***          - Display all threads

> * ***\~Ns***        - Set current thread

> * ***\!runaway 3*** - Display thread time

> * ***q*** 		 - quit the debugging sessions

> * ***.help***      - get help message about debugger commands

> * ***.hh command***      - route to debugger documentation for details information

> * ***.ecxr***            - switch to current thread context

Hope, this guide might have helped you to get start with windbg quickly. Please provide
your feedback if it was useful.  

[link1]: http://msdn.microsoft.com/library/windows/hardware/ff551063%28v=vs.85%29.aspx
[link2]: http://windows.microsoft.com/en-us/windows/home
[link3]: http://msdn.microsoft.com/en-sg/windows/hardware/hh852365.aspx
[link4]: http://msdn.microsoft.com/en-us/library/windows/hardware/ff558847%28v=vs.85%29.aspx
[img1]:  /img/debugging.png "http://xkcd.com/583/"
[link5]: http://xkcd.com/583/

