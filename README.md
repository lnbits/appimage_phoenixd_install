# Fresh Lunanode VPS LNBits Install with Phoenixd with url/https
Creates a fresh LNbits install, with Phoenixd (self-custodial "managed" node) and url/https (using Caddy) on a VPS. We use lunanode.com for the VPS.

### Installing LNbits
Create a screen for LNbits
```
screen -S lnbits
```
Fetch/install latest LNbits Appimage
```
sudo apt-get install libfuse2
wget $(curl -s https://api.github.com/repos/lnbits/lnbits/releases/latest | jq -r '.assets[] | select(.name | endswith(".AppImage")) | .browser_download_url') -O LNbits-latest.AppImage
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
sudo apt-get install unzip
wget <THE LINK YOU COPIED>
unzip
chmod +x phoenix-0.4.2-linux-x6/phoenixd
./phoenix-0.4.2-linux-x6/phoenixd
```
Exit the screen
```
ctrl + a + d
```
### Grab phoenixd key

```
cat ./phoenix/phoenix.conf
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
Add this to the Caddfile.
```
<your url> {
    reverse_proxy 0.0.0.0:5000
}
```
`ctrl + s` to save, then `ctrl + x` to exit.
Run caddy (it will use your Caddyfile.
```
sudo caddy start
```

Go to your url 🚀


### Useful screen commands: 
Create a screen: `screen -S <screen name>`
List all screens: `screen -ls`
Attach to a screen: `screen -r <screen name>`
Detach from screen: `ctrl + a + d`
