# Chihuahua - angryandy Testnet

The purpose of this testnet is to simulate the future upgrade to v1.0.1

To simulate the correct upgrade with the 5% min commission we ask a few validators to set up a commission below 5% make sure to coordinate with other participants on the dedicated Discord channel #testnet.

We will wait for at least 5 gentx before launching the testnet and therefore update the draft_genesis.json with the last one and coordinate the launch.

Before launching this README will be updated with a full list of instruction.


## Setup

**Prerequisites:** Make sure to have [Golang >=1.17.5](https://golang.org/).

#### Build from source

You need to ensure your gopath configuration is correct. If the following **'make'** step does not work then you might have to add these lines to your .profile or .zshrc in the users home folder:

```sh
nano ~/.profile
```

```
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
```

Source update .profile

```sh
source .profile
```

```sh
git clone https://github.com/ChihuahuaChain/chihuahua
cd chihuahua
git checkout main
make build && make install
```

This will build and install `chihuahuad` binary into `$GOBIN`.

Note: When building from source, it is important to have your `$GOPATH` set correctly. When in doubt, the following should do:

```sh
mkdir ~/go
export GOPATH=~/go
```

## Setup validator node

Below are the instructions to generate & submit your genesis transaction

### Generate genesis transaction (gentx)

1. Initialize Chihuahua directories and create the local genesis file with the correct chain-id:

   ```bash
   chihuahuad init <moniker-name> --chain-id=angryandy-1
   ```

2. Create a local key pair:

   ```sh
   > chihuahuad keys add <key-name>
   ```

3. Add your account to your local genesis file with a given amount and the key you just created. Use only `10000000000uhuatest`, other amounts will be ignored.

   ```bash
   chihuahuad add-genesis-account $(chihuahuad keys show <key-name> -a) 10000000000uhuatest
   ```

4. Create the gentx, use only `9000000000uhuatest`:

   ```bash
   chihuahuad gentx <key-name> 9000000000uhuatest --chain-id=angryandy-1
   ```

   If all goes well, you will see a message similar to the following:

   ```bash
   Genesis transaction written to "/home/user/.chihuahua/config/gentx/gentx-******.json"
   ```

### Submit genesis transaction

- Fork [the testnets repo](https://github.com/ChihuahuaChain/testnets) into your Github account

- Clone your repo using

  ```bash
  git clone https://github.com/<your-github-username>/testnets
  ```

- Copy the generated gentx json file to `<repo_path>/angryandy/gentx/`

  ```sh
  > cd testnets
  > cp ~/.chihuahua/config/gentx/gentx*.json ./angryandy/gentx/
  ```

- Commit and push to your repo
- Create a PR onto https://github.com/ChihuahuaChain/testnets
- Only PRs from individuals / groups with a history successfully running nodes will be accepted. This is to ensure the network successfully starts on time.

If you have not installed cosmovisor, create a systemd file for your Chihuahua service:

```sh
sudo nano /etc/systemd/system/chihuahuad.service
```

Copy and paste the following and update `<YOUR_USERNAME>` and `<CHAIN_ID>`:

```sh
Description=Chihuahuad Service
After=network-online.target

[Service]
User=<YOUR_USERNAME>
ExecStart=/home/<YOUR_USERNAME>/go/bin/chihuahuad start
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
```

Enable and start the new service:

```sh
sudo systemctl enable chihuahuad
sudo systemctl start chihuahuad
```

Check status:

```sh
chihuahuad status
```

Check logs:

```sh
journalctl -u chihuahuad -f
```
