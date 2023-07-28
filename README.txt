# A malicious Minecraft mod

This is a Minecraft mod that can be compiled into a .jar file, dropped in the mods/ folder, and loaded into Minecraft.
Once the `onClientSetup` class is called on initialization the mod will execute the command `calc.exe`, popping the calulator app. 
Replace this command to execute arbitrary commands on the system and drop second stage payloads.


### Code
The code responsible for launching commands calls the `Runtime.getRuntime().exec()` function. 
![alt-text](images/code.png)

### Execution
[](https://i.imgur.com/lzX7git.png)
