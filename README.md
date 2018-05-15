# RSSC2018 Unsecured Devices Lab

This lab will guide you through using an internet search engine called Shodan. Shodan crawls and indexes information about the devices it finds on the internet. This data can then be searched by anyone. Shodan records and indexes information such as host IP address and host name. It also performs a port scan and saves information about what it finds such as open TCP/UDP ports. It tries to initiate a session with the servies listening on those open ports and then indexes the data retrieved from that session.  The result is a powerful tool for gathering intelligence about potential attack targets. This information is often used by hackers of all shades of hats as they conduct their business.

### Warning - You will be using Shodan to query information about other people and you will be connecting to their unsecured equipment during the course of this lab. If you feel uncomfortable with this, please abort this exercise now. If you intend to continue with this exercise, please be corteous and don't perform any malicious actions on systems that are not yours.

## Prerequisites

In order to execute this lab, there are a few things you will need to get started.
1. Computer system with Python installed and the easy_install module loaded. These tools are available from Kali Linux and we will assume Kali Linux is used for this lab.
2. A modern web browser with access to the internet.
3. A free Shodan account and associated API key.

## Getting Started

1. We need to open a command terminal. If there is not already a terminal window open, click Applications > Terminal.
2. We need to make sure that the Shodan command line tools are installed. 
```bash
easy_install shodan
```
3. We need to initiate our API key.
```bash
shodan init <API key>
```
## Exercise 1 - Shodan CLI basics

We can use the Shodan CLI to perform queries and have the results returned to our terminal window.  This is an easy and quick way to gather basic information about a host, and it can also be useful for scripting.

For starters, let's find out what Shodan knows about us:
```bash
shodan myip
```

