
# Command used to encrypt users's secrets
For more details have a look at the page [ansible user_guide/vault.html](https://docs.ansible.com/ansible/latest/user_guide/vault.html)

# Encrypt a passsord by using a SHA512 method

A Linux OS can require a password to be provided/updated for a user account within one of the vars file. To not version sensitive data into the code repository we use ansible-vault encryption over a SHA512 encryption. The later can be done by using python functions. The python code bellow then helps to convert a clear password string into its SHA512 encrypted counterpart.

## Encrypt method SHA512

```python
python -c 'import crypt,getpass; print(crypt.crypt("<my_password>", crypt.mksalt(crypt.METHOD_SHA512)))'
```

You can even generate on the fly a new password using a python function call at the place of `"<my_password>"` placeholder:

```python
getpass.getpass()
```

## Encrypt only a sensitive field within a file

## Conventions
* The ansible vars-file that contains sensitive information
  `inventories/vagrant/vars/secret.yml`

* The ansible vault-id
  `deploy-vagrant@~/.vault-pass.deploy-vagrant`

* The ansible vault password file
  `~/.vault-pass.deploy-vagrant` is where the password vault has been stored

* The ansible vault-id label
  `deploy-vagrant` is the label that identifies which vault password has been used to encrypt or edit the secret file/field.

```shell script
ansible-vault encrypt_string --vault-id deploy-vagrant@~/.vault-pass.deploy-vagrant '<my_password_SHA512_Encrypted>' --name 'password'
```

## Encrypt the file content as a whole

```shell script
ansible-vault encrypt --vault-id deploy-vagrant@~/.vault-pass.deploy-vagrant inventories/vagrant/vars/secret.yml
```

```shell script
ansible-vault edit --vault-id deploy-vagrant@~/.vault-pass.deploy-vagrant inventories/vagrant/vars/secret.yml
```
