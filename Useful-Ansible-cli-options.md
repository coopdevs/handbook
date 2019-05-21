## Run only some parts of a given playbook

### Without modifying the playbook

_Source [[https://tech.aabouzaid.com/2015/11/run-playbook-starting-of-a-certain-task-ansible.html]]_

> With "--start-at-task" you can start a playbook at a particular task, as following it will start from that task called "Init Odoo database" regardless its place in playbook.

```
ansible-playbook playbook.yml --start-at-task="Init Odoo database"
```

> Other option is "--step", which will ask you before each task, and you can choose if you want to run this task or not.

```
ansible-playbook playbook.yml --step
Perform task: Init Odoo database? (y/n/c):
```
**Note**: It's important not to skip ansible's facts gathering task or any other task that the one we want to try depends on.

### By tagging tasks or roles

You can also prepare the playbook with _tags_ to be able to filter out some tasks when running the playbook. Roughly equivalent to create other playbooks.

_Source: [[https://docs.ansible.com/ansible/latest/user_guide/playbooks_tags.html]]_

> If you wanted to just run the “configuration” and “packages” part of a very long playbook, you can use the --tags option on the command line:

`ansible-playbook example.yml --tags "configuration,packages"`

> On the other hand, if you want to run a playbook without certain tagged tasks, you can use the --skip-tags command-line option:

`ansible-playbook example.yml --skip-tags "packages"`

```yaml
tasks:
    - yum:
        name: "{{ item }}"
        state: installed
      loop:
         - httpd
         - memcached
      tags:
         - packages

    - template:
        src: templates/src.j2
        dest: /etc/foo.conf
      tags:
         - configuration
```
---

## Overwrite variables in just one playbook run

_Source: https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#passing-variables-on-the-command-line_

Overwritting enables us to try different values without modifying inventories or to set flags that are honored at tasks. For instance, to disable backups in a local environment, you can do:

At playbook `playbooks/provision.yml`:
```yaml
- name: provision
  roles:
    - role: backups
      when: not development_environment
```
At the command line:
```bash
ansible-playbook -i inventory/hosts playbooks/provision.yml  --extra-vars='{"backups_role_enabled": false}'
```
There's a shorter version of `--extra-vars`: `-e`.  
There's also a simpler syntax for extra-vars but it parses everything as strings. That is:
```sh
# backups_role_enabled = string("false")
-e '{backups_role_enabled: "false"}'
-e "backups_role_enabled=false"

# backups_role_enabled = boolean(false)
-e '{backups_role_enabled: false}'
```


---

## Test jinja2 templates without running a playbook

### Using python cli
```python3
from jinja2 import Environment, Template, FileSystemLoader

# Start loading from templates dir
myloader = FileSystemLoader("/path/to/templates/dir")
myenv = Environment(loader=myloader)

myloader.list_templates()
# OUTPUT
# ['cron-main.sh.j2', 'cron-prepare.sh.j2', 'cron-upload.sh.j2', 'sudoers.j2']

mytemplate = myloader.load(myenv, 'sudoers.j2')
# End loading from templates dir

# You can also load a template as a string like:
mytemplate = Template('# Grant to user only the ability to create backups using tar as root.\n# user  host = (user to become) no prompt for password: allowed commands\n{{ backups_role_user_name }} ALL=(root) NOPASSWD: {{ backups_role_sudoers_cmd_pattern }}')

mytemplate.render(
    backups_role_user_name="backups",
    backups_role_sudoers_cmd_pattern='tar -cvzf *'
)
# OUTPUT
# # Grant to user only the ability to create backups using tar as root.
# # user  host = (user to become) no prompt for password: allowed commands
# backups ALL=(root) NOPASSWD: tar -cvzf *'
```

### Using Ansible own tools (untested)
_Source: [[https://stackoverflow.com/a/41682944]]_

`ansible all -i "localhost," -c local -m template -a "src=test.j2 dest=./test.txt" --extra-vars='{"users": ["Mike", "Smith", "Klara", "Alex"]}'`

or else

`ansible all -i localhost, -c local -m template -a "src=test.j2 dest=./test.txt" --extra-vars=@group_vars/all.yml`

---

## SSH keys

_Source: https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#common-options_

Ansible connects to hosts specified at `hosts` file and aplying the filters from `--limit`. It will connect to them using ssh respecting its configuration, therefore, if your ssh config enables you tu log in without password, so will ansible. However, in the case that you don't have an ssh config entry for a given host, or you want to modify it temporarily, you may need these params:
```sh
--private-key # identity certificate. Equivalent to ssh's -i
--user        # Remote system user. Equivalent to ssh's **user**@host
--port        # Remote system's ssh port. Equivalent to ssh's -p
```
Example:
```
ansible-playbook \
  -i inventory/hosts \
  --private-key ~/.ssh/example-org_id_rsa \
  --user ubuntu \
  --port 2255 \
  playbooks/provision.yml
```