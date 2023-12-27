# MetaCoin TronBox Example


### Install TronBox

If you don't have Node on your Mac/Linux computer, install it using preferably NVM. On Linux/Mac you can run

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash
```
after open a new terminal and install the version of Node you prefer, for example the stable v10:
```
nvm install lts/dubnium
```

On Windows, you can install Nvm following the instructions at
https://github.com/coreybutler/nvm-windows

In any case, when you have Node installed, install TronBox globally:
```
npm i -g tronbox
```

### Configure Network Information for TronBox

Network configuration is required by TronBox.
In our case we use Tron Quickstart for local testing, and TroGrid for as testnet. In the following example, we assume you are using TronQuickstart as local testnet, and you connect to Shasta/Nile testnet.

```
module.exports = {
  networks: {
    development: {
      // For trontools/quickstart docker image
      privateKey: '0000000000000000000000000000000000000000000000000000000000000001',
      consume_user_resource_percent: 0,
      fee_limit: 100000000,
      fullHost: "http://127.0.0.1:9090",
      network_id: "9090"
    },
    shasta: {
      // Shasta testet
      privateKey: process.env.PRIVATE_KEY_SHASTA,
      consume_user_resource_percent: 50,
      fee_limit: 100000000,
      fullHost: "https://api.shasta.trongrid.io",
      network_id: "2"
    },
    nile: {
      privateKey: process.env.PRIVATE_KEY_NILE,
      userFeePercentage: 100,
      feeLimit: 1000 * 1e6,
      fullHost: 'https://api.nileex.io',
      network_id: '3'
    }
  }
}
```
In order to run the dApp you don't have to change anything in the `tronbox.js` file.

### Use your own private network

`tronbox migrate` by default will use the `development` network that is set to use Tron Quickstart. In order to test the smart contracts and deploy them you must install Tron Quickstart.

1. [Install Docker](https://docs.docker.com/install/).

2. Run Tron Quickstart:
```
docker run -it --rm -p 9090:9090 --name tron trontools/quickstart
```

### TronBox commands
```
tronbox compile
tronbox migrate --reset  --network nile
tronbox test  --network nile
```

### Run the example dApp on Shasta

1. You need an account with some Shasta/Nile TRX.

2. If you don't have a Tron wallet, install the Chrome Extension version of TronLink, from https://www.tronlink.org/ and create an account.

3. Click the TronLink extension, click on Settings and Node Manage and select Shasta/Nile.

4. If you don't have any Nile TRX, open https://nileex.io/join/getJoinPage and for Shasta TRX, open https://www.trongrid.io/faucet and require some Nile TRX at the bottom of the page.

5. Add a file called `.env` in the root of this repo and edit it, adding a line with your Private Key, somethink like:
 ```
 export PRIVATE_KEY_SHASTA=0000000000000000000000000000000000000000000000000000000000000001
 export PRIVATE_KEY_Nile=0000000000000000000000000000000000000000000000000000000000000001
 ```
 and save it. You can find an example in `sample-env`.

6. Set the dApp. The dApp needs to know the address where the MetaCoin contract has been deployed. Configure using below command

```
npm run migrate --network nile
```

It will execute the migration, retrieve the contract address and save it in the file `src/js/metacoin-config.js`. This won't work if does not find the `.env` file.

7. Run the dApp:

```
npm run dev
```
It automatically will open the dApp in the default browser.


