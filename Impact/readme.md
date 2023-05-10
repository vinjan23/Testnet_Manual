

```
sudo apt install build-essential
```
```
apt-get install screen git
```
```
sudo apt install --assume-yes git clang curl libssl-dev protobuf-compiler
```

```
sudo apt install --assume-yes git clang curl libssl-dev llvm libudev-dev make protobuf-compiler
```
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
```
source $HOME/.cargo/env
```
```
rustc --version
```
```
rustup default stable
rustup update
```
```
rustup update nightly
rustup target add wasm32-unknown-unknown --toolchain nightly
```
```
rustup show
rustup +nightly show
```

```
ufw allow 22 && ufw allow 30333 && ufw allow 9945 && ufw allow 9933
ufw enable
```

```
git clone https://github.com/GlobalBoost/impactprotocol
```
```
screen -S impact
```

```
cd impactprotocol
```

```
cargo build --release
```
```
./target/release/impact generate-mining-key --chain=impact-testnet
```

```
./target/release/impact import-mining-key "phrase" --base-path /tmp/impactnode --chain=impact-testnet
```
```
./target/release/impact key insert \
  --base-path ~/.impactnode01 \
  --chain=impact-testnet \
  --scheme Ed25519 \
  --suri "CHANGE_THIS_WITH_YOUR_SEED_PHRASE//vj-1///impact" \
  --password-interactive \
  --key-type gran
```

```
./target/release/impact \
--base-path /tmp/impactnode \
--chain=impact-testnet \
--port 30333 \
--ws-port 9945 \
--rpc-port 9933 \
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0" \
--validator \
--author "0xc25c46207a898270d8638585309c6e8e685b0da2147e346e9c0f64151eba6723" \
--rpc-methods Unsafe \
--name yournodename \
--password-interactive
```

```
curl -H "Content-Type: application/json" -d'{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[ ]}' http://localhost:9933
```



