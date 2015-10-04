---
title: Setting dnsmasq for wildcard domains in ubuntu
tags:
    - Development
    - DNS
    - Linux

---

This is useful for development environments like vagrant and docker, where
you can play with FQDN instead of IP:

    # edit according to your needs
    DOMAIN=dev
    IP=10.0.10.100

    # append to configuration file
    sudo echo "address=/$DOMAIN/$IP" > /etc/NetworkManager/dnsmasq.d/dev.conf

    # restart network manager
    sudo stop network-manager
    sudo start network-manager

The above will resolve the `*.dev` domain on IP 10.0.10.100.

Note that this is the version bundled with network manager and not the dnsmasq package.
