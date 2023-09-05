Using this project, you can run a docker-based shadowsocks proxy by its v2ray plugin.

It comes originally from [here](https://github.com/AhmadRafiee/shadowsocks-with-v2ray) with some extra elaboration.

### Get `project`
```bash
git clone https://github.com/zianazari/shadowsocks-v2ray-tls.git
```

### Install `certbot` on your ubuntu
```bash
cd shadowsocks-v2ray-tls
mkdir certs
sudo apt-add-repository ppa:certbot/certbot
apt update
apt install certbot -y
certbot --version
```

### Create `certificate` and `privatekey` with certbot command

- change YOUR_EMAIN_ADDRESS and DOMAIN_NAME to your own.
```bash
certbot certonly --standalone --preferred-challenges http --non-interactive --agree-tos --email <YOUR_EMAIN_ADDRESS> -d <DOMAIN_NAME>
```
then, copy *pem* files in certs directory.
```bash
cp /etc/letsencrypt/live/ffuf.ir/fullchain.pem certs
cp /etc/letsencrypt/live/ffuf.ir/privkey.pem certs
```

### change `config/config.json`
- change DOMAIN_NAME to your own
- change PASSWORD to your own


### Add a new non roor user and change the directory permissions
- It is better to run the service as a nonroot user.
  ```bash
  sudo adduser vpn
  ```
- remember to restrict this user to login through ssh (you can add list of users who can just ssh to the server
```bash
sudo vi /etc/ssh/sshd_config
```
- press i and add `AllowUsers user1 user2` to end of the file
 then make vpn user the owner of required files 
```bash
chown -R vpn:vpn shadowsocks-v2ray-tls
chmod 755 -R shadowsocks-v2ray-tls
```

### Run the service
```bash
docker compose up -d
#check it is ok
docker ps -a
```

### client tools
In order to you the service in client-side, you need to get appropriate files from below links:
- https://github.com/shadowsocks/
- https://github.com/shadowsocks/v2ray-plugin/releases
