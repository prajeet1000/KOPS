# update ssh port and keys
## Step 1. change port
## uncomment port and change according to need 
```bash
vi /etc/ssh/sshd_config
```
## reload ssh deamon
```bash
systemctl reload sshd
```
## check ssh port configurations
```bash
netstat -lntp | grep sshd
```

## Step 2. change ssh public key

## Generate ssh key
```bash
ssh-keygen -t ed25519
```
## Copy public key 
```bash
cd ~/.ssh/
cat id_ed25519.pub
```
## Paste public key to authorized_key
```bash
vi /home/ubuntu/.ssh/authorized_key
```
## convert private key (id_ed25519) to .pem
```bash
mv id_ed25519 id_ed25519.pub
```

## check it
```bash
ssh -i ~/.ssh/id_ed25519.pem ubuntu@----ip------  -p port-no
```
