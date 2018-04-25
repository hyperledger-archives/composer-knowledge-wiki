
## :sos: :eyes: COMPOSER KNOWLEDGE BASE :eyes: :sos:

This Wiki is a knowledge base to help Composer folks having issues we class as  "_we've seen something similar before"_ or "_gotchas_" :-)  :1st_place_medal:  :2nd_place_medal: :3rd_place_medal: :rocket: :recycle: :sos: :face_with_head_bandage:. The idea is we help steer you towards a resolution :ok_man: :thumbsup:

Using is easy-peasy :last_quarter_moon_with_face: - jump to <strong></a> [**TOPICS**](#top)</strong> below, and check out related answers/resolutions. If you don't see an answer - you can 1) check our [**Open Composer Issues**](https://github.com/hyperledger/composer/issues) list for known issues ; 2) search [**Stack Overflow list**](https://stackoverflow.com/questions/tagged/hyperledger-composer) for possible answers. Otherwise go to [**Stack**](https://stackoverflow.com/questions/tagged/hyperledger-composer) and ask a question - see guidance here ~ [**Creating Stack Overflow issues**](#issue)

Our [**Documentation**](https://hyperledger.github.io/composer) is the 'first port of call'  :ship:  :boat: to understand concepts/examples/usage incl the [**Reference**](https://hyperledger.github.io/composer/reference/reference-index.html) :books: :books:  section (eg for CLI/ACLs/APIs)  Glossary etc) :relieved:
Our [**Tutorials**](https://hyperledger.github.io/composer/tutorials/tutorials.html) should be used to get going and consolidate your learning :smiley_cat:

<a name="top"></a>
***
###  :o: TOPIC INDEX  :o:

| [**ACLs**](#acls) | [**Authorization Errors**](#authorization) | [**Angular**](#angular) | [**Blockchain Recap**](#recap) |[**Business Network Themes**](#biznet) 
| :---------------------- | :-----------------------| :----------------------- | :-------------------- |:-----------------
| [**Business Network Cards**](#bizcards) | [**Client APIs Usage**](#clientapis) | [**Cloud/Kubernetes envs**](#varcloud) | [**Composer Install Issues**](#installissues) | [**Composer Network Start Issues**](#startissues) | [**Data Migration**](#migration) 
 [**Debugging**](#debug) | [**Endorsement Policy**](#endorse) | [**Events**](#events) | [**Event Hub Problems**](#eventhub) | [**Filters**](#filters)  
|  [**Identity Issues**](#identity) | [**Historian**](#historian) | [**Modeling**](#model) | [**Miscellaneous Items**](#misccomposer) | [**Multi Org Setup/BYFN**](#multiorg)
| [**Node / NVM Issues**](#node-issues) | [**NodeJS App Dev Qs**](#node-app) | [**Passport Strategies**](#passport-strategy) | [**Production Questions**](#production) | [**Queries & Query Issues**](#queries)  
| [**REST APIs**](#restapis)  | [**REST Authentication**](#restauth) | [**Runtime Install Errors**](#runtime-install)| [**Sample Networks**](#samples) | [**Scripting Tips**](#scripting) 
|[**Transaction Processors**](#transproc) | [**Upgrading Composer Runtime**](#upgrade) | [**Updating Biz Networks**](#upgradebn) | [**empty**](#samples)  | [**empty**](#samples) 


Have Fabric Related issues? (ie when used with Composer Dev Env Setup or Tutorials)

| ~ [**Fabric type Issues**](#fabricsetup)  | [**Misc items**](#misccomposer) |
| :---------------------- | :---------------------- 

***
Each topic area has links to suggested solutions (you can open these in a new window) sourced from:
 
* **Documented Resolutions** 
* **Stack Overflow threads**
* **Rocketchat threads**
 

If you still have an issue,  see :link:  [here ](#issue)    


<a name="angular"></a>


### :information_source:  Angular Generator Questions


The following are a selection of answers, to help understand what you may be encountering:

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Current state of Angular Generator | Only creates Assets (not Transaction or Participants yet) - we have a current work-in-progress on improving the Angular generator to include transactions / transaction classes in the generated app -> https://github.com/hyperledger/composer/issues/3136 - this sample app here -> https://github.com/IBM/Decentralized-Energy-Composer/tree/master/angular-app/src/app has the `data service` for transactions added that you may want to take a look at - this may also be of use -> https://blog.angular-university.io/how-to-build-angular2-apps-using-rxjs-observable-data-services-pitfalls-to-avoid/



#### :card_index: [back to base camp :camping: ](#top) 


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
| ..(continued ..) | rule sampleRule {    description: "only allowed users"    participant(p): "org.acme.model.Participant"    operation: ALL    resource(r): "org.acme.model.Asset"    condition: ( ( r.Authorised.indexOf(hashCode(p.getIdentifier()) > -1 ) || p.getIdentifier() === r.owner.getIdentifier()    action: ALLOW    where `hashCode` is a function defined in a script file in js directory .... and `owner` is a relationship field to the participant


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



<a name="biznet"></a>


### :information_source:  Business Networks - themes, contexts

The themes or questions below, reflect different elements of themes of 'modus operandi' of a business network. So, channels and privacy and business networks, is a theme - and so on:


| Message/Issue encountered | Resolution 
| :---------------------- | :-----------------------
| what a user can access, its rights: why divide a network into different channels, how are ACLs applied? |Answer: channels provide privacy, between organisations (say 2 organisations of _n_) on the same blockchain network. You can have many channels, and some organisations may not be allowed to join that channel (eg. two-party channel excludes a third) but still be part of the same 'greater' blockchain network. Access Conrol rules (ACLs) work within the realms of a business network, controlling participants, asset access, transactions types allowed etc, what CRUD operations, etc etc - the business network (with its ACL rules) are deployed to a channel - there is a one-to-one mapping between a ledger and a channel in Fabric 1.x. Usually an example of ACLs being used - is restricting either Org's participants (ie as identified in the business network) according to what each Org deems to be allowed to do/see on the ledger, that they both have access to (on the channel) - they agree the rules, like they would agree the 'constitution' of a club, when they set it up (or tweak later on). ACLs are in the 'runtime' of the business network - so filtering if you like. Each channel can have more than one deployed business network on that channel - so the scenario where the same '2' orgs (etc) are running different kinds of business networks, and can share the same channel and therefore ledger - but in essence, the views/updates into that ledger are through the interaction in the relevant business network, from where the source transactors can be identified and assets, participants, transactions etc are collated in an orderly, organised fashion through registries (specific to that business network).

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

#### Why can't I use PeerAdmin to do transactions, why can't he be a Network Admin too ? 

Firstly, its to do with 'roles' that FABRIC lays down - so **admin** (in the sample Fabric network) is is bound as a network admin in the Composer system registry,  with authority to issue identities via Fabric CA. The **PeerAdmin** (who is not bound as a network admin in the registry) that you use is bootstrapped for the Dev Fabric environment(ie  PeerAdmin a label for for someone that has 'PeerAdmin' authority (to deploy chaincode) in Fabric in the Development environment setup (in reality, someone setting up their own blockchain network infrastructure, would create their own distinct identities via their CA and give them the right Fabric roles. I suggest to read this to understand a bit more than I've paraphrased here -> https://hyperledger.github.io/composer/business-network/bnd-deploy.html - suffice to say,  that when a business network card is created for a business network, its good practice to have a clear demarcation for someone to deploy chaincode as opposed to doing admin / operational / identity issuance tasks on the network - in that Org etc. Neither of those 'users' would be considered 'day to day business transactors' (creating transactions) - those activities would be done by other participants/consumers of the business network,  for whom the business network was set up - cross-org transactions. For which you would issue identities (each Org, that is) and create business network cards, as it includes their issued blockchain identity etc etc)
 

<a name="cardfaq"></a>

### Card Errors / Resolutions

| Message/Issue encountered | Resolution 
| :---------------------- | :-----------------------
|HSM related issues | If you want to HSM,  enable all the identities used by your fabric network (and specifically fabric-network here, not your composer business network) then you would have to find a way to recreate all the identities used to setup the fabric utilising your HSM, that is a fabric setup requirement and so you would need to ask on the fabric channels. If you want to then HSM manage the identities used by the composer business network you would need to use a connection profile with HSM enabled to perform the various identity management requests available in composer. There are a couple of important points to note. CLI commands that create cards will take the connection profile of the card currently being used to perform the request. If that card is not HSM enabled, then the newly created card will also have a connection profile that isn't HSM enabled. This means having a mixed environment of non HSM and HSM enabled identities requires more work in order to allow that kind of environment and isn't something that is documented. The HSM support assumes that all identities used both for fabric interaction and business network interaction are all HSM managed. HSM capability in Composer is really only for an everything is HSM managed environment, ie your fabric identities are HSM managed. It is possible to have a mixed environment but that requires much more work to do, but really doesn't make sense to have some identities HSM managed and some not managed. To create an HSM managed yperledger Fabric network is something you would have to build yourself. I am not aware of any tutorial (presently) on the Hyperledger Fabric docs page that would tell you how to do this so you would have to work this out based on their existing tutorials and understanding the various commands for building a fabric.
| Cannot access local Fabric from dockerised Playground | See answer (bottom) https://stackoverflow.com/questions/47319089/hyperledger-composer-0-15-0-sharing-network-with-the-local-playground
| Cannot access local Fabric from dockerised REST server or Node js App | The simplest approach to sharing a card inside and outside a container is to replace the 'localhost' with the IP Number of your docker host in the connection.json file of the card you are using. See  https://stackoverflow.com/questions/47804516/hyperledger-composer-cannot-connect-with-dockerized-node-js-app/47818372 
| Authorization errors Playground local | See **answer** at https://stackoverflow.com/questions/47617442/authorization-failure-when-creating-new-business-network-in-local-playground
| Can't export BN card  in Playground | you're using the 'in-browser' connector (not connected to a running Fabric) - you can only export cards for a runtime Fabric environment
| How to export card from Playground | After issuing the identity/participant,add to wallet - then connect to the business network via 'Use Now' in ID registry. Return to 'My business networks' then click on the 'export' icon alongside the card you wish to export (from the list of BN card modals displayed). 

<a name="cardapis"></a>


### Card API errors / Resolutions

| Message/Issue encountered | Resolution 
| :---------------------- | :-----------------------
| Programmatic Issue, upload and import card to wallet / std HTTP or using react and the axios http library or  | see this snippet https://pastebin.com/ST684NCw or see Caroline Church blog -> https://medium.com/@CazChurchUk/developing-multi-user-application-using-the-hyperledger-composer-rest-server-b3b88e857ccc
| How to issue identities / create cards via JS APIs: Examples for issuing participants, identities and importing  | We have an issue https://github.com/hyperledger/composer/issues/3088 as the docs are not updated yet with examples. You can refer to the source for composer identity issue to see how it calls BusinessNetworkConnection.issueIdentity (as an admin with authority to issue identities) and creates a business network card from the result:-> https://github.com/hyperledger/composer/blob/master/packages/composer-cli/lib/cmds/identity/lib/issue.js#L46 and also another answer at the bottom here -> https://stackoverflow.com/questions/47393096/nodejs-test-hyperledger-composer-v0-15-fails-with-error-card-not-found-peeradm/47417082#47417082 (albeit with MemoryCardStore being replaced by FileSystemCardStore). This blog is also useful -> https://medium.com/@aniketengg.225/hyperledger-composer-issue-identity-import-card-7e07af378447
| How to switch BN cards using JS APIs | `const BusinessNetworkConnection = require('composer-client').BusinessNetworkConnection;  let businessNetworkConnection = new BusinessNetworkConnection();   return businessNetworkConnection.connect('lenny@digitalPropertyNetwork')  .then(() => { .........blah blah do something ; } return businessNetworkConnection.disconnect(); <switch to next card>` See also example of `useIdentity` here -> https://github.com/hyperledger/composer-sample-networks/blob/master/packages/pii-network/test/pii.js#L168
| Issue Identity and use Card | Issue identity, import card, connect to the business network using the card `businessNetworkConnection.issueIdentity(NS + '#' + userData.id, userData.user); ....  var userCard = new IdCard({...});  userCard.setCredentials(credentials); ...  adminConnection.importCard(userCardName, userCard); .... .then(() => {     // Connect to the business network using the network admin identity ...     businessNetworkConnection = new BusinessNetworkConnection({ cardStore: cardStore });   businessNetworkConnection.connect(userCardName); } ...`
|Issue Identity and switch between issued cards | see https://github.com/hyperledger/composer-sample-networks/blob/master/packages/pii-network/test/pii.js#L139 onwards (from the PII sample networks) - the principle of connecting to the network with the card ycreated and being persisted to a File CardStore (as opposed to the in Memory CardStore example used in this test script) is the same.


#### :card_index: [back to base camp :camping: ](#top)   


<a name="clientapis"></a>



### :information_source:  Client API Usage and Guidelines

Below are some generic or specific questions on using client-side JS APIs

| Message encountered | Resolution 
| :---------------------- | :-----------------------
|AutoGenerate ID, Auto-Increment inside a TP issues | you should generate the autoincrement ID client-side (custom generated, or using UUIDs for example - using uuid on the client side and pass it in as part of the TP payload) and pass that into the Composer APIs. The only safe, deterministic way to "generate" an ID on the 'Blockchain' side in a Composer transaction processor function is to use the transaction ID. You should not generate an autoincrement id within a transaction processor doing so could be non-deterministic and the transaction would be rejected because the results of the transaction processor running on different peers would not match. 



#### :card_index: [back to base camp :camping: ](#top)  


<a name="varcloud"></a>


### :information_source:  Cloud Issues (eg. providers like IBM/Azure Cloud / Kubernetes Support / Cloud hosted Container service)

Please seek support through the official channels for the Provider hosting your Hyperledger Fabric/Composer environment. (Composer cannot provide assistance with 3rd party Cloud environments per se, as support in this knowledge base is provided for local Development or local DevTest environments) - see link below for more info

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| IBM Sandbox / Kubernetes support  |for support with your particular environment on IBM Cloud you should go to this page https://console.bluemix.net/docs/support/index.html#contacting-support
| Microsoft Azure | See [Microsoft support](https://support.microsoft.com/en-us) or [here](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azureblockchain) for more information.


#### :card_index: [back to base camp :camping: ](#top)  


<a name="installissues"></a>

### :information_source:  Composer Installation Issues

More info on the kinds of debugging, logging and Editor breakpoint setting is shown below.

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| npm install errors on install Composer (option 1) |Follow the best practices here https://docs.npmjs.com/getting-started/fixing-npm-permissions including recommendation to install  a Node Version Manager (install NVM then use that to manage Node installs)
| npm install errors on installing Composer (option 2) | 1. INSTALLING COMPOSER AS A NON-PRIVILEGED USER - IE NON-ROOT.     Ideally, when you install composer modules globally (eg. composer-cli) you should install using a designated, non-root user. If there are issues (eg, on Ubuntu with permissions to write/update node directories located in system directories like /usr/local) - the solution is perform the npm install to a directory you have access to - rather than resort to root or superuser access, as this is not good practice. Here is what to do to set the npm prefix to a given directory, ...
| ..continued .... | `npm config set prefix /home/myuser/` In this case, global binaries are placed in /home/myuser/bin which is in your PATH, and the modules are placed in /home/myuser/lib ... This is a method to do all the 'global' installs as non-root
|Error: Failed to load connector module "composer-connector-hlfv1" for connection type "hlfv1".   | "I installed Composer Playground on my Linux environment and am trying to deploy a sample perishable-network, but am getting the following error when deploying,  Cannot find module '/usr/lib/node_modules/composer-playground/node_modules/grpc/src/node/extension_binary/node-v59-linux-x64/grpc_node.node'-Cannot find module '/usr/lib/node_modules/composer-connector-hlfv1' - RESOLUTION: its likely the user installed Composer as 'root' or used 'sudo' - uninstalled node and npm as root - and then following these directions, https://docs.npmjs.com/getting-started/installing-node to reinstall it. Then, follow the directions to install Composer here, https://hyperledger.github.io/composer/installing/development-tools.html. The key is to not use 'su' or 'sudo' - as the instructions clearly state :-)
| Error: Failed to load connector module "composer-connector-hlfv1" for connection type "hlfv1". Unexpected identifier-Unexpected identifier-Unexpected identifier-Unexpected identifier-Unexpected identifier-Unexpected identifier-Unexpected identifier-Unexpected identifier-Unexpected identifier-Unexpected identifier-Unexpected identifier | RESOLUTION: Its likely the user has used Node 5/6 - they need to switch to Node 8.9.x (using NVM to switch or uninstall node 5/6 and re-install with node 8.9 if required).
Command failed

#### :card_index: [back to base camp :camping: ](#top)


<a name="startissues"></a>

### :information_source:  Composer Network Start Issues


More info on troubleshooting or understanding issues related Composer business network start issues, please see the table below for possible resolutions to the issue / problem you are seeing. There are several kinds of errors that could occur with a `composer network start` and will attempt to explain below.  All error messages usually start with "Error: No valid responses from any peers." - and the table below looks at the error following that line. 


| Message encountered | Resolution 
| :---------------------- | :----------------------- 
| :---------------------- | :-----------------------
| Error: 14 UNAVAILABLE: Connect Failed  | This error is a failure of Composer to connect to the Fabric, usually because the Fabric is not started. **Resolution:** Start the fabric - depending on how you started the Fabric, it may be necessary to repeat the `composer network install` command.
| Error: 2 UNKNOWN: chaincode error (status: 500, Message: Unknown chaincodeType: NODE)  | This error is seen when using Composer v0.19 with an outdated Fabric v1.0.x.  Composer requires Fabric v1.1 GA.
| continued.........  | To continue with Composer v0.19: 
| |1. stop and remove all Docker Containers for Fabric v1.0
| |2. if you are working with the Development Fabric, download a new version of Fabric Tools (From step 4 in this doc https://hyperledger.github.io/composer/latest/installing/development-tools.html  )
| Error: 2 UNKNOWN: transaction returned with failure: ReferenceError: alert is not defined  | There is an error in the transaction logic JS script is this example a function called 'alert'. Transaction processing function cannot have 'include' or 'requires', nor include JS code that relies on Browser functionality will also fail.
| Error: 2 UNKNOWN: chaincode error (status: 500, message: cannot get package for chaincode (test-network:0.0.2))  | The network start has failed for the particular network name and version specified.  This can occur because a composer network install has not been run, but it is more likely that there is a mismatch. Run the `composer archive list` command to see the **exact name** and **version** used in the .bna file. Remember that the version number must be changed and saved in the package.json. This error can also occur with the `composer network upgrade` command.
| Error: REQUEST_TIMEOUT  | As part of the Composer Network Start, Fabric tries to build a new chaincode Container which includes `npm install` commands. A **`REQUEST_TIMEOUT`** can occur with the failure to build the Chaincode containers for all peers within the default timeout period of 5 mins through lack of system resources or poor network connections.This can be a problem for the Multi-Org tutorial, but also for single peer installs. If you are using our simple Hyperledger Composer development server environment from composer-tools github repo, then you can add the following to the peer definition to see if it addresses the problem:
| continued .... | CORE_CHAINCODE_STARTUPTIMEOUT=1200s in the file ~/fabric-tools/fabric-scripts/hlfv11/composer/docker-compose.yml eg, the above is a snippet from the peer definition. You would then have to do a `docker-compose stop` - then `docker-compose start` from that directory location to take effect. If you are using a more complex Fabric you will need to find the docker-compose files used to configure your Fabric and modify those. After modifying a docker-compose file it will be necessary to run the `startFabric.sh` script and re-run the `composer network install` command, and then finally the `composer network start` command.
| continued .... | these Stack Overflow links may also shed further light https://stackoverflow.com/questions/49751259/error-in-starting-hyperledger-fabric-network-with-hyperledger-composer/49758354#49758354 and https://stackoverflow.com/questions/49290943/v0-18-1-error-cant-start-network
| Error: 2 UNKNOWN: error starting container: Failed to generate platform-specific docker build:  | As part of the Composer Network Start, Fabric tries to build a new chaincode Container which includes `npm install` commands.  This error is usually a Failure to build the container because of an underlying npm issue. 
| continued .... | Using `docker logs <PEER Container Name>` will show if there are npm errors. Generally npm Warnings can be ignored - but serious errors such as "Error: getaddrinfo EAI_AGAIN nodejs.org:443" need to be resolved. The most common cause of the error is a corporate proxy or firewall preventing access outbound. More specifically, the need to include an npmrcFile as part of the `composer network start` sequence. These npm errors need to be fixed before a chaincode container can be successfully built.  You may recognise the problem and be immediately able to fix, but if not it is advisable to create a temporary container based on the same image that Composer uses.  Having a dedicated test container will be a fast way to identify and solve your local npm issues.  The following commands may help:
|continued .... | Create a test container: `docker run -it --name npmtest --network composer_default  --entrypoint "/bin/sh" hyperledger/composer-cli` | When the container starts, install 'a' (a small npm package)    `npm install -g a`
| continued .... | Errors with npm here need to be understood and resolved in this test container, then the same resolutions in an npmrc file should be passed to the Composer command. 
| continued ... | Once resolved: (if you are running the Development Fabric with a single peer) you can now just re-run the `composer network start` command adding the npmrcFile option e.g.  `composer network install -c PeerAdmin@hlfv1 -a digitalproperty-network.bna -o npmrcFile=/tmp/npmrcFile`
|continued ... | If you are running a more complex fabric, you may need to re-run the `composer network install` command.
| |
| Error: 2 UNKNOWN: chaincode error (status: 500, message: Authorization for INSTALL has been denied (error-Failed verifying that proposal's creator satisfies local MSP principal during channelless check policy with policy [Admins]: [This identity is not an admin])) | A business Network Card is being used with an ID that does not have Admin rights. For Composer, this can mean a 'PeerAdmin' card was NOT used, or the Card was created with incorrect certificates.



#### :card_index: [back to base camp :camping: ](#top)


<a name="migration"></a>

 
### :information_source:  Data Migration eg. from Playground, the ledger - for future use etc

More info on the kinds of on migrating data from Playground, Fabric etc - best practices

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| I've lost my data in Playground | see below
| How to migrate existing data from blockchain - | see below
| How to save my data in Playground/Fabric? |  If you add new fields to a Participant you are correct : you can no longer see the data in Playground (but the record with that participant id or asset ID etc, still exists on the ledger). If you remove the field from the model - you can see the data again. If you add the new field but with 'optional' after the field in the model, you will see the original data (again) - that might be all you need to solve your short-term issue. Going beyond that, it is easy to extract the data to JSON files using the REST APIs (https://hyperledger.github.io/composer/integrating/getting-started-rest-api.html ). You can easily extract the JSON data from the registry in question to a text file - then change it to match your new model etc etc if you wish. I would think writing a simple to extract the data (eg using curl REST API GET) and re-adding (a REST API POST) with the new fields/data for relevant participants/assets might be a way forward if you wish to retain data going forward. Then you can see it in Playground change / add there etc etc.
|Migrate using the REST Server? | 1. Execute a GET on each of the registries (Asset Types and Participant Types), and put these in text files 2. Use a text editor to do 'global' search and replace to add your new fields  3. Deploy new extended business network  4. Execute a POST for each of the registries using the text files as the JSON input. 
|Migrate using Node-Red | see the Node RED to export / then import the data - We have a tutorial that shows how to import: https://hyperledger.github.io/composer/latest/tutorials/nodered-tutorial.html


 
#### :card_index: [back to base camp :camping: ](#top)

<a name="debug"></a>
 
 
### :information_source:  Debugging Code  etc

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
| How to listen for ALL events on eventHub? |`const evtListener = businessNetworkConnection.connection.eventHubs[0].registerChaincodeEvent('businessNetworkName', 'composer', (event) => { console.log('Tx EVENT', event) })`
| continued ...... |` process.on('exit', () => { businessNetworkConnection.connection.eventHubs[0].unregisterChaincodeEvent(evtListener)
})`
| continued ..... | this is a 'stopgap'' for 'System Events' (not application events you can program) - there will be an issue created to enable emission of 'System' type events such as Asset CRUD operations, to be added here when created (March 2018).
| Can I emit events and catch (err) using JS ? | Check this : https://hyperledger.github.io/composer/reference/js_scripts.html, section "Error handling in transaction processor functions" ```return function()     .then(function(){})      .catch(function(err){          emit(someevent);          throw err;       }) ```
| (continued ...) | While you can issue an 'emit' at any point in your transaction logic, that event will not actually be emitted until the transaction is committed.  However, throwing the error makes the transaction roll back - changes made by transactions are atomic, either the transaction is successful and all changes are applied, or the transaction fails and no changes are applied.
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
| '**Error: No valid responses from any peers**'  | **The Fabric is Not Started** - The error has been seen when a Developer's Fabric has not been started (or restarted).  A simple check is the command `docker ps` that will show if the Fabric Containers are running. If the Fabric is not running either run the `startFabric.sh` script under **fabric-tools** or see the entry in this Wiki for Restarting Development Fabric. 
| **Starting/Stopping Dev Environment Impact / How to retain Docker container state**  | The `startFabric.sh` under **fabric-tools** does more than just start the Fabric - it removes existing Fabric Containers and recreates new Containers from the Docker Images.  The impact of this is that you lose all your data and your Business Network from the Fabric.  All Business Network Cards except PeerAdmin@hlfv1 are now useless. If you want to stop and start your Fabric after you have created it, retaining your Business Network and data follow these commands:  * Change to the directory where the `docker-compose.yml` file is (e.g. `/home/_\<user\>_/fabric-tools/fabric-scripts/hlfv1/composer` ) * Run `docker-compose stop` to stop the Fabric Containers   * Run `docker-compose start` to restart from where you left off with the current state of your Fabric.  
| '**The Fabric is not accessible**' | The error can be seen when the Business Network Card uses IP Names or Addresses for Docker Containers that are not resolveable or accessible.   For instance a Card my refer to Fabric Containers on localhost which work on a Developer's machine, but won't work if a card is passed to another person.  Examine the addresses in the connection.json file to see if they can be reached.  The connection.json file will be located in a folder similar to this example: `/home/_<user\>_/.composer/cards/admin@tutorial-network` 
|MVCC_READ_CONFLICT error | MVCC_READ_CONFLICT` is a response from the Fabric network. Stackoverflow for example has info about the meaning, eg https://stackoverflow.com/questions/45347439/mvcc-read-conflict-when-submitting-multiple-transactions-concurrently
|How to disable TLS in my environment ?  | see example of non TLS environment here -> https://discourse.skcript.com/t/setting-up-a-blockchain-business-network-with-hyperledger-fabric-composer-running-in-multiple-physical-machine/602 - see examples of connection profiles with or without TLS enabled here -> https://hyperledger.github.io/composer/reference/connectionprofile.html and here https://hyperledger.github.io/composer/tutorials/deploy-to-fabric-single-org


#### :card_index: [back to base camp :camping: ](#top)  


<a name="filters"></a>


### :information_source:  Filters (Loopback)

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



### :information_source:  Historian Questions


jump to [**Historian Queries**](#historianqueries) for more info on Historian related queries or examples


Some typical examples of historian related questions below:

| Message Encountered / Question  | Answer/Resolution
| :---------------------- | :-----------------------
| Does Historian prevent 'false' / wrong transactions? | Historian records what was consensually agreed as the 'truth' - the whole point of the blockchain is to detect the 'poison' ?? If ledger data had been altered or corrupted on a peer then the transaction results would be inconsistent across endorsing peers, the 'bad' peer will be found out, and the application client can throw out the results from the bad peer before submitting the transaction for ordering/commit. If client application tries to submit a transaction with inconsistent endorsement regardless, this will be detected and the transaction will be invalidated by all peers at commit time.If there is doubt about state database integrity on a specific peer, the state database can be dropped and it will automatically get regenerated from the chain source of truth. If there is doubt about the chain integrity itself, then the peer itself will need to be rebuilt and the chain will be state transferred 



#### :card_index: [back to base camp :camping: ](#top)  


<a name="historianqueries"></a>


### :information_source:  Historian Queries - common asks

Some typical examples of historian queries asked are below (obviously you would build this query per the docs) :

| Query goal / aim  | Example
| :---------------------- | :-----------------------
| SELECT ALL | `SELECT org.hyperledger.composer.system.HistorianRecord `
| Select Transaction Type | ` SELECT org.hyperledger.composer.system.HistorianRecord  WHERE (transactionType == 'myTranType' `
| Select Transaction Class | `SELECT org.acme.biznet.PaymentTxn WHERE (mypaymentID == '001' `     ie just one transaction class
| with Order By  | `SELECT org.hyperledger.composer.system.HistorianRecord  WHERE (transactionType == 'myTranType') ORDER BY [transactionTimestamp DESC] `
| Select by Txn ID | SELECT org.hyperledger.composer.system.HistorianRecord WHERE (transactionId==_$trxID) 
|Find the History of a particular Asset(1/2) | we have a current issue open for Historian to showhistory of changes/ deltas for a particular asset - https://github.com/hyperledger/composer/issues/2458 As a workaround you can do the following - so for sample network `trade-network`
|Find History ...Asset ...(2/2) | `query selectTransaction{ description: "choose a specific commodity " statement: SELECT org.acme.biznet.Trade WHERE (commodity  == _$commodity ) }` finds all transactions of transaction `Trade` (that are used to update an asset) for a particular asset id - can also add a date range if need be - so that's a query showing what changed - obviously there is an initial AddAsset transaction (see more on that here -> chat.hyperledger.org/channel/composer?msg=eZK96GouZhbAzniB2 - So once you've updated your network to recognise the newly added query defined in your `queries.qry` and the named query is defined in your .cto model file you can try it out . The other way is to use Filters (as opposed to queries):on your REST client - so use a filter using `{"where":{"commodity":"resource:org.acme.biznet.Commodity#ABC"}, "include":"resolve"} `.. so find all transactions for asset with tradingSymbol (ID field) of 'ABC' - ie a relationship - and I can resolve the transaction and the particular asset it relates to. In this case it was 1 transaction (could be more) showing changes and I can see the transaction (with resolved) detail, for that asset. See other link I sent you above `targetRegistry` - the example shown is 'AddAsset' - transactions are ordinarily the 'update' but in any case the targetRegistries for Update/Delete would be UpdateAsset and RemoveAsset respectively



#### :card_index: [back to base camp :camping: ](#top)  

<a name="identity"></a>



### :information_source:   Identity related issues ('issue', 'request', 'register' etc in Dev/Multi-Org setup)

The following are a selection of answers, to help understand what you may be encountering. Check also [**Review card errors / resolutions**](#cardfaq) for identity issues relating to Business Network cards (part of which has identity metadata).


| Message encountered | Resolution 
| :---------------------- | :-----------------------
|Identity request failure - SSL (1) | Running `composer identity request -c PeerAdmin@byfn-network-org1-only -u admin -s adminpw -d alice` with error "Error: failed to request identity. Error trying to enroll user and return certificates. Error: Calling enrollment endpoint failed with error [Error: write EPROTO 140337610980224:error:1411713E:SSL routines:ssl_check_srvr_ecc_cert_and_alg:ecc cert not for signing:../deps/openssl/openssl/ssl/ssl_lib.c:2520: 140337610980224:error:14082130:SSL routines:ssl3_check_cert_and_algorithm:bad ecc cert:../deps/openssl/openssl/ssl/s3_clnt.c:3550:]" **Resolution:** its likely you have `https` set for the `"ca":`URL entry in connection.json file - when SSL is not enabled in your environment - change it to `http://<url>:7054` for the `ca` entry, and rebuild your card file (you can use `composer card delete user@biznetwork` to remove the old then import the new `user.card` file (with updated json file) using `composer card import -f user.card`  - then connect with it `composer network ping -c user@biznetwork` to get the credentials set.
|Identity request failure - SSL (2) | Running `composer identity request` command yields "error Error: failed to request identity. Error trying to enroll user and return certificates. Error: Calling enrollment endpoint failed with error [Error: write EPROTO 139748722722624:error:140770FC:SSL routines:SSL23_GET_SERVER_HELLO:unknown protocol:../deps/openssl/openssl/ssl/s23_clnt.c:797:  ]" **Resolution:** its likely you have `https` set for the `"ca":`URL entry - see above for resolution
|Identity enrolment failure - multi-org | Error: Error trying login and get user Context. Error: Error trying to enroll user or load channel configuration. Error: Calling enrollment endpoint failed with error [Error: write EPROTO 139720483138376:error:1411713E:SSL routines:ssl_check_srvr_ecc_cert_and_alg:ecc cert not for signing:../deps/openssl/openssl/ssl/ssl_lib.c:2520: 139720483138376:error:14082130:SSL routines:ssl3_check_cert_and_algorithm:bad ecc_cert:../deps/openssl/openssl/ssl/s3_clnt.c:3550:]**Resolution:**  This is related to TLS, if you've enabled TLS in your peers and CA nodes, make sure you pasted in the certificates (PEMs) in the connection profile


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
|Multi-stage [transaction/smart-contract related] approval with Composer ? | As opposed to multiple endorsement at a Fabric level. Example: each of the 3 participants (3 of n) submit a transaction, approving as part of a multi-stage approval. You then have 3 approvals recorded on the blockchain for the same asset id. The last approver's transaction (based on a read of a count field in the asset record in the assetRegistry) - on submitting the 3rd approval - updates the asset and resets the count to 0 (maybe also updates its approval status to 'APPROVED' etc) and immediately emits an APPROVED event back to your calling application with the asset ID and status to proceed to the next stage of your lifecycle. https://hyperledger.github.io/composer/business-network/publishing-events.html . The event would only be emitted (on approval 3) if that update was successful.
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

##### Other Misc items

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Is CouchDB the default database for Composer ? Also, can we swap CouchDB for a different database? | LevelDB is the default database for Hyperledger Fabric for the World State database. Composer uses whatever State Database is defined for the Fabric it is connecting to. CouchDB is Optional for Fabric and gets started in a separate docker container. Queries are very limited for Composer when LevelDB is used - so the Dev Fabric Composer uses has a CouchDB. The Fabric documentation and #Fabric channel will know if there are other options for the State database.

#### :card_index: [back to base camp :camping: ](#top)  

<a name="multiorg"></a>



### :information_source:  Multi Org / BYFN Composer tutorial - issues.

The following are a selection of answers, to help understand what you may be encountering. Check also [Runtime Install errors](#runtime-install) for runtime issues.

We only use the Fabric 'BYFN' sample network as a means to demonstrate the Multi-Org tutorial. We only provide for assistance for the multi-org [tutorial](https://hyperledger.github.io/composer/tutorials/deploy-to-fabric-multi-org.html) we have built to make use of that sample Fabric network


| Message encountered | Resolution 
| :---------------------- | :-----------------------
|How to add another Org to my BYFN network | Follow this tutorial -> https://www.ibm.com/developerworks/cloud/library/cl-add-an-organization-to-your-hyperledger-fabric-blockchain/index.html adding in an 'Org3' to an existing Fabric (eg. a BYFN from the Fabric docs site) - once you've deployed the Fabric marbles https://github.com/IBM-Blockchain/marbles/tree/v4.0/chaincode/src/marbles to your altered blockchain Fabric network, you are ready to build your Composer connection profiles and ultimately, multi-org Composer business network cards for blockchain participants (identities) in each organsation to be able to interact with a business network you deploy using composer runtime install and composer network start for the configured Fabric channel(s) the organisations share a ledger upon.
| Creating a blockchian network, multi-Org  - across separate physical/virtual machines | see this tutorial examples here -> https://medium.com/@wahabjawed/hyperledger-fabric-on-multiple-hosts-a33b08ef24f . There are 3 main things to consider.  1) - Initial setup and using the same generated Crypto material and sharing public keys needed for business network cards on 'each other's nodes'. 2) - Networking - making sure the containers on the **separate** machines know the addresses of the other docker containers (on each machine) and can connect. Network planning and testing is required at the beginning. 3) Create/modify the Fabric startup scripts (eg, BYFN scripts, yamls) for multihost. General advice would be to **first** try the MultiOrg tutorial on a single machine with sufficient resource. Using a MultiOrg setup on different machines, can have a few pitfalls, best to avoid. How have you set up the machines? Are you sharing the Crypto material between them? ie if you generated 2 sets of Crypto material that will be a problem. Also how are you managing addressing, resolution/lookup and communication between your 2 physical/virtual machines? These issues are that must be addressed in advance.
| Creating a blockchain network, single Org - across multiple machines (external tutorial based on 0.16.x) | See https://discourse.skcript.com/t/setting-up-a-blockchain-business-network-with-hyperledger-fabric-composer-running-in-multiple-physical-machine/602 for example on adding a peer on a separate machine. Here are some pointers if you have issues: 1) Remember your containers can't 'see' each other properly across the physical machines (docker can't do it for you!) unless you provide some means of resolution (like hosts files). You may also find (say) that if you have 2 peers running on one machine and another (one, set of) peer(s) on the other machine are trying to hit on the same Port (on that machine). Perhaps trying a Fabric setup - with just 1 peer on each machine might be a good place to start? Then build from there. 2) From a Composer perspective the key is the connection.json file (as part of the business network card you build) that you use to connect to your Fabric runtime (eg peers etc). (If you are using TLS then you need to sort out certificates and hostnames too.) 3) From a Fabric perspective, ALL your Peer containers and your orderer on your Physical machines /VMs need to have connectivity/resolution between each other. You will need to plan out the Address resolution and Routing, with port forwarding too. Depending on the way you have set up your machines you may find that the "extra_hosts:" feature of Docker Compose is useful. (I think that the Peers try to 'chat' with each other on the same port number so you will need to manage any conflicts.) 4) Before putting Composer on top of your custom Fabric we generally advise people to install the Fabric Marbles Sample demo on all their peers -> to prove Fabric is working as expected before adding Composer. If you have problems with Fabric Marbles - please check the #fabric channel. 5) At this time we don't have a Multi-Machine tutorial, but one of the members of the channel has a doc for a Single Org on 2 machines that you can read more on here . Hope this helps.
| Adding another organisation, to a multi-Org Fabric environment - eg such as BYFN | https://www.ibm.com/developerworks/cloud/library/cl-add-an-organization-to-your-hyperledger-fabric-blockchain/index.html



#### :card_index: [back to base camp :camping: ](#top)  


<a name="node-issues"></a>


### :information_source:  Node Related Issues (install issues)

The following are a selection of answers, to help understand what you may be encountering:

| Message encountered | Resolution 
| :---------------------- | :-----------------------
Error: Failed to load connector module "composer-connector-hlfv1" for connection type "hlfv1". | Often see this after running `./createPeerAdminCard.sh` for example. RESOLUTION: 1) check Node version is a [**supported version**](https://hyperledger.github.io/composer/installing/installing-prereqs.html)  from our docs 2) If a wrong version - a) uninstall node - see instructions on Stack Overflow [here](https://stackoverflow.com/questions/11177954/how-do-i-completely-uninstall-node-js-and-reinstall-from-beginning-mac-os-x) then b) install nvm c) install correct node version using NVM you installed earlier d) uninstall the composer modules using `npm uninstall -g composer-cli` etc etc  e) do a teardown (from fabric-tools then f) `rm -rf $HOME/.composer` (dot-composer) to remove old business network cards then g) decide if you're installing 0.16.x or 0.18.x Composer - you will need to set the Fabric version ; export FABRIC_VERSION=hlfv11 if (0.18.x or indeed older 0.17.x) - so that it pulls the correct Fabric runtime docker containers g) re-install composer using npm install  (again, choose 0.16.x or 0.18.x - the latter requires specific `@next` tag upon install of modules - see [release info](https://github.com/hyperledger/composer/releases) -g composer-cli etc etc - as per 'Step 1' of the Install docs page. We recommend that you use nvm to manage your node install thereafter and do everything under your current user, ie don't use sudo for Composer installs specifically - just read the install docs for guidance.


#### :card_index: [back to base camp :camping: ](#top) 


<a name="node-app"></a>

### :information_source:  Node JS Application Development Questions (eg build real-time apps, login etc)

The following are a selection of answers, to help understand what you may be encountering:

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| How to build a real-time app, login, registration, multiuser | Resolution: suggest to read the following links -> https://stackoverflow.com/questions/47791061/building-a-real-time-application-using-composer and https://stackoverflow.com/questions/48893410/hyperledger-composer-web-application-user-authentication for user registration bit. See item 2 of here -> https://github.com/hyperledger/composer-knowledge-wiki/blob/latest/knowledge.md#information_source--miscellaneous-items---fabric-uncategorised for more insight - lastly these two blogs will help you get started -> https://medium.com/hyperlegendary/how-to-build-nodejs-application-for-your-hyperledger-composer-networks-2b08ff25665f and https://medium.com/@CazChurchUk/developing-multi-user-application-using-the-hyperledger-composer-rest-server-b3b88e857ccc


#### :card_index: [back to base camp :camping: ](#top) 


<a name="passport-strategy"></a>

### :information_source:  Passport Strategy Info

The following are a selection of answers, to help understand what you may be encountering:

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Is Passport-local supported?  |Not tested - see Rocketchat thread [here](chat.hyperledger.org/channel/composer?msg=jP6znqHXa6fChLiAX)
| Passport-local (custom)  | as-is - see Rocketchat thread [here](chat.hyperledger.org/channel/composer?msg=uruWP9jJbCEQcQqNo)
| Passport-jwt info | See here https://github.com/hyperledger/composer/issues/2038 in particular comments here -> https://github.com/hyperledger/composer/issues/2038#issuecomment-340540726 and also Rocketchat thread [here](chat.hyperledger.org/channel/composer?msg=etkJ7wzdbdFnSXW79)
| Custom Passport strategy |  Useful Rocketchat thread (as-is) [here](chat.hyperledger.org/channel/composer?msg=KW4DbESMZKkPRWmPQ)
  and Rocketchat thread here for more info -> chat.hyperledger.org/channel/composer?msg=etkJ7wzdbdFnSXW79


#### :card_index: [back to base camp :camping: ](#top)  



<a name="production"></a>

### :information_source:  Production-Related Questions

The following are a selection of answers, to help understand what you may be encountering:

| Message encountered | Resolution 
| :---------------------- | :-----------------------
|Is it safe to use Composer in Production? | Hyperledger Composer is currently in pre-release with new releases most weeks and often with breaking changes - this means that at this time most people would find this too risky for production. Looking forward, a production version of Composer requires Hyperledger Fabric 1.1 - there is currently a release candidate for this so I would think we are not too far away from a Fabric 1.1 release. I'm not aware of any published date for a 1.0 production release of Composer. This post form the Mailing List for Composer explains the production issue: https://lists.hyperledger.org/pipermail/hyperledger-composer/2017-November/000044.html . Also keep an eye on the regular releases for latest information: https://github.com/hyperledger/composer/releases and Issues: https://github.com/hyperledger/composer/issues - and finally the main Hyperledger site: https://www.hyperledger.org/
| Is Composer production capable ? Isn't it just a Dev framework for prototyping? |Indeed, Hyperledger Composer is a development framework but it is also a runtime abstraction layer for executing smart contracts in which a business network (eg. say 'trade settlement'  or 'supply chain finance' etc etc) is deployed as a runtime smart contract, on the blockchain, between the parties involved, and upon which all have agreed the terms (what the model is, the data elements, the security, the access control, how identities are issued, , the contract terms blah blah). Sure, Composer makes it easier to develop blockchain applications and smart contracts - that's one of its aims, as well as adding in all of the consistency, validation or gruntwork you would otherwise have to do as an application developer (you can get an insight into that here -> https://blog.selman.org/2017/07/08/getting-started-with-blockchain-development where Fabric and Composer are compared. That is the development perspective but also the runtime perspective as I've alluded to above. Hyperledger Composer currently uses Hyperledger Fabric as the underlying 'blockchain infrastructure' or underlying blockchain technology, put simply. This can be configured in many ways in a secure Cloud environent etc etc. Composer is aimed at production deployment/scalability - ultimately, you'll be deploying business networks and smart contract transaction logic (written in a mainstream app dev language so app developers don't need specialist language skills) that will execute on the blockchain network (wherever that is deployed, however configured, privacy etc) that you (as an organisation etc) will configure (ie between the parties involved). Composer delivers native Node.js support with Composer (since v0.17.x preview release in fact), that runs with Fabric v1.1 (alpha version) and that version of Fabric will be available as a GA production ready release in the near future (obviously Fabric v1.0.x release is already out there).Furthermore, your Composer's Javascript language and engine can call the native Fabric APIs (to pull in all of the functionality that offers inside Composer contract logic)  So Composer is both the development framework, the modeler and the runtime execution on the blockchain ; as well as all managing all the other important aspects of working with that business network, such as managing identity,access control, participation, API support and connectivity elements essential to working with/transacting on a blockchain network. Hope this helps.

#### :card_index: [back to base camp :camping: ](#top)  

<a name="queries"></a>


jump to [**Query runtime issues / resolutions**](#queryissue) to see more on runtime Query related errors & resolutions


### :information_source:  Queries and Query support / examples

The following are a selection of answers, to help understand what you may be encountering or need more info on:

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Retrieving a FIELD in an Asset/Resource | Not presently supported (retrieves the whole record there is an issue raised https://github.com/hyperledger/composer/issues/3020
| ORDER BY (single) not working | for a single `ORDER BY` field you may be hitting this index issue  (you need to define an index) -> https://stackoverflow.com/questions/45919898/order-by-not-working-in-named-query/45966828#45966828 
|Indexes at the cto file level, in terms of composer concepts | Reliant on Fabric to provide the capability, before Composer is involved: see https://jira.hyperledger.org/browse/FAB-3067
| ORDER BY (multiple) not working | Not supported presently. Multiple ORDER BY fields is a current limitation of CouchDB see here -> https://github.com/hyperledger/composer/issues/1640 
| LIMIT/SKIP operators not working | LIMIT/SKIP support is blocked by Fabric presently as described here -> https://github.com/hyperledger/composer/issues/1015
| Error: Use Serializer.toJSON to convert resources | you need to do serialize the results of a query to work in the TP - see example should help you -> https://stackoverflow.com/questions/46686996/search-for-an-specific-asset-inside-a-transaction
|Query Historian for history of created Assets | the attribute `targetRegistry` is a field available to `transaction` (from `transactionInvoked` from the system transaction registry class eg. `org.hyperledger.composer.system.AddAsset` - see here https://github.com/hyperledger/composer/blob/master/packages/composer-common/lib/system/org.hyperledger.composer.system.cto#L114 - so you can query the AddAsset class and match on the target Asset Registry (eg 'BankAccount' asset) type eg something like `SELECT org.hyperledger.composer.system.AddAsset WHERE targetRegistry == 'resource:org.hyperledger.composer.system.AssetRegistry#org.acme.account.BankAccount' `
| Query an array element  |see CONTAINS below - Contains works where you supply value(s) (one of) in the list provided in a StringArray
|CONTAINS Example 1: multi-value - must contain all 3 ('AND') | ```SELECT org.acme.sample.TestAsset WHERE (stringArrayValues CONTAINS ["pumpkin", "jello", "candycane"]) ```
|CONTAINS Example 2 - parameter based string - any string in Array field 'Offers' | ```SELECT org.acme.mynetwork.Offer WHERE ( Offers CONTAINS _$offerId) ```
|CONTAINS Example 3: single-value search (Concept) in a query | ```query myquery {          description: "Example CONTAINS Query" statement:  SELECT org.acme.sample.TestAsset WHERE (conceptArrayValues CONTAINS (value == "pumpkin"))```
|CONTAINS Example 4: multi-field Concept with 'OR' example |Given data of `{ "$class": "org.acme.trading.CAsset",   "c": "Asset1",   "someConceptArray": [ { $class": "org.acme.trading.someConcept", "name": "Varun", "desc": "test1" } ] }`
|CONTAINS Example 4: continued .... |Query to find a value in either 'name' or 'desc' fields -> `query CQuery2 { description: "Select all Cs"  statement: SELECT org.acme.trading.ConceptAsset  WHERE (someConceptArray CONTAINS (name == "Varun") OR (desc == "test1") ) }` and where the model specifies the field (on an asset) as of type concept array `o someConcept[] someConceptArray`
|CONTAINS Example 5: Query the fields of an EventEmitted array (which has a concept) | chat.hyperledger.org/channel/composer?msg=se8AS3bDAuWaCjy84) programmatically, see Stack Overflow -> https://stackoverflow.com/questions/49104387/how-to-access-the-eventemitted-field-in-transaction-history-of-hyperledger-fab - or in a query definition, normally you would use CONTAINS to search in an array field. I tried this `query eventQuery {   description: "Select all events in history containing a certain Event ID"  statement:     SELECT org.hyperledger.composer.system.HistorianRecord   WHERE ( eventsEmitted CONTAINS  ( eventId == "678cb4d1-b687-4c58-8067-de9ec9be3bbf#0") ) }`  (eventId is a standard field on the event emitted - but you could use your event field)

### :information_source:  Query runtime issues / resolutions

<a name="queryissues"></a>

The following are a selection of answers, to help understand what you may be encountering or need more info on:

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Query error - Invalid operator $class:     | Error detail:  {   "error": {     "statusCode": 500,     "name": "Error",     "message": "Error trying to query business network. Error: error executing chaincode: transaction returned with failure: [mainchannel-7a107334]Calling chaincode Invoke() returned error response [Error: Couch DB Error:invalid_operator,  Status Code:400,  Reason:Invalid operator: $class]. Sending ERROR message back to peer",   "stack": "Error: Error trying to query business network. Error: error executing chaincode: transaction returned with failure: [mainchannel-7a107334]Calling chaincode Invoke() returned error response [Error: Couch DB Error:invalid_operator,  Status Code:400,  Reason:Invalid operator: $class]. Sending ERROR message back to peer\n    at channel.queryByChaincode.then.catch (/home/composer/.npm-global/lib/node_modules/composer-rest-server/node_modules/composer-connector-hlfv1/lib/hlfconnection.js:820:34)\n    at <anonymous>  }  }   **Resolution**: This was due to a match of composer versions to versions of fabric. The release notes detail which versions of composer will work with which versions of fabric. eg
https://github.com/hyperledger/composer/releases/tag/v0.18.x
 
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

The following are a selection of answers, to help understand what you may be encountering - in some cases, multiple resolution choices may be offered:

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Error: No valid responses from any peers  | see below
| Error: Error: Endpoint read failed |  This is likely to be a connection (.json) config issue -  it needs to be configured for TLS (or non-TLS if that's your setup) communications - depending on your desired configuration.
| Error: Error: Error trying to ping. Error: Composer runtime (e.g 0.16.1) is not compatible with client (eg. 0.16.0) | You have a mismatch of versions. This could occur using Composer REST server, APIs, App Generator or CLI. Suggest to uninstall the relevant modules eg `composer uninstall -g composer-cli` (also `composer-rest-server`, `generator-hyperledger-composer`, `composer-playground` npm modules that you have installed - then `npm install -g` the same level - check [**release notes**](https://github.com/hyperledger/composer/releases/) for the current release level - ensure you do so as a non-privileged user.
|Error: Error trying to instantiate composer runtime. Error: Error: Invalid results returned ::NOT_FOUND (1/2)| Has a `composer runtime install` been done? Which document/tutorial are you following? Have you forgotten to create/start the Fabric channel your BN profile is referring to ? Are you trying to run on Windows 7/10 ? Please check these and take appropriate action(s).
|Error: Error trying to instantiate composer runtime. Error: Error: Invalid results returned ::NOT_FOUND (2/2)|Are the channel names correct? Do they match or are there different names specified for the channel in your Fabric config metadata / files vis-a-vis the channel name in the `connection.json` file (which is a constituent part of the .card file you're using for the composer CLI command) - this is often a reason / cause for the error you see.
|Error: Failed to load composer module "composer-connector-undefined" for connection type "undefined"  | This occurs because you have a HLF v1.1 runtime environment (fabric v1.1-alpha containers) and composer runtime of 0.16.x - this will not work - please see the 'ready reckoner' table of 'which Composer version works with which Fabric version -> https://github.com/hyperledger/composer/wiki STEPS TO FIX so you can use Composer v0.16.x :  1) unset FABRIC_VERSION variable  2) npm uninstall -g composer-cli 3) npm uninstall -g composer-playground and other modules eg composer-rest-server 4) rm -rf $HOME/.composer 5) from fabric-tools run ./teardownFabric.sh 6) remove any lingering dev-xx business network containers you may previously have deployed (they are separate docker containers eg `docker stop xx` ; `docker rm xxx ` ) 7) npm install -g composer-cli as non-root user 8 ) rm -rf fabric-tools dir && mkdir fabric-tools dir 9) get newer fabric tools zip -> curl -O https://raw.githubusercontent.com/hyperledger/composer-tools/master/packages/fabric-dev-servers/fabric-dev-servers.zip && unzip fabric-dev-servers.zip inside fabric-tools 10) export FABRIC_VERSION=hlfv11 ; and from fabric-tools ./startFabric.sh ; ./createPeerAdminCard.sh 9) npm install -g composer-playground, composer-rest-server etc 10) then try composer runtime install -n mynetworkname -c PeerAdmin@hlfv1 and it should work - then you can proceed to do a `composer network start` command to instantiate a business network, as normal
|[eventhub_producer] Chat -> ERRO 37f error during Chat, stopping handler: rpc error: code = Canceled desc = context canceled | The problem you are seeing is is likely because the identity you are using to try to install the runtime onto a peer is not authorised to do so. Unfortunately the logs info from Fabric don't indicate this . If you are using the Composer development server then it creates an identity for you called PeerAdmin which you should use to deploy business networks. If you are using your own fabric then you need to look into how to import crypto material when building a peer administrator card and for which it has the PeerAdmin role to do so. Or are you following this tutorial ? https://hyperledger.github.io/composer/tutorials/deploy-to-fabric-single-org

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

<a name="scripting"></a>



### :information_source:  Scripting Tips, Aids & Links

The following are a selection of answers, to help understand what you may be encountering - in some cases, multiple resolution choices may be offered:

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| would like to convert JSON object to JSON string (or vice versa) in TP function. How can I do this? eg JSON.parse and JSON.stringify?| some examples here for you -> https://stackoverflow.com/questions/46686996/search-for-an-specific-asset-inside-a-transaction/46959640#46959640 and JSON.parse -> https://stackoverflow.com/questions/46185606/would-like-to-upload-json-format-datafile-into-hyperledger-composer-asset-regist


#### :card_index: [back to base camp :camping: ](#top)  


<a name="transproc"></a>


#### :information_source: Transaction processor errors


The following are a selection of answers, to help understand what you may be encountering:

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Timestamps in transactions - how to get the same timestamp across all peers | The timestamp is client side generated pls note - so will be the same across all endorsing peers. Client application don't actually generate the timestamp, it is generated by Composer so the format will be consistent - the timestamp  represents when a  transaction is submitted, not committed.
| Transaction rollback - errors in transactions | Changes made by **transactions** are atomic, either the transaction is successful and all changes are applied, or the transaction fails and no changes are applied. Any exceptions thrown from a transaction processor function will cause the entire **transaction** to be rolled back, leaving no changes to the blockchain or world state. So nothing is persisted if a transaction fails, Composer throw an exception and the transaction gets rolled back by Fabric. https://hyperledger.github.io/composer/reference/js_scripts.html
|Can a Composer transaction call another Transaction? |No - a transaction cannot presently be called from another transaction. In Hyperledger Fabric (which Hyperledger Composer is currently using), you are not able to read your own writes from within a transaction, meaning that you cannot add something to a registry in your TP and then read it, in the same transaction. However, a transaction processor can call other functions to allow for code modularisation, but it will only be registered as a single transaction request, in the transaction registry. 
| Calling a TP **function** from another | see example below - two parts
| (1) Calling from main script file - sample callout from `script.js`: | `var returnVal;     //And now call the function in func2.js var returnVal= sayHello("Jon Doe"); 		 var code = returnVal.code; var msg= returnVal.message; console.log('The returned message and code are: Msg-> ' + msg + ' the code -> ' + code );` // output is "The returned message and code are: Msg -> Hello Jon Doe the code -> 11"
| (2) Called function - defined in file `func2.js` |  `function sayHello(name) { retMsg = "hello " + name; return {code: 11, message: retMsg}; }` 
|Catching errors in a JS TP function | sample code is shown (make sure to return a Promise) `return bizNetworkConnection.submitTransaction(transaction).then((result) => {             console.log("Tx Result", result) ; }).catch((exception) => { console.log("error", exception);` })
| Calling out of TPF using `post(url, resource)` and setting headers | here's an example here -> https://hyperledger.github.io/composer/integrating/call-out.html   application/json headers not yet supported in post -> see example https://stackoverflow.com/questions/43209924/rest-api-use-the-accept-application-json-http-header - issue raised for this -> https://github.com/hyperledger/composer/issues/3071
| Do I really need to model transactions? | there are the generic system transaction types (eg adding a new asset is `AddAsset` or AddParticipant or IssueIdentity etc etc) and there are your modeled transaction classes (that you refer to) like `SettleTrade` or `IssueTicket` or `PayVendor` etc etc . You can try out sample models (such as shown here https://github.com/hyperledger/composer-sample-networks/tree/master/packages/trade-network) under the 'sample-networks repo there is a 'model' directory in each business network sample you can look at. But if you want to (or your business processes dictate) update assets or participants in your business network, you would need to define a transaction of sorts (it defines the parameters of change (eg. change of owner, adding a deposit to a bank account etc etc), with which to update a target asset or participant registry). Unless of course you were just adding assets or participants only each time (create asset, create participants) and not require a specific transaction class to update them later (ie one-off record instance, for queries only etc) - see the intro here for more info -> https://hyperledger.github.io/composer/latest/introduction/introduction

#### :card_index: [back to base camp :camping: ](#top)  

<a name="upgrade"></a>


#### :information_source: Upgrading Composer (eg. Runtime, Modules, Clients, Generators etc)


The following are a selection of answers, to help understand what you may be encountering:

| Message encountered | Resolution 
| :---------------------- | :----------------------- 
| Upgrading Composer 0.16.x -> 0.18.x | INFO: Presently v0.16.x = 'latest' and v0.18.x = 'next' - when it comes to 'tag' names to append for npm install commands (see table at https://github.com/hyperledger/composer/wiki). Composer v0.18 (current @next release or older 0.17.x with alpha) works only with H/Fabric v1.1-rc1 (docker images), so you will need to pull those docker containers (fyi this is part of the Composer tools dev setup for "@next" environments). To work with v0.18.x environments (pulling the composer '@next' tagged modules) and to clear up v0.16.x environments - these steps should help: 1)	Make backups (eg to JSON text files etc) copies of your model, acl, queries, transaction js etc. If you have exported your Network BNA file, then all the better 2)	Remove your 'old' Fabric (v1.0.x) docker images with the Fabric-tools script teardownFabric.sh 3) 	uninstall existing composer code `npm uninstall -g composer-cli` (repeat for composer-rest-server and composer-playground as necessary) 4)	Remove the 'old' business network cards folder eg. rm -rf ~/.composer - again this is to removed old dev cards - be careful to save any `connection.json` metadata you created (if need be) for 0.16.x multi-machine environments that you've built - DO NOTE HOWEVER that the connection profile format in v0.18.x is different to v0.16.x (but you can merge in your v0.16.x profile sections to conform to 0.18.x) see here for 0.18.x (and indeed same as older 0.17.x) format -> https://hyperledger.github.io/composer/next/reference/connectionprofile.html  5)	Stop (using docker stop) and remove your old deployed 'chaincode' business network containers (ie v0.16.x) 6) export FABRIC_VERSION=hlfv11     7) Follow the instructions for 'next' install here https://hyperledger.github.io/composer/next/installing/development-tools.html  8) With new Fabric v1.1 RC1 up and running, deploy the current composer runtime to the peers and do a network start of your BNA on the channel configured in your connection profile. 9. recreate any business network cards as before, with the new connection profile format, import and test - for the business network in question 10. reconstitute any test data you have / had via Node-red JSON import etc or use Composer Playground.

#### :card_index: [back to base camp :camping: ](#top)  

<a name="upgradebn"></a>


#### :information_source: Updating Business Network definitions


The following are a selection of answers, to help understand what you may be encountering:

| Message encountered | Resolution 
| :---------------------- | :-----------------------
| Controlling authority to update a B N | this is a change management issue (a business network model and definition and a policy for aggregating signatures or authority to be able to update a business network using `composer network update`) . By the time it gets to `composer network update` it seems, its already too late. The Network ACLs in Composer, can restrict who (ie a participant of that network) was allowed to do the network update. Its possible that such a record is only created once all the signatures are collected, as a one-time operation (so that the network update could take place). A sample ACL restriction that could be used is `rule  networkControlPermission {   description:  "networkControl can access network commands"   participant:  "org.acme.vehicle.auction.networkControl"    operation: all    resource: "org.hyperledger.composer.system.Network"    action: ALLOW   }`


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
