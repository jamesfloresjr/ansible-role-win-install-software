# Ansible Role: *win_install_software*

Install/update software on Windows hosts in an air-gapped network with Ansible.

## Requirements

Having Ansible setup for Microsoft Windows host communication. Click here to follow the [Ansible Documentation](https://docs.ansible.com/ansible/latest/os_guide/windows_setup.html) step-by-step.

## Prerequisites

### 1. Setting up the repository

The directory structure is as follows:

```bash
[repository]
├── [software-1]
│   ├── 1.1
│   │   └── installer_1.1.exe
│   ├── 1.2_latest
│   │   └── installer_1.2.exe
│   └── args.json
└── [software-2]
    ├── 2.1_latest
    │   └── installer_2.1.msi
    └── args.json
```

To setup, create a `[repository]` in a network share. In the root of the repository you just set up, create folders with the `[software]` names (e.g. __"Firefox"__, __"Putty"__, __"Notepad++"__). Inside those folders should exist folders with the version numbers. Make sure to rename the latest software with the tag `_latest`. The PowerShell script will fail if there is not a folder with this tag. Finally, in those individual folders should contain the executables (__"exe"__ or __"msi"__).

In each `[software]` folder, create a `.json` file containing arguments for the PowerShell script. For example:

```json
{
    "exe_args":  "/S",
    "msi_args":  "/quiet",
    "preferred_installer": "exe"
}
```

| Variable              | Description                                                                            |
| --------------------- | -------------------------------------------------------------------------------------- |
| `exe_args`            | You can find these by Googling __"[software] installer command line options"__.        |
| `msi_args`            | You can find these by running __"msiexec.exe /help"__ in PowerShell or Cmd.            |
| `preferred_installer` | Specify which installer you want to prioritize (can be either __"exe"__ or __"msi"__). |

### 2. Creating /vars/role_vars.yml

These are variables that Ansible will pass through to the PowerShell script:

```yml
---
# Do not include the trailing "\"
repository: "\\\\share\\windows_software"

# Name must match inside the repository structure
apps:
  - firefox
  - putty
  - notepad++
```

| Variable     | Description                                                                                                                                               |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `repository` | Input your repository without the trailing __"\\"__. Also, make sure to escape each __"\\"__ with another __"\\"__ (e.g. __"\\"__ turns into __"\\\\"__). |
| `apps`       | This array will contain all of the software that you want to install/update. Make sure these match with the directory structure in your repository.       |

## Example Playbook

```yml
---
- name: Install Windows Software
  hosts: windows_hosts
  gather_facts: no
  roles:
  - win_install_software
```

## Working Software

|    Software     |     preferred_installer     |     exe_args     |     msi_args     |
| --------------- | --------------------------- | ---------------- | ---------------- |
| Firefox         | exe                         | /S               |                  |
| Putty           | msi                         |                  | /quiet           |
