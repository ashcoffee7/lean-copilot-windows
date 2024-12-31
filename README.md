# lean-copilot-windows
Adding Windows native compatability to Lean Copilot (https://github.com/lean-dojo/LeanCopilot). **Only edited the Lakefile.lean file in Lean Copilot.** I intended for setup for this file to work similarly to the instructions detailed in Lean Copilot, as there the processes used within the Lean Copilot setup instructions will work on a Windows interface as long as all dependecies are correctly installed.

Here is a more detailed version of the setup instructions:
1. Ensure that you have downloaded Git LFS and updated versions of Cmake, Lean, and C++ (**Cmake >= 3.7, Lean version of at least lean4:v4.3.0-rc2, and C++17 compatible compiler**).
  a. If you would like, or if you have a CUDA-enabled GPU, ensure you have a CUDA or cuDNN. Ensure your version of ***Lean is also compatible with other dependencies such as mathlib.
2. Open your project's lakefile.lean file and add the package configuration ```moreLinkArgs := #["-L./.lake/packages/LeanCopilot/.lake/build/lib", "-lctranslate2"]```. For example:

   ```
   package «my-package» {
     moreLinkArgs := #[
       "-L./.lake/packages/LeanCopilot/.lake/build/lib",
       "-lctranslate2"
     ]
   }
   ```
   If you have a lakefile.toml file rather than a lakefile.lean file, ensure it includes:
   ```
   moreLinkArgs = ["-L./.lake/packages/LeanCopilot/.lake/build/lib", "-lctranslate2"]
   ```
3. If you have a lakefile.lean file, add this line to your file, including the quotation marks:
   ```
   require LeanCopilot from git "https://github.com/lean-dojo/LeanCopilot.git" @ "LEAN_COPILOT_VERSION"
   ```
   If you are working with a stable Lean version, substitute the version number for LEAN_COPILOT_VERSION. If you are using an unstable Lean version, substitute "main" (without 
   quotations) for LEAN_COPILOT_VERSION.

   If you have a lakefile.toml file, add the line below.
   ```
   [[require]]  
   name = "LeanCopilot"  
   git = "https://github.com/lean-dojo/LeanCopilot.git"  
   rev = "LEAN_COPILOT_VERSION"
   ```  
5. Run ```lake update LeanCopilot```.
6. If you want to download the models from HuggingFace manually, use the links below:
   - [ct2-leandojo-lean4-tacgen-byt5-small](https://huggingface.co/kaiyuy/ct2-leandojo-lean4-tacgen-byt5-small)
   - [ct2-leandojo-lean4-retriever-byt5-small](https://huggingface.co/kaiyuy/ct2-leandojo-lean4-retriever-byt5-small)
   - [premise-embeddings-leandojo-lean4-retriever-byt5-small](https://huggingface.co/kaiyuy/premise-embeddings-leandojo-lean4-retriever-byt5-small)
   - [ct2-byt5-small](https://huggingface.co/kaiyuy/ct2-byt5-small)

   Otherwise, run ```lake exe LeanCopilot/download``` to download the built-in models from HuggingFace to ```~/.cache/lean_copilot/```.
7. Run ```lake build```.




This project entailed changing some of the terminal commands that were used in the original code to be compatabile with Windows CLI and adding cases to check for Windows OS within the code. 
Specifically:
- in ```getOS!```: added a conditional statement that returns ".windows" if Platform.isWindows
- in ```nproc```: "wmic" -> echo %NUMBER_OF_PROCESSORS in nproc function
- in ```getArch?```: "uname" -> echo %PROCESSOR_ARCHITECTURE% and also incorporates the fact that windows might output "AMD64" for x86-64 systems.
- in ```getCt2CmakeFlags```: added an option for .windows OS, to create flags for this system.
  
An inductive type for Windows was created in the type declaraton SupportedOS, and its use was distributed throughout the code as well.


