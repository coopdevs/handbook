_Copy from [Ansible Development Environment](https://gitlab.com/coopdevs/ansible-development-environment)_

_Needs to be adapted to be more generic and not refer to "this role" nor assume it only be used for ansible-related jobs_

---

This project is the base environment to develop the Ansible roles used in Coopdevs.

## Install & configure tools
### Pyenv and Pyenv-Virtualenv

We recommend to use a virtual environment and you need to be sure that what python version are you runing.
For this requirements, the best tool that we find is [Pyenv](https://github.com/pyenv/pyenv) with the [Pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv)
I recommend to use the authomathic installator.


#### Automatic installation

Visit the installer project: https://github.com/pyenv/pyenv-installer  
Requirements:
* `curl`
* `git`

#### Manual installation

Please follow the instructions of each project to install them:
1. [pyenv](https://github.com/pyenv/pyenv#installation)
2. [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv#installation)

> Be careful, if you are using Ubuntu, the modification to install the `pyenv` need to be in the `.bashrc` instead of in the `.bash_profile`.

Once the tools are installed and configured, we can continue!

### Python version

We are using the version X of Python.

You need to install this version in your system. Pyenv also makes this easy:

```
$ pyenv install X
```

### Create a virtualenv using this Python version

We need a virtualenv to install the libraries used to develop Ansible roles. To create the new environment run:

```
$ pyenv virtualenv X ansible_dev_env
```

This command creates a virtualenv using Python X and allows to install modules just in this scope. The virtualenv will live in `$(pyenv root)/X/envs/ansible_dev_env`. By default, this translates to `$HOME/.pyenv/X/envs/ansible_dev_env`

### Use an existing virtualenv

To use in a project the virtualenv we created a moment ago, we only need to state it. To do it we can use [`pyenv local`](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md#pyenv-local)

```
$ mkdir example-project && cd example-project
$ pyenv local ansible_dev_env
(ansible_dev_env) $ which python
```

The command `pyenv local` generates a file called `.python-version` with the virtualenv name to use inside this directory (`example-project` in the example).

If `eval "$(pyenv virtualenv-init -)"` is configured in your shell (it should be if you used the easy installer), then `pyenv-virtualenv` will automatically activate/deactivate virtualenvs whenever you cd into or out of the directory of the project.

Observe that instead of creating many virtual environments, we can now share and reuse them among many projects. In this case, we will use the same only virtualenv in all Ansible related projects.

## Install the requirements into the common virtualenv

Once we have completed the steps above, we can start using it all to install the tools we want to have as our Ansible development environment.

```
(ansible_dev_env) $ pip install -r requirements.txt
```

And that's it! Next time you create or download a project that needs the same environment, it will already be there with all the packages prepared.

## Common tools to all ansible projects

### Ansible
https://www.ansible.com/

### Ansible Lint
https://github.com/ansible/ansible-lint

### YAML Lint
https://github.com/adrienverge/yamllint

### Molecule
https://github.com/ansible/molecule
