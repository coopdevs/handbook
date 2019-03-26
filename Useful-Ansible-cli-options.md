## Executar només parts d'un playbook

### Sense modificar els playbooks

_Font [[https://tech.aabouzaid.com/2015/11/run-playbook-starting-of-a-certain-task-ansible.html]]_

> With "--start-at-task" you can start a playbook at a particular task, as following it will start from that task called "xTASK" regardless its place in playbook.

```
ansible-playbook playbook.yml --start-at-task="Init Odoo database"
```

> Other option is "--step", which will ask you before each task, and you can choose if you want to run this task or not.

```
ansible-playbook playbook.yml --step
Perform task: Init Odoo database? (y/n/c):
```
**Note**: It's important not to skip ansible's facts gathering task or any other task that the one we want to try depends on.

### Afegint etiquetes a les tasques o als rols

També es pot preparar el playbook amb _tags_ per tal de poder filtrar a l'hora d'executar. Equivaldria a crear altres playbooks.

_Font: [[https://docs.ansible.com/ansible/latest/user_guide/playbooks_tags.html]]_

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


## Provar templates jinja2 sense executar tot un playbook

### Amb la consola de python
```python3
from jinja2 import Environment, Template, FileSystemLoader

# Start loading from templates dir
myloader = FileSystemLoader("/path/to/templates/dir")
myenv = Environment(loader=myloader)

myloader.list_templates()
# OUTPUT
# ['cron-main.sh.j2', 'cron-prepare.sh.j2', 'cron-upload.sh.j2', 'sudoers.j2']

mytemplate = myloader.load(env, 'sudoers.j2')
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

### Amb eines pròpies d'Ansible
_Font: [[https://stackoverflow.com/a/41682944]]_

`ansible all -i "localhost," -c local -m template -a "src=test.j2 dest=./test.txt" --extra-vars='{"users": ["Mike", "Smith", "Klara", "Alex"]}'`

o bé

`ansible all -i localhost, -c local -m template -a "src=test.j2 dest=./test.txt" --extra-vars=@group_vars/all.yml`