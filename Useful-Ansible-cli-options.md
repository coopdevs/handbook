## Executar només parts d'un playbook

### Sense modificar els playbooks

_Font [[https://tech.aabouzaid.com/2015/11/run-playbook-starting-of-a-certain-task-ansible.html]]_

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
---

## Sobreescriure variables a l'hora d'executar el playbook

_Font: https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#passing-variables-on-the-command-line_

Pot ser útil per provar valors diferents sense modificar inventoris, o per activar flags que es es respecten a les tasques. Per exemple, per activar development environment i evitar que es facin backups:

En el playbook `playbooks/provision.yml`:
```yaml
- name: provision
  roles:
    - role: backups
      when: not development_environment
```
En la cli:
```bash
ansible-playbook -i inventory/hosts playbooks/provision.yml  --extra-vars='{"development_environment": true}'
```
La versió curta d'`--extra-vars` és `-e`.  
Existeix una sintaxi més senzilla però només sap tractar amb strings. Per exemple
```sh
# backups_role_enabled = string("true")
-e '{backups_role_enabled: "true"}'
-e "backups_role_enabled=true"

# backups_role_enabled = boolean(true)
-e '{backups_role_enabled: true}'
```


---

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

---

## Claus SSH

_Font: https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#common-options_

Ansible es connecta als hosts especificats al fitxer `hosts` aplicant els filtres de `--limit`. S'hi connectarà per ssh respectant la configuració de ssh, per tant, si amb ssh et pots connectar sense contrasenya, amb ansible també. Ara bé, si no tens configurat un host, o has de modificar els paràmetres, necessitaràs:
```sh
--private-key # el certificat d'identitat. Equivalent a -i de ssh
--user        # l'usuària del sistema remot. Equival a **user**@host de ssh
--port        # el port ssh remot. Equival a -p de ssh
```
Exemple:
```
ansible-playbook \
  -i inventory/hosts \
  --private-key ~/.ssh/example-org_id_rsa \
  --user ubuntu \
  --port 2255 \
  playbooks/provision.yml
```