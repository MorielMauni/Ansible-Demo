## Ansible Project

### **All users and pc names are not real, it's more easy for testing.**

Working on a project on Ansible for Windows machines.
I use a test environment that I created to test everything:
1. DC1: Windows server 2022- ~~WSL Ansible node controller~~.
2. DC2: Windows server 2022.
3. Machine1: Windows 11.
4. Machine2: Windows 11.
5. Debian 13 VM: New node controller

---

### Checklist
- [x] Building test environment
- [x] Ansible on Windows
- [x] Install packages
- [x] Install exe files
- [x] DNS resolve
- [X] Run multiple playbooks at once
- [ ] Make better playbooks- best practices
- [ ] Web GUI

---
### Updates

**20/11/25 Update**

Got approve from Cyber in the compony to procees with the project on the system.

**6-5/11/25 Update**

I was having some problem installing WSL on diffrent VMs so I took the project and moved it to a Debian 13 VM, took me around 10 minutes to see a ping with DNS resolving. That means I can recreate the environment with few minuts.

Made a directory "multibooks"
- playbook_1.yaml, playbook_2.yaml, and playbook_3.yaml: "edited" yaml file without the "---".
- multibok.yaml: it looked weird and have an error on VS Code ("All playbooks should be named") it's a valit playbook that run everything as needed.

**29/10/25 Update**

Was looking arount for some best practices:
- In /group_vars:
    - Deleted all the spesific .yaml and created "all.yaml" for all the hosts
    - Also, the User + Password are in "all.yaml"
- In /host_vars:
    - Changes the connection from "local" to "winrm"- and back to local
        - Still trying to understand what is the best for this.
- Added some packages to /playbooks/packages.yaml

**28/10/25 Update**

Huge update:
I enables ICMP with a GPO and then changed the "/etc/resolv.conf" file to this:
```
nameserver 192.168.232.128
search moriel.local
```

And it's worked!!

Did a little demo to show my Team Leader about it and we tried to install another exe (lenovo.yaml), worked as a charm!
- Downloaded the exe file from the website.
- checked the arguments with "/?".
- Re-Write the playbook.

Also, we cheked a little bit about "state":
- present
- latest
- absent
- downgrade
- reinstalled

I think now its the time to up the playbook game, and if needed making the infrastructure better.

**27/10/25 Update**

Made a new VM (L-DevOps) to showcase the workflow to others at work.
- Added /group_vars/test_server.yaml and /host_vars/L-DevOps.yaml .

I was looking more about the installation of exe files, and shaw that some arguments are mandetory.
```
**Support for true silent mode**: No user input is needed when started with the correct arguments (e.g., /S, /silent, /quiet).
**Do not require GUI interactions**: No pop-ups, EULAs, or buttons to click.
**Return standard error codes**: Installation completes and returns a clear success/failure code the module can read.
Do not dynamically download other installers or prompt for network access.

**Common EXE installer technologies that work well**:
Inno Setup (e.g., /VERYSILENT /NORESTART)
NSIS (Nullsoft Scriptable Install System) (e.g., /S)
InstallShield (silent options vary, but usually /s /v"/qn")
WiX Burn Bundles (/quiet /norestart)
Microsoft-provided redistributables (e.g., Visual C++ Redistributables)
```

Made "7zip.yaml" and took the installation as a link and didn't downloaded the exe
- I need internet to remote install so I don't care if I have to "download" the exe again.
IT WORKED! 

**26/10/25 Update**

Added "adobe.yaml"
For what I understand to install an exe file I need to have the right arguments.
To know what arguments I need for every exe I got an option to run ``` setup.exe /?``` and to get what arguments are available.
I tried to to it for the "Adobe Acrobat XI Standard" that we use.
I'm getting a new error which is an exe error "1603 fatal installer error"

**22/10/25 Update**

Created a new test playbook named "vmtools.yaml":
- Install VM Tools on every VM that I have for testing- More a thing for me.
Created a playbook named "ps_admin.yaml" that do few stuff:
- Open PowerShell and print "whoami"
```
ok: [L-MorielM] => {
    "user_output.output": [
        "moriel\\admansible"
    ]
}
```

- Checking elevation details
```
ok: [L-MorielM] => {
    "msg": [
        "User: ADMAnsible",
        "Domain: MORIEL",
        "Elevation level: High Mandatory Level"
    ]
}
```
I had an idea that I will open the Powershell as admin and will loop all the exe files and install them silently but it's just won't work.


**20/10/25 Update**
I modifyed the exe.yaml file trying to make it work.
- first got an error on ``` Windows cannot access the specified device, path, or file. You may not have the appropriate permissions to access the item.```
- Then got errors about the path it want to be insalled ```C://Users/Ansible/%APPDATA%/Roaming/..``` which not exit because this user never logged in this PC.
- Used ```"/silent /dir=%USERPROFILE%\\AppData\\Roaming\\Zoom"``` but then I tried some more stuff.
I getting no error in vervose mode ```ansible-playbook -i inventory/hosts playbooks/exe.yaml -vvv``` but I don't see the app in appwiz.cpl.
Tried another approch with copying the exe file into a directory and install it, but it's doing everything exept installing the exe (ZoomInstaller.exe).
I want to check with other exe files maybe it's only Zoom.

**19/10/25 Update**
Made some changes in files:
1. created 2 playbooks: exe.yaml and packages.yaml
2. in "/group_vars/windows_server.yaml" I change "ansible_winrm_transport" to "credssp".
  - Added GPO for the PCs for "credssp" to work.
  - Sending ping getting a Pong.
3. Made a Shared folder for testing the non-local exe files. 
