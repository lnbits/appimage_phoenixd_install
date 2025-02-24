# Fresh VPS LNbits Install with Phoenixd and URL/HTTPS
Creates a fresh LNbits install, with Phoenixd (self-custodial, super-easy "managed" node) and url/https (using Caddy) on a VPS.

Head on over to https://lunanode.com and `Create a VM`. I always use 2nd to cheapest, but cheapest will do. Pick ` Ubuntu 24.04 64-bit (template)`.

### Installing LNbits
Create a screen for LNbits
```
screen -S lnbits
```
Fetch/install latest LNbits Appimage
```
sudo apt-get install libfuse2
wget $(curl -s https://api.github.com/repos/arcbtc/lnbits/releases/latest | jq -r '.assets[] | select(.name | endswith(".AppImage")) | .browser_download_url') -O LNbits-latest.AppImage
chmod +x LNbits-latest.AppImage
./LNbits-latest.AppImage --host 0.0.0.0 --port 5000
```
Exit the screen
```
ctrl + a + d
```
### Installing Phoenixd
Create a screen for Phoenixd
```
screen -S phoenixd
```
Copy the link of the latest `phoenix-0.4.2-linux-x64.zip` from https://github.com/ACINQ/phoenixd/releases
```
wget <THE LINK YOU COPIED>
```
Fetch and run Phoenixd.
```
sudo apt-get install unzip
unzip <phoenixd version you fetched>.zip
chmod +x <phoenixd version you fetched>/phoenixd
./<phoenixd version you fetched>/phoenixd
```
Read the disclaimers and type `I understand` a bunch.

Once Phoenixd is running exit the screen
```
ctrl + a + d
```
### Grab phoenixd key

```
cat .phoenix/phoenix.conf
```
Copy the key and put somewhere safe.

### Setup DNS
Add an `A` record to your DNS
<pic>

### Install Caddy
https://caddyserver.com/docs/install#debian-ubuntu-raspbian
```
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https curl
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install caddy
```
Close caddy
```
sudo caddy stop
```
Create `Caddfile`
```
sudo nano Caddyfile
```
Add this to the Caddyfile.
```
<your url> {
    reverse_proxy 0.0.0.0:5000
}
```
`ctrl + s` to save, then `ctrl + x` to exit.
Run caddy (it will use your Caddyfile).
```
sudo caddy start
```

Go to your url, create the Super-User, add your key to the LNbits funding source, hit restart button.

Phoenixd requires 20ksats sent in before outgoing payments are possible.

Profit 🚀

### Useful screen commands: 
* Create a screen: `screen -S <screen name>`
* List all screens: `screen -ls`
* Attach to a screen: `screen -r <screen name>`
* Detach from screen: `ctrl + a + d`
* If you end up outside of a locked screen: `screen -d <screen name>`
