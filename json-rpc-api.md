---
description: >-
  This page contains all Boost API definitions. Interfaces defined here are
  exposed as JSON-RPC 2.0 endpoints by the boostd daemon.
---

# JSON-RPC API

## Go JSON-RPC client

To use the Boost Go client, the Go RPC-API library can be used to interact with the Boost API node.

1. Import the necessary Go module:

```
go get github.com/filecoin-project/go-jsonrpc
```

1. Create the following script:

```go
package main

import (
    "context"
    "fmt"
    "log"
    "net/http"

    jsonrpc "github.com/filecoin-project/go-jsonrpc"
    boostapi "github.com/filecoin-project/boost/api"
)

func main() {
    authToken := "<value found in ~/.boost/token>"
    headers := http.Header{"Authorization": []string{"Bearer " + authToken}}
    addr := "127.0.0.1:1288"

    var api boostapi.BoostStruct
    closer, err := jsonrpc.NewMergeClient(context.Background(), "ws://"+addr+"/rpc/v0", "Filecoin", []interface{}{&api.Internal, &api.CommonStruct.Internal}, headers)
    if err != nil {
        log.Fatalf("connecting with boost failed: %s", err)
    }
    defer closer()

    // Now you can call any API you're interested in.
    netAddrs, err := api.NetAddrsListen(context.Background())
    if err != nil {
      log.Fatalf("calling netAddrsListen: %s", err)
    }
    fmt.Printf("Boost is listening on: %s", netAddrs.Addrs[0])
}
```

1. Run `go mod init` to set up your `go.mod` file
2. You should now be able to interact with the Boost API.

## Python JSON-RPC client

The JSON-RPC API can also be communicated with programmatically from other languages. Here is an example written in Python. Note that the `method` must be prefixed with `Filecoin.`

```
import requests
import json

def main():
    url = "http://localhost:3051/rpc/v0"
    headers = {'content-type': 'application/json', "Authorization": "Bearer <token>"}
    payload = {
        "method": "Filecoin.BoostOfflineDealWithData",
        "params": [
            "<deal-uuid>",
            "<file-path>",
            True
        ],
        "jsonrpc": "2.0",
        "id": 1,
    }
    response = requests.post(url, data=json.dumps(payload), headers=headers)
    print(response.text)

if __name__ == "__main__":
    main()
```

## Groups

* [Actor](json-rpc-api.md#actor)
  * [ActorSectorSize](json-rpc-api.md#actorsectorsize)
* [Auth](json-rpc-api.md#auth)
  * [AuthNew](json-rpc-api.md#authnew)
  * [AuthVerify](json-rpc-api.md#authverify)
* [Blockstore](json-rpc-api.md#blockstore)
  * [BlockstoreGet](json-rpc-api.md#blockstoreget)
  * [BlockstoreGetSize](json-rpc-api.md#blockstoregetsize)
  * [BlockstoreHas](json-rpc-api.md#blockstorehas)
* [Boost](json-rpc-api.md#boost)
  * [BoostDagstoreDestroyShard](json-rpc-api.md#boostdagstoredestroyshard)
  * [BoostDagstoreGC](json-rpc-api.md#boostdagstoregc)
  * [BoostDagstoreInitializeAll](json-rpc-api.md#boostdagstoreinitializeall)
  * [BoostDagstoreInitializeShard](json-rpc-api.md#boostdagstoreinitializeshard)
  * [BoostDagstoreListShards](json-rpc-api.md#boostdagstorelistshards)
  * [BoostDagstorePiecesContainingMultihash](json-rpc-api.md#boostdagstorepiecescontainingmultihash)
  * [BoostDagstoreRecoverShard](json-rpc-api.md#boostdagstorerecovershard)
  * [BoostDagstoreRegisterShard](json-rpc-api.md#boostdagstoreregistershard)
  * [BoostDeal](json-rpc-api.md#boostdeal)
  * [BoostDealBySignedProposalCid](json-rpc-api.md#boostdealbysignedproposalcid)
  * [BoostDummyDeal](json-rpc-api.md#boostdummydeal)
  * [BoostIndexerAnnounceAllDeals](json-rpc-api.md#boostindexerannouncealldeals)
  * [BoostMakeDeal](json-rpc-api.md#boostmakedeal)
  * [BoostOfflineDealWithData](json-rpc-api.md#boostofflinedealwithdata)
* [Deals](json-rpc-api.md#deals)
  * [DealsConsiderOfflineRetrievalDeals](json-rpc-api.md#dealsconsiderofflineretrievaldeals)
  * [DealsConsiderOfflineStorageDeals](json-rpc-api.md#dealsconsiderofflinestoragedeals)
  * [DealsConsiderOnlineRetrievalDeals](json-rpc-api.md#dealsconsideronlineretrievaldeals)
  * [DealsConsiderOnlineStorageDeals](json-rpc-api.md#dealsconsideronlinestoragedeals)
  * [DealsConsiderUnverifiedStorageDeals](json-rpc-api.md#dealsconsiderunverifiedstoragedeals)
  * [DealsConsiderVerifiedStorageDeals](json-rpc-api.md#dealsconsiderverifiedstoragedeals)
  * [DealsPieceCidBlocklist](json-rpc-api.md#dealspiececidblocklist)
  * [DealsSetConsiderOfflineRetrievalDeals](json-rpc-api.md#dealssetconsiderofflineretrievaldeals)
  * [DealsSetConsiderOfflineStorageDeals](json-rpc-api.md#dealssetconsiderofflinestoragedeals)
  * [DealsSetConsiderOnlineRetrievalDeals](json-rpc-api.md#dealssetconsideronlineretrievaldeals)
  * [DealsSetConsiderOnlineStorageDeals](json-rpc-api.md#dealssetconsideronlinestoragedeals)
  * [DealsSetConsiderUnverifiedStorageDeals](json-rpc-api.md#dealssetconsiderunverifiedstoragedeals)
  * [DealsSetConsiderVerifiedStorageDeals](json-rpc-api.md#dealssetconsiderverifiedstoragedeals)
  * [DealsSetPieceCidBlocklist](json-rpc-api.md#dealssetpiececidblocklist)
* [I](json-rpc-api.md#i)
  * [ID](json-rpc-api.md#id)
* [Log](json-rpc-api.md#log)
  * [LogList](json-rpc-api.md#loglist)
  * [LogSetLevel](json-rpc-api.md#logsetlevel)
* [Market](json-rpc-api.md#market)
  * [MarketCancelDataTransfer](json-rpc-api.md#marketcanceldatatransfer)
  * [MarketDataTransferUpdates](json-rpc-api.md#marketdatatransferupdates)
  * [MarketGetAsk](json-rpc-api.md#marketgetask)
  * [MarketGetRetrievalAsk](json-rpc-api.md#marketgetretrievalask)
  * [MarketImportDealData](json-rpc-api.md#marketimportdealdata)
  * [MarketListDataTransfers](json-rpc-api.md#marketlistdatatransfers)
  * [MarketListIncompleteDeals](json-rpc-api.md#marketlistincompletedeals)
  * [MarketListRetrievalDeals](json-rpc-api.md#marketlistretrievaldeals)
  * [MarketPendingDeals](json-rpc-api.md#marketpendingdeals)
  * [MarketRestartDataTransfer](json-rpc-api.md#marketrestartdatatransfer)
  * [MarketSetAsk](json-rpc-api.md#marketsetask)
  * [MarketSetRetrievalAsk](json-rpc-api.md#marketsetretrievalask)
* [Net](json-rpc-api.md#net)
  * [NetAddrsListen](json-rpc-api.md#netaddrslisten)
  * [NetAgentVersion](json-rpc-api.md#netagentversion)
  * [NetAutoNatStatus](json-rpc-api.md#netautonatstatus)
  * [NetBandwidthStats](json-rpc-api.md#netbandwidthstats)
  * [NetBandwidthStatsByPeer](json-rpc-api.md#netbandwidthstatsbypeer)
  * [NetBandwidthStatsByProtocol](json-rpc-api.md#netbandwidthstatsbyprotocol)
  * [NetBlockAdd](json-rpc-api.md#netblockadd)
  * [NetBlockList](json-rpc-api.md#netblocklist)
  * [NetBlockRemove](json-rpc-api.md#netblockremove)
  * [NetConnect](json-rpc-api.md#netconnect)
  * [NetConnectedness](json-rpc-api.md#netconnectedness)
  * [NetDisconnect](json-rpc-api.md#netdisconnect)
  * [NetFindPeer](json-rpc-api.md#netfindpeer)
  * [NetLimit](json-rpc-api.md#netlimit)
  * [NetPeerInfo](json-rpc-api.md#netpeerinfo)
  * [NetPeers](json-rpc-api.md#netpeers)
  * [NetPing](json-rpc-api.md#netping)
  * [NetProtectAdd](json-rpc-api.md#netprotectadd)
  * [NetProtectList](json-rpc-api.md#netprotectlist)
  * [NetProtectRemove](json-rpc-api.md#netprotectremove)
  * [NetPubsubScores](json-rpc-api.md#netpubsubscores)
  * [NetSetLimit](json-rpc-api.md#netsetlimit)
  * [NetStat](json-rpc-api.md#netstat)
* [Online](json-rpc-api.md#online)
  * [OnlineBackup](json-rpc-api.md#onlinebackup)
* [Pieces](json-rpc-api.md#pieces)
  * [PiecesGetCIDInfo](json-rpc-api.md#piecesgetcidinfo)
  * [PiecesGetMaxOffset](json-rpc-api.md#piecesgetmaxoffset)
  * [PiecesGetPieceInfo](json-rpc-api.md#piecesgetpieceinfo)
  * [PiecesListCidInfos](json-rpc-api.md#pieceslistcidinfos)
  * [PiecesListPieces](json-rpc-api.md#pieceslistpieces)
* [Runtime](json-rpc-api.md#runtime)
  * [RuntimeSubsystems](json-rpc-api.md#runtimesubsystems)
* [Sectors](json-rpc-api.md#sectors)
  * [SectorsRefs](json-rpc-api.md#sectorsrefs)

## Actor

### ActorSectorSize

There are not yet any comments for this method.

Perms: read

Inputs:

```json
[
  "f01234"
]
```

Response: `34359738368`

## Auth

### AuthNew

Perms: admin

Inputs:

```json
[
  [
    "write"
  ]
]
```

Response: `"Ynl0ZSBhcnJheQ=="`

### AuthVerify

Perms: read

Inputs:

```json
[
  "string value"
]
```

Response:

```json
[
  "write"
]
```

## Blockstore

### BlockstoreGet

There are not yet any comments for this method.

Perms: read

Inputs:

```json
[
  {
    "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
  }
]
```

Response: `"Ynl0ZSBhcnJheQ=="`

### BlockstoreGetSize

Perms: read

Inputs:

```json
[
  {
    "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
  }
]
```

Response: `123`

### BlockstoreHas

Perms: read

Inputs:

```json
[
  {
    "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
  }
]
```

Response: `true`

## Boost

### BoostDagstoreDestroyShard

Perms: admin

Inputs:

```json
[
  "string value"
]
```

Response: `{}`

### BoostDagstoreGC

Perms: admin

Inputs: `null`

Response:

```json
[
  {
    "Key": "baga6ea4seaqecmtz7iak33dsfshi627abz4i4665dfuzr3qfs4bmad6dx3iigdq",
    "Success": false,
    "Error": "\u003cerror\u003e"
  }
]
```

### BoostDagstoreInitializeAll

Perms: admin

Inputs:

```json
[
  {
    "MaxConcurrency": 123,
    "IncludeSealed": true
  }
]
```

Response:

```json
{
  "Key": "string value",
  "Event": "string value",
  "Success": true,
  "Error": "string value",
  "Total": 123,
  "Current": 123
}
```

### BoostDagstoreInitializeShard

Perms: admin

Inputs:

```json
[
  "string value"
]
```

Response: `{}`

### BoostDagstoreListShards

Perms: admin

Inputs: `null`

Response:

```json
[
  {
    "Key": "baga6ea4seaqecmtz7iak33dsfshi627abz4i4665dfuzr3qfs4bmad6dx3iigdq",
    "State": "ShardStateAvailable",
    "Error": "\u003cerror\u003e"
  }
]
```

### BoostDagstorePiecesContainingMultihash

Perms: read

Inputs:

```json
[
  "Bw=="
]
```

Response:

```json
[
  {
    "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
  }
]
```

### BoostDagstoreRecoverShard

Perms: admin

Inputs:

```json
[
  "string value"
]
```

Response: `{}`

### BoostDagstoreRegisterShard

Perms: admin

Inputs:

```json
[
  "string value"
]
```

Response: `{}`

### BoostDeal

Perms: admin

Inputs:

```json
[
  "07070707-0707-0707-0707-070707070707"
]
```

Response:

```json
{
  "DealUuid": "07070707-0707-0707-0707-070707070707",
  "CreatedAt": "0001-01-01T00:00:00Z",
  "ClientDealProposal": {
    "Proposal": {
      "PieceCID": {
        "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
      },
      "PieceSize": 1032,
      "VerifiedDeal": true,
      "Client": "f01234",
      "Provider": "f01234",
      "Label": "",
      "StartEpoch": 10101,
      "EndEpoch": 10101,
      "StoragePricePerEpoch": "0",
      "ProviderCollateral": "0",
      "ClientCollateral": "0"
    },
    "ClientSignature": {
      "Type": 2,
      "Data": "Ynl0ZSBhcnJheQ=="
    }
  },
  "IsOffline": true,
  "CleanupData": true,
  "ClientPeerID": "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf",
  "DealDataRoot": {
    "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
  },
  "InboundFilePath": "string value",
  "Transfer": {
    "Type": "string value",
    "ClientID": "string value",
    "Params": "Ynl0ZSBhcnJheQ==",
    "Size": 42
  },
  "ChainDealID": 5432,
  "PublishCID": null,
  "SectorID": 9,
  "Offset": 1032,
  "Length": 1032,
  "Checkpoint": 1,
  "CheckpointAt": "0001-01-01T00:00:00Z",
  "Err": "string value",
  "Retry": "auto",
  "NBytesReceived": 9,
  "FastRetrieval": true,
  "AnnounceToIPNI": true
}
```

### BoostDealBySignedProposalCid

Perms: admin

Inputs:

```json
[
  {
    "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
  }
]
```

Response:

```json
{
  "DealUuid": "07070707-0707-0707-0707-070707070707",
  "CreatedAt": "0001-01-01T00:00:00Z",
  "ClientDealProposal": {
    "Proposal": {
      "PieceCID": {
        "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
      },
      "PieceSize": 1032,
      "VerifiedDeal": true,
      "Client": "f01234",
      "Provider": "f01234",
      "Label": "",
      "StartEpoch": 10101,
      "EndEpoch": 10101,
      "StoragePricePerEpoch": "0",
      "ProviderCollateral": "0",
      "ClientCollateral": "0"
    },
    "ClientSignature": {
      "Type": 2,
      "Data": "Ynl0ZSBhcnJheQ=="
    }
  },
  "IsOffline": true,
  "CleanupData": true,
  "ClientPeerID": "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf",
  "DealDataRoot": {
    "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
  },
  "InboundFilePath": "string value",
  "Transfer": {
    "Type": "string value",
    "ClientID": "string value",
    "Params": "Ynl0ZSBhcnJheQ==",
    "Size": 42
  },
  "ChainDealID": 5432,
  "PublishCID": null,
  "SectorID": 9,
  "Offset": 1032,
  "Length": 1032,
  "Checkpoint": 1,
  "CheckpointAt": "0001-01-01T00:00:00Z",
  "Err": "string value",
  "Retry": "auto",
  "NBytesReceived": 9,
  "FastRetrieval": true,
  "AnnounceToIPNI": true
}
```

### BoostDummyDeal

Perms: admin

Inputs:

```json
[
  {
    "DealUUID": "07070707-0707-0707-0707-070707070707",
    "IsOffline": true,
    "ClientDealProposal": {
      "Proposal": {
        "PieceCID": {
          "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
        },
        "PieceSize": 1032,
        "VerifiedDeal": true,
        "Client": "f01234",
        "Provider": "f01234",
        "Label": "",
        "StartEpoch": 10101,
        "EndEpoch": 10101,
        "StoragePricePerEpoch": "0",
        "ProviderCollateral": "0",
        "ClientCollateral": "0"
      },
      "ClientSignature": {
        "Type": 2,
        "Data": "Ynl0ZSBhcnJheQ=="
      }
    },
    "DealDataRoot": {
      "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
    },
    "Transfer": {
      "Type": "string value",
      "ClientID": "string value",
      "Params": "Ynl0ZSBhcnJheQ==",
      "Size": 42
    },
    "RemoveUnsealedCopy": true,
    "SkipIPNIAnnounce": true
  }
]
```

Response:

```json
{
  "Accepted": true,
  "Reason": "string value"
}
```

### BoostIndexerAnnounceAllDeals

There are not yet any comments for this method.

Perms: admin

Inputs: `null`

Response: `{}`

### BoostMakeDeal

Perms: write

Inputs:

```json
[
  {
    "DealUUID": "07070707-0707-0707-0707-070707070707",
    "IsOffline": true,
    "ClientDealProposal": {
      "Proposal": {
        "PieceCID": {
          "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
        },
        "PieceSize": 1032,
        "VerifiedDeal": true,
        "Client": "f01234",
        "Provider": "f01234",
        "Label": "",
        "StartEpoch": 10101,
        "EndEpoch": 10101,
        "StoragePricePerEpoch": "0",
        "ProviderCollateral": "0",
        "ClientCollateral": "0"
      },
      "ClientSignature": {
        "Type": 2,
        "Data": "Ynl0ZSBhcnJheQ=="
      }
    },
    "DealDataRoot": {
      "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
    },
    "Transfer": {
      "Type": "string value",
      "ClientID": "string value",
      "Params": "Ynl0ZSBhcnJheQ==",
      "Size": 42
    },
    "RemoveUnsealedCopy": true,
    "SkipIPNIAnnounce": true
  }
]
```

Response:

```json
{
  "Accepted": true,
  "Reason": "string value"
}
```

### BoostOfflineDealWithData

Perms: admin

Inputs:

```json
[
  "07070707-0707-0707-0707-070707070707",
  "string value",
  true
]
```

Response:

```json
{
  "Accepted": true,
  "Reason": "string value"
}
```

## Deals

### DealsConsiderOfflineRetrievalDeals

Perms: admin

Inputs: `null`

Response: `true`

### DealsConsiderOfflineStorageDeals

Perms: admin

Inputs: `null`

Response: `true`

### DealsConsiderOnlineRetrievalDeals

Perms: admin

Inputs: `null`

Response: `true`

### DealsConsiderOnlineStorageDeals

There are not yet any comments for this method.

Perms: admin

Inputs: `null`

Response: `true`

### DealsConsiderUnverifiedStorageDeals

Perms: admin

Inputs: `null`

Response: `true`

### DealsConsiderVerifiedStorageDeals

Perms: admin

Inputs: `null`

Response: `true`

### DealsPieceCidBlocklist

Perms: admin

Inputs: `null`

Response:

```json
[
  {
    "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
  }
]
```

### DealsSetConsiderOfflineRetrievalDeals

Perms: admin

Inputs:

```json
[
  true
]
```

Response: `{}`

### DealsSetConsiderOfflineStorageDeals

Perms: admin

Inputs:

```json
[
  true
]
```

Response: `{}`

### DealsSetConsiderOnlineRetrievalDeals

Perms: admin

Inputs:

```json
[
  true
]
```

Response: `{}`

### DealsSetConsiderOnlineStorageDeals

Perms: admin

Inputs:

```json
[
  true
]
```

Response: `{}`

### DealsSetConsiderUnverifiedStorageDeals

Perms: admin

Inputs:

```json
[
  true
]
```

Response: `{}`

### DealsSetConsiderVerifiedStorageDeals

Perms: admin

Inputs:

```json
[
  true
]
```

Response: `{}`

### DealsSetPieceCidBlocklist

Perms: admin

Inputs:

```json
[
  [
    {
      "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
    }
  ]
]
```

Response: `{}`

## I

### ID

Perms: read

Inputs: `null`

Response: `"12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf"`

## Log

### LogList

Perms: write

Inputs: `null`

Response:

```json
[
  "string value"
]
```

### LogSetLevel

Perms: write

Inputs:

```json
[
  "string value",
  "string value"
]
```

Response: `{}`

## Market

### MarketCancelDataTransfer

Perms: write

Inputs:

```json
[
  3,
  "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf",
  true
]
```

Response: `{}`

### MarketDataTransferUpdates

Perms: write

Inputs: `null`

Response:

```json
{
  "TransferID": 3,
  "Status": 1,
  "BaseCID": {
    "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
  },
  "IsInitiator": true,
  "IsSender": true,
  "Voucher": "string value",
  "Message": "string value",
  "OtherPeer": "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf",
  "Transferred": 42,
  "Stages": {
    "Stages": [
      {
        "Name": "string value",
        "Description": "string value",
        "CreatedTime": "0001-01-01T00:00:00Z",
        "UpdatedTime": "0001-01-01T00:00:00Z",
        "Logs": [
          {
            "Log": "string value",
            "UpdatedTime": "0001-01-01T00:00:00Z"
          }
        ]
      }
    ]
  }
}
```

### MarketGetAsk

Perms: read

Inputs: `null`

Response:

```json
{
  "Ask": {
    "Price": "0",
    "VerifiedPrice": "0",
    "MinPieceSize": 1032,
    "MaxPieceSize": 1032,
    "Miner": "f01234",
    "Timestamp": 10101,
    "Expiry": 10101,
    "SeqNo": 42
  },
  "Signature": {
    "Type": 2,
    "Data": "Ynl0ZSBhcnJheQ=="
  }
}
```

### MarketGetRetrievalAsk

Perms: read

Inputs: `null`

Response:

```json
{
  "PricePerByte": "0",
  "UnsealPrice": "0",
  "PaymentInterval": 42,
  "PaymentIntervalIncrease": 42
}
```

### MarketImportDealData

Perms: write

Inputs:

```json
[
  {
    "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
  },
  "string value"
]
```

Response: `{}`

### MarketListDataTransfers

Perms: write

Inputs: `null`

Response:

```json
[
  {
    "TransferID": 3,
    "Status": 1,
    "BaseCID": {
      "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
    },
    "IsInitiator": true,
    "IsSender": true,
    "Voucher": "string value",
    "Message": "string value",
    "OtherPeer": "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf",
    "Transferred": 42,
    "Stages": {
      "Stages": [
        {
          "Name": "string value",
          "Description": "string value",
          "CreatedTime": "0001-01-01T00:00:00Z",
          "UpdatedTime": "0001-01-01T00:00:00Z",
          "Logs": [
            {
              "Log": "string value",
              "UpdatedTime": "0001-01-01T00:00:00Z"
            }
          ]
        }
      ]
    }
  }
]
```

### MarketListIncompleteDeals

Perms: read

Inputs: `null`

Response:

```json
[
  {
    "Proposal": {
      "PieceCID": {
        "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
      },
      "PieceSize": 1032,
      "VerifiedDeal": true,
      "Client": "f01234",
      "Provider": "f01234",
      "Label": "",
      "StartEpoch": 10101,
      "EndEpoch": 10101,
      "StoragePricePerEpoch": "0",
      "ProviderCollateral": "0",
      "ClientCollateral": "0"
    },
    "ClientSignature": {
      "Type": 2,
      "Data": "Ynl0ZSBhcnJheQ=="
    },
    "ProposalCid": {
      "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
    },
    "AddFundsCid": null,
    "PublishCid": null,
    "Miner": "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf",
    "Client": "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf",
    "State": 42,
    "PiecePath": ".lotusminer/fstmp123",
    "MetadataPath": ".lotusminer/fstmp123",
    "SlashEpoch": 10101,
    "FastRetrieval": true,
    "Message": "string value",
    "FundsReserved": "0",
    "Ref": {
      "TransferType": "string value",
      "Root": {
        "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
      },
      "PieceCid": null,
      "PieceSize": 1024,
      "RawBlockSize": 42
    },
    "AvailableForRetrieval": true,
    "DealID": 5432,
    "CreationTime": "0001-01-01T00:00:00Z",
    "TransferChannelId": {
      "Initiator": "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf",
      "Responder": "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf",
      "ID": 3
    },
    "SectorNumber": 9,
    "InboundCAR": "string value"
  }
]
```

### MarketListRetrievalDeals

There are not yet any comments for this method.

Perms: read

Inputs: `null`

Response:

```json
[
  {
    "PayloadCID": {
      "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
    },
    "ID": 5,
    "Selector": {
      "Raw": "Ynl0ZSBhcnJheQ=="
    },
    "PieceCID": null,
    "PricePerByte": "0",
    "PaymentInterval": 42,
    "PaymentIntervalIncrease": 42,
    "UnsealPrice": "0",
    "StoreID": 42,
    "ChannelID": {
      "Initiator": "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf",
      "Responder": "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf",
      "ID": 3
    },
    "PieceInfo": {
      "PieceCID": {
        "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
      },
      "Deals": [
        {
          "DealID": 5432,
          "SectorID": 9,
          "Offset": 1032,
          "Length": 1032
        }
      ]
    },
    "Status": 0,
    "Receiver": "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf",
    "TotalSent": 42,
    "FundsReceived": "0",
    "Message": "string value",
    "CurrentInterval": 42,
    "LegacyProtocol": true
  }
]
```

### MarketPendingDeals

Perms: write

Inputs: `null`

Response:

```json
{
  "Deals": [
    {
      "Proposal": {
        "PieceCID": {
          "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
        },
        "PieceSize": 1032,
        "VerifiedDeal": true,
        "Client": "f01234",
        "Provider": "f01234",
        "Label": "",
        "StartEpoch": 10101,
        "EndEpoch": 10101,
        "StoragePricePerEpoch": "0",
        "ProviderCollateral": "0",
        "ClientCollateral": "0"
      },
      "ClientSignature": {
        "Type": 2,
        "Data": "Ynl0ZSBhcnJheQ=="
      }
    }
  ],
  "PublishPeriodStart": "0001-01-01T00:00:00Z",
  "PublishPeriod": 60000000000
}
```

### MarketRestartDataTransfer

Perms: write

Inputs:

```json
[
  3,
  "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf",
  true
]
```

Response: `{}`

### MarketSetAsk

Perms: admin

Inputs:

```json
[
  "0",
  "0",
  10101,
  1032,
  1032
]
```

Response: `{}`

### MarketSetRetrievalAsk

Perms: admin

Inputs:

```json
[
  {
    "PricePerByte": "0",
    "UnsealPrice": "0",
    "PaymentInterval": 42,
    "PaymentIntervalIncrease": 42
  }
]
```

Response: `{}`

## Net

### NetAddrsListen

Perms: read

Inputs: `null`

Response:

```json
{
  "ID": "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf",
  "Addrs": [
    "/ip4/52.36.61.156/tcp/1347/p2p/12D3KooWFETiESTf1v4PGUvtnxMAcEFMzLZbJGg4tjWfGEimYior"
  ]
}
```

### NetAgentVersion

Perms: read

Inputs:

```json
[
  "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf"
]
```

Response: `"string value"`

### NetAutoNatStatus

Perms: read

Inputs: `null`

Response:

```json
{
  "Reachability": 1,
  "PublicAddr": "string value"
}
```

### NetBandwidthStats

Perms: read

Inputs: `null`

Response:

```json
{
  "TotalIn": 9,
  "TotalOut": 9,
  "RateIn": 12.3,
  "RateOut": 12.3
}
```

### NetBandwidthStatsByPeer

Perms: read

Inputs: `null`

Response:

```json
{
  "12D3KooWSXmXLJmBR1M7i9RW9GQPNUhZSzXKzxDHWtAgNuJAbyEJ": {
    "TotalIn": 174000,
    "TotalOut": 12500,
    "RateIn": 100,
    "RateOut": 50
  }
}
```

### NetBandwidthStatsByProtocol

Perms: read

Inputs: `null`

Response:

```json
{
  "/fil/hello/1.0.0": {
    "TotalIn": 174000,
    "TotalOut": 12500,
    "RateIn": 100,
    "RateOut": 50
  }
}
```

### NetBlockAdd

Perms: admin

Inputs:

```json
[
  {
    "Peers": [
      "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf"
    ],
    "IPAddrs": [
      "string value"
    ],
    "IPSubnets": [
      "string value"
    ]
  }
]
```

Response: `{}`

### NetBlockList

Perms: read

Inputs: `null`

Response:

```json
{
  "Peers": [
    "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf"
  ],
  "IPAddrs": [
    "string value"
  ],
  "IPSubnets": [
    "string value"
  ]
}
```

### NetBlockRemove

Perms: admin

Inputs:

```json
[
  {
    "Peers": [
      "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf"
    ],
    "IPAddrs": [
      "string value"
    ],
    "IPSubnets": [
      "string value"
    ]
  }
]
```

Response: `{}`

### NetConnect

Perms: write

Inputs:

```json
[
  {
    "ID": "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf",
    "Addrs": [
      "/ip4/52.36.61.156/tcp/1347/p2p/12D3KooWFETiESTf1v4PGUvtnxMAcEFMzLZbJGg4tjWfGEimYior"
    ]
  }
]
```

Response: `{}`

### NetConnectedness

Perms: read

Inputs:

```json
[
  "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf"
]
```

Response: `1`

### NetDisconnect

Perms: write

Inputs:

```json
[
  "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf"
]
```

Response: `{}`

### NetFindPeer

Perms: read

Inputs:

```json
[
  "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf"
]
```

Response:

```json
{
  "ID": "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf",
  "Addrs": [
    "/ip4/52.36.61.156/tcp/1347/p2p/12D3KooWFETiESTf1v4PGUvtnxMAcEFMzLZbJGg4tjWfGEimYior"
  ]
}
```

### NetLimit

Perms: read

Inputs:

```json
[
  "string value"
]
```

Response:

```json
{
  "Memory": 9,
  "Streams": 123,
  "StreamsInbound": 123,
  "StreamsOutbound": 123,
  "Conns": 123,
  "ConnsInbound": 123,
  "ConnsOutbound": 123,
  "FD": 123
}
```

### NetPeerInfo

Perms: read

Inputs:

```json
[
  "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf"
]
```

Response:

```json
{
  "ID": "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf",
  "Agent": "string value",
  "Addrs": [
    "string value"
  ],
  "Protocols": [
    "string value"
  ],
  "ConnMgrMeta": {
    "FirstSeen": "0001-01-01T00:00:00Z",
    "Value": 123,
    "Tags": {
      "name": 42
    },
    "Conns": {
      "name": "2021-03-08T22:52:18Z"
    }
  }
}
```

### NetPeers

Perms: read

Inputs: `null`

Response:

```json
[
  {
    "ID": "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf",
    "Addrs": [
      "/ip4/52.36.61.156/tcp/1347/p2p/12D3KooWFETiESTf1v4PGUvtnxMAcEFMzLZbJGg4tjWfGEimYior"
    ]
  }
]
```

### NetPing

Perms: read

Inputs:

```json
[
  "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf"
]
```

Response: `60000000000`

### NetProtectAdd

Perms: admin

Inputs:

```json
[
  [
    "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf"
  ]
]
```

Response: `{}`

### NetProtectList

Perms: read

Inputs: `null`

Response:

```json
[
  "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf"
]
```

### NetProtectRemove

Perms: admin

Inputs:

```json
[
  [
    "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf"
  ]
]
```

Response: `{}`

### NetPubsubScores

Perms: read

Inputs: `null`

Response:

```json
[
  {
    "ID": "12D3KooWGzxzKZYveHXtpG6AsrUJBcWxHBFS2HsEoGTxrMLvKXtf",
    "Score": {
      "Score": 12.3,
      "Topics": {
        "/blocks": {
          "TimeInMesh": 60000000000,
          "FirstMessageDeliveries": 122,
          "MeshMessageDeliveries": 1234,
          "InvalidMessageDeliveries": 3
        }
      },
      "AppSpecificScore": 12.3,
      "IPColocationFactor": 12.3,
      "BehaviourPenalty": 12.3
    }
  }
]
```

### NetSetLimit

Perms: admin

Inputs:

```json
[
  "string value",
  {
    "Memory": 9,
    "Streams": 123,
    "StreamsInbound": 123,
    "StreamsOutbound": 123,
    "Conns": 123,
    "ConnsInbound": 123,
    "ConnsOutbound": 123,
    "FD": 123
  }
]
```

Response: `{}`

### NetStat

Perms: read

Inputs:

```json
[
  "string value"
]
```

Response:

```json
{
  "System": {
    "NumStreamsInbound": 123,
    "NumStreamsOutbound": 123,
    "NumConnsInbound": 123,
    "NumConnsOutbound": 123,
    "NumFD": 123,
    "Memory": 9
  },
  "Transient": {
    "NumStreamsInbound": 123,
    "NumStreamsOutbound": 123,
    "NumConnsInbound": 123,
    "NumConnsOutbound": 123,
    "NumFD": 123,
    "Memory": 9
  },
  "Services": {
    "abc": {
      "NumStreamsInbound": 1,
      "NumStreamsOutbound": 2,
      "NumConnsInbound": 3,
      "NumConnsOutbound": 4,
      "NumFD": 5,
      "Memory": 123
    }
  },
  "Protocols": {
    "abc": {
      "NumStreamsInbound": 1,
      "NumStreamsOutbound": 2,
      "NumConnsInbound": 3,
      "NumConnsOutbound": 4,
      "NumFD": 5,
      "Memory": 123
    }
  },
  "Peers": {
    "abc": {
      "NumStreamsInbound": 1,
      "NumStreamsOutbound": 2,
      "NumConnsInbound": 3,
      "NumConnsOutbound": 4,
      "NumFD": 5,
      "Memory": 123
    }
  }
}
```

## Online

### OnlineBackup

There are not yet any comments for this method.

Perms: admin

Inputs:

```json
[
  "string value"
]
```

Response: `{}`

## Pieces

### PiecesGetCIDInfo

Perms: read

Inputs:

```json
[
  {
    "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
  }
]
```

Response:

```json
{
  "CID": {
    "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
  },
  "PieceBlockLocations": [
    {
      "RelOffset": 42,
      "BlockSize": 42,
      "PieceCID": {
        "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
      }
    }
  ]
}
```

### PiecesGetMaxOffset

Perms: read

Inputs:

```json
[
  {
    "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
  }
]
```

Response: `42`

### PiecesGetPieceInfo

Perms: read

Inputs:

```json
[
  {
    "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
  }
]
```

Response:

```json
{
  "PieceCID": {
    "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
  },
  "Deals": [
    {
      "DealID": 5432,
      "SectorID": 9,
      "Offset": 1032,
      "Length": 1032
    }
  ]
}
```

### PiecesListCidInfos

Perms: read

Inputs: `null`

Response:

```json
[
  {
    "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
  }
]
```

### PiecesListPieces

Perms: read

Inputs: `null`

Response:

```json
[
  {
    "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
  }
]
```

## Runtime

### RuntimeSubsystems

RuntimeSubsystems returns the subsystems that are enabled in this instance.

Perms: read

Inputs: `null`

Response:

```json
[
  "Markets"
]
```

## Sectors

### SectorsRefs

Perms: read

Inputs: `null`

Response:

```json
{
  "98000": [
    {
      "SectorID": 100,
      "Offset": 10485760,
      "Size": 1048576
    }
  ]
}
```