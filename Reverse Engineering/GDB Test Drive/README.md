## Name: GDB Test Drive
#### Points: 100
#### Description: Can you get the flag? Download this binary. Here's the test drive instructions
#### Files: `gdme binary`

As the name suggests, this is a "challenge" to check that we are able to use the gdb debugger. The description also gives us the following
commands to use:

* chmod +x gdbme - this will make the binary executable
* gdb gdbme - this will start the debugger on the newly created debugger
* layout asm - We specify `asm` to view command window, in addition to assembly window
* break * (main+99) - Set breakpoint at given point. The `*` means that we are specifying an address.
* run - Run the executable until we hit the set breakpoint (main+99)

At this point, we have setup the binary to be an executable, and launched the file in gdb, where we inspect and run the file at a given breakpoint.
At the `main+99` breakpoint, we can see a function call to `sleep@plt` (which technically isn't the real sleep function, but this is far beyond the scope of 
this task to discuss)

The final command given in the description is:

* jump *(main+104) - Means that we resume execution from a specified location

Following these commands, the flag is printed in the command window 🚩:

<details>
  <summary>Flag [SPOILER]</summary>
  
  ```console
  
  Breakpoint 1, 0x000055555555532a in main ()
  (gdb) jump *(main+104)
  Continuing at 0x55555555532f.
  picoCTF{d3bugg3r_dr1v3_93b87433}
  (gdb) ior 1 (process 672) exited normally]
  
  ```
  
</details>
