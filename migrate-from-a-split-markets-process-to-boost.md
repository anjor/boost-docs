# Migrate from a split markets process to Boost

If you are running a `markets` service as a separate `lotus-miner` process:

1\. Stop accepting incoming deals

2\. Wait for incoming deals to complete

3\. Shutdown the markets process

4\. Backup the markets repository

5\. Follow [getting-started.md](getting-started.md "mention"), but use `boostd migrate` instead of `boostd init`:

```
boostd --vv migrate \
       --import-markets-repo=~/.my-markets-repo \
       --wallet-publish-storage-deals=$PUBLISH_STORAGE_DEALS_WALLET \
       --wallet-collateral-pledge=$PLEDGE_COLLAT_WALLET \
       --max-staging-deals-bytes=50000000000
```

The migrate command

* Initializes a Boost repository
* Migrates markets datastore keys to Boost
  * Storage and retrieval deal metadata
  * Storage and retrieval ask data
* Migrates markets libp2p keys to Boost
* Migrates markets config to Boost (libp2p endpoints, settings etc)
* Migrates the markets DAG store to Boost