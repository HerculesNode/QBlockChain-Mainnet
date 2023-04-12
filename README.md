<h1 align="center"> <img src="https://user-images.githubusercontent.com/101635385/231371153-e2f0574f-80be-40f8-8b2d-e470108bbd48.png" width="350"></h1>
<h1 align="center"> QBlockChain-Mainnet </h1>
<h1 align="center"> Hercules
</h1>


## System requirements

* Linux machine with SSH access;
* Min. 1(v)Core (x86), 20 GB storage and 2 GB RAM;
* Rec. 2(v)Cores (x86), 30 GB storage and 4 GB RAM;
* Installed applications: docker, docker-compose, git (optional).

<hr>

## 1- System update
```shell
sudo apt update && sudo apt upgrade -y
```
<hr>

## 2- Libraries install
```shell
apt install ca-certificates curl gnupg lsb-release git htop
```
<hr>

## 3- Docker Setup
```shell

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
apt-get update
apt-get install docker-ce docker-ce-cli containerd.io
docker version
```
<hr>

## 4- Clone the repository
```shell
git clone https://gitlab.com/q-dev/mainnet-public-tools
```

## 5- Creating keystore Folder and pwd.txt File
```shell
cd mainnet-public-tools/validator/
```

```shell
mkdir keystore
```

* Type the password you will use in your wallet here. 
```shell
echo YOUR-PASSWORD > keystore/pwd.txt
```
* Now we have created the keystore folder and our pwd.txt file. Now it's time to create a wallet.

* " If you have a UTC file you have used before, put it into the keystore folder and do not do the wallet creation step. If you are going to create a new wallet, you need to do this step."

## 6- Creating a wallet
```shell
docker run --entrypoint="" --rm -v $PWD:/data -it qblockchain/q-client:1.2.3 geth account new --datadir=/data --password=/data/keystore/pwd.txt
```

* If you get the following output after using this command, everything is fine. Your wallet has been created. 

![image](https://user-images.githubusercontent.com/101635385/231374987-ca342387-5149-4898-bbb1-654dba9613c6.png)

* Write save  your wallet address. Your Keystore folder should look like this.

![image](https://user-images.githubusercontent.com/101635385/231375507-601a937b-1734-4753-9262-956bebb376e2.png)



## 7- Edit our .env file
```shell
cp .env.example .env
```

```shell
nano .env
```

* Edit your .env file as shown in the picture. 
* do not write 0x at the beginning of your wallet address

![image](https://user-images.githubusercontent.com/101635385/231376152-6407d925-84aa-47f2-93a9-baba37e60128.png)


## 8- Edit our docker-compose
```shell
nano docker-compose.yaml
```

![image](https://user-images.githubusercontent.com/101635385/231377206-5acdca81-c109-4f5e-8d18-f50d05e8c0f5.png)


* To see your node on the statistics page, add the following code to your docker-compose file as shown in the image and save it. 

* Open a ticket on the Q Discord channel. Request the access key for the mainnet.

```shell
"--ethstats=<Your_Validator_Name>:<Mainnet_access_key>@stats.q.org",
```


## 9- Edit our config.json
```shell
nano config.json
```

* Enter your wallet address and password here. Do not put 0x at the beginning of your wallet address

![image](https://user-images.githubusercontent.com/101635385/231378686-373b374e-71d0-4e0d-adf4-282e2771fa2e.png)


## 10-Start our Node.
```shell
docker-compose up -d
```

## 11-Log 
```shell
docker-compose logs -f --tail "100"
```

![image](https://user-images.githubusercontent.com/101635385/231379628-58282d75-e524-491b-ae99-973c04b0fc30.png)



## 12- let's fast synchronise our node.

* Let's exit the log screen with ctrl + c. 
* Enter the following codes one by one.

```shell
docker-compose down && cd
```
```shell
rm -rf /var/lib/docker/volumes/validator_validator-node-data/_data/geth/chaindata
```
```shell
mkdir /var/lib/docker/volumes/validator_validator-node-data/_data/geth/chaindata
```
```shell
cd /var/lib/docker/volumes/validator_validator-node-data/_data/geth/chaindata
```

https://snapshots.stakecraft.com/
go to this address and copy the link shown in the picture.

![image](https://user-images.githubusercontent.com/101635385/231380301-5c0ec02b-88b5-4a83-a54a-a6d7a27b300e.png)


* copy the link you copied from the code below " https://snapshots.stakecraft.com/q-mainnet_2023-04-11.tar " in this section and enter the code.

* After this process a loading will take place, please wait until it is finished.  Then start your node.

```shell
wget -O - https://snapshots.stakecraft.com/q-mainnet_2023-04-11.tar | tar xf -
```

```shell
cd $HOME/mainnet-public-tools/validator/
```

```shell
docker-compose up -d
```

<hr>

## 🟢 OmniBridge Setup

```shell
cd mainnet-public-tools
```

```shell
cd mainnet-public-tools/omnibridge-oracle
```
```shell
cp .env.mainnet .env
```
```shell
nano .env
```

* edit and save the following places.

* ORACLE_VALIDATOR_ADDRESS	= type your validator wallet address
* ORACLE_VALIDATOR_ADDRESS_PRIVATE_KEY = type the private key of your wallet address
* COMMON_FOREIGN_RPC_URL = Get eth mainnet RPC from infura, blockpi, Alchemy, ankr or another provider.

```shell
docker-compose up -d
```

## 🟢 OmniBridge-UI Setup

```shell
cd ../omnibridge-ui
```
```shell
cp .env.mainnet .env
```
```shell
nano .env
```

* edit and save the following places.

* REACT_APP_FOREIGN_RPC_URL	= Get eth mainnet RPC from infura, blockpi, Alchemy, ankr or another provider.

```shell
docker-compose up -d
```

## 🟢 Omnibridge-ALM Setup

```shell
cd ../omnibridge-alm
```
```shell
cp .env.mainnet .env
```
```shell
nano .env
```

* edit and save the following places.

* PORT	= 8090 You can change the port address if you want.
* COMMON_FOREIGN_RPC_URL = Get eth mainnet RPC from infura, blockpi, Alchemy, ankr or another provider.

```shell
docker-compose up -d
```





