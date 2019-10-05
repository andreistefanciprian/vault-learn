# Hashicorp Vault cheat sheet

### Install and run Vault in development mode on Ubuntu
```buildoutcfg
# Install vault on Ubuntu 18.04
wget https://releases.hashicorp.com/vault/1.2.3/vault_1.2.3_linux_amd64.zip
sudo unzip vault_1.2.3_linux_amd64.zip -d /usr/bin

# Check if vault was installed
vault version

# Run Vault in development mode
vault server -dev
```

### Run Vault in development mode inside container
```buildoutcfg
# Have vault running inside docker container
docker run --cap-add=IPC_LOCK \
-p 8200:8200 \
-d \
--name vault \
-e 'VAULT_DEV_ROOT_TOKEN_ID=myroot' \
-e 'VAULT_DEV_LISTEN_ADDRESS=0.0.0.0:8200' \
-e 'VAULT_ADDR=http://127.0.0.1:8200' \
vault

# Get shell access inside container
docker exec -ti sh

# Define VAULT_TOKEN env var
docker exec -ti vault export VAULT_TOKEN=myroot

```


### Set the following environment variable:

 ```buildoutcfg
 export VAULT_ADDR='http://127.0.0.1:8200'
 export VAULT_TOKEN=<Root Token>
 ```
 
 ### Monitoring commands
 ```buildoutcfg
 # Vault health check
 vault status
 
 # Help
 vault path-help secrets
 ```
 
 ### Write and read secrets
 ```buildoutcfg
# Create secret: vault kv put secret/path key=value
vault kv put secret/hello foo=world

# Read secret: vault kv get secret/path
vault kv get secret/hello

# Read secret in json format
vault kv get -format json secret/hello

# List secrets
vault kv list secret

# Update secret
vault kv put secret/hello foo=world excited=yes
vault kv put secret/hello foo='First Secret goes here'
# Update secret from STIN
echo -n 'Updated from echo STIN' | vault kv put secret/hello foo=-
echo -n '{"foo": "Read from STIN json"}' | vault kv put secret/hello -
# Update secret from file
vault kv put secret/hello @input.json
 ```
 
### APIs


```buildoutcfg
# Create secret
curl --header "X-Vault-Token: myroot" \
--request POST \
--data @secret.json \
http://localhost:8200/v1/app_name/data/dev/db_creds

# Read secret
curl --header "X-Vault-Token: myroot" \
--request GET \
http://localhost:8200/v1/app_name/data/dev/db_creds
```