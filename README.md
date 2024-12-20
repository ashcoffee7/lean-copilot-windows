# lean-copilot-windows
Adding Windows native compatability to Lean Copilot (https://github.com/lean-dojo/LeanCopilot).

This entailed changing some of the terminal commands that were used in the original code to be compatabile
with Windows CLI (e.g. wmic to uname) and adding cases to check for Windows OS within the code. 
An inductive type for Windows was created in the type declaratoin SupportedOS, and 
its use was distributed throughout the code as well, especially in checking the OS.
