# VPNoy

## Requirements

1. NODE
2. NPM
2. DOCKER
3. DOCKER-COMPOSE

## Setup

    npm run ovpn:setup

## Client Management

#### Client Create

Generate a client certificate [Change clientname to your desired client's name]

    sudo docker run -v $PWD/data:/etc/openvpn --log-driver=none --rm -it kylemanna/openvpn easyrsa build-client-full clientname

Retrieve the client configuration with embedded certificates [Change clientname to your desired client's name] [There are two clientname in there.]

    sudo docker run -v $PWD/data:/etc/openvpn --log-driver=none --rm kylemanna/openvpn ovpn_getclient clientname > $PWD/clients/clientname.ovpn

The new ovpn file can be found on `$PWD/clients/clientname.ovpn`

#### Client List

See an overview of the configured clients, including revocation and expiration status:

    sudo docker run -v $PWD/data:/etc/openvpn --log-driver=none --rm kylemanna/openvpn ovpn_listclients

#### Revoking Client Certificates

Revoke `clientname`'s certificate and generate the certificate revocation list (CRL) using [`ovpn_revokeclient`](/bin/ovpn_revokeclient) script :

    sudo docker run -v $PWD/data:/etc/openvpn --log-driver=none --rm kylemanna/openvpn ovpn_revokeclient clientname remove

The OpenVPN server will read this change every time a client connects (no need to restart server) and deny clients access using revoked certificates.

