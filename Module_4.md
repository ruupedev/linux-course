#### Connecting via SSH, password change and update

The first problem I faced when trying to connect to the server via SSH was that ssh-client was not installed on the vm. 

![[img4/module4_img1.png]]

After installing openssh-client connecting,changing the password and updating and upgrading worked without any issues.

![[img4/module4_img2.png]]
![[img4/module4_img3.png]]

#### Checking Status

Everything looked normal

![[img4/module4_img4.png]]

#### Generating SSH-keys

Nothing out of the ordinary here

![[img4/module4_img5.png]]

#### Copying the keys to the server

At this stage I first tried to do the copying of the public key on the server. After a few minutes I figured out that it (kind of obviously) needed to be done on the "machine side".

![[img4/module4_img6.png]]

After getting the SSH-keys set up connecting with them worked as intended.

![[img4/module4_img7.png]]

#### Installing and configuring the firewall and security updates

Right after the SSH authentication has confirmed I moved on to installing the firewall and allowed SSH. After that turned the firewall on as well as the automatic security updates. Later on I also allowed HTTP and HTTPS.

![[img4/module4_img8.png]]
![[img4/module4_img9.png]]![[img4/module4_img10.png]]![[img4/module4_img11.png]]

#### Networking
##### What I can see
- The VM's private IP: 208.24.0.40
- The default gateway: 208.24.0.1
- The subnet: 208.24.0.0/24
- MAC address
- Routes set by DHCP
##### What I cannot see
- I cannot see the actual public IP. Azure's maps the IP outside the VM so it cannot see the actual public IP.

![[img4/module4_img12.png]]

#### Apache

At this point I installed apache and updated the index.html file with some custom html. I installed micro cause nano started to feel a bit too "nano" for this job.

![[img4/module4_img14.png]]
![[img4/module4_img15.png]]
![[img4/module4_img16.png]]

#### ngrep

###### HTTP

Installing ngrep went without problems. But trying to track the curl with the given IP address did not work. Only after changing the IP to 208.24.0.40 ngrep started showing logs. 

When user accesses the website I see a GET request and HTTP 200 OK when GET is succesfully answered. 

What I understood when reseaching the topic is that if ``ngrep`` would have worked on the public IP I was given it would show the HTML file in plaintext because we don't have TLS enabled. Unfortinately I was not able to actually demo this.

|Parameter|Meaning|
|---|---|
|`sudo`|needs root privileges to capture network packets|
|`-d eth0`|listen on network interface `eth0`|
|`-W byline`|print each packet line by line for readability|
|`""`|no content filter — capture everything|
|`host 65.52.68.180`|only capture traffic to/from this IP|
|`and port 80`|only capture HTTP traffic|

![[img4/module4_img13.png]]
![[img4/module4_img17.png]]
![[img4/module4_img18.png]]
![[img4/module4_img19.png]]

Later on in the "ping" section I figured out that I'm running the ngrep in the wrong maschine. Also ``eth0`` did not work and I had to use ``enp0s3`` in order for this to work. ngrep now cought successful GET request and the HTML of the website.

![[module4_img23 1.png]]
![[module4_img24 1.png]]
###### SSH

The output for `sudo ngrep -d eth0 -W byline "" port 22` is just a random string of characters. 

This tells me that SSH protocol is encrypted. From some reseach the switch from the old ``telnet`` to `SSH` was because telnet sent everything in plain text.

![[img4/module4_img20.png]]

###### ping

I believe I'm running to the same issue on this one. Pinging ``8.8.8.8`` seems to be ok with 0% packet loss. However ngrep did not capture any ICMP traffic even with `-d any`.

![[img4/module4_img21.png]]![[img4/module4_img22.png]]

At this point I was thinking how could our Azure server even see pings going to Googles servers? I now realised that I understood the task a bit wrong and with slight tinkering got the results that were expected. (running ngrep on my local VM and using ``enp0s3`` instead of ``eth0``).

![[img4/module4_img25.png]]

Ping packets going back and forth between my local VM `10.0.2.15` and Google's DNS `8.8.8.8`. Each request gets a reply. The content is just meaningless filler data, ICMP doesn't carry any real application data.

ICMP is a protocol used to test network connectivity. It doesn't transfer files or messages, it just sends echo request and reply packets to check if a host is reachable. The `ping` tool uses ICMP and calculates the round trip time based on when the reply arrives.

###### tcpdump

![[img4/module4_img26.png]]

**tcpdump output:** Clean and readable, shows timestamp, source, destination, and packet type clearly:

**ngrep output:** Shows the same packets but also tries to display the raw packet content, resulting in the dots and gibberish alongside the IP addresses.

**Key differences:**

|              | ngrep                                 | tcpdump                  |
| ------------ | ------------------------------------- | ------------------------ |
| Output       | Raw packet content visible            | Clean, structured output |
| Readability  | Harder to read                        | Easier to read           |
| Best for     | Inspecting packet content (like HTTP) | Quick traffic monitoring |
| ICMP display | Gibberish payload                     | Clear request/reply info |

![[img4/module4_img26.png]]