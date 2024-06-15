# Vulnerability Managment using VMware, Kali Purple, Greenbone, and Metasploitable

![10]()


## Overview

Vulnerability management using VMware, Kali Purple, Greenbone, and Metasploitable involves creating a virtualized network environment with VMware to simulate real-world conditions, utilizing Kali Purple for defensive security operations and real-time threat detection, employing Greenbone for comprehensive vulnerability scanning and risk assessment, and leveraging Metasploit for penetration testing and validating identified vulnerabilities. This integrated approach provides a robust framework for identifying, analyzing, and mitigating security risks in a controlled and effective manner.ce.

## Objectives

![11]()

During this lab, we will:

- Set up Active Directory Domain Services from scratch.

## Technologies Used:

- VMware: Provides the virtualization platform for creating and managing the virtual machines.

- Kali Purple: Used for defensive security operations, including threat detection and incident response.

- Greenbone: Utilized for vulnerability scanning and risk assessment.

- Metasploitable: Employed for penetration testing and validating vulnerabilities.

## Installation of VMware Workstation Player 17

Download the executable file at the VMware website. Once downloaded, double-click the .exe file to run the installer. I mainly used default settings, with 60GB capacity for HD space for Windows OS.
        
## Create the first Virtual Environment instance in VMware for Metasploitable
![IMG_1735]()

-Launch VMware Workstation Player 17 and select "Open a Virtual Machine".

-Find the Metasploitable exe. file and run it as a Virtual Machine.

-After it boots up, the main account is "msfadmin" and the password is also "msfadmin".

-Leave this running and available on the side as the first Virtual instance.


## Create the second Virtual Environment instance for Kali Purple

-Return to VMware and select "Create a new Virtual Machine".

-Locate the Kali Purple.iso, then follow through the setup wizard to install and configure the virtual environment.

-Once the virual machine as Kali Purple has launched, open up a terminal and type "ifconfig", do the same on Metasploitable.

-The ifconfig information should display an ip address for both virtual environments, as shown here.

![IMG](https://github.com/StevenNguyenCyber/Images/blob/main/1.PNG)
![IMG](https://github.com/StevenNguyenCyber/Images/blob/main/4.PNG)



## Integrating Active Directory Domain Services

- Upon booting up Windows Server, I ventured into the Server Manager, and from there, I meticulously added the "Active Directory Domain Services" role.

![ACTIVEDIRECTORY LAB2]()


## Crafting a Domain Admin Account

- Open Active Directory Users and Computers.
- Right-click and select New > User.
- Provide details for your new Domain Admin account and set a strong password.

## Set Up DHCP Server

- Back in the Server Manager, I incorporated the "DHCP Server" role, ensuring our client machine would receive an IP address dynamically.

## Structuring the Organizational Unit

- Open Active Directory Users and Computers.
- Right-click on your domain, go to New > Organizational Unit, and create an OU.

## Configure Routing and Remote Access

- Go to Server Manager and add the "Routing" role.
- Configure Routing and Remote Access to set up an internal network.

## Configure NIC for Internet Access

- Open Network and Sharing Center and go to "Change adapter settings."
- Configure the NIC to connect to the outside internet.

## Add Users via PowerShell

In an organizational setup, it's not uncommon to have hundreds, if not thousands, of users. Manually entering each would be an inefficient use of resources. That's why I've incorporated a PowerShell script for batch user creation. Let's dissect the script ```BulkAddUserScript.ps1``` to understand its structure and function:

### Define Variables:

    # ----- Edit these Variables for your own Use Case ----- #
    $PASSWORD_FOR_USERS   = "Password1"
    $USER_FIRST_LAST_LIST = Get-Content .\names.txt
    # ------------------------------------------------------ #

Here, two variables are being defined:

- ```$PASSWORD_FOR_USERS = "Password1":``` This variable sets a default password for all users. This script is set as "Password1." While setting a universal password isn't the best practice for production, it's suitable for this lab.
- ```$USER_FIRST_LAST_LIST = Get-Content .\names.txt:``` This line reads a file ```names.txt``` that contains a list of first and last names. Each name in this file is presumably formatted as "FirstName LastName", one pair per line.

### Convert Plain Text Password:

      $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force

- For security reasons, PowerShell commands related to user management often require passwords to be in the form of a secure string rather than plain text. Here, we're converting our plain text password from the ```$PASSWORD_FOR_USERS``` variable into a secure string and storing it in the ```$password``` variable.

### Create Organizational Unit (OU):

    New-ADOrganizationalUnit -Name _USERS -ProtectedFromAccidentalDeletion $false

- Here, an Organizational Unit (OU) named ```_USERS``` is being created. The ```-ProtectedFromAccidentalDeletion $false``` parameter ensures that the OU is not protected from accidental deletions. This is handy for a lab environment, but in a production scenario, you should consider protecting it.

### Loop Through Names and Create Users:

    foreach ($n in $USER_FIRST_LAST_LIST) {
    ...
    }

- For each name in our list ```($USER_FIRST_LAST_LIST)```, the following actions are performed.

### Extract the first and last names:

    $first = $n.Split(" ")[0].ToLower()
    $last = $n.Split(" ")[1].ToLower()

- The ```.Split(" ")``` method splits the full name into a first and last name based on the space between them. The ```.ToLower()``` method then converts these names to lowercase.

### Construct the username:
    
    $username = "$($first.Substring(0,1))$($last)".ToLower()
- This line constructs a username by taking the first letter of the first name and appending the full last name, all in lowercase. For example, for the name ```"John Doe"```, the username would be ```"jdoe"```.

### Display progress in the console:

    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
- This line displays a message in the console to show which user is being created.

![Creating User]()


### Create the new user:


    New-AdUser ...

- The ```New-AdUser``` cmdlet creates a new user in Active Directory. The script specifies various parameters such as the given name, surname, display name, etc., and places the user in the previously created ```_USERS OU```. The ```-PasswordNeverExpires $true``` parameter ensures that the user's password does not expire, which is fine for a lab environment. Still, you generally want to set an expiration in a live production environment.


### Why are we doing this?

The main goal of this script is to demonstrate automation capabilities in Active Directory management, especially when dealing with a large number of users. Rather than manually entering each user, which can be a time-consuming and error-prone process, this script streamlines and automates the task, ensuring consistency and efficiency. It showcases the power of PowerShell scripting in managing Windows environments and offers a practical example of its application in real-world scenarios.

### Client Interaction and User Experience

With our Windows 10 VM ready, I configured it to connect to our server's environment. Subsequently, I logged off and back in, simulating the experience of one of our batch-created users.

![ACTIVEDIRECTORY LAB 2]()


## Final Thoughts and Conclusions

### Achieved Objectives

Throughout this lab, we traversed the essentials of setting up and configuring a Windows-based Active Directory environment. The walkthrough guided us through everything from initial setup and domain configurations to intricate networking adjustments and automation. The PowerShell script's employment for bulk user creation stands as a testament to how scalable and automatable an Active Directory environment can be.

### Learning Outcomes

The lab offered an invaluable hands-on experience, helping to demystify the often complex domain of Windows networking and Active Directory services. It provided insights into foundational aspects like DHCP setup, user management, Organizational Unit (OU) creation, and networking nuances. Perhaps most notably, it demonstrated how automation could significantly enhance administrative efficiency, a key takeaway for any IT professional.

### Real-World Applicability

The skills and knowledge gained from this lab extend far beyond its experimental confines. These competencies are highly transferable to real-world scenarios, where Active Directory is a cornerstone for organizational IT infrastructures. The automation aspects, particularly, equip you with the capability to manage large-scale environments effectively.

### Future Exploration

Although this lab covers significant ground, it's worth noting that Active Directory and Windows networking encompass much more. Advanced topics like Group Policies, advanced security measures, multi-site configurations, and integration with cloud services are some areas where you can extend your learning journey.

### Closing Remarks

In conclusion, this lab is a stepping stone into Windows-based networking and Active Directory. It equips you with the technical know-how and instills a strategic understanding of these services' roles within larger IT ecosystems.

The complexity and richness of Active Directory can be intimidating, but as this lab has demonstrated, a structured and hands-on approach can go a long way in gaining mastery over it. For those who aim to either enter the IT field or enhance their existing skill set, understanding Active Directory is almost a rite of passage, and this lab seeks to make that transition as seamless as possible.



  
