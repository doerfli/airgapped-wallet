**Important**: This repository is unmaintained. Please fork and update dependencies to safe versions before use. 

# Airgapped wallet

## Enchanted vault

The enchanted vault is a tool to manage your encrypted wallet in an airgapped environment. 
It is a simple cli tool that allows you to create a new wallet, get its address and sign transactions.

### Creating a new wallet

Generate a new random mnemonic

```bash
python enchanted_vault/run.py --generate
```

Generate an encrypted wallet with interactive password prompt

```bash
python enchanted_vault/run.py --initialize 
```

or with a password as an argument

```bash
python enchanted_vault/run.py --initialize -p <your password>
```

### Get the address

```bash
python enchanted_vault/run.py --address
```

### Sign a transaction

```bash
python enchanted_vault/run.py --sign-transaction <transaction_data_json>
```

or if you want to read the transaction data json from a file 

```bash
python enchanted_vault/run.py --sign-transaction --input-file <inputfile>
```

Use `--password` to provide the password as an argument and `--output-file` to write the signed transaction to the given file.

### Use docker image

All the commands can also be run using the container image (`ghcr.io/doerfli/airgapped-wallet/enchanted_vault`).
Instead of `python enchanted_vault/run.py` use 

```bash
docker run -it ghcr.io/doerfli/airgapped-wallet/enchanted_vault:main <command>
```

The other arguments are the same as when running the script directly.
To provide an existing wallet file (or have access to a newly created one), mount the file to the container

```bash
docker run -it -v <path to data directory>:/data ghcr.io/doerfli/airgapped-wallet/enchanted_vault:main <command>
```

Don't forget to log into the ghcr.io registry before pulling the image.

## Other tools

### Forger - safely create a new random wallet 

```bash
python forger.py
```

#### Docker

```bash
docker build -t forger .
docker run -rm forger
```

## Enchanter - get encrypted private key from the mnemonic

```bash
python enchanter.py <your password>
``` 

or 

```bash
cat << EOF | python enchanter.py
<your password>
EOF
```

### Docker

```bash
docker build -t enchanter .
docker run -rm enchanter <your password>
```

or

```bash
docker run -i -rm enchanter
<your password><EOF>
```

### Address weaver - get the address from the mnoemonic

```bash
cat << EOF | python address_weaver.py
<your mnemonic>
EOF
```

or 

```bash
python address_weaver.py <your mnemonic>
```

#### Encrypted private key to address (e.g created via enchanter)

```bash
python address_weaver/enchanted_address_weaver.py <your encrypted key> <your password>
```

#### Docker

```bash
docker build -t address_weaver .
docker run -rm address_weaver <your mnemonic>
```

or 

```bash
docker run -i -rm address_weaver
<your mnemonic><EOF>
```

## Pipenv

Initialize the virtual environment with pipenv

```bash
pipenv shell
```

## References

https://docs.ethers.org/v6/api/wordlists/

https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki
https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki

https://github.com/trezor/python-mnemonic
https://eth-account.readthedocs.io/en/stable/
https://web3py.readthedocs.io/en/stable/
