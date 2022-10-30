
**Overview**: Our company has been working on a secret project for almost a year. None knows about the subject, although rumor is that it is about an old Halloween legend where an old witch in the woods invented a potion to bring pumpkins to life, but in a more up-to-date approach. Unfortunately, we learned that malicious actors accessed our network in a massive cyber attack. Our security team found that the hack had occurred when a group of children came into the office's security external room for trick or treat. One of the children was found to be a paid actor and managed to insert a USB into one of the security personnel's computers, which allowed the hackers to gain access to the company's systems. We only have a network capture during the time of the incident. Can you find out if they stole the secret project?

**Difficulty:** Easy

A PCAP file is provided for us to analyze.

To analyze the PCAP file, let us use Wireshark !!

![image](https://user-images.githubusercontent.com/117036153/198899694-da6ac2e9-1b30-4747-8090-a7dbc522deab.png)

Upon opening the PCAP file, we can easily observe the multiple DNS packets contaning the request and response from pumpkincop.com

![image](https://user-images.githubusercontent.com/117036153/198899705-5db19696-ff5f-4853-bc25-e23210952324.png)

Using the below filter in wireshark, we can focus on the DNS request sent to pumpkincorp.com:

**frame.len == 126**

![image](https://user-images.githubusercontent.com/117036153/198899717-e1e2cbe3-206f-4e0a-abfb-0b706e539dd5.png)

Adding the DNS name query as a column, notice the substring before the domain name (pumpkincorp.com) :

![image](https://user-images.githubusercontent.com/117036153/198899734-b8dad8e5-322d-4bdd-92f3-47b2a5483696.png)

We can use tshark script below to compile the substrings into a file:

![image](https://user-images.githubusercontent.com/117036153/198899743-8ed5114f-1346-40b4-ad20-94702f6aa8a5.png)

``` tshark
sudo tshark -r capture.pcap | grep .pumpkincorp.com | grep -v response | awk '{print $12}' | cut -d '.' -f1 | tr -d \\n | xxd -r -p > flag
```

Open the file using libreoffice to obtain the flag.

![image](https://user-images.githubusercontent.com/117036153/198899755-7c654584-0c9b-4cfc-abbc-6daf7762734f.png)

![image](https://user-images.githubusercontent.com/117036153/198899764-ed4ee3bf-2161-440e-b919-a442bdbbb7a0.png)

**Flag:** HTB{M4g1c_c4nn0t_pr3v3nt_d4t4_br34ch}
