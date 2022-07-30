export PATH=$PATH:$(go env GOPATH)/bin

evmosd config chain-id blockx_12346-1
evmosd config keyring-backend file

evmosd keys add LY --recover --keyring-backend file

evmosd init LY_VPS_NODE --chain-id blockx_12346-1 --keyring-backend file
evmosd init LY_AWS_NODE --chain-id blockx_12346-1 --keyring-backend file

cp ~/keyring-file/ .

evmosd add-genesis-account LY 100000000000000000000000000aevmos --keyring-backend=file --chain-id="blockx_12346-1"
evmosd gentx LY 1000000000000000000000aevmos --keyring-backend=file --chain-id="blockx_12346-1"
evmosd collect-gentxs

evmosd validate-genesis

evmosd start --chain-id blockx_12346-1 --keyring-backend file


# check balance and send transaction
evmosd query bank balances evmos1k92f3wtyh9jwd4dcqj67x4zek0ltx88hulphp5 --chain-id blockx_12346-1 --keyring-backend file

evmosd tx bank send evmos1k92f3wtyh9jwd4dcqj67x4zek0ltx88hulphp5 evmos143sfffxnqaz7xkfc6uu438zkagsh5d3phwzuz3 1000000000000000000000aevmos --chain-id blockx_12346-1 --keyring-backend file

evmosd query bank balances evmos143sfffxnqaz7xkfc6uu438zkagsh5d3phwzuz3 --chain-id blockx_12346-1 --keyring-backend file

# check node id
evmosd tendermint show-node-id
a7c1cc2f98bed1e89b22b4a09cbdeac4ecd4e987@95.216.251.219:26656

# other node start
config.toml update: persistant peers:
a7c1cc2f98bed1e89b22b4a09cbdeac4ecd4e987@95.216.251.219:26656

# account
evmosd keys add LY2 --recover

evmosd tx bank send evmos143sfffxnqaz7xkfc6uu438zkagsh5d3phwzuz3 evmos1k92f3wtyh9jwd4dcqj67x4zek0ltx88hulphp5 1973460aevmos --chain-id blockx_12346-1 --keyring-backend file


# create validator
evmosd tx staking create-validator   --amount=100000000000000000000aevmos   --pubkey=$(evmosd tendermint show-validator)   --moniker="LY_VPS_NODE"   --chain-id="blockx_12346-1"   --commission-rate="0.10"   --commission-max-rate="0.20"   --commission-max-change-rate="0.01"   --min-self-delegation="1000000"     --from=LY2 --keyring-backend=file --gas="400000"   --gas-prices="0.025aevmos" 


# find my validator pubkey
evmosd tendermint show-validator --chain-id blockx_12346-1 --keyring-backend file

# check validator running
evmosd query tendermint-validator-set | grep "$(evmosd tendermint show-address)"

# LY
roast siege fork over hello addict vintage fossil meadow soda canvas average busy bundle public frost crime soccer horn piano harsh end rail run

# LY2
verb iron mass replace current wood monitor vote consider food network mistake left sudden summer chest moral sell actor universe dinosaur choose empty lift