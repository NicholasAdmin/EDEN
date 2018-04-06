## Installation

NOTE: This installation guide is provided as is with no warranties of any kind.

If you follow the steps and use a newly created Ubuntu 16.04 VPS, it will automatically configure and start your Master Node. You will need to input your public VPS IP address and masternode private key.

Steps:

0) Create a new VPS or use existing one. Recommended config is similar to vultr's $5/mo (25GB SSD/1xCPU/1GB RAM, Ubuntu 16.04). It can handle several MNs running simultaneously on the same IP address but they have to use dirfferent ports. Therefore you cannot easily run more than one EDEN MN on the same box. Different coins are fine.

1) In Windows wallet, create a new receiving address and name it mn1 for example.

2) Send exactly 2500 EDEN to this new address.

3) Generate your Masternode Private Key in Windows wallet
```bash
masternode genkey
```
Write this down or copy it somewhere safe. You will need this key to set up MN on VPS later.

4) View your Output transaction ID in Windows wallet console

```bash
masternode outputs
```
Write this down or copy it somewhere safe. You will use this in the masternode.conf file for your Windows wallet later.

5) SSH (Putty Suggested) to your VPS, login to root, and clone the script Github repository. 
(NOTE: Currently this repo contains Linux wallet binaries wich are necessary to run master node on VPS. The location of these binaries will be changed to an official release github folder later)

```bash
git clone https://github.com/fasterpool/EDEN-MN-SETUP
```
6) Navigate to the install folder:

```bash
cd EDEN-MN-SETUP
```

7) Run the bash script which will install & configure your desired master node with all necessary options.

For Ubuntu 16.04 and Ubuntu 17.10

```bash
bash install_ubuntu_17.10.sh
```

When the script asks, input your public VPS IP Address and Private Key created in the very beginning (You can copy your private key and paste into the VPS if connected with Putty by right clicking).

When the script configures the ufw firewall, answer 'y' to proceed with operation.
Once done, the VPS will ask you to start your masternode in your Windows wallet.

8) On your Windows machine, close wallet app. Navigate to %appdata%/roaming/Eden, open masternode.conf with Notepad.

Insert as a new line the following line for each masternode you are running:

```bash
masternodealias vpspublicipaddress:3595 masternodeprivatekey output
```
Example:
```bash
mn1 231.321.11.22:3595 7rG2VYFdibSdgWiVUMgfAnhsNKYbWAkL3GHYrJyPhAviaiccC1Z 06ffb02bf3ae7281392e82fa682b1e3c07c8d49c8f19ac9a429cfbf40a0d16da 1
```


Open up the wallet, unlock with your encryption password, and open up the Debug Console

```bash
masternode start-alias <masternodename>
```
If done correctly, it will indicate that the masternode has been started correctly. 

Go back to your VPS and hit the spacebar. It will attempt to start your new masternode using the following commands:

```bash
eden-cli masternode start
eden-cli masternode status
```

The status check will say that it needs to sync. This initial sync process may take several minutes to several hours.

At this point you're all done!

Now you just need to wait for the VPS to sync up the blockchain and await your first masternode payment.

Finally, to monitor your masternode status you can use:

```bash
eden-cli masternode status

eden-cli mnsync status
```

If you are really bored waiting for the sync to complete, you can watch what masternode is doing on the network at any time by using tail to monitor the debug.log file in realtime:

```bash
sudo tail -f ~/.eden/debug.log
```

In conclusion, try restarting your VPS server and see if masternode comes back online automatically. The script adds a cron job which starts edend daemon and the masternode after every reboot.

Enjoy and hope that EDEN makes it to the moon!
note that we still have 0% pool fees at https://fasterpool.com :)

--Allroad
