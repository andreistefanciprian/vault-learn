# Hashicorp Vault cheat sheet

### Install Vault
```
# Install vault on Ubuntu 18.04
wget https://releases.hashicorp.com/vault/1.2.3/vault_1.2.3_linux_amd64.zip
sudo unzip vault_1.2.3_linux_amd64.zip -d /usr/bin

# Check if vault was installed
vault version
```

### Start Vault in development mode
```vault server -dev```

### Set the following environment variable:

 ```export VAULT_ADDR='http://127.0.0.1:8200'```
 
 ### Monitoring commands
 ```
 # Vault health check
 vault status
 
 # Help
 vault path-help secrets
 ```
 
 ### Write and read secrets
 ```
# Create secret: vault kv put secret/path key=value
vault kv put secret/hello foo=world

# Read secret: vault kv get secret/path
vault read secret/hello

# Read secret in json format
vault kv get -format json secret/hello

# Update secret
vault kv put secret/hello foo=world excited=yes
vault kv put secret/hello foo='First Secret goes here'
 ```