Of the computers that may have sync issues-- here is 1 solution to check

Port 8444 is the [port](https://en.wikipedia.org/wiki/Port_%28computer_networking%29) through which other Chia computers can communicate with your PC, Chia can quickly talk to other PCs, link up, and start downloading and syncing with Chia blockchain.

The network is undergoing rapid growth and expansion. Many of the newly arrived Chia peers (computers) do not open up port 8444. It makes it very hard for the network. 

Please use this link to check if your router's port 8444 is closed. It is recommended you open it: 
* [https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/
)
* [https://portchecker.co/](https://portchecker.co/)

All peers (computers) with a closed port 8444 are completely dependent on pc peers with open port 8444. They are the only PCs they can talk to. If you got 1,000 nodes with an open port 8444, but 20,000 nodes with a closed port 8444, trying to sync, it will only just be able to theoretically have enough IP's estimated 3,000 can sync at a time, while the other wait for another open chia user with open port 8444.   Right now (Mid April '20) it seems that number is even much worse. And it causes a scenario where there just isn't enough open port 8444 peers to serve all the closed port 8444 peers. The only way around this is to ensure that you got an open port 8444.

If you somehow are able to open up your port 8444, within a minute, you will have peers connecting to you, at least, you should have a much easier time to get connections established. 

If you would like to speed up connecting to other nodes and syncing, add one of these introducer nodes:
* North Asia `introducer-apne.chia.net:8444`
* South Asia `introducer-apse.chia.net:8444`
* Western North America: `introducer-or.chia.net:8444`
* Eastern North America `introducer-va.chia.net:8444`
* Europe: `introducer-eu.chia.net:8444`

There is a public node share the available 8444 peers every hour.

* [chia.keva.app](https://chia.keva.app)

On the subject of how to port forward, all I'm going to say, it's different from router brand to brand, even from model to model.
Google.com presents 60+ million hits on "[Port forwarding](https://www.google.com/search?q=port+forwarding&source=hp&ei=HDB-YILlDoOB9u8P-72_4AM&iflsig=AINFCbYAAAAAYH4-LJZmDqT9olTepGniZToDYoAz5s4Q&oq=port+for&gs_lcp=Cgdnd3Mtd2l6EAMYADICCAAyAggAMgIIADICCAAyAggAMgIIADICCAAyAggAMgIIADICCAA6CAgAELEDEIMBOgUIABCxAzoICC4QsQMQgwE6CwgAELEDEMcBEKMCOgUILhCxAzoICAAQxwEQrwE6CAguELEDEJMCOgIILjoECAAQCjoGCAAQChATOggIABAKEB4QEzoGCAAQHhATUMMPWLghYJgqaANwAHgAgAFeiAGlBZIBAjEwmAEAoAEBqgEHZ3dzLXdperABAA&sclient=gws-wiz)"

**Note:** The Chia software does what it can to open port 8444 automatically for you. But the UPnP protocol is a hit and miss kind of thing, some dont have a good implementation of it, others simply don't have it enabled by default. Please test for open port 8444!

# Detailed explanation 
A regular pc can communicate **out** with endless ports-- if the user is sending a signal out -- pc opens a port -- signal goes out, pc closes the port. 
Chia uses port 8444 as instant verified communication.  So an open port can allow instant communication and start the blockchain sync.  Signal comes in on port 8444- that Chia pc is verified, then **both** user's pc, opens their own "communication ports ex port 8421" and that new user can now sync and now they are linked together forming part of Chia mesh. 

 If the users port 8444 is closed, the users pc has to start sending multiple signals out and hope that a pc with open port 8444 will link with them, then the sync starts. (1) pc can only link up a few pc and with so many other chia users coming on board, they all have to wait.  Keep in mind, Chia is built on a mesh network, the blockchain is shared among all the users, not from central pc. 

## Dealing With Carrier-Grade NAT

Opened port 8444 on your router but still not getting connections? With the exhaustion of the IPv4 space, it's increasingly common for ISPs to use [Carrier-Grade NAT](https://en.wikipedia.org/wiki/Carrier-grade_NAT) (CGN, CG-NAT, NAT444) to put multiple customers behind a single IP address. In this case, even if you open 8444 on your router, other nodes will not be able to connect to you. There are a couple options:

1. Ask your ISP for a dedicated IP address. They'll probably want more money, and may require you to upgrade to a "business" plan.

2. Establish a VPN tunnel through the NAT to a cloud server with a public IP address. It's easier than it sounds, and can cost as little as $3-5 a month for a cheap cloud server (some common cloud server providers: AWS, GCP, Digital Ocean, Vultr, Hetzner, Linode). When selecting a provider and server size, pay careful attention to bandwidth; a Chia fullnode isn't too demanding, but can require several GB per day. 1 TB per month is typical of lower-cost VPSs and should be plenty (do keep an eye on it though, bandwidth overage costs can be expensive!).

Setting up a VPN used to be a daunting task, but [Wireguard](https://www.wireguard.com) has greatly simplified it. The summary is you run Wireguard on both your home server and the cloud server:
* the cloud server is configured to listen for VPN connections from your home server, and route all traffic incoming on 8444 to your home server, while also routing outgoing traffic from your home server to the internet.
* the home server is configured to route all internet traffic (but not local) through the cloud server, while periodically sending a "keepalive" packet to ensure the connection stays open.

A more detailed write-up with example wireguard configuration can be found [here](https://www.kmr.me/posts/wireguard/).