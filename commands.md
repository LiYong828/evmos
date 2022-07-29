export PATH=$PATH:$(go env GOPATH)/bin

evmosd config chain-id blockx_12346-1
evmosd config keyring-backend file

evmosd init LY_VPS_NODE --chain-id blockx_12346-1 --keyring-backend file

cp ~/keyring-file/ .

evmosd add-genesis-account LY 10000000000000000000000000aevmos --keyring-backend=file --chain-id="blockx_12346-1"
evmosd gentx LY 100000000000000000000aevmos --keyring-backend=file --chain-id="blockx_12346-1"
evmosd collect-gentxs

evmosd start --chain-id blockx_12346-1 --keyring-backend file


# check balance and send transaction
evmosd query bank balances evmos1k92f3wtyh9jwd4dcqj67x4zek0ltx88hulphp5 --chain-id blockx_12346-1 --keyring-backend file

evmosd tx bank send evmos1k92f3wtyh9jwd4dcqj67x4zek0ltx88hulphp5 evmos143sfffxnqaz7xkfc6uu438zkagsh5d3phwzuz3 100000000000000000000aevmos --chain-id blockx_12346-1 --keyring-backend file

evmosd query bank balances evmos143sfffxnqaz7xkfc6uu438zkagsh5d3phwzuz3 --chain-id blockx_12346-1 --keyring-backend file

# check node id
evmosd tendermint show-node-id
a7c1cc2f98bed1e89b22b4a09cbdeac4ecd4e987@95.216.251.219:26656

# other node start
config.toml update: persistant peers:
a7c1cc2f98bed1e89b22b4a09cbdeac4ecd4e987@95.216.251.219:26656

# account
evmosd keys add LY2 --recover



# create validator
evmosd tx staking create-validator   --amount=100000000000000000000aevmos   --pubkey=$(evmosd tendermint show-validator)   --moniker="LY_AWS_NODE"   --chain-id="blockx_12346-1"   --commission-rate="0.10"   --commission-max-rate="0.20"   --commission-max-change-rate="0.01"   --min-self-delegation="1"   --gas="auto"   --gas-prices="0.025aevmos"   --from=LY2 --keyring-backend=file


# find validator pubkey
evmosd tendermint show-validator --chain-id blockx_12346-1 --keyring-backend file

# check validator running
evmosd query tendermint-validator-set | grep "$(evmosd tendermint show-address)"