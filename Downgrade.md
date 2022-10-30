
**Overview:** During recent auditing, we noticed that network authentication is not forced upon remote connections to our Windows 2012 server. That led us to investigate our system for suspicious logins further. Provided the server's event logs, can you find any suspicious successful login?

**Difficulty:** Medium

On this challenge, two resources were provided:

A docker to access the questions and obtain the flag.

![image](https://user-images.githubusercontent.com/117036153/198899076-d390a955-635c-4739-986d-6e7db5ef9989.png)

The log files to identify the suspicious successful login.

![image](https://user-images.githubusercontent.com/117036153/198899080-7923c803-fe89-4216-9e8b-91d17e9937a9.png)

To easily summarize and check the log files, we will be using the tool apt-hunter (https://github.com/ahmedkhlief/APT-Hunter).

![image](https://user-images.githubusercontent.com/117036153/198899103-f2677cc1-9cf2-4330-8454-bd7edc141e1a.png)

When APT-hunter successfully run, three files will be generated:

	Downgrade_Logon_Events.csv  
	Downgrade_Report.xlsx  
	Downgrade_TimeSketch.csv

Focusing on the overview of this challenge, we will only be needing file - Downgrade_Logon_Events.csv 

Using the filter below once we opened the file in libreoffice, we were able to get the suspicious succesful login.

![image](https://user-images.githubusercontent.com/117036153/198899121-e4078365-b732-4279-87ee-8119e51aba6c.png)

Now, all we need to do is to run the docker using netcat and answer the questions correctly to get the flag.

![image](https://user-images.githubusercontent.com/117036153/198899134-ef12e341-c33b-452f-aa72-28623b9ab99c.png)

**Flag:** HTB{4n0th3r_d4y_4n0th3r_d0wngr4d3...}
