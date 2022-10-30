
**Overview:** "I told them it was too soon and in the wrong season to deploy such a website, but they assured me that theming it properly would be enough to stop the ghosts from haunting us. I was wrong." Now there is an internal breach in the `Spooky Network` and you need to find out what happened. Analyze the the network traffic and find how the scary ghosts got in and what they did.

**Difficulty:** Easy

A PCAP file was provided to analyze the network traffic on the `Spooky Network`.

![image](https://user-images.githubusercontent.com/117036153/198899853-a797941b-7fe1-4b7b-83b4-702dceec838e.png)

To analyze the PCAP file, let us use Wireshark !! 

If we open the PCAP file (capture.pcap), we can easily observe that there were multiple packets in TCP and HTTP. 

![image](https://user-images.githubusercontent.com/117036153/198899859-cf51205d-4513-4ed3-9803-b8b270165900.png)

The first thing we can do is to analyze the different source and destination port on TCP packets using the below filter and follow their TCP stream:

**tcp.srcport == 1337 and tcp.dstport == 45416**

![image](https://user-images.githubusercontent.com/117036153/198899870-b9f84d69-bd44-41a9-9db7-bafaa0ed6db7.png)

Upon following the TCP stream of source port 1337, we can see that there is a suspicious command containing a reversed base64 script.

![image](https://user-images.githubusercontent.com/117036153/198899883-594661af-dee9-4f11-85e7-762afd17e23e.png)

We can use the python script below to inverse the reversed base64 script.

```python
flag = "==gC9FSI5tGMwA3cfRjd0o2Xz0GNjNjYfR3c1p2Xn5WMyBXNfRjd0o2eCRFS" [::-1]
print(flag)
```

![image](https://user-images.githubusercontent.com/117036153/198899893-feed7ef4-7427-486b-a499-922df406daff.png)

Once we obtain the base64 script, we can use CyberChef to decode the flag.

![image](https://user-images.githubusercontent.com/117036153/198899900-5ccb14da-350a-4e40-aaca-8282ec32f195.png)

**Flag:** HTB{j4v4_5pr1ng_just_b3c4m3_j4v4_sp00ky!!}
