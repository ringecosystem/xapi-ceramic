# XAPI Ceramic

## Init

```bash
npm install --location=global @ceramicnetwork/cli
npm install --location=global @composedb/cli
npm install # in the project dir
```

## Start Ceramic db

```bash
npx @ceramicnetwork/cli daemon
```

## Using your account

```bash
# Generate private key
composedb did:generate-private-key
export CERAMIC_PRIVATE_KEY=<your private key>
# Generate DID
composedb did:from-private-key your-private-key
cd ~/.ceramic
vim daemon.config.json
# put your did in "admin-dids"
# Restart ceramic
ceramic daemon --network=testnet-clay
```

## Composites

```bash
# Edit xapi-schema.graphql ondemand
# Generate composites
composedb composite:create xapi-schema.graphql --output=xapi-composite.json --did-private-key=$CERAMIC_PRIVATE_KEY
# Deploying composites
composedb composite:deploy xapi-composite.json --ceramic-url=http://localhost:7007 --did-private-key=$CERAMIC_PRIVATE_KEY
```

## GraphQL

```bash
# Compiling composites
composedb composite:compile xapi-composite.json xapi-runtime-composite.json
# Start graphql server
composedb graphql:server --ceramic-url=http://localhost:7007 --graphiql xapi-runtime-composite.json --did-private-key=$CERAMIC_PRIVATE_KEY --port=5005
```

### Create OrmpSignature Example

```graphql
mutation MyMutation($i: CreateOrmpSignatureInput!) {
  createOrmpSignature(input: $i) {
    document {
      channel
      executionCall
      id
      msgIndex
      signature
      signer
      srcChainId
      timestamp
    }
  }
}

{
  "i": {
    "content": {
      "channel": "0xdf7b91c92ac62447ccb92bd39f41727466534043",
      "executionCall": "0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000665aad4c00000000000000000000000000000000000000000000000000000000000000840a863e6c0000000000000000000000000000000000000000000000000000000000aa36a7000000000000000000000000df7b91c92ac62447ccb92bd39f41727466534043000000000000000000000000000000000000000000000000000000000000007096812becb8254f71302001436ee1d63bccb28a3154e0e47893d2691b74e47f9800000000000000000000000000000000000000000000000000000000",
      "msgIndex": "112",
      "signature": "0xdb161c24ca3756cd174e5a0122caead0ec6411d82f6ca6a2867cd2c1b4282d0027c8e459ce180e012e0ce8bf9e74345d51f7cb01d572f0cff9972e495e1917d11b",
      "signer": "0x0b001c95e86d64c1ad6e43944c568a6c31b53887",
      "srcChainId": "11155111",
      "timestamp": "1716355836"
    }
  }
}
```