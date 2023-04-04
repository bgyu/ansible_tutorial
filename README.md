# Ansible Tutorial

## Ansible configuraitons
* ansible.cfg : basic configurations
* inventory: inventory file to define server nodes

### 1. Run adhoc ping module
```bash
# Use full command without configuration file
ansible all --key-file ~/.ssh/id_rsa_ansible -i inventory -m ping
# Use configuration file ansible.cfg to save typing.
ansible all -m ping --extra-vars "ansible_user=ben"
```

* `all` means use all hosts defined in `inventory` file.
* `--key-file` specify ssh private key file
* `-i` inventory, specify what server nodes the comand applies to
* `-m` means module, here we are using module `ping`.
* `--extra-var` specify extra variables passed to `ansible`.

If your current user name is different from ssh user name, then you need to speficy `ansible_user` via `--extra-vars` options.
If your current user name is the same with ssh user name, then you don't need to specify `ansible_user`.


### 2. Run adhoc gather_facts module
```bash
# gather all nodes facts
ansible all -m gather_facts --extra-vars "ansible_user=ben"

# gather specific node facts
ansible all -m gather_facts --limit 192.168.0.61 --extra-vars "ansible_user=ben"

```

### 3. Run `sudo apt update`
```bash
ansible all -m apt -a update_cache=true --become --ask-become-pass --extra-vars="ansible_user=ben"

# install vim-nox
ansible all -m apt -a name=vim-nox --become --ask-become-pass

# update snapd to latest
ansible all -m apt -a "name=snapd state=latest" --become --ask-become-pass

# update all packages
asnible all -m apt -a "upgrade=dist" --become --ask-become-pass
```

* `-m apt` to run `apt` command in Ubuntu
* `--become` to add `sudo` permission
* `--ask-become-pass` prompt for `sudo` password


### 4. Use playbook
```bash
ansible-playbook --ask-become-pass --extra-vars="anisble_user=ben" install_apache.yml

# run playbook with tags
ansible-playbook --tags web --ask-become-pass install_apache.yml


# run playbook without --ask-become-pass
# We can do this because in ansible.cfg, we use `remote_user` as simone
# and user simone is a sudoer, and no password is needed due to /etc/sudoers.d/simone
ansible-playbook site.yml

```
