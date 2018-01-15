## :sos: :eyes: COMPOSER KNOWLEDGE BASE :eyes: :sos:

This Wiki is a knowledge base to help Composer folks having issues we class as  "_we've seen something similar before"_ or "_gotchas_" :-)  :1st_place_medal:  :2nd_place_medal: :3rd_place_medal: :rocket: :recycle: :sos: :face_with_head_bandage:. The idea is we help steer you towards a resolution :ok_man: :thumbsup:

Using is easy-peasy :last_quarter_moon_with_face: - jump to <strong></a> [**TOPICS**](#top)</strong> below, and check out related answers/resolutions. If you don't see an answer - you can 1) check our [**Open Composer Issues**](https://github.com/hyperledger/composer/issues) list for known issues ; 2) search [**Stack Overflow list**](https://stackoverflow.com/questions/tagged/hyperledger-composer) for possible answers. Otherwise go to [**Stack**](https://stackoverflow.com/questions/tagged/hyperledger-composer) and ask a question - see guidance here ~ [**Creating Stack Overflow issues**](#issue)

Our [**Documentation**](https://hyperledger.github.io/composer) is the 'first port of call'  :ship:  :boat: to understand concepts/examples/usage incl the [**Reference**](https://hyperledger.github.io/composer/reference/reference-index.html) :books: :books:  section (eg for CLI/ACLs/APIs)  Glossary etc) :relieved:
Our [**Tutorials**](https://hyperledger.github.io/composer/tutorials/tutorials.html) should be used to get going and consolidate your learning :smiley_cat:

<a name="top"></a>
***
###  :o: TOPIC INDEX  :o:

| [**ACLs**](#acls) | [**Authorization Errors**](#authorization) | [**Blockchain Recap**](#recap)   | [**Business Network Cards**](#bizcards) 
| :---------------------- | :-----------------------| :----------------------- | :-------------------- 
| [**Client APIs Usage**](#clientapis) | [**Composer Install Issues**](#installissues) | [**Debugging**](#debug)  | [**Endorsement Policy**](#endorse) 
| [**Events**](#events) | [**Event Hub Problems**](#eventhub) | [**Filters**](#filters)  | [**Historian**](#historian) 
| [**Cloud/Kubernetes envs**](#varcloud)  | [**Modeling**](#model) | [**Miscellaneous Items**](#misccomposer) | [**Multi Org Setup/BYFN**](#multiorg) | [**Passport Strategies**](#passport-strategy) 
| [**Queries**](#queries)  | [**REST APIs**](#restapis)  | [**REST Authentication**](#restauth)  | [**Runtime Install Errors**](#runtime-install)
| [**Sample Networks**](#samples) | [**Transaction Processors**](#transproc) | [**Upgrading Composer Runtime**](#upgrade) | [**Updating Biz Networks**](#upgradebn)  
| [**Topic Name**](#samples)  | [**Topic Name**](#bizcards) 


Have Fabric Related issues? (ie when used with Composer Dev Env Setup or Tutorials)

| ~ [**Fabric type Issues**](#fabricsetup)  | [**Misc items**](#misc) |
| :---------------------- | :---------------------- 

***
Each topic area has links to suggested solutions (you can open these in a new window) sourced from:
 
* **Documented Resolutions** 
* **Stack Overflow threads**
* **Rocketchat threads**
 

If you still have an issue,  see :link:  [here ](#issue)    

<a name="authorization"></a>



### :information_source:  Authorization Issues / Errors


The following are a selection of answers, to help understand what you may be encountering:

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Error: Error trying login and get user Context. Error: Error trying to enroll user or load channel configuration. Error: Enrollment failed with errors [[{"code":400,"message":"Authorization failure"}]]  | review logs of your ca server to see errors.  Eg. `docker logs ca.org1.example.com` to get info on the auth failure


#### :card_index: [back to base camp :camping: ](#top)  

<a name="acls"></a>


### :information_source:  Access Control Lists (ACLs)

The following are a selection of answers, to help understand what you may be encountering:

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Target multiple txn types ++  | see Rocketchat thread for target definition [here](https://chat.hyperledger.org/channel/composer?msg=HaxJg3sHESrPCvzcd)  
| Minimum ACLs to discover a BN | see Rocketchat thread [here](https://chat.hyperledger.org/channel/composer?msg=ZkMakmszLdQWNNYsj)
| What does .** in an ACL or what do these ACLs mean? | 1. .system.** -  System ACL to permit all access - access to all operations and commands in the business network, including network access and business access. It is recursive (denoted by .** )
|  | 2.  .* means the rule definition is non-recursive
|  | 3. org.acme.perishable.* - resources under the namespace `perishable` (non-recursive)
|  | 4. org.acme.perishable.**  all resources and everything under that namespace (recursive)
| Controlling access to a particular field |currently not possible for property (field) based access control in ACL runtime -> https://github.com/hyperledger/composer/issues/983 
| ..(continued ..) | If your singular field control relateds to 'authorisation' (who's allowed to see a particular asset) you can maybe store a hash of the authorised list (of participants allowed) in an array eg. `String[] Authorised optional` then a rule something like 
```

rule sampleRule {
   description: "only allowed users"
   participant(p): "org.acme.model.Participant"
   operation: ALL
   resource(r): "org.acme.model.Asset"
   condition: ( ( r.Authorised.indexOf(hashCode(p.getIdentifier()) > -1 ) || p.getIdentifier() === r.owner.getIdentifier()
   action: ALLOW
```
where `hashCode` is a function defined in a script file in js directory .... and `owner` is a relationship field to the participant


++ in the model, transaction `spTransaction` is `abstract` and `SampleTransaction` and `SampleTransaction2` are `extended` transaction types

#### :card_index: [back to base camp :camping: ](#top)  


<a name="recap"></a>


### :information_source:  Blockchain Recap

There are two place which "store" data in Hyperledger Fabric (the underlying blockchain infrastructure used by Composer):

    * the ledger
    * the state database ('World state')

The ledger is the actual **"blockchain"**. It is a file-based ledger which stores serialized blocks. Each block has one or more transactions. Each transaction contains a 'read-write set' which modifies one or more key/value pairs. The ledger is the definitive source of data and is immutable.

The **state database** (or 'World State') holds the last known committed value for any given key - an indexed view into the chain’s transaction log. It is populated when each peer validates and commits a transaction. The state database can always be rebuilt from re-processing the ledger (ie replaying the transactions that led to that state). There are currently two options for the state database: an embedded LevelDB or an external CouchDB.

As an aside, if you are familiar with Hyperledger Fabric 'channels', there is a separate ledger for each channel.

The chain is a transaction log, structured as hash-linked blocks, where each block contains a sequence of _N_ transactions. The block header includes a hash of the block’s transactions, as well as a hash of the prior block’s header. In this way, all transactions on the ledger are sequenced and cryptographically linked together.

The state database is simply an indexed view into the chain’s transaction log, it can therefore be regenerated from the chain at any time.

Source: http://hyperledger-fabric.readthedocs.io/en/release/ledger.html

#### :card_index: [back to base camp :camping: ](#top)  


<a name="bizcards"></a>


### :information_source:  Business Network Cards

jump to [**Review card errors / resolutions**](#cardfaq) to see more on current **Core* card errors & resolutions

jump to [**Review Card APIS issues / resolutions**](#cardapis) to see more on card-related **JS API** errors & resolutions


A Business Network Card provides the means for a user (such as an application user or identity in Playground) to connect to a Composer business network which runs in its own runtime container. It is only possible to access a Composer business network through a valid Business Network Card. It consists of a connection profile, some metadata for the identity using it, and ultimately, a set of credentials (certificate/private key). An identity (linked to a participant in Composer) can have one or more cards, to connect to one or more business networks.

The benefits are that once you export a card, it is a portable card. So it can be issued to a new user or given to someone (usually that real identity in that Organisation) to then connect and transact on business network, on the blockchain network. Yes, of course - they should be handled with care. We recommend that you only send identity cards that have been encrypted. See separate note on use of cards with a multi-user REST server environment.


**Best practices with cards:**

* If you issue an identity in Playground (connected to a Fabric): 
    1. Add it to your wallet (it only has an enroll id / one-time secret at this point)
    2. Connect to the business network (to download its certificate/key in exchange for the one-time secret) then (from 'My Business Networks' in Playground):
    3. Export it using the export icon on the identity/card in question.
    
* If you [issue an identity](https://hyperledger.github.io/composer/reference/composer.identity.issue.html) from the CLI or via JS APIs (connected to a Fabric): eg. `  composer identity issue -c admin@mynetwork -f JonDoe.card -u jdoe1 -a "resource:net.acme.biz.Person#jondoe1@biznet.org"` where `mynetwork` is the business network name and `net.acme.biz` is the namespace in the model.
    1. Initially, the .card file only has an enroll id / one-time secret at this point - so you need to import it
    2. From the CLI, import (example): `composer card import --file JonDoe.card`
    3. Use it (to download certificate/key to the local wallet and now primed with credentials)  - example is to do a `composer network ping -c JonDoe.card`
    4. Export it ie *from the Composer credentials vault* using `composer card export -f JDoeExport.card --name jdoe1@mynetwork`  
    5. Now as an identity administrator, you can securely share this exported .card file with the real user, so they can interact with the `mynetwork` business network from (say) their Node application, or authenticated via REST in a browser (once imported to their local wallet) etc and so on.
    6. Suggest to remove/delete the 'old' `JonDoe.card` file because it is now redundant (because we have the portable .card file,  complete with credentials)

In general:

* If you create a card on disk (eg exported unused from Playground, using the CLI, or the APIs) - the file will contain the single-use enrol id/secret and needs to be exchanged for a certificate from the CA server. 
   1. Import the card (.card file) (either through Playground - choose file, or via CLI import command or APIs) into your Composer wallet
   2. Either connect to it (eg. in Playground, via APIs) or use `composer ping` (eg. via CLI) to ping it using the imported card name eg `composer ping -c donald@tutorial-network`
   3. Export the card *from the Composer credentials vault* to a .card file (via Playground, CLI or APIs) - and replace the original .card file (it has redundant enrol info now). That new .card file can now be given to a designated user (eg on another system). Obviously, you may wish to keep a 'template' .card file elsewhere if you wish to retain that for future use.
   
   That's it.

* The  CLI command `composer card list --name user4@trade-network` command displays details of a named card, and its state - ie a boolean value at the bottom stating whether **Credentials Set** or **Credentials Not Set** - this indicates whether the Certificates have been downloaded and stored in the wallet (or not - in which case, the id is not yet enrolled with the CA)



Important: If you export a Business Network Card that **has never been used** (eg in Playground for example), it will still just contain the one-time enrollment ID and enrollment secret only. The first time that exported card gets used, wherever that is -  it connects with the one-time secret and the identity's certificate/key combo is downloaded to that user's local wallet. This is a perfectly normal scenario (as shown above).  If you (or the destined user) plan on using that card elsewhere (eg another system) - You **must export** this card, to a new .card file, so it can then be shared with the destined identity (that was registered with the CA earlier). Don't re-use the 'original .card file (just remove it) - otherwise re-using the 'old' will just register a different identity each time (because it still has an enrol id remember) and this can be a cause of major pain for many (this is how security, identity and certificates work, its not a 'Composer' thing).

More technical:

Together, both `cards` and `client-data` directories in `$HOME/.composer` are an integral part of the whole wallet structure in the card store (credentials vault). `client-data` is where the hlfv1 composer connector will store the identity credentials for a card (either by importing an existing card or when it has enrolled an identity, eg. connect to the business network). So a card `admin@tutorial-network` it will have the usual client crypto file artifacts such as : 'admin' xx-priv, xx-pub . Meanwhile,an imported card get persisted to the `cards` subdirectory. If that card contains only the initial enrollment secret, on 'first use' it will retrieve the cert/key combo from the CA server and those get stored in`client-data`. If the card in the walet is subsequently exported then the certificate/key gets retrieved from `client-data` too and added to the exported card. You should not in any way change anything manually under the `$HOME/.composer` directory PLEASE NOTE !

If you are moving from an 'older' version (say you had once installed a Composer release prior to v0.15.0) make sure you remove any directory called`.composer-credentials` directory under your $HOME directory (for whatever user being used) - it is not used by Composer and removing it removes potential confusion when troubleshooting.

#### What is the PeerAdmin Card provided with the Dev environment ?

PeerAdmin card (imported to the wallet/credentials store during the `createPeerAdminCard.sh` step), has 2 roles in our dev fabric server setup. It has the authority to install chaincode onto the peer and it has the authority to instantiate chaincode onto the channel. It's name 'PeerAdmin' is a little confusing. Also in a real world scenario it's highly unlikely you would have a AdminCard for each Peer. What is more likely is you would have a PeerAdmin card for each organisation so you can install chaincode on all Peers in that organisaton. There would then be a separate ChannelAdmin card so that within the Consortium the Composer business network can be started.

#### What is the 'NetworkAdmin' card created in Composer tutorials using the Dev environment (per the Docs site) ?

The Network Admin card is a card, once imported, that provides access to the deployed business network (eg. deployed by PeerAdmin above). The default is that the credentials you supply either by the flags -A/-S or -A/-C (when you deploy or start a business network) is then bound to an instance of the in-built NetworkAdmin Participant type, with a name usually the same as that specified in the -A part. To access a business network as a 'normal' non-admin user,  you have to use an identity that is mapped to a regular participant created as a resource/record in your business network namespace.

<a name="cardfaq"></a>

### Card Errors / Resolutions

| Message/Issue encountered | Resolution 
| :---------------------- | :-----------------------
| Cannot access local Fabric from dockerised Playground | See answer (bottom) https://stackoverflow.com/questions/47319089/hyperledger-composer-0-15-0-sharing-network-with-the-local-playground
| Cannot access local Fabric from dockerised REST server or Node js App | The simplest approach to sharing a card inside and outside a container is to replace the 'localhost' with the IP Number of your docker host in the connection.json file of the card you are using. See  https://stackoverflow.com/questions/47804516/hyperledger-composer-cannot-connect-with-dockerized-node-js-app/47818372 
| Authorization errors Playground local | See **answer** at https://stackoverflow.com/questions/47617442/authorization-failure-when-creating-new-business-network-in-local-playground
| Can't export BN card  in Playground | you're using the 'in-browser' connector (not connected to a running Fabric) - you can only export cards for a runtime Fabric environment
| |  
  

<a name="cardapis"></a>
### Card API errors / Resolutions

| Message/Issue encountered | Resolution 
| :---------------------- | :-----------------------
| How to export from Playground | After issuing the identity/participant, connect to the business network, return to 'My business networks' then click the 'export' icon alongside the card you wish to export (from the list of cards).
| How to switch BN cards using JS APIs | `const BusinessNetworkConnection = require('composer-client').BusinessNetworkConnection;  let businessNetworkConnection = new BusinessNetworkConnection();   return businessNetworkConnection.connect('lenny@digitalPropertyNetwork')  .then(() => { .........blah blah do something ; } return businessNetworkConnection.disconnect(); <switch to next card>` See also example of `useIdentity` here -> https://github.com/hyperledger/composer-sample-networks/blob/master/packages/pii-network/test/pii.js#L168
| Issue Identity and use Card | Issue identity, import card, connect to the business network using the card `businessNetworkConnection.issueIdentity(NS + '#' + userData.id, userData.user); ....  var userCard = new IdCard({...});  userCard.setCredentials(credentials); ...  adminConnection.importCard(userCardName, userCard); .... .then(() => {     // Connect to the business network using the network admin identity ...     businessNetworkConnection = new BusinessNetworkConnection({ cardStore: cardStore });   businessNetworkConnection.connect(userCardName); } ...`


#### :card_index: [back to base camp :camping: ](#top)   


<a name="clientapis"></a>



### :information_source:  Client API Usage and Guidelines

Below are some generic or specific questions on using client-side JS APIs

| Message encountered | Resolution 
| :---------------------- | :-----------------------
|AutoGenerate ID, Auto-Increment inside a TP issues | you should generate the autoincrement ID client-side (custom generated, or using UUIDs for example - using uuid on the client side and pass it in as part of the TP payload) and pass that into the Composer APIs. The only safe, deterministic way to "generate" an ID on the 'Blockchain' side in a Composer transaction processor function is to use the transaction ID. You should not generate an autoincrement id within a transaction processor doing so could be non-deterministic and the transaction would be rejected because the results of the transaction processor running on different peers would not match. 



#### :card_index: [back to base camp :camping: ](#top)  



<a name="installissues"></a>

### :information_source:  Composer Installation Issues

More info on the kinds of debugging, logging and Editor breakpoint setting is shown below.

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| npm install errors on installing Composer | 1. INSTALLING COMPOSER AS A NON-PRIVILEGED USER - IE NON-ROOT.     Ideally, when you install composer modules globally (eg. composer-cli) you should install using a designated, non-root user. If there are issues (eg, on Ubuntu with permissions to write/update node directories located in system directories like /usr/local) - the solution is perform the npm install to a directory you have access to - rather than resort to root or superuser access, as this is not good practice. Here is what to do to set the npm prefix to a given directory, ...
| ..continued .... | `npm config set prefix /home/myuser/` In this case, global binaries are placed in /home/myuser/bin which is in your PATH, and the modules are placed in /home/myuser/lib ... This is a method to do all the 'global' installs as non-root

#### :card_index: [back to base camp :camping: ](#top

<a name="debug"></a>


### :information_source:  Code Debugging etc

More info on the kinds of debugging, logging and Editor breakpoint setting is shown below.

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Debug / Logs | https://hyperledger.github.io/composer/problems/diagnostics.html
| printing to debug console | see S/O: https://stackoverflow.com/questions/47177296/hyperledger-composer-playground-can-you-see-results-of-console-logsomething 
| Setting Breakpoints (Atom,VSCode) | You can just use the embedded connector  (eg TP functions) to try out / step through each breakpoint - for more info on VSCode  -> https://code.visualstudio.com/docs/editor/debugging and Atom -> https://stackoverflow.com/questions/36964280/how-do-i-set-a-breakpoint-inside-of-atoms-package 
|Debugging a TP transaction `approveContract` | having trouble accessing (say) the referred (relationship) fields when handling a transaction. Using tx, `approveContract`, which points to a related contract. In this case use `console.log( 'contract is: ' + JSON.stringify(getSerializer().toJSON(approveContract)) );` to see what's in the transaction object.


#### :card_index: [back to base camp :camping: ](#top)  

<a name="endorse"></a>


### :information_source:  Endorsement Policy use with Composer etc

More info on troubleshooting or understanding issues related to endorsement of transactions by Fabric and endorsement policy. 



| Message encountered | Resolution 
| :---------------------- | :-----------------------
| General Endorsement Troubleshooting | The main caveat to pass on to clients seeking assistance is to remind them that troubleshooting endorsement policy is fundamentally a Hyperledger Fabric issue (may best be asked on #fabric channel on Rocketchat or they can review more on endorsement at the [Fabric docs site](https://hyperledger-fabric.readthedocs.io/en/release/endorsement-policies.html)) - this is because Composer merely passes on the endorsement policy metadata. Important to note that endorsement policies used for a Composer business network must be in the JSON format, as used by the Hyperledger Fabric Node.js SDK (the examples in doc link above will not be in JSON format FYI).
| ENDORSEMENT_POLICY_FAILURE error seen in Multi-Org tutorial (2 orgs) .. `compareProposalResponseResults - read/writes result sets do not match index=1` |Received proposals back from 2 or more peers, but at least 1 of the proposal results is different from the others. If for example an endorsement policy has stated that all the peers must endorse then it also means that all the peers must agree on the same results. If they don't -  you get an endorsement policy failure ;  because at least 1 peer doesn't agree and so you cannot satisfy the endorsement policy, hence the message.

<a name="events"></a>



### :information_source:  Events (consumed/subscribed to by Applications, emitted by TPs)


| Message encountered | Resolution 
| :---------------------- | :-----------------------
|How do i retrieve events, using a query? | Events are emitted by Txn Processors and 'subscribed to' by applications - you cannot query them using Composer Queries as they are not 'resources' per se - you will get an error eg.  `Error: The query compiler does not support resources of this type`


#### :card_index: [back to base camp :camping: ](#top)  


<a name="eventhub"></a>


### :information_source:  Event Hub Issues

Event Hub issues can vary in the kind of error reported - for example 'Unhandled Promise Rejection' is a case in point.
See below for suggested resolutions and follow the link in a new window.

* Have you checked the listening Fabric Event Hub is actually up and running (listening for events) ?
* Are you configuring a Multi-Org environment ? EventURLs are only defined for 'this' Org's peers (and not other Org's peers)
* grpc or grpcs (http or https for CA) protocol defined correctly in your connection info ?

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Unhandled Promise Rejections  | Your connection profile info (in your card) has been incorrectly defined
| #1 | See https://chat.hyperledger.org/channel/composer?msg=wnz6YZpvFMdCrgHZJ
| #2| https://stackoverflow.com/questions/46270080/node8232-unhandledpromiserejectionwarning-error-could-not-find-chaincode-wit

#### :card_index: [back to base camp :camping: ](#top)   


<a name="fabricsetup"></a>


### :information_source:  Fabric Issues relating to your Dev Environment setup

The following are a selection of answers, to help understand what you may be encountering. Check also [Runtime Install errors](#runtime-install) for runtime issues.


| Message encountered | Resolution 
| :---------------------- | :----------------------- 
| '**Error: No valid responses from any peers**'  | *The Fabric is Not Started** - The error has been seen when a Developer's Fabric has not been started (or restarted).  A simple check is the command `docker ps` that will show if the Fabric Containers are running. If the Fabric is not running either run the `startFabric.sh` script under **fabric-tools** or see the entry in this Wiki for Restarting Development Fabric. 
| **Starting/Stopping Dev Environment Impact / How to retain Docker container state**  | The `startFabric.sh` under **fabric-tools** does more than just start the Fabric - it removes existing Fabric Containers and recreates new Containers from the Docker Images.  The impact of this is that you lose all your data and your Business Network from the Fabric.  All Business Network Cards except PeerAdmin@hlfv1 are now useless. If you want to stop and start your Fabric after you have created it, retaining your Business Network and data follow these commands:  * Change to the directory where the `docker-compose.yml` file is (e.g. `/home/_\<user\>_/fabric-tools/fabric-scripts/hlfv1/composer` ) * Run `docker-compose stop` to stop the Fabric Containers   * Run `docker-compose start` to restart from where you left off with the current state of your Fabric.  
| '**The Fabric is not accessible**' | The error can be seen when the Business Network Card uses IP Names or Addresses for Docker Containers that are not resolveable or accessible.   For instance a Card my refer to Fabric Containers on localhost which work on a Developer's machine, but won't work if a card is passed to another person.  Examine the addresses in the connection.json file to see if they can be reached.  The connection.json file will be located in a folder similar to this example: `/home/_<user\>_/.composer/cards/admin@tutorial-network` 


#### :card_index: [back to base camp :camping: ](#top)  

<a name="filters"></a>


### :information_source:  Filters

The syntax examples below are Loopback Filter syntax FYI

Examples of stringified JSON Filters 

| Filter Type  | Example
| :---------------------- | :-----------------------
| Two condition 'and' filter | `{ "where":{ "and":[ { "isAccepted":true }, { "isClosed":false }] } }`
| Three condition 'and' filter | ` { "where":{ "and":[ { "isAccepted":true }, { "isClosed":false }, { "providertype":"Broker"} ] } } `

Examples of non-stringified JSON Filters 

| Filter Type  | Example
| :---------------------- | :-----------------------
| Three Condition 'and' filter | `[where][and][0][isAccepted]=1&filter[where][and][1][isClosed]=0&filter[where][and][2][providerType]=Broker"" `

eg. `curl -g -X GET "http://127.0.0.1:3000/api/Commodity?filter[where][and][0][isAccepted]=1&filter[where][and][1][isClosed]=0&filter[where][and][2][providerType]=Broker"" `

Example of Filter resolve: (meaning: Search on a modeled asset and resolve the data for the related model (eg. Commodity owner below)

| Filter goal  | Example
| :---------------------- | :-----------------------
| resolve all relationships | `{"include":"resolve"}`
| resolve specific an asset relationship | `{"where":{"asset_id":"resource:org.acme.biznet.Commodity#ABC"}, "include":"resolve"}`
| resolve specific asset by id | `{"where":{"shipmentId":1000}, "include":"resolve"}`
| resolve 'between' ascii sorted - Traders between 1 and 10  | `{"where":{"owner":{"between": ["resource:org.acme.trading.Trader#TRADER1","resource:org.acme.trading.Trader#TRADER10"]}}, "include":"resolve"}`



#### :card_index: [back to base camp :camping: ](#top)   

<a name="historian"></a>

### :information_source:  Historian Queries - common asks

Some typical examples of historian queries asked are below (obviously you would build this query per the docs) :

| Filter goal  | Example
| :---------------------- | :-----------------------
| SELECT ALL | `SELECT org.hyperledger.composer.system.HistorianRecord `
| Select Transaction Type | ` SELECT org.hyperledger.composer.system.HistorianRecord  WHERE (transactionType == 'myTranType' `
| with Order By  | `SELECT org.hyperledger.composer.system.HistorianRecord  WHERE (transactionType == 'myTranType') ORDER BY [transactionTimestamp DESC] `
| Select by Txn ID | SELECT org.hyperledger.composer.system.HistorianRecord WHERE (transactionId==_$trxID)



#### :card_index: [back to base camp :camping: ](#top)  

<a name="varcloud"></a>


### :information_source:  Cloud Issues eg. providers like IBM/Azure Cloud / Kubernetes Support / Cloud hosted Container service

Please seek support through the official channels for the Provider hosting your Hyperledger Fabric/Composer environment. (Composer cannot provide assistance with 3rd party Cloud environments per se, as support in this knowledge base is provided for local Development or local DevTest environments) - see link below for more info

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| IBM Sandbox / Kubernetes support  |for support with your particular environment on IBM Cloud you should go to this page https://console.bluemix.net/docs/support/index.html#contacting-support
| Microsoft Azure | See [Microsoft support](https://support.microsoft.com/en-us) or [here](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azureblockchain) for more information.


#### :card_index: [back to base camp :camping: ](#top)  



<a name="model"></a>


### :information_source:  Modeling and Modeling types

The following are a selection of answers, to help understand what you may be encountering or how to handle certain types from a modeling perspective


#### :card_index: [back to base camp :camping: ](#top)  


<a name="miscfabric"></a>



### :information_source:  Miscellaneous Items - Fabric (Uncategorised)


##### Media/Images/Storage

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Modeling/Storing images/PDFs/media | You can use String and base64 encode it - as a field in an Asset for example, and have responsibility for decoding later etc etc But would you want to store it on the blockchain? You decide of course.  If you wanted to you could see an SO thread here https://stackoverflow.com/questions/21878404/how-can-i-convert-mp3-file-to-base64-encoded-string/23665155 or here see https://stackoverflow.com/questions/47751609/how-to-deal-with-forms-images-videos-of-an-asset-in-hyperledger-composer .  Storing images, scans, audio files is not a 'best practice' - rather, a cryptographic hash of it (referenced off-chain) is verifiable proof that the source is the exact image/media file that was 'hashed' at the time the 'transaction' was recorded on the blockchain and link out of the chain, to a URL containing the verifiable source (and comparable hash). Examples may be: doctor/patient audio discussions (not least the privacy elements!) & consultation recordings, PDFs, mp3s, image files. Another issue is that an encoded base64 image string (if you chose to encode the media/image file that is) will also need to be transmitted to the other peers participating in consensus and written to their copy of the master ledger. It is therefore more efficient, to only share the hash (not the base64 encoded contents with each peer).


#### :card_index: [back to base camp :camping: ](#top)  

<a name="misccomposer"></a>



### :information_source:  Miscellaneous Items - Composer (Uncategorised)


##### JSON metadata FAQs
| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Adding JSON based metadata inside a JSON element | `  "$class": "org.acme.network.NwData", "nwDataId": "Nw01", "name": "testnw", "data_elements": "'attributes': {, 'indexed_public': ['/node/attribute/address/DE/NRW/Aachen/samplestreet/100/1/1'],'public': {}},'uris' ['calvinip://localhost:5000'],'control_uris': ['http://localhost:5001']}"}`

##### End-to-End Business Network deploy and use
| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Create archive.....deploy ....use | `composer archive create -t dir -n .` ; `composer runtime install -c PeerAdmin@hlfv1 -n acme-network` ; `composer network start -c PeerAdmin@hlfv1 -A admin -S adminpw -a acme-network.bna -f networkadmin.card` ; `composer card import -f networkadmin.card` ; `composer network ping -c admin@acme-network` ; `composer card export -f nwadmin.card -n admin@acme-network` 

#### :card_index: [back to base camp :camping: ](#top)  

<a name="multiorg"></a>



### :information_source:  Multi Org / BYFN Composer tutorial - issues.

The following are a selection of answers, to help understand what you may be encountering. Check also [Runtime Install errors](#runtime-install) for runtime issues.

We only use the Fabric 'BYFN' sample network as a means to demonstrate the Multi-Org tutorial. We only provide for assistance for the multi-org [tutorial](https://hyperledger.github.io/composer/tutorials/deploy-to-fabric-multi-org.html) we have built to make use of that sample Fabric network


| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Multi-Org on separate physical machines | See Rocketchat here ->  https://chat.hyperledger.org/channel/composer?msg=JtwfHPJmbSgLG5dr5
| As above - other things to consider | https://chat.hyperledger.org/channel/composer?msg=f8yyrCfaBKXwt9qKH



#### :card_index: [back to base camp :camping: ](#top)  


<a name="passport-strategy"></a>

### :information_source:  Passport Strategy Info

The following are a selection of answers, to help understand what you may be encountering:

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Is Passport-local supported?  |Not tested - see Rocketchat thread [here](https://chat.hyperledger.org/channel/composer?msg=jP6znqHXa6fChLiAX)
| Passport-local (custom)  | as-is - see Rocketchat thread [here](https://chat.hyperledger.org/channel/composer?msg=uruWP9jJbCEQcQqNo)
| Passport-jwt info | Not tested - see Rocketchat thread [here](https://chat.hyperledger.org/channel/composer?msg=etkJ7wzdbdFnSXW79)
| Custom Passport strategy |  Useful Rocketchat thread (as-is) [here](https://chat.hyperledger.org/channel/composer?msg=KW4DbESMZKkPRWmPQ)


#### :card_index: [back to base camp :camping: ](#top)  

<a name="queries"></a>



### :information_source:  Queries and Query support / examples

The following are a selection of answers, to help understand what you may be encountering or need more info on:

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Retrieving a FIELD in an Asset | Not presently supported (retrieves the whole record there is an issue raised https://github.com/hyperledger/composer/issues/3020
| ORDER BY (single) not working | for a single `ORDER BY` field you may be hitting this index issue  (you need to define an index) -> https://stackoverflow.com/questions/45919898/order-by-not-working-in-named-query/45966828#45966828 
|Indexes at the cto file level, in terms of composer concepts | Reliant on Fabric to provide the capability, before Composer is involved: see https://jira.hyperledger.org/browse/FAB-3067
| ORDER BY (multiple) not working | Not supported presently. Multiple ORDER BY fields is a current limitation of CouchDB see here -> https://github.com/hyperledger/composer/issues/1640 
| LIMIT/SKIP operators not working | LIMIT/SKIP support is blocked by Fabric presently as described here -> https://github.com/hyperledger/composer/issues/1015
| Error: Use Serializer.toJSON to convert resources | you need to do serialize the results of a query to work in the TP - see example should help you -> https://stackoverflow.com/questions/46686996/search-for-an-specific-asset-inside-a-transaction
| Query an array element  |see CONTAINS below - Contains works where you supply value(s) (one of) in the list provided in a StringArray
|CONTAINS Example 1: multi-value | ```SELECT org.acme.sample.TestAsset WHERE (stringArrayValues CONTAINS ["pumpkin", "jello", "candycane"]) ```
|CONTAINS Example 2: single-value with a Query def | ```query myquery {          description: "Example CONTAINS Query" statement:  SELECT org.acme.sample.TestAsset WHERE (stringArrayValues CONTAINS (value == "pumpkin"))```
|CONTAINS Example 3 - parameter based  | ```SELECT org.acme.mynetwork.Offer WHERE ( Offers CONTAINS _$offerId) ```
|Query Historian for history of created Assets | the attribute `targetRegistry` is a field available to `transaction` (from `transactionInvoked` from the system transaction registry class eg. `org.hyperledger.composer.system.AddAsset` - see here https://github.com/hyperledger/composer/blob/master/packages/composer-common/lib/system/org.hyperledger.composer.system.cto#L114 - so you can query the AddAsset class and match on the target Asset Registry (eg 'BankAccount' asset) type eg something like `SELECT org.hyperledger.composer.system.AddAsset WHERE targetRegistry == 'resource:org.hyperledger.composer.system.AssetRegistry#org.acme.account.BankAccount`

### Queries - Current issues

#### :card_index: [back to base camp :camping: ](#top)  


<a name="restapis"></a>


### :information_source:  REST APIs questions


The following are a selection of answers, to help understand what you may be encountering:

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| REST API return codes | 200 -  eg. a transaction, asset create, etc - means the record/transaction has been committed. The REST API waits for a block notification that says the transaction has been committed before returning 200 to the REST API client.


#### :card_index: [back to base camp :camping: ](#top)  

<a name="restauth"></a>


### :information_source:  REST Authentication questions


The following are a selection of answers, to help understand what you may be encountering:

| Message encountered | Resolution 
| :---------------------- | :-----------------------


#### :card_index: [back to base camp :camping: ](#top)  


<a name="runtime-install"></a>


### :information_source:  Runtime install errors (Composer)

The following are a selection of answers, to help understand what you may be encountering:

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Error: No valid responses from any peers  | see below
| Error: Error: Endpoint read failed |  This is likely to be a connection (.json) config issue -  it needs to be configured for TLS (or non-TLS if that's your setup) communications - depending on your desired configuration.
| Error: Error: Error trying to ping. Error: Composer runtime (0.16.1) is not compatible with client (0.16.0) | You have a mismatch of versions. This could occur using Composer REST server, APIs, App Generator or CLI. Suggest to uninstall the relevant modules eg `composer uninstall -g composer-cli` (also `composer-rest-server`, `generator-hyperledger-composer`, `composer-playground` npm modules that you have installed - then `npm install -g` the same level - check [**release notes**](https://github.com/hyperledger/composer/releases/) for the current release level - ensure you do so as a non-privileged user.
|Error: Error trying to instantiate composer runtime. Error: Error: Invalid results returned ::NOT_FOUND| Has a `composer runtime install` been done? Which document/tutorial are you following? Have you forgotten to create/start the Fabric channel your BN profile is referring to ? Are you trying to run on Windows 7/10 ? Please check these and take appropriate action(s).

#### :card_index: [back to base camp :camping: ](#top)  

<a name="samples"></a>


### :information_source:  Composer Sample Networks/Applications/Models

The links to our Sample Networks/Applicaions are provided below - to clone it simply do a `git clone https://github.com/hyperledger/composer-sample-networks.git` from the command line or from the same Github repo in a browser.

| Description | Link
| ----------- | ---------
| Sample Networks | https://github.com/hyperledger/composer-sample-networks
| Sample Applications | https://github.com/hyperledger/composer-sample-applications
| Sample Models | https://github.com/hyperledger/composer-sample-models

#### :card_index: [back to base camp :camping: ](#top)  


<a name="transproc"></a>



#### :information_source: Transaction processor errors


The following are a selection of answers, to help understand what you may be encountering:

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Timestamps in transactions - how to get the same timestamp across all peers | The timestamp is client side sourced - so will be the same across all endorsing peers. Client application don't actually generate the timestamp, it is generated by Composer so the format will be consistent - the timestamp  represents when a  transaction is submitted, not committed.
| Transaction rollback - errors in transactions | Changes made by **transactions** are atomic, either the transaction is successful and all changes are applied, or the transaction fails and no changes are applied. Any exceptions thrown from a transaction processor function will cause the entire **transaction** to be rolled back, leaving no changes to the blockchain or world state. So nothing is persisted if a transaction fails, Composer throw an exception and the transaction gets rolled back by Fabric. https://hyperledger.github.io/composer/reference/js_scripts.html
|Can a Composer transaction call another Transaction? |No - a transaction cannot presently be called from another transaction - however, a transaction processor can call other functions to allow for code modularisation, but it will only be registered as a single transaction request, in the transaction registry. 
| Calling a TP **function** from another | see example below - two parts
| (1) Calling from main script file - sample callout from `script.js`: | `var returnVal;     //And now call the function in func2.js var returnVal= sayHello("Jon Doe"); 	var code = returnVal.code; var msg= returnVal.message; console.log('The returned message and code are: Msg-> ' + msg + ' the code -> ' + code );` // output is "The returned message and code are: Msg -> Hello Jon Doe the code -> 11"
| (2) Called function - defined in file `func2.js` |  `function sayHello(name) { retMsg = "hello " + name; return {code: 11, message: retMsg}; }` 
|Catching errors in a JS TP function | sample code is shown (make sure to return a Promise) `return bizNetworkConnection.submitTransaction(transaction).then((result) => {             console.log("Tx Result", result) ; }).catch((exception) => { console.log("error", exception);` })
| Calling out of TPF using `post(url, resource)` and setting headers | here's an example here -> https://hyperledger.github.io/composer/integrating/call-out.html   application/json headers not yet supported in post -> see example https://stackoverflow.com/questions/43209924/rest-api-use-the-accept-application-json-http-header - issue raised for this -> https://github.com/hyperledger/composer/issues/3071


#### :card_index: [back to base camp :camping: ](#top)  

<a name="upgrade"></a>


#### :information_source: Upgrading Composer (eg. Runtime, Modules, Clients, Generators etc)


The following are a selection of answers, to help understand what you may be encountering:

| Message encountered | Resolution 


#### :card_index: [back to base camp :camping: ](#top)  

<a name="upgradebn"></a>


#### :information_source: Updating Business Network definitions


The following are a selection of answers, to help understand what you may be encountering:

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Controlling authority to update a B N | this is a change management issue (a business network model and definition and a policy for aggregating signatures or authority to be able to update a business network using `composer network update`) . By the time it gets to `composer network update` it seems, its already too late. The Network ACLs in Composer, can restrict who (ie a participant of that network) was allowed to do the network update. Its possible that such a record is only created once all the signatures are collected, as a one-time operation (so that the network update could take place). A sample ACL restriction that could be used is `rule networkControlPermission {
  description:  "networkControl can access network commands"
  participant: "org.acme.vehicle.auction.networkControl"
  operation: all
  resource: "org.hyperledger.composer.system.Network"
  action: ALLOW  
}`


#### :card_index: [back to base camp :camping: ](#top)  


<a name="issue"></a>



#### :information_source:  Creating a problem report (issue)



The best place to get it answered is actually on [Stack Overflow](https://stackoverflow.com/questions/tagged/hyperledger-composer). 

You simply create a problem with the tag 'hyperledger-composer'  and provide: 

* The problem title
* background context
* Composer version
* Operating System/version
* what steps you took to reach your current situation or state
* any supporting logs, script or model files - so as to help troubleshoot or identify where the problem lies. 

Thank you

#### :card_index: [back to base camp :camping: ](#top)   
