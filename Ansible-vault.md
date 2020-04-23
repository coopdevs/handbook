### Encrypt a new key
To add a new encrypted key to a file: execute the following command, copy the output and paste it into the file.
```sh
ansible-vault encrypt_string --ask-vault-pass --name 'vault_key' 'vault_value'
```
If you need to add many keys to the same file you can temporary paste the vault password in a file (`.vault` in the example), and use the following command:
```sh
ansible-vault encrypt_string --vault-password-file .vault --name 'vault_key' 'vault_value' >> path/to/file.yml
```
In this way you can save some vault password input and pasting the output by hand multiple times.
### View an encrypted key
Since `ansible-vault view` doesn't work with inline vaults, we need to execute the following command:
```sh
ansible localhost -m debug -a var="vault_key" -e "@path/to/file.yml" --ask-vault-pass
```