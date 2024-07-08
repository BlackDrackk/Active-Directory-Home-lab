# Active-Directory home lab 

## Home lab Setup :

First let’s create our first Active Directory Home lab using VMs here I’m going to use the Hypervisor VMware and install 1 Windows Servers VMs (2022 and 2019) and 1 Windows 11 VMs.

### *VM intallation tips :*

- Windows cannot find the Microsoft Liscence : VM settings → Floppy → Unselect “connect at power on” → reboot your VM
- To install VMware tools : VM settings → Select Floppy → Remove

- # Our First domain :

---

It’s time to create our first Domain based on this scheme :

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b88ef871-c6b6-47b5-aace-b73d798e6cb1/d04036b9-a78c-42b8-aaae-a6d455973133/Untitled.png)

- **DC01 configuration :**
    - Change the IP adress/DC name (if you need)
    - Install the **ADDS role**
    - Promote the Windows server to a DC :
        - Create a new forest (new domain named ***domain1.local***)
    - Create a user account
- **PC01 configuration :**
    - Link the client to the DC :
        - Go to “about this PC (advanced)” enter the domain name and choose an account to link with the DC.
        - You can now connect to the PC with the account created in the DC

Now our active directory is setup, its time to populate our AD using a tool named Badblood : 

[GitHub - davidprowe/BadBlood: BadBlood by @davidprowe, Secframe.com, fills a Microsoft Active Directory Domain with a structure and thousands of objects. The output of the tool is a domain similar to a domain in the real world.  After BadBlood is ran on a domain, security analysts and engineers can practice using tools to gain an understanding and prescribe to securing Active Directory. Each time this tool runs, it produces different results.  The domain, users, groups, computers and permissions are different. Every. Single. Time.](https://github.com/davidprowe/BadBlood?tab=readme-ov-file)

BadBlood fills a Microsoft Active Directory Domain with a structure and thousands of objects. The output of the tool is a domain similar to a domain in the real world which is perfect for our lab.

Once the Badblood repository is on your Desktop, we can bypass the execution policy and then run the script :

```powershell
Set-ExecutionPolicy bypass
.\Invoke-BadBlood.ps1
```

It can take some time… :

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b88ef871-c6b6-47b5-aace-b73d798e6cb1/84f3ba2a-6e09-4bbd-9ddb-85159427d130/Untitled.png)

And here is the result, with new OUs , groups and users :

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b88ef871-c6b6-47b5-aace-b73d798e6cb1/dc02e576-be4e-45de-9542-ec38dd7c0aa0/Untitled.png)

Now that our domain is populated, it's time to check the health of our AD using ping castle. Once installed, run the tool and check the results:

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b88ef871-c6b6-47b5-aace-b73d798e6cb1/9c832ae7-4112-4b4e-86ae-a289800bba35/Untitled.png)

The ping castle scan is pretty bad, and the domain's risk level is 100/100, which is not good news. Our aim is to reduce this score by reinforcing the AD.
