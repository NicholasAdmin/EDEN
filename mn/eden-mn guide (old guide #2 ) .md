## Installation

Generate your Masternode Private Key
```bash
masternode genkey

Write this down or copy it somewhere safe.
```
View your Output

```bash
masternode outputs

Write this down or copy it somewhere safe. 
```

SSH (Putty Suggested) to your VPS, login to root, and clone the Github repository:

```bash
git clone https://github.com/fasterpool/EDEN-MN-SETUP
```
Navigate to the install folder:

```bash
cd EDEN-MN-SETUP
```

Install & configure your desired master node with options. ```

For Ubuntu 16.04 and Ubuntu 17.10

```bash
bash install_ubuntu_17.10.sh
```

When the script asks, input your VPS IP Address and Private Key (You can copy your private key and paste into the VPS if connected with Putty by right clicking)

Once done, the VPS will ask you to go start your masternode in the local wallet

In appdata/roaming/Eden, open up masternode.conf

Insert as a new line the following:

```bash
masternodename ipaddress:3595 privatekey output
```

Open up the local wallet, unlock with your encryption password, and open up the Debug Console

```bash
masternode start-alias <masternodename>
```
If done correctly, it will indicate that the masternode has been started correctly. 

Go back to your VPS and hit the spacebar. It will say that it needs to sync. You're all done!

Now you just need to wait for the VPS to sync up the blockchain and await your first masternode payment.
