# Angry Andy UPGRADE (v1.1.1)

![Angry Andy](https://raw.githubusercontent.com/ChihuahuaChain/testnets/main/angryandy/angryandy.jpg)


Angry Andy upgrade [Proposal Number #3](https://www.mintscan.io/chihuahua/proposals/3)


The Upgrade is scheduled for BLOCK `535000`, should be around _16:00 UTC on January 18, 2022_.

Time is only an estimate and can vary by -/+3 hours, check on the #priv-validators channel on our Discord or check the [upgrade monitor](https://chain-monitor.cros-nest.com/d/Upgrades/upgrades?orgId=1&refresh=1m&var-chain_id=chihuahua-1&var-version=angryandy)

#Using Cosmovisor

```bash
# download the new version

cd ~/chihuahua
git fetch --tags
git checkout v1.1.1
make install

# check the version - should return v1.1.1
chihuahuad version

# create the upgrade directory if you haven't
mkdir -p $DAEMON_HOME/cosmovisor/upgrades/angryandy/bin

# if you are using cosmovisor you then need to copy this new binary
cp /home/<YOUR_USER>/go/bin/chihuahuad $DAEMON_HOME/cosmovisor/upgrades/angryandy/bin

# check the version you're about to run is v1.1.1
$DAEMON_HOME/cosmovisor/upgrades/angryandy/bin/chihuahuad version
```

# For the (lazy) screen lovers

If you are a lazy screen lover and you are not using cosmovisor

```bash
# download the new version
cd ~/chihuahua
git fetch --tags
git checkout v1.1.1
make install

# check the version - should be v1.1.1
chihuahuad version
```

Wait for the upgrade block height 535000 and wait for your node to gracefully stop, take some time to backup your data directory then start the node again.

# Thank you

A special Thank You to Jacob Gadikian (faddat), Lydia Pierce and Evan Forbes for all the codebase cleaning, version bumps and the 5% commission enforcing code.

Thank you to all the validators who helped testing the upgrade before mainnet! Thank You! Woof!

_The spirit of the wolf shall now be released, and by the way, no, Andy is not angry with any of you!_
