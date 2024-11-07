# Install Software on Windows Hosts via Ansible

⚠️ Currently still in development ⚠️

Install/update software on Windows hosts in an air-gapped network with Ansible. Requires a repository to be setup on a network share.

## Prerequisites

### Setting up the repository

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
