# Vulnerability Management Lab Using VMware, Kali Purple, and Metasploit

## Overview

Project Objective

The primary goal of this project is to set up a controlled lab environment to simulate, identify, analyze, and mitigate security vulnerabilities using VMware for virtualization, Kali Purple for security analysis, and Metasploit for exploitation testing. This lab will provide hands-on experience in vulnerability management and enhance understanding of cybersecurity practices.
## Objectives


During this lab, we will:

- Set up Active Directory Domain Services from scratch.
- Create a dedicated Domain Admin account, ensuring security and control.
- Deploy a DHCP server to allocate IP addresses dynamically.
- Show the creation of an Organizational Unit for better user management.
- Delve into the intricacies of Routing and Remote Access to emulate a corporate intranet.
- Configure the Network Interface Card (NIC), ensuring local and internet connectivity.
- Demonstrate mass user management by leveraging PowerShell to batch-add over 1000 users.
- Interact with our client machine, simulating a login process using one of our newly added users.

## Technologies Used:

Components

    VMware:
        Purpose: To create isolated virtual environments for testing.
        Role: Host multiple virtual machines (VMs) running different operating systems and applications to simulate real-world network environments.

    Kali Purple:
        Purpose: To provide a comprehensive security toolset specifically for defensive cybersecurity operations.
        Role: Perform vulnerability assessments, security monitoring, and analysis within the lab environment.

    Metasploit:
        Purpose: To serve as a penetration testing framework.
        Role: Identify and exploit vulnerabilities in the lab's virtual machines to simulate potential attacks and assess the effectiveness of defenses.

Lab Setup

    Environment Preparation:
        Install VMware on a host machine.
        Download and set up the latest versions of Kali Purple and Metasploit.

    Virtual Machines Configuration:
        VM1: Kali Purple:
            Install and configure Kali Purple on the first VM.
            Update all tools and repositories to ensure the latest vulnerabilities can be detected.
        VM2: Target Machine:
            Install an operating system (e.g., Windows, Linux) with known vulnerabilities.
            Configure necessary services and applications that are commonly targeted (e.g., web servers, databases).

    Networking Setup:
        Configure network settings in VMware to allow communication between VMs.
        Ensure the network simulates a real-world environment with internet access where necessary.
        
## Installation

### Create Virtual Machines in VirtualBox

- Open Oracle VirtualBox and click "New."
- Name the first machine as "DC" and select Microsoft Windows as the type and Windows (64-bit) as the version.
- Assign at least 2 GB of RAM and create a new virtual hard disk of at least 50 GB.
- Repeat the process for our Windows 10 machine and name it "CLIENT."

### Installing the Operating Systems and Initial Setup

I initiated the installations by attaching the ISO files to their respective VMs. Windows Server 2019 required additional attention during setup to ensure proper administrative access. Here are the specifics:

- **Install Windows Server 2019:** Follow the on-screen instructions until you reach the account setup stage.
  
- **Create an Admin Account:** As part of the installation, you'll be prompted to create an administrator account. Here, I provided a username and a robust and unique password for enhanced security. This account will have elevated permissions, so it's crucial to protect it adequately.


### Laying Down the Operating Systems and Configuring NICs

I initiated the installation process after attaching the ISO files to their respective VMs. The on-screen instructions are generally straightforward, leading to a successful installation of both operating systems.

In addition, for our Domain Controller, it's imperative to configure dual Network Interface Cards (NICs) for effective network management. Here's how I set them up

- **NIC for Open Internet:** The first NIC is configured to use Network Address Translation (NAT), enabling our Domain Controller to connect to the open Internet.
  
- **NIC for Internal Network:** The second NIC is allocated exclusively to our internal virtual network, ensuring isolated communication within our simulated environment.

This dual NIC setup equips the Domain Controller with the network versatility needed for real-world applications, aligning perfectly with our lab's objectives. 

Doing so has established a secure yet flexible network topology indispensable for our Active Directory Lab.




## Integrating Active Directory Domain Services

- Upon booting up Windows Server, I ventured into the Server Manager, and from there, I meticulously added the "Active Directory Domain Services" role.


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



### Create the new user:


    New-AdUser ...

- The ```New-AdUser``` cmdlet creates a new user in Active Directory. The script specifies various parameters such as the given name, surname, display name, etc., and places the user in the previously created ```_USERS OU```. The ```-PasswordNeverExpires $true``` parameter ensures that the user's password does not expire, which is fine for a lab environment. Still, you generally want to set an expiration in a live production environment.


### Why are we doing this?

The main goal of this script is to demonstrate automation capabilities in Active Directory management, especially when dealing with a large number of users. Rather than manually entering each user, which can be a time-consuming and error-prone process, this script streamlines and automates the task, ensuring consistency and efficiency. It showcases the power of PowerShell scripting in managing Windows environments and offers a practical example of its application in real-world scenarios.

### Client Interaction and User Experience

With our Windows 10 VM ready, I configured it to connect to our server's environment. Subsequently, I logged off and back in, simulating the experience of one of our batch-created users.




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



  
