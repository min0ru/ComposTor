# ComposTor
This project demonstrates usage of docker-compose to build set of load-balanced tor clients providing socks5 proxies with

Each tor service provides socks5 proxy running on port 9150.

Nginx server and docker built-in DNS (in round-robbin mode) is used to balance incoming socks5 connections between multiple Tor client instances.

## Disclaimer
**This is proof-of-conept project created for learning purposes only.**

**Please do not use it for anything that needs real security or purpose of anonymity.**

**Provided configuration was not verified by security experts.**

## Requirements
```bash
sudo apt install docker.io docker-compose # (Debian/Ubuntu)
```

## Run Services
```bash
docker-compose up --scale tor=10
```

Use --scale tor=NUMBER parameter to set number of tor instances run in parallel.

Note that exit node for each instance is selected randomly so there is no garantee that you will get unique exit node ip for each instance.

Point your proxy settings for an applicaton of choice to socks5://localhost:4000

Tor services need some time to setup connection chains.

## Demonstration

Run 50 requests in 10 parallel curl processes.

```bash
sudo apt install curl # (Debian/Ubuntu)
yes "https://icanhazip.com/" | head -n 50 | xargs -n1 -P 10 curl --proxy socks5://localhost:4000/
```

Output should show you multiple different Tor exit nodes ip addresses.

## Inspiration
Based on https://github.com/tor-on-synology/tor-client-minimal tor service image and configuration.

Inspired by https://github.com/trimstray/multitor project.
