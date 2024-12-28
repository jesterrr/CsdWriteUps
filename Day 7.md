# CTF

## Description

Santa needs your help to investigate the **Downdate** vulnerability! He has a few urgent questions about it that he needs answered ASAP. Can you provide the answers to help keep the North Pole safe from these threats?

1. What is the surname of the researcher who discovered the Downdate vulnerability.
2. Which technology company is responsible for addressing the Downdate vulnerability?
3. What is the CVE identifier associated with the Windows Downdate vulnerability?
4. Which registry key is manipulated to disable Virtualization-Based Security (VBS) in the Windows Downdate exploit?
5. What is the name of the XML file that the Windows Downdate exploit manipulates to control the update process?
6. Which Windows kernel module is downgraded in the Windows Downdate proof-of-concept to exploit the "ItsNotASecurityBoundary" vulnerability?

Answer the questions with a single word @ `nc ctf.csd.lol 5000`

# Solution

This can easily be solved by simple google searches:

```
Answer Question 1: leviev
Correct!
Answer Question 2: microsoft
Correct!
Answer Question 3: cve-2024-21302
Correct!
Answer Question 4: EnableVirtualizationBasedSecurity 
Correct!
Answer Question 5: pending.xml
Correct!
Answer Question 6: ci.dll
Correct!
Here is your flag: csd{d0wNda73_15_W31RD}
```

The answer: `csd{d0wNda73_15_W31RD}`
