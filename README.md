# Suez

Tool for pretty printing and optimizing Lightning Network channels.

![screenshot](screenshot.png)

## Installation

1. Install [poetry](https://python-poetry.org/)
2. `poetry install`
3. `poetry run ./suez`

## Channel fee policy

You can set channel fees by passing `--base-fee` and `--fee-rate` parameters.

For example:

`poetry run ./suez --base-fee 1000 --fee-rate 200`

You can override the channel fee policy by changing the `FeePolicy` class.

Example implementation does the following:

* sets lower fee rate for channels with mostly local balance
* sets higher fee rate for channels with mostly remote balance
* sets medium (close to specified) fee rate for balanced channels

You control the spread via the `--fee-spread` argument. By default `--fee-spread` is set to 0.0 (no spread).

For example:

`poetry run ./suez --base-fee 1000 --fee-rate 500 --fee-spread 1.8`

This will set the fee rate above 500 for channels with mostly remote balance and below 500
for channels with mostly local balance.

## Lightning node support

Currently, Suez supports LND (both via `lncli` and via the REST API) and c-lightning.

By default it uses LND (`lncli`).

You can use it with c-lightning as follows:

`poetry run ./suez --client=c-lightning`

You can connect to LND using the REST API as follows:

`SSL_CERT_FILE=</path/to/tls.cert> poetry run ./suez --client=lnd-rest --client-args=rpcserver=https://<rpc-ip>:<rpc-port> --client-args=macaroonpath=</path/to/admin.macaroon> --client-args=tlscertpath=</path/to/tls.cert>`

If you need to pass additional options to the lncli/lightning-cli you can do so:

(single argument)

`poetry run ./suez --client=c-lightning --client-args=--conf=/usr/local/etc/lightningd-bitcoin.conf`

(multiple arguments)

`poetry run ./suez --client-args=--rpcserver=host:10009 --client-args=--macaroonpath=admin.macaroon --client-args=--tlscertpath=tls.cert`

Adding support requires writing a client similar to `lndclient.py` and instantiating it in `suez.py`.

## Donate

You can tip me some satoshis via [tippin.me/@pavolrusnak](https://tippin.me/@pavolrusnak)

or you can donate via Spontaneous AMP Payment (data field encodes `tip=suez`):

`lncli sendpayment --amt 10000 --amp --dest 0385218f0e307b6a0e989d2a717d346942d96b4fd550e937de5f8ffe1568510a18 --data 7629168=7375657a`

## License

This software is licensed under the [GNU General Public License v3](COPYING).
