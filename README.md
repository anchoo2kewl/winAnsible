This guide will walkthough the steps required to install WinRM and allow Ansible access to a Windows Host.

**Step 1:** Open a PowerShell in Admin mode on the host machine. Then run this command:

```bash
PS C:\WINDOWS\system32> iex(iwr https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1).Content
```

**Step 2:** Next on the Ansible controller node (assuming it is a Mac), install Ansible.

```bash
$ brew install ansible
```

**Step 3:** There are few more extra packages you might need for Ansible to support windows modules. If you are using Ansible with Python3 use PIP3 to install pywinrm

```bash
$ pip3 install pywinrm
```

**Step 4:**

While you are executing this command, you might get a pop up and can see the Python is crashing.

You might see either of these or both error messages which can be solved by setting this environment variable

```bash
$ export OBJC_DISABLE_INITIALIZE_FORK_SAFETY=YES
```

**Step 5:** Create an `ansible_hosts` file:
```bash
[win]
192.168.1.93

[win:vars]
ansible_connection=winrm
ansible_user=Anshuman Biswas
ansible_password=qweasd
ansible_winrm_server_cert_validation=ignore
```

**Step 6:** Test with pinging the server

```bash
$ ansible win -m win_ping -i ansible_hosts_local
```

Step 7: Create a simple playbook:


```bash
---

- name: Windows Test Playbook

hosts: win

tasks:

- name: Remote Execute the mqsc files

win_shell: |

hostname

Get-Date

register: scriptoutput

- name: Script output

debug: var=scriptoutput.stdout
```