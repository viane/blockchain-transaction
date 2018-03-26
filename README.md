# Blockchain Transaction

### namespace: org.acme.biznet

#### 1. Generate a business network archive

```bash
$ cd blockchain-transaction

$ composer archive create -t dir -n .
```

#### 2. Deploying the business network

After creating the `.bna` file, the business network can be deployed to the instance of Hyperledger Fabric. 

Deploying a business network to the Hyperledger Fabric requires the Hyperledger Composer chaincode to be installed on the peer, then the business network archive (`.bna`) must be sent to the peer, and a new participant, identity, and associated card must be created to be the network administrator. Finally, the network administrator business network card must be imported for use, and the network can then be pinged to check it is responding.

1. To install the composer runtime, run the following command:

```bash
composer runtime install --card PeerAdmin@hlfv1 --businessNetworkName blockchain-transaction
```

> The `composer runtime install` command requires a PeerAdmin business network card (in this case one has been created and imported in advance), and the name of the business network.

2. To deploy the business network, from the blockchain-transaction directory, run the following command:

```bash
composer network start --card PeerAdmin@hlfv1 --networkAdmin admin --networkAdminEnrollSecret adminpw --archiveFile blockchain-transaction@0.0.1.bna --file networkadmin.card
```

> The `composer network start` command requires a business network card, as well as the name of the admin identity for the business network, the file path of the .bna and the name of the file to be created ready to import as a business network card.

3. To import the network administrator identity as a usable business network card, run the following command:

```bash
composer card import --file networkadmin.card
```

> The `composer card import` command requires the filename specified in composer network start to create a card.

4. To check that the business network has been deployed successfully, run the following command to ping the network:

```bash
composer network ping --card admin@blockchain-transaction
```

> The `composer network ping` command requires a business network card to identify the network to ping.

#### 3. Generating a REST server

Hyperledger Composer can generate a bespoke REST API based on a business network. For developing a web application, the REST API provides a useful layer of language-neutral abstraction.

1. To create the REST API, navigate to the blockchain-transaction directory and run the following command:

```bash
composer-rest-server
```

2. Enter admin@blockchain-transaction as the card name.

3. Select never use namespaces when asked whether to use namespaces in the generated API.

4. Select No when asked whether to secure the generated API.

5. Select Yes when asked whether to enable event publication.

6. Select No when asked whether to enable TLS security.

The generated API is connected to the deployed blockchain and business network.

#### 4. Test REST API

Try at [http://localhost:3000/explorer](http://localhost:3000/explorer) :

1. Create (POST) 2 traders.

2. Create (POST) 1 commodity and set owner to be the first created trader's tradeID.

3. Check (GET) commodity verify it's owner is the tradeID of first created trader.

4. Create (POST) a trade that use commodity's (created in #2) tradingSymbol and set new trader to be the 2nd trader's tradeID.

5. Check (GET) commodity verify is's owner now has changed to the sencond created trader's tradeID.