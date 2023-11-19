---
description: 'status: active - Linux Installation Instructions'
---

# ✅ ar io

## source

[https://ar.io/docs/gateways/ar-io-node/linux-setup.html#overview](https://ar.io/docs/gateways/ar-io-node/linux-setup.html#overview)

## system requirements (minimum) <a href="#system-requirements" id="system-requirements"></a>

* 4 core CPU
* 4 GB Ram
* 500 GB storage (SSD recommended)
* Stable 50 Mbps internet connection
* a domain like [www.dieuts.top](https://dieuts.top/) (register here [https://www.namecheap.com/](https://www.namecheap.com/))

## required packages <a href="#required-packages" id="required-packages"></a>

1.  Update your software:

    ```
    sudo apt update
    sudo apt upgrade   
    ```
2.  Enable your firewall and open necessary ports:

    ```
    sudo ufw enable

    # Optional: If using SSH, allow port 22 
    sudo ufw allow 22

    # Allow ports 80 and 443 for HTTP and HTTPS
    sudo ufw allow 80
    sudo ufw allow 443
    ```
3.  Install nginx:

    ```
    sudo apt install nginx -y
    ```
4.  Install git:

    ```
    sudo apt install git -y
    ```
5.  Install Docker:

    ```
    sudo apt install docker-compose -y
    ```
6.  Install Certbot:

    ```
    sudo apt install certbot -y
    ```
7.  Install ssh (optional, for remote access to your Linux machine):

    ```
    sudo apt install openssh-server -y
    sudo systemctl enable ssh
    ```
8.  Install Yarn:

    ```
    curl -sSL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -

    echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

    sudo apt-get update -y

    sudo apt-get install yarn -y
    ```
9.  Install NVM (Node Version Manager):

    ```
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
    source ~/.bashrc
    ```
10. Install Node.js:

    ```
    nvm install 18.8.0
    ```
11. Install build tools

    ```
    sudo apt install build-essential 
    ```
12. Install SQLite:

    ```
    sudo apt install sqlite3 -y
    ```

## install the node <a href="#install-the-node" id="install-the-node"></a>

Clone the ar-io-node repository and navigate into it:

<pre><code><strong>cd $HOME
</strong><strong>git clone -b main https://github.com/ar-io/ar-io-node
</strong>cd ar-io-node
</code></pre>

Create an environment file:

```
nano .env
```

Paste the following content into the new file:

```
GRAPHQL_HOST=arweave.net
GRAPHQL_PORT=443
START_HEIGHT=0
RUN_OBSERVER=true
ARNS_ROOT_HOST=<your-domain>
AR_IO_WALLET=<your-public-wallet-address>
OBSERVER_WALLET=<hot-wallet-public-address>
```

Replace:

`<your-domain>` with the domain address (ex `dieuts.top` not `www.dieuts.top`);&#x20;

`<your-public-wallet-address>`, `<hot-wallet-public-address>` with your AR addresses (ex `O6-axsGOMGzl1-et5xxxmoUexxx3ps6jK-OHsHxxx3g, open at` [`https://arweave.app/`](https://arweave.app/)



Create a file named `<Observer-Wallet-Address>.json`, replacing "\<Observer-Wallet-Address>" with the public address of the wallet. This should match your `OBSERVER_WALLET` environmental variable. The file is filled with private key of your `OBSERVER_WALLET` and must be saved in the `wallets` directory in the root of the repository.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Build the Docker container:

```
sudo docker-compose up -d --build
```

Check the logs for errors:

```
sudo docker-compose logs -f --tail=0
```

## set up networking <a href="#set-up-networking" id="set-up-networking"></a>

### config DNS

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

`Value`: your VPS IP

### Create SSL (HTTPS) Certificates for Your Domain:

```
sudo certbot certonly --manual --preferred-challenges dns --email <your-email-address> -d <your-domain>.com -d '*.<your-domain>.com'
```

Replace `<your-domain>.com` with your domain (ex `dieuts.top`)

### Configure nginx

create the configuration file:

<pre><code># remove default file
<strong>sudo rm /etc/nginx/sites-available/default
</strong>
# create configuration file for your domain, replace &#x3C;your-domain>.com with your domain
sudo nano /etc/nginx/sites-available/&#x3C;your-domain>.com.conf
</code></pre>

Paste the following configuration (replace `<your-domain>.com` when necessary). Save then exit:

```
# Force redirects from HTTP to HTTPS
server {
    listen 80;
    listen [::]:80;
    server_name <your-domain>.com *.<your-domain>.com;

    location / {
        return 301 https://$host$request_uri;
    }
}

# Forward traffic to your node and provide SSL certificates
server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name <your-domain>.com *.<your-domain>.com;

    ssl_certificate /etc/letsencrypt/live/<your-domain>.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/<your-domain>.com/privkey.pem;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_http_version 1.1;
    }
}
```

create a symbolic link (replace `<your-domain>.com` when necessary)

```
sudo ln -s /etc/nginx/sites-available/<your-domain>.com.conf /etc/nginx/sites-enabled/<your-domain>.com.conf
```

Test the configuration

```
sudo nginx -t
```

If there are no errors, restart nginx:

```
sudo service nginx restart
```

Your node should now be running and connected to the internet. Test it by entering `https://<your-domain>/3lyxgbgEvqNSvJrTX2J7CfRychUD5KClFhhVLyTPNCQ` in your browser.

## join the AR.IO testnet

[https://ar.io/docs/gateways/testnet/#join-the-ar-io-testnet](https://ar.io/docs/gateways/testnet/#join-the-ar-io-testnet)

## check your work

<pre><code><strong>https://gateways.&#x3C;your-domain>.com
</strong></code></pre>

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>
