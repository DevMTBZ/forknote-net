---
title: RPC Wallet overview and configuration | Forknote
layout: documentation
body_class: developers
subnav_class: docs-documentation
sidebar_nav: sidebar-documentation
---

# RPC Wallet overview and configuration

* TOC
{:toc}

This wiki section describes Forknote integration process into your service with Forknote e-commerce solution called Forknote RPC Wallet. Forknote RPC Wallet is a HTTP server which provides JSON 2.0 RPC interface for Forknote payment operations and address management. Forknote RPC Wallet allows you to accept incoming payments, generate an address for each user via [Forknote RPC Wallet JSON RPC API][Forknote_RPC_Wallet_JSON_RPC_API] and much more. [Forknote RPC Wallet JSON RPC API][Forknote_RPC_Wallet_JSON_RPC_API] page contains detailed description of every method. 


##Configure Forknote RPC Wallet

To configure RPC wallet you can use both command line and config file. Config file allows you to configure your settings only once and use "--config" option further. The command below launches Forknote RPC Wallet with a specific config file:

     $ ./walletd --config /home/Downloads/myconfig.conf

To get help on available options run:

     $ ./walletd -h

Please note, Forknote RPC Wallet config file may consist only of these options:

Option | Description
-----------|-----------|
bind-address | Which address to bind Forknote RPC Wallet to. Default value is 0.0.0.0
bind-port | Which port to bind Forknote RPC Wallet to. Default value is 8070
daemon-address | Forknote daemon (forknoted) address for remote daemon connection infrastructure
daemon-port | Forknote daemon (forknoted) port for remote daemon connection infrastructure. Default Forknote daemon ports are 8080 and 8081
container-file | Mandatory'''. Your container's file name
container-password | Mandatory. Your container's password
log-file | A name of log file that you want to use for logging. Default is walletd.log
server-root | Working directory that you wish to use for Forknote RPC Wallet. Default is current working directory.
log-level | Level of logging. Default is 1.
testnet | Allows you to run Forknote RPC Wallet in testnet.
local | Option that allows you to start Forknote RPC Wallet as an in-process node
p2p-bind-ip | Interface for p2p network protocol
p2p-bind-port | Port for p2p network protocol
p2p-external-port | External port for p2p network protocol (if port forwarding used with NAT)
add-peer | Manually add peer to local peerlist
add-priority-node | Specify list of peers to connect to and attempt to keep the connection open
add-exclusive-node | Specify list of peers to connect to only. If this option is given the options add-priority-node and seed-node are ignored
seed-node | Connect to a node to retrieve peer addresses, and disconnect
hide-my-port | Do not announce yourself as peerlist candidate
MAX_TRANSACTION_SIZE_LIMIT | Maximum size of the transactions sent
SYNC_FROM_ZERO | Sync from block 0. Use for premine wallet or brainwallet

### Blockchain options

See [Blockchain options][Blockchain_options]


##Example of a config file

<pre class="terminal">$ cat ./fakecoin.conf 

BYTECOIN_NETWORK=10101010-1010-1010-1010-101010101010
CRYPTONOTE_DISPLAY_DECIMAL_POINT=12
CRYPTONOTE_NAME=fakecoin
CRYPTONOTE_PUBLIC_ADDRESS_BASE58_PREFIX=72
GENESIS_COINBASE_TX_HEX=010a01ff0001ffffffffffff0f029b2e4c0271c0b42e7c53291a94d1c0cbff8883f8024f5142ee494ffbbd08807121013c086a48c15fb637a96991bc6d53caf77068b5ba6eeb3c82357228c49790584a
P2P_STAT_TRUSTED_PUB_KEY=

UPGRADE_HEIGHT_V2=1
UPGRADE_HEIGHT_V3=2

p2p-bind-port=8080
rpc-bind-port=8081

seed-node=127.0.0.1:8080

container-file = mycontainer
container-password = mypassword
daemon-port = 8081
bind-port = 9090

</pre>

*Note: config file's path is relative to current working directory, not server root.*

*Note: options "container-file" and "container-password" should ALWAYS be set (in either command line or config file mode).*

*Note: "container-file" and "log-file" options are relative to "server-root". "server-root" default is the current working directory.*


##Generate a new wallet

To start using RPC wallet you must first generate a container. Container file is the only file that stores all data required to run your service. It contains user addresses and private keys required to operate them. Make sure to backup this file regularly.

To generate a new wallet you should run the following command:

     $ ./walletd --config /home/Downloads/myconfig.conf --generate-container

where: 

* **&lt;mycontainer&gt;** is the container file name and a path to it (relative or absolute); path is optional in this argument, specifying only a container's name will result in new file located in the same folder as RPC Wallet
* **&lt;mypass&gt;** is a secret password for the new wallet file. Whichever you like;
* **--generate-container** option tells RPC wallet to generate container file and exit.

*Note: if **&lt;mycontainer&gt;** exists Bytecoin RPC Wallet will show you the notification and will ask you to provide a different name.*

If the operation was successful you will get a corresponding message with your new Bytecoin address. At the same time Bytecoin RPC Wallet will save your container on the local disk (in the same folder where Bytecoin RPC Wallet is located) and shut down


##Start Forknote RPC Wallet

There are two ways to start Forknote RPC Wallet: 

###Start with a remote connection to the Daemon

Remote connection allows you to bind your Forknote RPC Wallet to a remote Forknote daemon (forknoted). You may establish Forknote daemon on both local and remote machines and connect to. Such type of connection allows you to start Forknote RPC Wallet on a relatively slow machine while heavy loaded daemon is going to work on a separate powerful server. 

* For local daemons use localhost or 127.0.0.1 as an IP address.
* For remote daemons specify the remote daemon's IP address.

Default Forknote daemon ports are 8080 and 8081. 

Add the following lines to your configuration file to start Forknote RPC Wallet with a remote connection: 

    daemon-address=<remote_ip>
    daemon-port=8080

*Note: Forknote daemon (forknoted) should be running at the moment RPC wallet is starting in a remote connection mode.*

*Note: Forknote RPC Wallet will still provide some functionality even if Daemon server fails. For example, you will be able to generate addresses for your users.*

###Start as in-process node

You can also start Forknote RPC Wallet with an in-process node. This allows you to start RPC Wallet out-of-box with no external daemon required. You will get a fully functional node for Forknote network inside Forknote RPC Wallet. You don't have to download or install anything besides Forknote RPC Wallet. This approach will help reduce the overheads required for infrastructure maintenance.

Use the following command to start Forknote RPC Wallet with an in-process node

     $ ./walletd --config /home/Downloads/myconfig.conf --local

##Run Forknote RPC Wallet

Forknote RPC wallet can be started in both daemon and console modes. 

* **Daemon mode** - Forknote RPC Wallet is launched in the background, while you can continue to work with a console window. 

* **Console mode** - Forknote RPC Wallet is launched and prints log messages on the screen.

Forknote RPC wallet starts in console mode by default.

###Start as daemon (UNIX only)

To start RPC wallet as daemon just set "--daemon" (or short "-d") option.

     ./walletd --config /home/Downloads/myconfig.conf --daemon

*Note: it's a common practice for daemons to set server root directory.*

Server root is the directory where RPC Wallet stores all its files. All relative paths in RPC Wallet configuration are relative to the server root directory.

###Start as service (Windows only)

To run RPC wallet as a service on Windows you have to do the following:

1. Create a config file and place it in the same directory as your RPC wallet's executable resides in. 

    A note for Windows Users: In case the server root in config file is not specified all paths should be ABSOLUTE. If you set server root you can use relative paths (relative to your server root);

2. Register your Forknote RPC Wallet as a service. To do so, run the following command as an ADMINISTRATOR:

        walletd.exe --config /home/Downloads/myconfig.conf --register-service

3. After you see message about successful service registration you can run it in your Services panel.


###Uninstall service (Windows only)

If you want to delete RPC wallet you have to unregister windows service first (if you have registered it before). Run as an ADMINISTRATOR:

     walletd.exe --config /home/Downloads/myconfig.conf --unregister-service

##Forknote RPC Wallet JSON RPC API

Forknote RPC Wallet API allows you to create addresses for your users, accept and send transactions and much more.

Detailed description for every Forknote RPC Wallet API method can be found here: [Forknote RPC Wallet JSON RPC API][Forknote_RPC_Wallet_JSON_RPC_API]


[Forknote_RPC_Wallet_JSON_RPC_API]: /documentation/rpc-wallet/json-rpc-api/
[Blockchain_options]: /documentation/daemon/#blockchain-options
