# Vulnerability Managment using VMware, Kali Purple, Greenbone, and Metasploitable

## Overview

Vulnerability management using VMware, Kali Purple, Greenbone, and Metasploitable involves creating a virtualized network environment with VMware to simulate real-world conditions, utilizing Kali Purple for defensive security operations and real-time threat detection, employing Greenbone for comprehensive vulnerability scanning and risk assessment, and leveraging Metasploit for penetration testing and validating identified vulnerabilities. This integrated approach provides a robust framework for identifying, analyzing, and mitigating security risks in a controlled and effective manner.ce.

## Objectives

Use Kali Purple and GVM to scan and analyze Metaploitable to validate its vulnerabilities.

## Technologies Used:

- VMware: Provides the virtualization platform for creating and managing the virtual machines.

- Kali Purple: Used for defensive security operations, including threat detection and incident response.

- Greenbone: Utilized for vulnerability scanning and risk assessment.

- Metasploitable: Provides an intentionally vulnerable virtual machine employed for penetration testing and validating vulnerabilities.

## Installation of VMware Workstation 17 Player

Download the executable file at the VMware website. Once downloaded, double-click the .exe file to run the installer. I mainly used default settings, with 60GB capacity for HD space for Windows OS.
        
## Create the first Virtual Environment instance in VMware for Metasploitable

- Launch VMware Workstation 17 Player  and select "Open a Virtual Machine".

- Find the Metasploitable exe. file and run it as a Virtual Machine.

- After it boots up, the main login is "msfadmin" and the password is also "msfadmin".

- Leave this running and available on the side as the first virtual instance.


## Create the second Virtual Environment instance for VMware for Kali Purple

- Return to VMware and select "Create a new Virtual Machine".

- Locate the Kali Purple.iso, then follow through the setup wizard to install and configure the virtual environment.

- Log in after the set up wizard is complete.


## Confirming IP addresses and using Nmap

- Once the virual machine as Kali Purple has launched, open up a terminal and type "ifconfig".

- Do the same on Metasploitable.

- The ifconfig information should display an IP address for both virtual environments to confirm that they are in the same network, as shown here.

![IMG](https://raw.githubusercontent.com/StevenNguyenCyber/Images/main/stevencyberip.PNG?token=GHSAT0AAAAAACTJL6TNT4XYMNC7NON7PPS6ZTOOHGQ)
![IMG](https://raw.githubusercontent.com/StevenNguyenCyber/Images/main/meta%20ip.PNG?token=GHSAT0AAAAAACTJL6TMZGBFPW6JX3Z7ZCIUZTOOIDQ)

- Finally, type 'nmap' then the 'IP address' on Kali Purple Terminal. It will show a report after it scans the network for its hosts and services.

- Here it is possible to learn about active devices in the netowrk, their IP addresses, open ports, running services and potential vulnerabilities. In this case, the network is our virtual instance created for Metasploitable.

![IMG](https://github.com/StevenNguyenCyber/Images/blob/main/2.PNG)

## Updating Kali Purple

- On the terminal, run a the following to refresh the package index to ensure that the latest information and software updates are availabe.

        sudo apt update -y
  
- Run this command to install the latest versions of the packages currently installed in your system. Proceed through the prompts.
  
        sudo apt upgrade -y

## Greenbone Installation Set up

- Enter this command in the Kali Purple Terminal to prepare the installation.

       sudo apt install gvm -y
  
- Once complete, enter this command to install the initial set up for GVM.

       sudo gvm-setup

- A admin user password will be created for GVM at the end of installation, save that password.

- Next, enter this command to initialize and configure the GVM environment server after installing. This prepares the server for usage.

        sudo gvm-check-setup
  
## Greenbone Log-in and new user creation

- Open the browser in Kali Purple, and enter this address to access the webpage.

        https://127.0.0.1:9392

- Log in for the first time using default credentials. Enter 'admin' as the username and the password from the setup that was saved during the installation process.

## Checking Feed Status

- Understanding the feed status is important because it provides vulnerability definitions and updates, scan accuracy, security compliance, and system performance.

- On the gray bar, go to Adminstration > Feed Status.
  
- This will show a column list of contents in the network system. The status should all be current but some maybe still in progress so wait until everything is current.

![IMG](https://github.com/StevenNguyenCyber/Images/blob/main/feed%20status.PNG)


## Targeting the Metasploitable and intiating the scan.

- On Greenbone, go to Configuration > Targets.

- Create a new target, give it a name then on 'Host' next 'Manual', enter the Metaploitable's IP address.

![IMG](https://github.com/StevenNguyenCyber/Images/blob/main/targeting%20meta.PNG)

- Once completed, the scan cannot be executed until the Feed Status is completely current for the content items.

- Go to Scans > Tasks > Select New Tasks. Name your task, then on 'Scan Targets' drop down to the target that was recently created and Save.

![IMG](https://github.com/StevenNguyenCyber/Images/blob/main/new%20task.PNG)

- At the bottom should have the target, then start the scan. 

![IMG](https://github.com/StevenNguyenCyber/Images/blob/main/requested%20task%20to%20scan.PNG)

## Interpreting the Results

- When the scan is completed, select the Report.

- The results shown here are the current vulnerabilities that on Metasploitable, from levels of most to least severe.
  
![IMG](https://github.com/StevenNguyenCyber/Images/blob/main/results.PNG)

- The page lists several vulnerabilities that can be exploited, and each one can be also selected in individually to view a summary as well as on how to patch them.
  
![IMG](https://github.com/StevenNguyenCyber/Images/blob/main/results%20extra%20info.PNG)

## Final Thoughts and Conclusions


### Achieved Objectives

In this comprehensive guide, we successfully utilized VMware, Kali Purple, Greenbone, and Metasploitable to create a virtualized environment for vulnerability management. The primary objectives were to set up and configure the virtual machines, scan and analyze vulnerabilities using Greenbone, and interpret the results to understand the security posture of Metasploitable. Each step was meticulously executed, ensuring a robust and controlled approach to identifying and mitigating security risks.

### Learning Outcomes

- Virtualization Proficiency: Gained hands-on experience with VMware Workstation for creating and managing virtual environments.

- Defensive Security Operations: Learned to utilize Kali Purple for real-time threat detection and incident response.

- Vulnerability Scanning: Mastered the installation and configuration of Greenbone for comprehensive vulnerability scanning.

- Penetration Testing: Validated vulnerabilities using Metasploitable and understood the importance of testing in a controlled environment.

- Result Interpretation: Developed skills in interpreting vulnerability scan results and identifying remediation steps.


### Real-World Applicability

The skills and knowledge acquired through this guide are directly applicable to real-world scenarios in cybersecurity. Virtual environments provide a safe space to test and refine security measures without risking production systems. The ability to detect and respond to threats in real-time, conduct thorough vulnerability assessments, and perform penetration testing are critical components of an effective cybersecurity strategy. By simulating these processes, we enhance our preparedness for actual security incidents and improve our overall security posture.







  
