## Ansible Project

### **All users and pc names are not real, it's more easy for testing.**

Working on a project on Ansible for Windows machines.
I use a test environment that I created to test everything:
1. DC1: Windows server 2022.
2. DC2: Windows server 2022.
3. Machine1: Windows 11.

---

**22/19/25 Update**

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


**20/19/25 Update**
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
