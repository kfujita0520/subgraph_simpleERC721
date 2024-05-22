# Set up Subgraph for ERC721 tokens

## Introduction

This repo contains subgraph schema and templates to index the activity of OpenZeppelin ERC721 Contracts. This will offer the graph query interface to retrieve specific ERC721 data such as owner list of given ERC721 contract or ERC721 token hold list of given user, which was difficult to retrieve otherwise:

## Set up Procedure
1. Run the Postgres  
$ docker-compose up postgres

2. Run the IPFS node  
Initialize the IPFS data directory (Only for the first time).
$ docker-compose run --rm --entrypoint ipfs ipfs init  
This initializes IPFS node at /data

3. Modify the configuration file(./data/ipfs/config)  
Prevent connection to other nodes.  
```
# before
"Bootstrap": [
  // list of bootstrap nodes
]
# after
"Bootstrap": null
```
  Allow API access from external.

```
# before
"Addresses": {
  "API": "/ip4/127.0.0.1/tcp/5001"
}

# after
"Addresses": {
  "API": "/ip4/0.0.0.0/tcp/5001"
}
```

5. Run the IPFS and graph node 
``` 
docker-compose up ipfs  
docker-compose up graph
```

6. Deploy Subgraph  
``` 
npm run create-local
npm run deploy-local
``` 

## Implementation Overview  
The files has already been prepared in this repository. This implementaion procedure is just for your information.

1. Prepare IERC721Metadata.json  
This is abi for ERC721 contracts to be indexed. This can be found in openzepplin package.

2. create package.json and tsconfig.json  
create these file by graph init command.
Also add dependency of @amxx/graphprotocol-utils

3. create schema.graphql and subgraph.yaml  
Write the configuration by referring following document and its implementaion.  
https://docs.openzeppelin.com/subgraphs/0.1.x/

4. Prepare mapping logic folder under src folder  
The files can be found on https://github.com/OpenZeppelin/openzeppelin-subgraphs  
Please note that we don't use the latest implemention of above repository, instead use the verion relased as npm package. https://www.npmjs.com/package/@openzeppelin/subgraphs

5. Generate type files under generated folder  
``` 
graph codegen
``` 

## Reference
Oasys Indexer build  
https://docs.oasys.games/docs/verse-developer/how-to-build-verse/the-graph

OpenZeppelin Subgraph  
https://docs.openzeppelin.com/subgraphs/0.1.x/
