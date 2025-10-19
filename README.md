### Ansible Project

Working on a project on Ansible for Windows machines.
I use a test environment that I created to test everything:
1. DC1: Windows server 2022.
2. DC2: Windows server 2022.
3. Machine1: Windows 11.
**All users and pc names are not real, it's more easy for testing.**

---
**19/10/25 Update**
Made some changes in files:
1. created 2 playbooks: exe.yaml and packages.yaml
2. in "/group_vars/windows_server.yaml" I change "ansible_winrm_transport" to "credssp".
  - Added GPO for the PCs for "credssp" to work.
  - Sending ping getting a Pong.
3. Made a Shared folder for testing the non-local exe files. 
