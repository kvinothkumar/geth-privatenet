
#### Repository contents

01. *01-private-ethereum-testnet*

# Notes


http://ethdocs.org/en/latest/


https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console


https://etherscan.io/


https://stats.ethdev.com/

Create & save a *.json file called CustomGenesis.json and put these contents in it:

Sidenote
As mentioned in this blog post, there’s a Python tool available to generate your own custom Genesis file. For simplicity’s sake, you can just copy paste this example:
{
    "config": {
        "chainId": 15,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
    "difficulty": "0x400",
    "gasLimit": "0x2100000",
    "alloc": {
        "93f932b3b87e08cdaf0877994e44feb4c93e81aa": 
         { "balance": "0x1337000000000000000000" }     
    }
}
The file acts as a ‘seed’ for your private testnet.

Sidenote
We’ve lowered the difficulty down to 400, so mining new blocks goes faster.
Notice that we added our account address 93f932b3b87e08cdaf0877994e44feb4c93e81aa to the "alloc" object, and that we gave it a balance of 1337000000000000000000 wei (1337 ETH):

"alloc": {
        "93f932b3b87e08cdaf0877994e44feb4c93e81aa": 
         { "balance": "0x1337000000000000000000" }     
    }
You can use this technique to pre-fund your account. You can define multiple accounts to include in the seed. Note that this step is optional, as you can easily mine the Ether yourself, as you’re in control of the (low) difficulty!

You will reference this file (CustomGenesis.json) when initialising your genesis block using the following command:

geth --identity "MyTestNetNode" --nodiscover --networkid 1999 --datadir /path/to/test-net-blockchain  init /path/to/CustomGenesis.json
Pro Tip
I’m using a custom `datadir` flag to separate the testnet blockchain from the real one. I suggest you do as well. This can be any folder you want.
To learn more about the other flags used, please navigate to the documentation.
Creating an account on the private tesnet
Create or find a directory where you’d like to store your local private testnet data. For this example, we’ll be using

/path/to/test-net-blockchain
After the previous geth --version command ran successfully, run

geth account new --datadir /path/to/test-net-blockchain
This command will ask you for a passphrase (=password). Do not forget this.

Pro Tip
I’m using a custom `datadir` flag to indicate that I’d like to use my personal, local testnet. This keeps it separated from the real testnet.
By default geth will use the same directory for network related files as for the public mainnet. Thus you are advised to set a custom --datadir to keep the public network’s chaindata from being reset.
To learn more about this, see documentation.
This will create an account on your fresh testnet node, and will return the private testnet address like so:

Address: {93f932b3b87e08cdaf0877994e44feb4c93e81aa}
Save this address and your password for later use.

Pre funding your account
This is an optional step. As the difficulty is so low, it would only take you a few minutes to mine enough Ether to get you going.

Start by removing your previously created blockchain database:

geth removedb --datadir /path/to/test-net-blockchain
Update your ‘CustomGenesis.json’ file:

{
    "config": {
        "chainId": 15,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
    "difficulty": "0x400",
    "gasLimit": "0x2100000",
    "alloc": {
        "93f932b3b87e08cdaf0877994e44feb4c93e81aa": 
         { "balance": "0x1337000000000000000000" }     
    }
}
Notice that we added our account address 93f932b3b87e08cdaf0877994e44feb4c93e81aa to the "alloc" object, and that we gave it a balance of 1337000000000000000000 wei (1337 ETH):

You can define multiple accounts to include in the seed.

Run the Genesis block initialisation command again

geth --identity "MyTestNetNode" --nodiscover --networkid 1999 --datadir /path/to/test-net-blockchain  init /path/to/CustomGenesis.json
If you haven’t done the prefunding
Use this command after starting the JavaScript Console (see next step: ‘Interact with Geth console’):

geth --identity "MyTestNetNode" --datadir /path/to/test-net-blockchain --nodiscover --networkid 1999 console
Interact with Geth console
To interact with Geth through the console, called the Geth JavaScript Console use:

geth --identity "MyTestNetNode" --datadir /path/to/test-net-blockchain --nodiscover --networkid 1999 console
If it ran successfully, you should see a confirmation:


If you see this message you can start running Geth JavaScript commands.
Try to use this one to check the balance of your primary account

web3.fromWei(eth.getBalance(eth.accounts[0]), "ether")
Pro Tip
Store these long Geth commands in separate *.sh files so you can re use the configuration (and it’s custom flags) at a later time. You’ll need this every time you’ll want to use your (custom) private testnet.
Folder Structure
Purely informational, this is what my folder structure looks like:


My folder structure
As you can see, I created some *.sh script to re-use later on.

*UPDATE*
I’ve created a repository containing the ones I used as a reference for this article: https://github.com/WWWillems/medium-attachments/tree/master/01-private-ethereum-testnet


Image from https://giphy.com/gifs/hoppip-charlie-chaplin-film-hoppip-S7i2sED2yfDGg
Congratulations!
You now have a functioning private Ethereum testnet blockchain and an account/wallet running on your personal computer. You can now start mining, send transactions, etc. I’ll be writing some new articles regarding these topics in the near future.

Useful commands/tips
geth removedb
Deletes/removes your locally synced blockchain data of the public testnet.
geth removedb --datadir /path/to/test-net-blockchain
Deletes/removes your private blockchain testnet data.
geth --fast --cache=1024
Synchronises the blockchain more quickly. If you choose to use the — fast flag to perform an Ethereum fast sync, you will not retain past transaction data.
When using geth attach when running a private testnet,
be sure to include the IPC endpoint. 
The IPC endpoint is shown when starting your Geth JavaScript console 
as such:IPC endpoint opened: /path/to/endpoint/geth.ipc 
Attach the path to your command so it becomes: 
geth attach /path/to/endpoint/geth.ipc
Geth JavaScript console commands
Use these commands once your logged into the Geth console.

If you receive an error saying ‘Error: authentication needed: password or unlock’, use this command to unlock your primary account in the Geth JavaScript console:
personal.unlockAccount(eth.coinbase, 'your account password in quotes', 0)
To check the balance of your primary account
web3.fromWei(eth.getBalance(eth.accounts[0]), "ether")
3. To see a list of pending transactions

eth.pendingTransactions




Useful links
Ethereum docs
Geth JavaScript Console Wiki
Etherscan — An Ethereum Block Explorer and Analytics Platform
Ethereum Network stats monitor

