# lean-copilot-windows
Adding Windows native compatability to Lean Copilot (https://github.com/lean-dojo/LeanCopilot).

This entailed changing some of the terminal commands that were used in the original code to be compatabile
with Windows CLI and adding cases to check for Windows OS within the code. 
Specifically:
- "wmic" -> echo %NUMBER_OF_PROCESSORS in nproc function
- "uname" -> echo %PROCESSOR_ARCHITECTURE% and also incorporates the fact that windows might output "AMD64" for x86-64 systems.
  
An inductive type for Windows was created in the type declaraton SupportedOS, and 
its use was distributed throughout the code as well, especially in checking the OS.
