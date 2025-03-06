First, we will need to copy the excutable to the Velociraptor folder and going to the folder:


`cd \Velociraptor`


For this installation, we are going to set up Velociraptor in ubuntu 20.04 in the cloud deployment.  

Let’s get started:

`velociraptor config generate -i`

When it asks about the OS, please choose linux.  It should be the default.

When it asks about the Path to the datastore, just hit enter.  This will keep the default.

When it asks about the SSL certs, just hit enter.  It will choose the default of Self Signed SSL.

When it asks about the DNS name, jwe will insert the WAS DNS name.

For the ports you can use the default or configure other ports. 

here is a gif how to install the server on linux

![VirtualBoxVM_GqL1rFuh5k](https://github.com/user-attachments/assets/fb5ab085-722d-469c-8f4b-c8a497c504b3)

and here in windows:

![WindowsTerminal_B4Ae0DdAwB](https://github.com/user-attachments/assets/70ca9bde-8b85-43c1-b3dc-fca9d8825249)


For the GUI username, we gave a user name and password [you can give how many usernames and passwords you want - the tool will ask you for more users and passwords until you will hit enter without a name].


When it asks about the logs directory, just hit enter to accept the default.

When it asks where to write the server and client configs, just hit enter to accept the defaults.


If needed, you can add a GUI user.

`velociraptor --config server.config.yaml user add root --role administrator`

or any other roles in velociraptor:

For example, we can come up with the following roles:

- reader: This role provides the ability to read previously collected results but does not allow the user to actually make any changes. Sometimes we give customer sysadmins this role to allow them to see what we are doing on their network, but without allowing them to actually collect any data.

- analyst: The next level up is an analyst — they are able to read existing collected data and also run some server side VQL in order to do post processing of this data or annotate it. Analysts typically use the notebook or download collected data offline for post processing existing hunt data. Analysts may not actually start new collections or hunts themselves.

- investigator: The investigator role is the same as the analyst but can actually initiate new hunts or flow collections.

- artifact_writer: This role allows a user to create or modify new client side artifacts (They are not able to modify server side artifacts). This user typically has sufficient understanding and training in VQL to write flexible artifacts. Artifact writers are very powerful as they can easily write a malicious artifact and collect it on the endpoint. Therefore they are equivalent to domain admins on endpoints. You should restrict this role to very few people.

- administrator: Like any system, Velociraptor needs an administrator which is all powerful. This account can run arbitrary VQL on the server, reconfigure the server etc. Hopefully, the need for a user to have administrator level access is greatly reduced by the RBAC system.

When it asks for the password, please choose a password you will remember.


Later we need to cinfigure in the `server.config.yaml` file to get connection for any address `0.0.0.0`, so replace in the GUI configuration the address fron `127.0.0.1` to `0.0.0.0`

![velo1](https://github.com/user-attachments/assets/8c5d0479-aa64-4889-8a48-cc742e0e3956)

Now, let's start the server.

`velociraptor --config server.config.yaml frontend -v`


Next, let’s surf to the GUI and see if it worked!

When you load the page, there will be an SSL error about the self-signed cert.  That is fine.

select Advanced then proceed to the address.


When it asks for the Username and Password, please enter root and the password you chose earlier.

<img width="854" alt="VirtualBoxVM_GnOOc8U33p" src="https://github.com/user-attachments/assets/a300ec87-3e74-484d-91ac-43d390140221" />


![image](https://github.com/user-attachments/assets/5f087c22-650f-4e26-9557-4469f70ee572)


Please select Inspect the server's state.


