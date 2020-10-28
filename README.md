# Blocky
Private Blockchain Network
* I am going to lay out the steps needed to start, mine, and send transactions on your very own private blockchain network.
---
To begin, you'll need to download a few things. 
* MyCrypto Desktop App: https://download.mycrypto.com/ - We use this as our wallet and to connect to a network.
* Go Ethereum tools: https://geth.ethereum.org/downloads/ - We use this to create the blockchain and facilitate transactions.
---
#### Make sure you download the latest Geth & Tools release, not the one that only says Geth.
#### Once downloaded, find the geth-alltools-windows-amd64-1.9.7-a718daa6.zip folder and rename it to something easier to find. I chose blockchain-tools.

* That's it, you have everything you need to create your very own blockchain network!
---

1. Open your terminal or git-bash and navigate to your blockchain-tools folder and create two node accounts and passwords with a separate datadir for each. 
* ./geth --datadir node1 account new
* ./geth --datadir node2 account new
#### Save your passwords for later
#### Save the public key and path to secret key. We'll need this later. Never give anyone access to your Private Keys.
---
2. Next we'll genereate your genesis block. (The first block of the chain)
* Run puppeth by typing ./puppeth
* Create the name for your blockchain and click enter. I chose blocky.
* Type 2 to configure new genesis, then 1 to create genesis from scratch.
#### We'll be using a Proof-of-Authority consensus engine.

* So enter 2 for Clique- proof-of-authority
* Leave block time at default by clicking enter.
---

3. Next we'll choose the accounts to seal.
* Copy and paste the node1 public key excluding the 0x portion.
* Copy and paste the node2 public key excluding the 0x portion.

#### Do the same steps for the next portion asking which accounts will be pre-funded.
#### Do not pre-fund the precompile-addresses with wei. Type no
---

4. Now we'll specify the chain ID and export genesis specs.
* Specify your chain/network ID. I chose 123
#### It is important to do this since you'll need to know the ID to link MyCrypto later. 
#### Do not choose 1 or 222 as your chain ID. 1 is the default ETH ID and 222 is the permission ID.
* Now we will choose 2, for manage existing genesis
* Choose 2 again, to export genesis configurations.
* Just click enter to save the blockchain.json files in your current folder where your nodes and geth tools are located.
* Once completed, double tap ctrl+c and keep your terminal window open.
#### Now your genesis block creation is complete and we can move on to the next step.
---
5. We will now initialize the nodes with the genesis json file.(replace networkname with the network name you chose)
* ./geth --datadir node1 init networkname.json 
* ./geth --datadir node2 init networkname.json
##### Now these nodes can begin mining blocks!
* In the same terminal window, enter the following command and replace "SEALER_ONE_ADDRESS" with the public key for node1.
* ./geth --datadir node1 --unlock "SEALER_ONE_ADDRESS" --mine --rpc --allow-insecure-unlock
* Once it runs and you get a successful message, use ctrl+f and type enode into the find box. It will show self=enode... Copy from enode to the very end digits, 30303 and save this for later.
#### Now open a new terminal or git bash window.
* Navigate to your blockchain-tools folder and type the following command and replace "SEALER_TWO_ADDRESS" with the public key for node2. Also replace the "enode://SEALER_ONE_ENODE_ADDRESS@127.0.0.1:30303" portion with the enode address you saved in the last step.
* ./geth --datadir node2 --unlock "SEALER_TWO_ADDRESS" --mine --port 30304 --bootnodes "enode://SEALER_ONE_ENODE_ADDRESS@127.0.0.1:30303" --ipcdisable --allow-insecure-unlock
#### Enter your password from node1 to run the node. (You will not see any text appear when typing in your password)
---
6. Now we can open your MyCrypto desktop app.
* Open MyCrypto app and click "Change Network" at the bottom left. 
* Click "Add Custom Node" and enter the custom network information that you set in genesis.
* Node name is the network name you chose.
* Click Network box and type custom, choose custom.
* Network name is same as node name.
* Currency is ETH
* Chain ID is the ID you chose and saved earlier in the steps.
* URL is http://127.0.0.1:8545
* Click "Save & Use Custom Node".
#### Now that you have connected your custom network, we can test it by sending money between accounts.
---
7. Select the "View & Send" option at the top of the left menu pane. 
* Click "Keystore File" at the bottom middle of the page.
* Click "Select Wallet File" then navigate to the keystore directory inside your node1 directory. Select the file, enter your password, and click "Unlock".
#### This will open your wallet inside MyCrypto and you should see your account is pre-funded.
* Now in the "To Address" box, paste the public address from node2 and fill in an arbitrary amount of ETH to send.
* Confirm by clicking "Send Transaction" and "Send" in the pop-up window. 
#### A green box will display saying your transaction has been broadcasted. Click "Check TX Status".
#### You should see the transaction go from "Pending" to "Succsessful" in about 15 seconds or whatever you set as the block time in the genesis steps. 
#### If it doesn't switch after 30 seconds, refresh the page, if it still hasn't switched to "Successful", then go back to both of your open terminals that are running your nodes and type in your password then click enter. You will notice the block get mined and the transaction completed. Go back to the MyCrypto app and refresh the page to see a "Successful" message.
---
### Congratulations, you successfully created your own private blockchain and processed your first transaction!
