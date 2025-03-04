# Deploying Velociraptor client on windows and Linux

In this tutorial, you will learn how to install Velociraptor Client on Linux and Windows Systems. Velociraptor endpoint agents are called clients. Clients connect to the server and wait for instructions, which mostly consist of VQL statements, then run any VQL queries and return the result to the server.


## Deploy Velociraptor client on Linux Systems
Velociraptor binary used for Server and Client is the same, the usage is differentiated by config options.

- Step 1: Get velociraptor binary on client machine
On the target Linux Velociraptor client system, create a directory where to store the binary.

```
mkdir velociraptor
```

Navigate to the binary directory created above and download the Velociraptor binary for Linux systems.
```
cd velociraptor
curl -L -o velociraptor https://github.com/Velocidex/velociraptor/releases/download/v0.74/velociraptor-v0.74.0-rc1-linux-amd64
```
or use any newer version from [velociraptor releases](https://github.com/Velocidex/velociraptor/releases)

Make the Binary executable:

```
chmod +x velociraptor
```

- Step 2: Copy the Velociraptor client configuration file from the server to client
Login to the Velociraptor server and you will see the client configuration file.

![image](https://github.com/user-attachments/assets/810ab593-21ae-4906-ba68-6bc870aeff0d)


Once you have the configuration file, copy it to the respective client system. [you can copy it manually or wit `scp`]

- Step 3: Start the Velociraptor client

To start the Velociraptor client in standalone mode using the client configuration file generated, run the command below

```
 ./velociraptor --config client.config.yaml client -v
```

<img width="854" alt="VirtualBoxVM_GnOOc8U33p" src="https://github.com/user-attachments/assets/ff0946b0-fad0-4619-a506-c37bee395309" />



From the output above, we can see that the client is enrolled to the Velociraptor server.

- Step 4 (Optional): Install systemd Service
Additionally you can create systemd service file for Velociraptor client:

```
vim  /lib/systemd/system/velociraptor.service
```

Add the content below (**edit ExecStart file paths with regards to your files location**):

```
[Unit]
Description=Velociraptor linux amd64
After=syslog.target network.target

[Service]
Type=simple
Restart=always
RestartSec=120
LimitNOFILE=20000
Environment=LANG=en_US.UTF-8
ExecStart=/usr/local/bin/velociraptor --config /etc/velociraptor-client.config.yaml client -v

[Install]
WantedBy=multi-user.target
```

Reload systemd daemon:

```
systemctl daemon-reload
```

Start and enable velociraptor to start at boot time:
```
systemctl enable --now velociraptor 
```

Check the status of velociraptor.

```
systemctl status velociraptor
```
 
- Step 5: Confirm Client is Added on GUI
On the server GUI, navigate to homepage and select SHOW ALL next to the magnifying Glass to view connected clients:

<img width="958" alt="VirtualBoxVM_d8RvZBbTFy" src="https://github.com/user-attachments/assets/28f10b80-3e5c-4677-8df5-b144c0af0958" />


-----------------------------------------------------------------------------


## Deploy Velociraptor client on Windows Systems

- Step 1: Create Install Folder
Create Velociraptor folder on target client system in the path specified below:

```
mkdir "C:\Program Files\Velociraptor\"
cd "C:\Program files\Velociraptor"
```
**this path is important - because the MSI file that will be described below is configured [by velociraptor developers] to use this path!!!**

- Step 2: Download Velociraptor Client Windows Installer
Download the latest installer from [velociraptor releases](https://github.com/Velocidex/velociraptor/releases) page and save it in the folder created above.

```
wget https://github.com/Velocidex/velociraptor/releases/download/v0.74/velociraptor-v0.74.0-rc1-windows-amd64.exe
```
- Step 3: Copy Velociraptor Client Configuration file to Install folder
Copy client configuration file generated from the server [like in the linux steps] to the client install folder created above.

- Step 4: Download the velociraptor MSI and repack it

Download the msi file from [velociraptor releases](https://github.com/Velocidex/velociraptor/releases) page and save it in the folder created above.

```
wget https://github.com/Velocidex/velociraptor/releases/download/v0.74/velociraptor-v0.74.0-rc1-windows-amd64.msi
```

Run the below command to embed the client’s configuration in the windows binary

```
.\velociraptor-v0.74.0-rc1-windows-amd64.exe config repack --msi velociraptor-v0.74.0-rc1-windows-amd64.msi .\client.root.config.yaml velociraptor-windows-amd64.msi

```

<img width="960" alt="WindowsTerminal_YY7qkNjVhL" src="https://github.com/user-attachments/assets/14a2c9d2-23b7-4b21-b526-0a4e7a943382" />


[make sure that you running the command according your velociraptor version]

- Step 5: Connect the client to the server

Copy the repackaged Velociraptor client to target clients machine. Launch CMD as an administrator, and run the repacked msi to install the service:

```
.\velociraptor-windows-amd64.msi service install 
```

<img width="960" alt="WindowsTerminal_7JEtQGhzgr" src="https://github.com/user-attachments/assets/948433ee-5570-4ed2-81dc-146782529350" />

Chck if the server is installed:

![image](https://github.com/user-attachments/assets/cbaf48ca-2868-410b-ab74-55200ccf9ac1)

![image](https://github.com/user-attachments/assets/f1ccfb19-11d5-401a-b4f0-662be576bddc)


Than run the command to connect the client to the server:

```
.\velociraptor-v0.74.0-rc1-windows-amd64.exe --config .\client.root.config.yaml client -v
```

<img width="865" alt="WindowsTerminal_hXE4OtdSks" src="https://github.com/user-attachments/assets/a91d15ef-5aa8-4650-96c5-32b44ef684f0" />


- Step 6: Confirm Client is Added on GUI
On the server GUI, navigate to homepage and select SHOW ALL next to the magnifying Glass to view connected clients:

<img width="960" alt="2lnfDV9gsx" src="https://github.com/user-attachments/assets/06eacc1b-8dbd-4c95-a838-c32e810279d1" />

<img width="960" alt="KdcKBPXwen" src="https://github.com/user-attachments/assets/b00469d5-050c-49f5-8078-e4db5a493823" />


-----------------------------------------------------------------------------
> **_NOTE:_** If your installing the msi with `msiexec` like the next command:

```
msiexec.exe /i .\velociraptor-windows-amd64.msi /qn /l*v log.log

```

you shoud get a log file like the next log file:

```
=== Verbose logging started: 04/03/2025  13:45:54  Build type: SHIP UNICODE 5.00.10011.00  Calling process: C:\WINDOWS\system32\msiexec.exe ===
MSI (c) (48:44) [13:45:54:820]: Resetting cached policy values
MSI (c) (48:44) [13:45:54:820]: Machine policy value 'Debug' is 0
MSI (c) (48:44) [13:45:54:820]: ******* RunEngine:
           ******* Product: .\velociraptor-repack.msi
           ******* Action: 
           ******* CommandLine: **********
MSI (c) (48:44) [13:45:54:822]: Client-side and UI is none or basic: Running entire install on the server.
MSI (c) (48:44) [13:45:54:823]: Grabbed execution mutex.
MSI (c) (48:44) [13:45:54:875]: Cloaking enabled.
MSI (c) (48:44) [13:45:54:875]: Attempting to enable all disabled privileges before calling Install on Server
MSI (c) (48:44) [13:45:54:880]: Incrementing counter to disable shutdown. Counter after increment: 0
MSI (s) (48:88) [13:45:54:898]: Running installation inside multi-package transaction C:\Program Files\velociraptor\.\velociraptor-repack.msi
MSI (s) (48:88) [13:45:54:898]: Grabbed execution mutex.
MSI (s) (48:F4) [13:45:54:901]: Resetting cached policy values
MSI (s) (48:F4) [13:45:54:902]: Machine policy value 'Debug' is 0
MSI (s) (48:F4) [13:45:54:902]: ******* RunEngine:
           ******* Product: C:\Program Files\velociraptor\.\velociraptor-repack.msi
           ******* Action: 
           ******* CommandLine: **********
MSI (s) (48:F4) [13:45:54:903]: Machine policy value 'DisableUserInstalls' is 0
MSI (s) (48:F4) [13:45:54:986]: Note: 1: 2203 2: C:\WINDOWS\Installer\inprogressinstallinfo.ipi 3: -2147287038 
MSI (s) (48:F4) [13:45:54:988]: SRSetRestorePoint skipped for this transaction.
MSI (s) (48:F4) [13:45:54:989]: Note: 1: 1402 2: HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer 3: 2 
MSI (s) (48:F4) [13:45:54:989]: Note: 1: 1324 2: . 3: 1 
MSI (s) (48:F4) [13:45:54:989]: MainEngineThread is returning 2
MSI (s) (48:88) [13:45:54:990]: No System Restore sequence number for this installation.
MSI (s) (48:88) [13:45:54:991]: User policy value 'DisableRollback' is 0
MSI (s) (48:88) [13:45:54:991]: Machine policy value 'DisableRollback' is 0
MSI (s) (48:88) [13:45:54:992]: Incrementing counter to disable shutdown. Counter after increment: 0
MSI (s) (48:88) [13:45:54:992]: Note: 1: 1402 2: HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Installer\Rollback\Scripts 3: 2 
MSI (s) (48:88) [13:45:54:993]: Note: 1: 1402 2: HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Installer\Rollback\Scripts 3: 2 
MSI (s) (48:88) [13:45:54:993]: Decrementing counter to disable shutdown. If counter >= 0, shutdown will be denied.  Counter after decrement: -1
MSI (c) (48:44) [13:45:54:995]: Decrementing counter to disable shutdown. If counter >= 0, shutdown will be denied.  Counter after decrement: -1
MSI (c) (48:44) [13:45:54:997]: MainEngineThread is returning 2
=== Verbose logging stopped: 04/03/2025  13:45:55 ===


```
---------------------------------------------------------
