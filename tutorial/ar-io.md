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

















