
**Overview:** An email notification pops up. It's from your theater group. Someone decided to throw a party. The invitation looks awesome, but there is something suspicious about this document. Maybe you should take a look before you rent your banana costume.

**Difficulty:** Easy

A document file with macro (.docm) was provided in this challenge.

![image](https://user-images.githubusercontent.com/117036153/198899270-7896e15d-09ad-4afd-bb85-26400a024c9c.png)

To analyze and deobfuscate the VBA macro on the malicious file, let us use the tool ViperMonkey ([GitHub - decalage2/ViperMonkey: A VBA parser and emulation engine to analyze malicious macros.](https://github.com/decalage2/ViperMonkey)) and wait for it to finish:

![image](https://user-images.githubusercontent.com/117036153/198899276-77e8be0b-684e-4bde-8b4c-6f9cb6929191.png)

ViperMonkey easily does the job for you in analyzing VBA Macros. 

Once done, check the result and notice the base64 string value on the Intermediate IOCs:

![image](https://user-images.githubusercontent.com/117036153/198899287-0bdf9268-323c-4ef4-89ed-89345e63506a.png)

Copy the base64 string and decode it using CyberChef to obtain the flag (Remove period (.) between characters).

![image](https://user-images.githubusercontent.com/117036153/198899296-f9a13a36-3b52-4e31-8cc3-2253a444bd3d.png)

**Flag:** HTB{5up3r_345y_m4cr05}
