### View an encrypted key
Since `ansible-vault view` doesn't work with inline vaults, we need to execute the following command:
```sh
ansible localhost -m debug -a var="vault_key" -e "@path/to/file.yml" --ask-vault-pass
```