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
3. We need to initiate our API key. You can copy this from the text file called 'api-key.txt' from your home directory.
```bash
shodan init <API key>
```
## Exercise 1 - Shodan CLI basics

We can use the Shodan CLI to perform queries and have the results returned to our terminal window.  This is an easy and quick way to gather basic information about a host, and it can also be useful for scripting.

For starters, let's find out what Shodan knows about us:
```bash
shodan myip
```
Shodan will return the public IP address that you're using to query its servers.  Now, let's see what Shodan knows about us. Use the IP address returned in the previous step below:
```bash
shodan host <Your IP address>
```
At this point you may see a lot of information or a little but at the very least you'll should see (scroll up if necessary):
* Hostname(s)
* City
* Country
* Organization (ISP or company)
* Count of discovered open ports
* Information gathered about any open ports.

If Shodan does not return anything for your IP address, try another that you might know from work or school. Or you can try one of these

```bash
151.101.129.140

66.151.147.38

166.251.122.240
```

Let's see what Shodan knows about a few more common IP addresses:
```bash
shodan host 8.8.8.8

shodan host 1.1.1.1

shodan host 198.60.22.2
```
The last host in that list is a local internet provider in Utah.  As you can see, Shodan has cataloged information for each of those popular DNS providers.

Now we can start searching for things that might be more troubling to an infosec professional.
```bash
shodan count port:23
```
Are there really that many hosts on the internet listening on the default telnet port? Supposing that some of them are honeypots or other security devices, that still leaves a lot of hosts with telnet enabled publicly accessible from the internet.  

How about in your locale?
```bash
shodan count city:"Salt Lake City" port:23
```
Likely much lower but still someone alarming. How about in a big city like Los Angeles? We'll limit the output in the following example for terminal window size limitations. *Hit the letter 'q' to break out of the search results windows from here on out.*
```bash
shodan count city:"Los Angeles" port:23
shodan search --fields port,ip_str,hostnames city:"Los Angeles" port:23 --limit 10
```
Now what happens when you search for hosts listening on port 23 that have are running with known weak CLI authentication?
```bash
shodan search --fields port,transport,ip_str,hostnames "Easy Setup" port:23 --limit 10
```
Now try telnet'ing to one of the IPs that show up in your list.  If at first you don't succeed, try again and you'll eventually hit one.
```bash
telnet <IP address>
```
If you successfully connect, you can use the arrow keys to navigate around the main menu.  *Please remember that this is not your device and that to change the configuration or otherwise tamper with it is not only rude, it's probably illeagl.*
The device you've connected to is an old DSL modem, most likely from the early 2000s.

When you're done, send the ctrl+] key combination to break out of the telnet session, and issue the 'quit' command at the prompt.

For one final scare tactic, let's perform the following query:
```bash
shodan count port:445
```
Yes, be afraid. Be very afraid.

## Exercise 2 - The Shodan Web Interface

So it turns out that the CLI is not actually the easiest or most engaging method of interacting with Shodan. Shodan has a nice web interface that we'll now spend some time using. 
Click ```Applications > Firefox ESR``` to launch the Firefox browser. Enter ```shodan.io``` into the address bar once Firefox loads. Shodan does not require an account or login to do basic searches. For instance, enter ```198.35.26.96``` into the search field. You'll see that this is one of the IP addresses used by Wikipedia.  On the left side of the page There is basic information about the IP address itself, and on the right side you'll see the information that Shodan has gathered about the services running on this IP address. HTTP header information for both port 80 and 443 has been gathered and indexed, along with the SSL certificate for the site.

In order to perform more advanced queries, a free Shodan account is required.  Please login using your credentials for the next steps.

Enter ```port:32400``` into the search field. This should give us a good idea f how many publicly accessible Plex servers are available online. Note that the left column gives interesting statistics regarding geography, orgainziations, operating systems, and other details about TCP port 32400. We can gather that the Plex is most popular in the United States, running on Linux and being hosted on Comcast internet connections. Notice on the right column, you'll notice the little Plex icon.  This is the favicon for Plex, which is itself a searchable property as well.

The uptick in the number of botnets in the last few years has been largely due to the increased number of unsecured, internet connected devices such as cameras, thermostats, etc. Next we'll see if we can find some of those systems as well.  Enter ```Server: SQ-WEBCAM``` This search string is common among a popular line of inexpensive security camera DVRs. You'll see an alarming number of results that are returned, and this is only one type of DVR. Right click on one of the search results that says '--- VIDEO WEB SERVER ---' and choose 'Open Link in New Tab'.  It might take a couple of tries, but eventually you'll get the login page for someone's DVR. If you're feeling really adventurous, try logging in with the default credntials of admin/admin.  You're now logged into someone's DVR which is probably ripe to be owned by deploying an exploit to act as a pivot point for an intruder to get into their network.

For some more fun, let's see how many Cisco VPN gateways are listening in Provo, UT. Enter ```webvpn city:Provo``` into the search field.  How about routers that are still vulnerable to the 2013 insecure TCP port 32764 expoit? ```port:32764```

## Conclusion

As you can see, the days of needing an NMAP scan to complete before attacking a target are in the past with tools like Shodan. Spend a minute or two exploring the rest of the Shodan interface. The 'Explore' tab contains clickable links to some easy to use queries. Have fun and be safe!






