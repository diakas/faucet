# SmartHoldem Faucet

[![Wallet Releases](https://github.com/smartholdem/smartmedia/blob/master/mediakit/sh_faucet.jpg)](https://faucet.smartholdem.io/)

## Params

file config.json

- node - The IP address of the SmartHoldem node the backend will use to query SmartHoldem's blockchain
- port - The port the web server will listen on (default 8082)
- payPerClick - How much SmartHoldem to credit users per use of the faucet
- cooldown - How long users have to wait before using the faucet again (in seconds)
- paySchedule - When the faucet automatically pays out, in cron format https://github.com/node-schedule/node-schedule
- payMinimum - The minimum balance users must accrue before they are paid out
- txFee - The transaction fee to be subtracted from users' payouts. Set to 0 if you want to pay their transaction fees.
- log - Filename of log file
- database:username - MySQL username
- database:password - MySQL password
- recaptcha:siteKey - ReCaptcha site key https://www.google.com/recaptcha/admin?hl=ru
- recaptcha:secretKey - ReCaptcha secret key https://www.google.com/recaptcha/admin?hl=ru

## Installation

```shell

sudo apt-get install mysql-server
sudo apt-get install mysql-client

```

## Create mysqldb name faucet

```shell

mysql -u root -p
CREATE DATABASE faucet;
SHOW DATABASES;
exit

```

### Import mysql from /DataModel/db.sql

```shell

mysql -u <db_username> -p faucet < /home/<user>/faucet/DataModel/db.sql

```

```shell
git clone https://github.com/smartholdem/faucet.git
cd faucet
npm install
node server --pass "Your Faucet Address Passphrase" or forever start server.js --pass "Your Faucet Address Passphrase"
```
open http://server_ip:8082

## Install nginx

```shell

sudo apt-get update
sudo apt-get install nginx

nano /etc/nginx/sites-available/faucet
sudo ln -s /etc/nginx/sites-available/faucet /etc/nginx/sites-enabled/
sudo service nginx restart
```

## Nginx faucet config example

```shell
server {
    listen 80;
    listen [::]:80;
    server_name examplesite.com;

    location / {
    proxy_pass http://localhost:8082;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $remote_addr;
    add_header Cache-Control no-cache;
    expires 1800s;
    }
}
```
