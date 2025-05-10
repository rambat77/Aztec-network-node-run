# Aztec-sequencer-node
One-click sequencer node run guide by Aditya and thanks to the aztec team.

Aztec is building a decentralized, privacy-focused network and the sequencer node is a key part of it. Running a sequencer helps produce and propose blocks using regular consumer hardware. This guide will walk you through setting one up on the testnet.

> **Note**  
> There’s no official confirmation of any rewards, airdrop, or incentives. This is purely for learning, contribution and being early in a cutting-edge privacy project.

---

## 💻 System Requirements

| Component      | Specification                      |
|----------------|------------------------------------|
| CPU            | 8-core Processor                   |
| RAM            | 16 GB (6–8 GB can also run it)     |
| Storage        | 1 TB SSD                           |
| Internet Speed | 25 Mbps Upload / Download          |

> **Note**  
> You can start running this node on a `4-core CPU`, `6 GB of RAM` and `25 GB of storage`. However, as uptime increases, it's important to meet the recommended system requirements—otherwise, your node may eventually crash.

---

## 🌐 Rent VPS

> **Note**  
> Renting VPS is not necessarily needed if your main goal is to take the `Apprentice` role on Aztec Discord. You can run this node on WSL for 30 mins to get that role.

---

## ⚙️ Prerequisites

- Get Sepolia Ethereum RPC from [Alchemy](https://dashboard.alchemy.com/apps) or [Infura](https://developer.metamask.io/register).
- Get Consensus URL (Beacon RPC) from [Chainstack](https://chainstack.com/global-nodes).
- Create a new EVM wallet and fund it with **at least 2.5 Sepolia ETH** if you want to register as Validator.

> **IMPORTANT**  
> If you’re using the free version and hit the max request limit on either RPC, upgrade or rotate the endpoints.

---

## 📥 Installation

➤Install `curl` and `wget`:

```bash
command -v curl >/dev/null 2>&1 || apt-get update && apt-get install -y curl; command -v wget >/dev/null 2>&1 || apt-get install -y wget
```
➤Run your Aztec node using one of the following:

```bash
[ -f "aztec.sh" ] && rm aztec.sh; curl -sSL -o aztec.sh https://raw.githubusercontent.com/zunxbt/aztec-sequencer-node/main/aztec.sh && chmod +x aztec.sh && ./aztec.sh
```
or
```bash
[ -f "aztec.sh" ] && rm aztec.sh; wget -q -O aztec.sh https://raw.githubusercontent.com/zunxbt/aztec-sequencer-node/main/aztec.sh && chmod +x aztec.sh && ./aztec.sh
```

## ⚡ Commands

➤View node logs:
```bash
sudo docker logs -f --tail 100 $(docker ps -q --filter ancestor=aztecprotocol/aztec:latest | head -n 1)
```

➤Stop the node:
```bash
sudo docker stop $(docker ps -q --filter ancestor=aztecprotocol/aztec:latest | head -n 1)
```

## 🧩 Post-Installation
➤Note
Wait 10–20 minutes after starting the node before running the next steps.

➤Get block number:
```bash
curl -s -X POST -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","method":"node_getL2Tips","params":[],"id":67}' http://localhost:8080 | jq -r '.result.proven.number'
```

➤Use that block number to get the proof:
```bash
curl -s -X POST -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","method":"node_getArchiveSiblingPath","params":["block-number","block-number"],"id":67}' http://localhost:8080 | jq -r ".result"
```

➤Get the Apprentice role on [Aztec Discord](https://discord.com/invite/aztec):
```bash
/operator start
```
➤Provide your address, block-number, and proof as requested.


## 🚀 Register as Validator
⚠️ You may encounter ValidatorQuotaFilledUntil—convert the Unix timestamp to local time and try again later.

➤Register:
```bash
aztec add-l1-validator \
  --l1-rpc-urls SEPOLIA-RPC-URL \
  --private-key YOUR-PRIVATE-KEY \
  --attester YOUR-VALIDATOR-ADDRESS \
  --proposer-eoa YOUR-VALIDATOR-ADDRESS \
  --staking-asset-handler 0xF739D03e98e23A7B65940848aBA8921fF3bAc4b2 \
  --l1-chain-id 11155111
```
