<img src="https://raw.githubusercontent.com/NethermindEth/warp/main/resources/WARP.svg" width="900" height="512" />

# Warp

Warp brings Solidity to StarkNet, making it possible to transpile Ethereum
smart contracts to StarkNet Cairo Contracts.

## Installation :gear:

### Dependencies

<hr> 
 
1. You will need [z3](https://github.com/Z3Prover/z3) installed to use Warp.
  
- Install command on macOS:
```bash
brew install z3
```

- Install command on Ubuntu:

```bash
sudo apt install libz3-dev
```

2. Have Python 3.7 installed with the virtualenv ([`venv`](https://docs.python.org/3/library/venv.html)) module in your base env.

<br>

### Warp Installation Method 1:

<hr> 
Without any virtual environment activated perform the following in order:

1. Add the warp package from npm.

```bash
yarn global add @nethermindeth/warp
```

2. Ensure the package was added by checking the version number:

```bash
warp version
```

3. Install the dependencies:

```bash
warp install
```

4. Test the installation worked by transpiling an example ERC20 contract:

```bash
warp transpile example_contracts/ERC20.sol
```

<br>

### Warp Installation Method 2 (from source/for devs):

<hr>

Make sure you have the [dependencies](#dependencies) installed first.

With a virtual environment (recommended Python3.7) activated:

1. Clone this repo and change directory into the `warp` folder.

2. Install the JavaScript dependencies:

```bash
yarn
```

3. Install the Python dependencies:

```bash
pip install -r requirements.txt
```

If you are using a M1 chipped Mac and getting a `'gmp.h' file not found` error when installing Cairo run the following:

```bash
CFLAGS=-Ibrew --prefix gmp/include LDFLAGS=-Lbrew --prefix gmp/lib pip install ecdsa fastecdsa sympy
```

Then run the pip command above again.

4. Compile the project:

```bash
yarn tsc
yarn warplib
```

5. Test the installation worked by transpiling an example ERC20 contract:

```bash
bin/warp transpile example_contracts/ERC20.sol
```

## Usage :computer:

If you have used installation method 1 you can use the `warp` command in any folder. If you have used installation method 2, you will have to specify the path to the warp directory followed by `bin/warp` e.g `path_to_warp_repo/bin/warp ...`

### CLI Commands

<hr> 
To transpile a contract:

```bash
warp transpile <path to Solidity contract>
```

To deploy a Cairo contract:

```bash
warp deploy <path to Cairo contract>
```

The deploy command will generate the compiled json file as well as the abi json file.

<br>

### Libraries

<hr> 
Libraries are bundled into the point of use, therefore if you try transpile a standalone library it will result in no output.  If you would like to transpile and deploy a standalone library please alter its declaration to `contract`.

<br>

### Unsupported Solidity Features

<hr> 
Several features of Solidity are not supported/do not have analogs in Starknet yet.
We will try our best to add these features as StarkNet supports them, but some may not be
possible due to fundamental differences in the platforms.

Please see the list below:

|           Support Status            |      Symbol       |
| :---------------------------------: | :---------------: |
|   Will likely never be supported    |        :x:        |
|    Being developed/investigated     | :hammer_and_pick: |
| Currently Unknown/If added in Cairo |    :question:     |

|                      Solidity                       |  Support Status   |
| :-------------------------------------------------: | :---------------: |
|            fallback functions with args             | :hammer_and_pick: |
|                   delegate calls                    | :hammer_and_pick: |
|                   low level calls                   |        :x:        |
|                 indexed parameters                  |    :question:     |
|              abi methods (abi.encode)               |    :question:     |
|              nested tuple expressions               |    :question:     |
|                typeName expressions                 |    :question:     |
|                      gasleft()                      |    :question:     |
|                      msg.value                      |    :question:     |
|                       msg.sig                       |    :question:     |
|                      msg.data                       |    :question:     |
|                     tx.gasprice                     |    :question:     |
|                      tx.origin                      |    :question:     |
|                      try/catch                      |    :question:     |
|                   block.coinbase                    |    :question:     |
|                   block.gaslimit                    |    :question:     |
|                    block.basefee                    |    :question:     |
|                    block.chainid                    |    :question:     |
|                  block.difficulty                   |        :x:        |
|         precompiles (apart from ecrecover)          |    :question:     |
|                    selfdestruct                     |    :question:     |
|                      blockhash                      |    :question:     |
|                  functions as data                  |    :question:     |
|           sha256 (use keccak256 instead)            |        :x:        |
|                  ternary operator                   |    :question:     |
|                       receive                       |    :question:     |
|                     Yul Blocks                      |    :question:     |
|                 user defined errors                 |    :question:     |
|   function call options e.g x.f{gas: 10000}(arg1)   |    :question:     |
| member access of address object e.g address.balance |    :question:     |
|              nested tuple assignments               |    :question:     |

Note: We have changed the return of `ecrecover` to be `uint160` because we use the `address` type for StarkNet addresses.

## Docker :whale:

Build the image from source:

```bash
docker build -t warp .
```

Run the container with the same options and arguments as the Warp binary:

```bash
docker run --rm -v $PWD:/dapp --user $(id -u):$(id -g) warp transpile example_contracts/ERC20.sol
```

## Testing for contributors :stethoscope:

To test that your contribution doesn't break any features you can test that all previous example contracts transpile and then cairo compile by running the following:

```bash
warp test
```

For this to work, you must have the cairo-lang package installed.
To test try:

```bash
starknet-compile -v
```

Instructions to set this up can be found at
https://www.cairo-lang.org/docs/quickstart.html

Then to see that your contribution doesn't break the behaviour tests follow these steps:

1. Run the setup script:

```bash
tests/behaviour/setup.sh
```

2. In a separate terminal, start a StarkNet testnet server (in an environment with cairo-lang installed):

```bash
yarn testnet
```

3. Run the tests:

```bash
yarn test
```

To generate benchmarks locally during development:

```bash
yarn testnet:benchmark
yarn test
```

```python
python starknet-testnet/generateMarkdown.py
```

This saves the benchmarks at `benchmark/stats/data.md`

## Contact Us :phone:

If you run into any problems please raise an issue or contact us on our Nethermind discord server: https://discord.com/invite/PaCMRFdvWT
