
# 05. Ansible start

## 1. Setting Up Ansible

```bash
    2  sudo apt update
    3  sudo apt upgrade
    4  sudo apt install qemu-guest-agent -y
    5  sudo apt install ansible -y
    6  reboot
    12  ssh-keygen -t ed25519
    41  ssh-copy-id user@192.168.34.173
    42  ssh-copy-id user@192.168.34.207
    43  mkdir -p ~/ansible-playbooks/{inventories,playbooks}
    44  cd ~/ansible-playbooks/
    45  nano playbooks/hello.yaml
    47  ansible-playbook playbooks/hello.yaml
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit
localhost does not match 'all'

PLAY [HW1 Hello World] ************************************************************************

TASK [Gathering Facts] ************************************************************************
ok: [localhost]

TASK [Print Hello Message] ********************************************************************
ok: [localhost] => {
    "msg": "Hello, Ansible!"
}

PLAY RECAP ************************************************************************************
localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## 2. Managing Remote Hosts

```bash
user@ansible:~/ansible-playbooks$ ansible-playbook -i inventories/hosts playbooks/vim_install.yaml

PLAY [Install vim on remote hosts] *************************************************************************************

TASK [Gathering Facts] *************************************************************************************************
ok: [k8s-worker]
ok: [k8s-master]

TASK [Install vim] *****************************************************************************************************
ok: [k8s-worker]
ok: [k8s-master]

TASK [Verify installation] *********************************************************************************************
changed: [k8s-worker]
changed: [k8s-master]

TASK [Show version] ****************************************************************************************************
ok: [k8s-master] => {
    "vim_version.stdout_lines[0]": "VIM - Vi IMproved 9.1 (2024 Jan 02, сборка от Apr 24 2026 19:05:03)"
}
ok: [k8s-worker] => {
    "vim_version.stdout_lines[0]": "VIM - Vi IMproved 9.1 (2024 Jan 02, сборка от Apr 24 2026 19:05:03)"
}

PLAY RECAP *************************************************************************************************************
k8s-master                 : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
k8s-worker                 : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## 3. Managing Users and Groups

```bash
user@ansible:~/ansible-playbooks$ ansible-playbook -i inventories/hosts playbooks/manage_users.yaml
Введите имя пользователя [devops_user]: newuser
Введите пароль:
confirm Введите пароль:

PLAY [Manage users and groups on remote host] **************************************************************************

TASK [Gathering Facts] *************************************************************************************************
ok: [k8s-master]
ok: [k8s-worker]

TASK [Create group] ****************************************************************************************************
changed: [k8s-master]
changed: [k8s-worker]

TASK [Create user] *****************************************************************************************************
changed: [k8s-worker]
changed: [k8s-master]

TASK [Verify user] *****************************************************************************************************
changed: [k8s-worker]
changed: [k8s-master]

TASK [Display result] **************************************************************************************************
ok: [k8s-master] => {
    "user_id.stdout": "uid=1001(newuser) gid=1001(devops_group) groups=1001(devops_group)"
}
ok: [k8s-worker] => {
    "user_id.stdout": "uid=1001(newuser) gid=1001(devops_group) groups=1001(devops_group)"
}

PLAY RECAP *************************************************************************************************************
k8s-master                 : ok=5    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
k8s-worker                 : ok=5    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```