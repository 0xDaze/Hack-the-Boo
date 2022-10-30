**Overview:** In my company, we are developing a new python game for Halloween. I'm the leader of this project; thus, I want it to be unique. So I researched the most cutting-edge python libraries for game development until I stumbled upon a private game-dev discord server. One member suggested I try a new python library that provides enhanced game development capabilities. I was excited about it until I tried it. Quite simply, all my files are encrypted now. Thankfully I manage to capture the memory and the network traffic of my Linux server during the incident. Can you analyze it and help me recover my files? To get the flag, connect to the docker service and answer the questions.
WARNING! Do not try to run the malware on your host. It may harm your computer!

**Difficulty:** Easy

On this challenge, four resources were provided:

	A docker to access the questions and obtain the flag.

![image](https://user-images.githubusercontent.com/117036153/198899367-aa598780-427c-499b-8da0-3c4f740b3152.png)

	The encrypted file - candy_dungeon.pdf.boo

	The memory dump - mem.dmp

	The PCAP file - poof_capture.pcap

To begin, let us run the docker using netcat to see the questions: 

![image](https://user-images.githubusercontent.com/117036153/198899380-491946b8-d223-45cd-8e07-241c2fa8b5a4.png)

**Question #1: Which is the malicious URL that the ransomware was downloaded from? (for example: http[:]//maliciousdomain/example/file[.]extension)**

Let us inspect the PCAP file (poof_capture.pcap) using Wireshark.

Immediately, we can notice that the user downloaded a zip file - pygaming-dev-13.37.tar.gz

![image](https://user-images.githubusercontent.com/117036153/198899390-63093b62-4eec-4fa3-a2c2-3fb62e82d39b.png)

Now, let us get the full requested URI of the file to answer the 1st question.

![image](https://user-images.githubusercontent.com/117036153/198899396-96b60ca1-b107-443d-b151-66be2566358a.png)

![image](https://user-images.githubusercontent.com/117036153/198899404-85cdb682-48dd-4333-9c62-661900679e3b.png)

**Question #2: What is the name of the malicious process? (for example: malicious)**

Let us leverage the export objects feature of wireshark to analyze the zip file.

![image](https://user-images.githubusercontent.com/117036153/198899420-ff95e8c6-f519-4414-8b47-ab11b51a02d2.png)

![image](https://user-images.githubusercontent.com/117036153/198899431-b1ca30c3-b52e-4c23-8655-1ef8575d36c0.png)

Next, unzip the downloaded file and move to the extracted directory.

![image](https://user-images.githubusercontent.com/117036153/198899439-88b144e1-d739-46ec-a89b-e9e9cdfedc1c.png)

Notice, that an executable file named configure was extracted on the exported object. This answers the 2nd question.

![image](https://user-images.githubusercontent.com/117036153/198899451-f31f5ee0-c936-4bb7-8a1d-61a32bbdeb46.png)

**Question #3: Provide the md5sum of the ransomware file.**

To answer this, let us get the md5sum of executable file - configure.

![image](https://user-images.githubusercontent.com/117036153/198899458-b4066d4a-5e09-4548-ad52-8ec7ecbdea13.png)

![image](https://user-images.githubusercontent.com/117036153/198899463-a0abc935-01af-4dc1-a89a-0936b6b75533.png)

**Question #4: Which programming language was used to develop the ransomware? (for example: nim)**

To identify the programming language used in executable file named configure, let us extract its contents using the tool pyinstxtractor (https://github.com/extremecoders-re/pyinstxtractor) 

![image](https://user-images.githubusercontent.com/117036153/198899481-bc8c5b1b-2152-4f7f-a272-ae74000b3a66.png)

As seen on the screenshot above, the executable file was built using python 3.6. To answer the 4th question, python was used as a programming language to develop the ransomware.

![image](https://user-images.githubusercontent.com/117036153/198899496-c7eaa683-03f1-44ad-bf73-0a7125ed3346.png)

**Question #5: After decompiling the ransomware, what is the name of the function used for encryption? (for example: encryption)

Once running the tool pyinstxtractor, check the pyc file on the extracted directory (configure_extracted). For this challenge, the pyc file is configure.pyc

![image](https://user-images.githubusercontent.com/117036153/198899518-df68418d-27a0-448d-8904-282da323203a.png)

Run the tool uncompyle6 to decompile the pyc file.

![image](https://user-images.githubusercontent.com/117036153/198899528-e045964b-8c87-4aad-bfc8-58d815f53cd4.png)

Analyzing the decompiled file - configure.pyc, we can see that this function is encrypting the data of a file and answers our 5th question.

```python
def mv18jiVh6TJI9lzY(filename: str) -> None:
    data = open(filename, 'rb').read()
    key = 'vN0nb7ZshjAWiCzv'
    iv = b'ffTC776Wt59Qawe1'
    cipher = AES.new(key.encode('utf-8'), AES.MODE_CFB, iv)
    ct = cipher.encrypt(data)
    Pkrr1fe0qmDD9nKx(filename, ct)
```

![image](https://user-images.githubusercontent.com/117036153/198899542-b6109acc-f6c7-4faa-a4b8-2ba7ba08e73c.png)

**Question #6: Decrypt the given file, and provide its md5sum.**

To decrypt the file (candy_dungeon.pdf.boo), let us create a python script referencing the function encrypting the data of a file.

```python
from Crypto.Cipher import AES

data = open('candy_dungeon.pdf.boo', 'rb').read()
key = 'vN0nb7ZshjAWiCzv'
iv = 'ffTC776Wt59Qawe1'
cipher = AES.new(key.encode('utf-8'), AES.MODE_CFB, iv.encode('utf-8'))
ct = cipher.decrypt(data)
f = open('flag', 'wb')
f.write(ct)
```

Run the script and get the md5sum of the file named flag to answer the question.

![image](https://user-images.githubusercontent.com/117036153/198899554-4f78cd2c-0af2-4443-a5a1-7d21fd4b43a2.png)

![image](https://user-images.githubusercontent.com/117036153/198899561-a56a4c87-2610-4d1b-930a-5ab81552f15f.png)

After answering the series of questions correctly, we were able to get the flag.

![image](https://user-images.githubusercontent.com/117036153/198899572-e933b66f-daf1-4481-84bb-afbda6bd24fb.png)

**Flag:** HTB{n3v3r_tru5t_4ny0n3_3sp3c14lly_dur1ng_h4ll0w33n}
