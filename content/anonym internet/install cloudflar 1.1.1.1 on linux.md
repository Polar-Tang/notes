```sh
# Add cloudflare gpg key
curl -fsSL https://pkg.cloudflareclient.com/pubkey.gpg | sudo gpg --yes --dearmor --output /usr/share/keyrings/cloudflare-warp-archive-keyring.gpg


# Add this repo to your apt repositories
echo "deb [signed-by=/usr/share/keyrings/cloudflare-warp-archive-keyring.gpg] https://pkg.cloudflareclient.com/ $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/cloudflare-client.list


# Install
sudo apt-get update && sudo apt-get install cloudflare-warp
```

### Or
https://downloads.cloudflareclient.com/v1/download/noble-intel/version/2025.2.600.0
```sh
sudo dpkg --install ~/Downloads/cloudflare-warp_2025.2.600.0_amd64.deb
sudo systemctl enable warp-svc.service
sudo systemctl start warp-svc.service
warp-cli registration new 
warp-cli connect
```


