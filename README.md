# SOC-Automation-Lab
<a href="https://github.com/gojeayush">GOJE AYUSH</a>

# 1. Introduction
**1.1 Overview**


The SOC Automation Project aims to create an automated Security Operations Center (SOC) workflow that streamlines event monitoring, alerting, and incident response. By leveraging powerful open-source tools such as Wazuh, Shuffle, and TheHive, this project enhances the efficiency and effectiveness of SOC operations. The project involves setting up a Windows 10 client with Sysmon for detailed event generation, Wazuh for comprehensive event management and alerting, Shuffle for workflow automation, and TheHive for case management and coordinated response actions.

  <img width="711" height="758" alt="SOC_Automation_Lab drawio" src="https://github.com/user-attachments/assets/f4711fd6-b78c-4e9a-aabb-2409decc7f34" />



**1.2 Purpose and Goals**

* **Automate Event Collection and Analysis:** Ensure security events are collected and analyzed in real-time with minimal manual intervention, enabling proactive threat detection and response.
* **Streamline Alerting Process:** Automate the process of generating and forwarding alerts to relevant systems and personnel, reducing response times and minimizing the risk of overlooking critical incidents.
* **Enhance Incident Response Capabilities:** Automate responsive actions to security incidents, improving reaction time, consistency, and effectiveness in mitigating threats.
* **Improve SOC Efficiency:** Reduce the workload on SOC analysts by automating routine tasks, allowing them to focus on high-priority issues and strategic initiatives.


#  2.Prerequisites

**2.1 Hardware Requirements**
* A host machine capable of running multiple virtual machines simultaneously.
* Sufficient CPU, RAM, and disk space to support the VMs and their expected workloads.

**2.2 Software Requirements**
* **Virtual Box/Fusion:** Industry-standard virtualization platform for creating and managing virtual machines.
* **Windows 11:** The client machine for generating realistic security events and testing the SOC automation workflow.
* **Ubuntu 24.04:** The stable and feature-rich Linux distribution for deploying Wazuh and TheHive.
* **Sysmon:** A powerful Windows system monitoring tool that provides detailed event logging and telemetry.

**2.3 Tools and Platforms**
* **Wazuh:** An open-source, enterprise-grade security monitoring platform that serves as the central point for event collection, analysis, and alerting.
* **Shuffle:** A flexible, open-source security automation platform that handles workflow automation for alert processing and response actions.
* **TheHive:** A scalable, open-source Security Incident Response Platform designed for SOCs to efficiently manage and resolve incidents.
* **VirusTotal:** An online service that analyzes files and URLs to detect various types of malicious content using multiple antivirus engines and scanners.
* **Cloud Services or Additional VMs:** Wazuh and TheHive can be deployed either on cloud infrastructure or additional virtual machines, depending on your resource availability and preferences.

**2.4 prior Knowledge**
* **Basic Understanding of Virtual Machines:** Familiarity with setting up and managing VMs using Virtual Box or similar virtualization platforms.
* **Basic Linux Command Line Skills:** Ability to perform essential tasks in a Linux environment, such as installing software packages and configuring services.
* **Knowledge of Security Operations and Tools:** Foundational understanding of security monitoring, event logging, and incident response concepts and tools.

# 3. Setup
**3.1 Step 1: Install and configure windows 11 with Sysmon**


**3.1.1 Install Windows 11 on Virtual box:**

<img width="820" height="622" alt="Window_11pro" src="https://github.com/user-attachments/assets/6a96722b-e0f8-4914-8e33-82abfecd5ce4" />

**3.1.2 Download Sysmon:**

<img width="820" height="662" alt="image" src="https://github.com/user-attachments/assets/54b3c2c9-bd7c-429f-a6db-0d57553f4139" />

**3.1.3**Download Sysmon configuration files from <a href="https://github.com/olafhartong/sysmon-modular">Sysmon Modular Config</a>:

<img width="822" height="662" alt="image" src="https://github.com/user-attachments/assets/3be1cdb2-2d80-4d52-8c52-e285ed2dfc4a" />
<img width="816" height="603" alt="image" src="https://github.com/user-attachments/assets/6b60ad89-8b0c-43d3-93af-ed97fa7a17da" />

**3.1.4** Extract the Sysmon zip file and open Powershell as an administrator.
Navigate to the Sysmon directory extracted from the zip file:

<img width="823" height="481" alt="image" src="https://github.com/user-attachments/assets/290c0024-c51b-4979-a4da-d4bc12efc2bd" />

**3.1.5 Place the Sysmon configuration file into the Sysmon directory as well.**

**3.1.6 Install system by using this command:**

    .\Sysmon64.exe -i .\sysmonconfig.xml

<img width="821" height="632" alt="image" src="https://github.com/user-attachments/assets/84ba360c-9ef5-4b39-b383-2958bd84e6de" />

**3.1.7 After a short installation, verify that Sysmon is installed on the Windows:**

* Services
* Event Viewer > Applications and Services Logs > Microsoft > Windows
 
<img width="1219" height="914" alt="Services_running" src="https://github.com/user-attachments/assets/5eb55963-8584-4f08-98a8-c9607525d96e" />

With this step, our Windows 11 machine with Sysmon is ready. The next step is setting up Wazuh.

**3.2 Step 2: SetUp Wazuh Server**

**3.2.1 Create a Virtual Machine on Microsoft Azure:** To set up the Wazuh server, we will be using Azure cloud, a popular cloud service provider. However, you can use any other cloud platform or virtual machines as well. We start by creating a new VMs from the overview menu:

**3.2.2 Set Up a Firewall**: Next, we need to set up a firewall to prevent unauthorized access and external scan spams. From the Azure menu, go to Networking > Network settings > Create Firewall:

We modify the inbound rules to allow access only from our own IP address:

After setting up the firewall rules, we apply the firewall to our Azure VM:

<img width="1892" height="908" alt="Screenshot 2025-10-23 154246" src="https://github.com/user-attachments/assets/236db745-84ba-46ad-944d-2526bc60e34d" />

**3.2.3 Connect to the Wazuh Server via SSH:** From the Azure left-side menu, go to VM > Wazuh > Access > Launch VM Console. This allows us to connect to the Wazuh server using SSH:

<img width="812" height="636" alt="image" src="https://github.com/user-attachments/assets/0060f33f-f993-42a1-af2a-cd171f10c879" />

**3.2.4 Update and Upgrade the System:** First, we update and upgrade the system to ensure we have the latest packages and security patches:

      sudo apt-get update && sudo apt-get upgrade

**3.2.5 Install Wazuh:** We start the Wazuh installation using the official Wazuh installer script:

    curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
    
The installation process will begin:

<img width="895" height="473" alt="image" src="https://github.com/user-attachments/assets/316305aa-f865-4b76-844f-8523d7a38808" />

We take note of the generated password for the "admin" user:
    User: admin
    Password: *******************
    
**3.2.6 Access the Wazuh Web Interface**: To log in to the Wazuh web interface, we open a web browser and enter the Wazuh server's public IP address with https:// prefix:

<img width="805" height="716" alt="image" src="https://github.com/user-attachments/assets/c54fce45-9296-46e3-b92f-9647aa504893" />

Click "Proceed" and "Continue" to bypass the self-signed SSL certificate warning:

<img width="745" height="468" alt="image" src="https://github.com/user-attachments/assets/f99bb830-5d16-4558-9d47-62981c6e6455" />

Use the generated password with the username "admin" to log in to the Wazuh web interface:

<img width="1036" height="513" alt="Screenshot 2026-03-06 230715" src="https://github.com/user-attachments/assets/a25419e8-9af4-4e3a-8ffa-37f6d80a9b1e" />

Now we have our client machine and Wazuh server up and running. The next step is to install TheHive.

# 3.3 Step 3: Install TheHive

**3.3.1 Create a New Virtual Machine for TheHive:** We create another Virtual Machine on Azure with Ubuntu 24.04 for hosting TheHive:

<img width="1761" height="447" alt="Screenshot 2026-04-24 025245" src="https://github.com/user-attachments/assets/31c7dfc6-f596-4193-a409-41278afe712d" />

Also, enable the firewall that we set up earlier for the TheHive VM.

**3.3.2 Install Dependencies:** We start by installing the necessary dependencies for TheHive:

    apt install wget gnupg apt-transport-https git ca-certificates ca-certificates-java curl software-properties-common python3-pip lsb-release
    
<img width="626" height="502" alt="image" src="https://github.com/user-attachments/assets/46e6358e-2a0e-4ba2-8e76-ea078cc592c2" />


**3.3.3 Install Java:**

    wget -qO- https://apt.corretto.aws/corretto.key | sudo gpg --dearmor -o   /usr/share/keyrings/corretto.gpg
    echo "deb [signed-by=/usr/share/keyrings/corretto.gpg] https://apt.corretto.aws stable main" | sudo tee -a /etc/apt/sources.list.d/corretto.sources.list
    sudo apt update
    sudo apt install java-common java-11-amazon-corretto-jdk
    echo JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto" | sudo tee -a /etc/environment
    export JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto"  

**3.3.4 Install Cassandra**: Cassandra is the database used by TheHive for storing data.

    wget -qO - https://downloads.apache.org/cassandra/KEYS | sudo gpg --dearmor -o /usr/share/keyrings/cassandra-archive.gpg
    echo "deb [signed-by=/usr/share/keyrings/cassandra-archive.gpg] https://debian.cassandra.apache.org 40x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
    sudo apt update
    sudo apt install cassandra

**3.3.5 Install Elasticsearch:** Elasticsearch is used by TheHive for indexing and searching data.
    
    wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
    sudo apt-get install apt-transport-https
    echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
    sudo apt update
    sudo apt install elasticsearch

**3.3.6 Optional Elasticsearch Configuration:** Create a jvm.options file under /etc/elasticsearch/jvm.options.d and add the following configurations to optimize Elasticsearch performance:
    
    -Dlog4j2.formatMsgNoLookups=true
    -Xms2g
    -Xmx2g

**3.3.7 Install TheHive:**

    wget -O- https://archives.strangebee.com/keys/strangebee.gpg | sudo gpg --dearmor -o /usr/share/keyrings/strangebee-archive-keyring.gpg
    echo 'deb [signed-by=/usr/share/keyrings/strangebee-archive-keyring.gpg] https://deb.strangebee.com thehive-5.2 main' | sudo tee -a /etc/apt/sources.list.d/strangebee.list
    sudo apt-get update
    sudo apt-get install -y thehive

Default credentials for accessing TheHive on port 9000:

Username: admin@thehive.local
Password: secret

<img width="639" height="510" alt="image" src="https://github.com/user-attachments/assets/92638db4-5a95-4f1a-b5e6-7ebf1bc7f66b" />

**3.4 Step 4: Configure TheHive and Wazuh**

**3.4.1 Configure Cassandra:** Cassandra is TheHive's database. We need to configure it by modifying the cassandra.yaml file:

    nano /etc/cassandra/cassandra.yaml

This is where we customize the listen address, ports, and cluster name.

<img width="738" height="571" alt="image" src="https://github.com/user-attachments/assets/0641b501-7d69-43ef-8a8b-fb0ffbf39847" />

Set the listen_address to TheHive's public IP:

<img width="716" height="502" alt="image" src="https://github.com/user-attachments/assets/18bae944-dc73-4d08-abfc-10680df23bb7" />

Next, configure the RPC address by entering TheHive's public IP.

Lastly, change the seed address under the seed_provider section. Enter TheHive's public IP in the seeds field:
<img width="976" height="339" alt="image" src="https://github.com/user-attachments/assets/519cfd74-e7cc-43ae-ab14-178d612c1e3a" />

    Stop the Cassandra service:
    systemctl stop cassandra.service

Remove the old Cassandra data files since we installed TheHive using the package:

    rm -rf /var/lib/cassandra/*
    Start the Cassandra service again:
    systemctl start cassandra.service

Check the Cassandra service status to ensure it's running:

    systemctl status cassandra.service
  
<img width="750" height="357" alt="image" src="https://github.com/user-attachments/assets/c02b5207-b24b-4a80-bd26-7b7ca90433ca" />

**3.4.2 Configure Elasticsearch:** Elasticsearch is used for data indexing in TheHive. We need to configure it by modifying the elasticsearch.yml file:

    nano /etc/elasticsearch/elasticsearch.yml

Optionally, change the cluster name. Uncomment the node.name field. Uncomment the network.host field and set the IP to TheHive's public IP.

<img width="817" height="765" alt="Screenshot 2026-10-29 183738" src="https://github.com/user-attachments/assets/c2434479-c323-41d0-8661-fca9d85445a7" />


Optionally, uncomment the http.port field (default port is 9200). Optionally, uncomment the cluster.initial_master_nodes field, remove node-2 if not applicable.

Start and enable the Elasticsearch service:

    systemctl start elasticsearch
    systemctl enable elasticsearch
    
Check the Elasticsearch service status:

    systemctl status elasticsearch

<img width="731" height="432" alt="image" src="https://github.com/user-attachments/assets/52e33213-dd5c-4331-a538-9e00762d84fa" />

**3.4.3 Configure TheHive:** Before configuring TheHive, ensure the thehive user and group have access to the necessary file paths:

    ls -la /opt/thp


<img width="772" height="196" alt="image" src="https://github.com/user-attachments/assets/8eee2847-e3c9-43bc-ad4f-a0623da1a725" />


If root has access to the thehive directory, change the ownership:

    chown -R thehive:thehive /opt/thp

This command changes the owner to the thehive user and group for the specified directories.

<img width="776" height="181" alt="image" src="https://github.com/user-attachments/assets/d6f496f4-f45f-445d-8950-aa03eb3fc467" />


Now, configure TheHive's configuration file:

    nano /etc/thehive/application.conf
    
Modify the database and index config sections. Change the hostname IP to TheHive's public IP. Set the cluster.name to the same value as the Cassandra cluster name ("Test Cluster" in this example). Change the index.search.hostname to TheHive's public IP. At the bottom, change the application.baseUrl to TheHive's public IP.

By default, TheHive has both Cortex (data enrichment and response) and MISP (threat intelligence platform) enabled.


<img width="716" height="625" alt="image" src="https://github.com/user-attachments/assets/48de599f-98b1-43ae-99dc-3c6e38e6ca96" />

Save the file, start, and enable the TheHive service:

    systemctl start thehive
    systemctl enable thehive


<img width="733" height="399" alt="image" src="https://github.com/user-attachments/assets/20904a60-898f-44f8-a6ab-8fe8e68ddcd6" />

Important note: If you cannot access TheHive, ensure all three services (Cassandra, Elasticsearch, and TheHive) are running. If any of them are not running, TheHive won't start.

If all services are running, access TheHive from a web browser using TheHive's public IP and port 9000:

    http://143.198.56.201:9000/login


<img width="832" height="441" alt="image" src="https://github.com/user-attachments/assets/ce003393-844c-4b0d-8ee4-b704ba6b07e0" />

Log in to TheHive using the default credentials:

Username: admin@thehive.local

Password: secret

<img width="815" height="515" alt="image" src="https://github.com/user-attachments/assets/e192c07c-1ad7-4e12-9f3a-4c21feaddb89" />

# 3.5 Step 5: Configure Wazuh

**3.5.1 Add a Windows Agent in Wazuh:** Log in to the Wazuh web interface. Click on "Add agent" and select "Windows" as the agent's operating system. Set the server address to the Wazuh server's public IP.


<img width="482" height="444" alt="image" src="https://github.com/user-attachments/assets/b006a027-6199-4a04-b0d8-c8125b502120" />

Copy the installation command provided and execute it in PowerShell on the Windows client machine. The Wazuh agent installation will start.

<img width="786" height="358" alt="image" src="https://github.com/user-attachments/assets/931512b8-d98d-40df-8d11-b838e0153337" />

After the installation, start the Wazuh agent service using the net start wazuhsvc command or through Windows Services.

<img width="742" height="250" alt="image" src="https://github.com/user-attachments/assets/86f0c7aa-4cd2-4935-8e75-99b354354802" />
<img width="681" height="416" alt="image" src="https://github.com/user-attachments/assets/3e38f9f9-ddee-460c-b701-b8c339a7f580" />

**3.5.2 Verify the Wazuh Agent:** Check the Wazuh web interface to confirm the Windows agent is successfully connected.

<img width="649" height="311" alt="image" src="https://github.com/user-attachments/assets/cc772ac5-6e0f-4124-98a3-a7d26a1aa3a1" />

The Windows agent should be listed with an "Active" status.

# 4 Generating Telementry and Custom Alerts

**4.1 Configure Sysmon Event Forwarding to Wazuh**

**4.1.1 Modify Wazuh Agent Configuration:** On the Windows client machine, navigate to C:\Program Files (x86)\ossec-agent and open the ossec.conf file with a text editor (e.g., Notepad).

<img width="531" height="373" alt="image" src="https://github.com/user-attachments/assets/c1e99c93-5aa2-4533-8183-d8e106ac054b" />


**4.1.2 Add Sysmon Event Forwarding:** In the ossec.conf file, add a new <localfile> section to configure Sysmon event forwarding to Wazuh. Check the full name of the Sysmon event log in the Windows Event Viewer.

<img width="555" height="363" alt="image" src="https://github.com/user-attachments/assets/0e1ac180-7012-49be-9d13-39e552af2ec5" />

Add the following configuration to the ossec.conf file:

<img width="539" height="213" alt="image" src="https://github.com/user-attachments/assets/80541627-bf4a-41e7-a681-704444664765" />


Optional: You can also configure forwarding for other event logs like PowerShell, Application, Security, and System. In this lab, we will remove the Application, Security, and System sections to focus on Sysmon events.

**4.1.3 Save the Configuration File:** Since modifying the ossec.conf file requires administrator privileges, open a new Notepad instance with administrator rights and save the changes to the file.

**4.1.4 Restart the Wazuh Agent Service:** Restart the Wazuh agent service to apply the configuration changes.

<img width="711" height="411" alt="image" src="https://github.com/user-attachments/assets/ca1fde8a-df46-430d-a522-0b2a9f68c49a" />

Note: Whenever you modify the Wazuh agent configuration, you need to restart the service either through PowerShell or Windows Services.

**4.1.5 Verify Sysmon Event Forwarding:** In the Wazuh web interface, go to the "Events" section and search for Sysmon events to confirm they are being received.

<img width="689" height="437" alt="image" src="https://github.com/user-attachments/assets/3b0b8d40-23a3-4e8c-8c68-22ee697af5cf" />

# 4.2 Generate Mimikatz Telemetry

**4.2.1 Download Mimikatz:** On the Windows client machine, download Mimikatz, a tool commonly used by attackers and red teamers to extract credentials from memory. To download Mimikatz, you may need to temporarily disable Windows Defender or exclude the download directory from scanning.

<img width="662" height="382" alt="image" src="https://github.com/user-attachments/assets/05b7abdb-1e46-410e-ac44-f63f3763509d" />

**4.2.2 Execute Mimikatz:** Open PowerShell, navigate to the directory where Mimikatz is downloaded, and execute it.

<img width="961" height="402" alt="image" src="https://github.com/user-attachments/assets/970b28f4-06ef-4c87-a2f6-b0197e6e69e4" />


**4.2.3 Configure Wazuh to Log All Events:** By default, Wazuh only logs events that trigger a rule or alert. To log all events, modify the Wazuh manager's ossec.conf file. Connect to the Wazuh server via SSH and open /var/ossec/etc/ossec.conf. Create a backup of the original configuration file:

    cp /var/ossec/etc/ossec.conf ~/ossec-backup.conf

<img width="807" height="530" alt="image" src="https://github.com/user-attachments/assets/234d47e7-12c7-40e5-a9dd-f18c6bcdee2f" />
Change the <logall> and <logall_json> options under the <ossec_config> section from "no" to "yes". Restart the Wazuh manager service:

    systemctl restart wazuh-manager.service
    
This configuration forces Wazuh to archive all logs in the /var/ossec/logs/archives/ directory.

**4.2.4 Configure Filebeat:** To enable Wazuh to ingest the archived logs, modify the Filebeat configuration:

    nano /etc/filebeat/filebeat.yml

<img width="801" height="656" alt="image" src="https://github.com/user-attachments/assets/6c0b9b29-2927-47df-a78f-25380244fed3" />

Change the enabled: false to true for the "archives" input and restart the Filebeat service.

**4.2.5 Create a New Index in Wazuh:** After updating Filebeat and the Ossec configuration, create a new index in the Wazuh web interface to search the archived logs. From the left-side menu, go to "Stack Management" > "Index Management".

<img width="866" height="373" alt="image" src="https://github.com/user-attachments/assets/b1a605ca-3483-4f02-8581-6b2933d8428a" />

On the next page, select "timestamp" as the time field and create the index.

Create a new index named wazuh-archives-* to cover all archived logs.

<img width="862" height="282" alt="image" src="https://github.com/user-attachments/assets/2767e7e2-6d38-46f5-9d7c-a7d18255160e" />

On the next page, select "timestamp" as the time field and create the index.

<img width="907" height="611" alt="image" src="https://github.com/user-attachments/assets/b3ed4709-28bb-46b0-ab7b-3fee0f29fcf3" />

Go to the "Discover" section from the left-side menu and select the newly created index.

<img width="1172" height="697" alt="image" src="https://github.com/user-attachments/assets/eb0ef185-f348-42dc-a8d4-190c85eeaf7b" />

**4.2.6 Troubleshoot Mimikatz Logs:** To troubleshoot if Mimikatz logs are being archived, use cat and grep on the archive logs in the Wazuh manager CLI:

cat /var/ossec/logs/archives/archives.log | grep -i mimikatz

<img width="812" height="290" alt="image" src="https://github.com/user-attachments/assets/c923e5d1-b9f3-4e79-9b7c-cc21eada6505" />

If no Mimikatz events are found in the archives, it means no Mimikatz event was generated, and you won't see any related events in the Wazuh web interface.

**4.2.7 Relaunch Mimikatz:** Relaunch Mimikatz on the Windows client machine and check the Event Viewer to ensure Sysmon is capturing Mimikatz events.

<img width="815" height="686" alt="image" src="https://github.com/user-attachments/assets/ac77d61c-fcbe-4dfa-85aa-55360b9e3197" />

Check the archive file again for Mimikatz logs to confirm they are being generated.

<img width="810" height="463" alt="image" src="https://github.com/user-attachments/assets/fea4af6f-1d3a-4c0c-ac2b-c28e845f692f" />

<img width="689" height="437" alt="image" src="https://github.com/user-attachments/assets/f7ca9d75-5908-4eb6-8049-7b448b92fa65" />

**4.3 Create a Custom Mimikatz Alert**

**4.3.1 Analyze Mimikatz Logs:** Examine the Mimikatz logs and identify a suitable field for crafting an alert. In this example, we will use the originalfilename field.

<img width="671" height="477" alt="image" src="https://github.com/user-attachments/assets/0e886ac5-f00d-4de5-82e4-9d8f515d825b" />

Using the originalfilename field ensures the alert will trigger even if an attacker changes the Mimikatz executable name.

**4.3.2 Create a Custom Rule:** You can create a custom rule either from the CLI or the Wazuh web interface.

<img width="816" height="318" alt="image" src="https://github.com/user-attachments/assets/9efa7790-786d-469b-909f-a1723ecb618e" />

In the web interface, click on the "Manage rule files" button. Filter the rules by name (e.g., "sysmon") and view the rule details by clicking the eye icon

These are Sysmon-specific rules built into Wazuh for event ID 1. Copy one of these rules as a reference and modify it to create a custom Mimikatz detection rule.

    Example custom rule:
    <rule id="100002" level="15">
      <if_group>sysmon_event1</if_group>
      <field name="win.eventdata.originalFileName" type="pcre2">(?i)\\mimikatz\.exe</field>
      <description>Mimikatz Usage Detected</description>
      <mitre>
        <id>T1003</id>
      </mitre>
    </rule>

<img width="677" height="391" alt="image" src="https://github.com/user-attachments/assets/46332ba5-2ec7-499a-a74a-7c426bf6ca46" />

Save the file and restart the Wazuh manager service.

4.3.3 Test the Custom Rule: To test the custom rule, rename the Mimikatz executable on the Windows client machine to something different.

<img width="528" height="367" alt="image" src="https://github.com/user-attachments/assets/2db817bf-0212-45bf-bb07-8ed6b963c337" />

Execute the renamed Mimikatz

<img width="563" height="388" alt="image" src="https://github.com/user-attachments/assets/31032468-8935-4a60-821b-909b9a23ce6f" />


# 5 Automation with Shuffle and TheHive

**5.1 Set Up Shuffle**

**5.1.1 Create a Shuffle Account:** Go to the Shuffle website (shuffler.io) and create an account.

<img width="827" height="485" alt="image" src="https://github.com/user-attachments/assets/81297f6c-3165-4609-ae44-220c989c3a25" />

**5.1.2 Create a New Workflow:** Click on "New Workflow" and create a workflow. You can select any random use case for demonstration purposes.

<img width="822" height="698" alt="image" src="https://github.com/user-attachments/assets/db5af040-30ab-47b4-8bf6-6294cecfdffb" />

**5.1.3 Add a Webhook Trigger**: On the workflow page, click on "Triggers" at the bottom left. Drag a "Webhook" trigger and connect it to the "Change Me" node. Set a name for the webhook and copy the Webhook URI from the right side. This URI will be added to the Ossec configuration on the Wazuh manager.

<img width="821" height="521" alt="image" src="https://github.com/user-attachments/assets/f9e52178-8fb6-4756-9928-716d0ba53728" />

**5.1.4 Configure the "Change Me" Node:** Click on the "Change Me" node and set it to "Repeat back to me" mode. For call options, select "Execution argument". Save the workflow.

**5.1.5 Configure Wazuh to Connect to Shuffle:** On the Wazuh manager CLI, modify the ossec.conf file to add an integration for Shuffle:

    nano /var/ossec/etc/ossec.conf
    Add the following integration configuration:
    <integration>
      <name>shuffle</name>
      <hook_url>https://shuffler.io/api/v1/hooks/webhook_0af8a049-f2cb-420b-af58-5ebc3c40c7df</hook_url>
      <level>3</level>
      <alert_format>json</alert_format>
    </integration>

Replace the <level> tag with <rule_id>100002</rule_id> to send alerts based on the custom Mimikatz rule ID.

<img width="1037" height="143" alt="Screenshot 2026-03-06 231233" src="https://github.com/user-attachments/assets/c747af9f-274d-4a33-a312-4b4038764493" />

    Restart the Wazuh manager service:
    systemctl restart wazuh-manager.service

**5.1.6 Test the Shuffle Integration:** Regenerate the Mimikatz telemetry on the Windows client machine. 
In Shuffle, click on the webhook trigger ("Wazuh-Alerts") and click "Start".

<img width="497" height="399" alt="image" src="https://github.com/user-attachments/assets/0718eec6-3f78-4d1c-9613-3a10cd221025" />

Verify that the alert is received in Shuffle.

**5.2 Build a Mimikatz Workflow
Workflow Steps:**

Mimikatz alert sent to Shuffle
Shuffle receives Mimikatz alert / extract SHA256 hash from file
Check reputation score with VirusTotal
Send details to TheHive to create an alert
Send an email to the SOC analyst to begin the investigation

**5.2.1 Extract SHA256 Hash:** Observe that the return values for the hashes are appended by their hash type (e.g., sha1=hashvalue). To automate the workflow, parse out the hash value itself. Sending the entire value, including sha1=, to VirusTotal will result in an invalid query.

Click on the "Change Me" node and select "Regex capture group" instead of "Repeat back to me". In the "Input data", select the "hashes" option. In the "Regex" tab, enter the regex pattern to parse the SHA256 hash value: SHA256=([0-9A-Fa-f]{64}). Save the workflow.

<img width="811" height="565" alt="image" src="https://github.com/user-attachments/assets/c5aa5ab9-7ffe-4d38-9ed6-11707e6b0b73" />

Click on the "Show execution" button (running man icon) to verify that the hash value is extracted correctly.

<img width="812" height="507" alt="image" src="https://github.com/user-attachments/assets/60057255-2c91-44dd-ada2-106b7d6fa347" />

**5.2.2 Integrate VirusTotal:** Create a VirusTotal account to access the API.

<img width="825" height="210" alt="image" src="https://github.com/user-attachments/assets/245e7db1-4e75-4a6e-876b-aa3f4239a9a6" />

Copy the API key and return to Shuffle. In Shuffle, click on the "Apps" tab and search for "VirusTotal". Drag the "VirusTotal" app to the workflow, and it will automatically connect.

<img width="830" height="636" alt="image" src="https://github.com/user-attachments/assets/69268ec9-bf53-41c2-ba26-ced645d48f56" />

Enter the API key on the right side or click "Authenticate VirusTotal v3" to authenticate.

<img width="815" height="540" alt="image" src="https://github.com/user-attachments/assets/2c3b305f-bbb6-41f4-843a-97c5500c8d1d" />

Change the "ID" field to the "SHA256Regex" value created earlier.

<img width="812" height="776" alt="image" src="https://github.com/user-attachments/assets/99283101-437f-453f-acde-e8896c119395" />

Save the workflow and rerun it.

<img width="822" height="630" alt="image" src="https://github.com/user-attachments/assets/fb721467-8a76-4003-8932-69d1bc7dc2d0" />

Expand the results to view the VirusTotal scan details, including the number of detections.

<img width="692" height="391" alt="image" src="https://github.com/user-attachments/assets/c7d3a12b-2950-4c02-9b4d-917bdb48443c" />

**5.2.3 Integrate TheHive:** In Shuffle, search for "TheHive" in the "Apps" and drag it into the workflow.

TheHive can be connected using the IP address and port number (9000) of the TheHive Virtual Machine created on Azure.

Log in to TheHive using the default credentials:
Username: admin@thehive.local
Password: secret

**5.2.4 Configure TheHive:** Create a new organization and user for the organization in TheHive.

<img width="569" height="292" alt="image" src="https://github.com/user-attachments/assets/517002fe-d225-410e-958d-d9d9bb43fa01" />

Add new users with different profiles as needed.

<img width="660" height="304" alt="image" src="https://github.com/user-attachments/assets/5c33abee-6602-424a-9c3d-09e59604fc4c" />

Set new passwords for the users. For the SOAR user created for Shuffle integration, generate an API key.


Create an API key and store it securely. This key will be used to authenticate Shuffle. Log out from the admin account and log in with one of the user accounts.

<img width="818" height="307" alt="image" src="https://github.com/user-attachments/assets/fe3ad736-5152-44df-85c6-6592d0cea0c6" />

**5.2.5 Configure Shuffle to Work with TheHive:** In Shuffle, click on the orange "Authenticate TheHive" button and enter the API key created earlier. For the URL, enter the public IP address of TheHive along with the port number.

<img width="706" height="316" alt="image" src="https://github.com/user-attachments/assets/22ea2ded-6bd8-4346-8ca3-6e37c0a95e2a" />

Under "Find actions", click on "TheHive" and select "Create alerts". Set the JSON payload for TheHive to receive the alerts. Here's an example payload for the Mimikatz scenario:

{
  "description": "Mimikatz Detected on host: DESKTOP-HS8N3J7",
  "externallink": "",
  "flag": false,
  "pap": 2,
  "severity": "2",
  "source": "Wazuh",
  "sourceRef": "Rule:100002",
  "status": "New",
  "summary": "Details about the Mimikatz detection",
  "tags": [
    "T1003"
  ],
  "title": "Mimikatz Detection Alert",
  "tlp": 2,
  "type": "Internal"
}

Expand the "Body" section to set the payload.

<img width="813" height="627" alt="image" src="https://github.com/user-attachments/assets/9e4290e1-39e1-4658-b3a6-c096cc6afcfa" />

Set the payload on the left side and test the output on the left side.

<img width="820" height="488" alt="image" src="https://github.com/user-attachments/assets/76023b22-6157-4a92-b790-92df02289b28" />

Save the workflow and rerun it. An alert should appear in the TheHive dashboard.


<img width="964" height="292" alt="image" src="https://github.com/user-attachments/assets/5d151d4c-2a69-47e0-b9d5-43a622592f0e" />

Note: If the alert doesn't appear, ensure that the firewall for TheHive in your cloud provider allows inbound traffic on port 9000 from any source.

Click on the alert to view the details.

<img width="641" height="314" alt="image" src="https://github.com/user-attachments/assets/974b6d04-112d-431d-8a02-0ca7ba39e7a0" />

To include more information in the alert, customize the fields in TheHive's JSON payload. For example, let's create a more detailed summary. In Shuffle, click on the "Show Body" button to view the available JSON payload fields.

In the summary field, you can include additional details like the technique and command line associated with the alert.

<img width="801" height="368" alt="image" src="https://github.com/user-attachments/assets/7fc7dc07-86c9-4199-9842-c58a6b8649ca" />

Refer to the body part of TheHive in Shuffle to determine what to include in these fields.

<img width="703" height="747" alt="image" src="https://github.com/user-attachments/assets/9c7b3f79-d263-47e4-ba8c-d2d7a968bade" />

Save and rerun the workflow to see the updated alert with more information.

<img width="641" height="314" alt="image" src="https://github.com/user-attachments/assets/974b6d04-112d-431d-8a02-0ca7ba39e7a0" />

**5.2.6 Send Email Notification:**

In Shuffle, find "Email" in the "Apps" and connect VirusTotal to the email node.

<img width="812" height="520" alt="image" src="https://github.com/user-attachments/assets/a8b46028-d8d0-435a-bce7-f556cbae3a31" />

Configure the email settings, including the recipient, subject, and body, to send the alert with relevant event information.

<img width="816" height="535" alt="image" src="https://github.com/user-attachments/assets/cbc79a39-445c-44dc-8faa-7c1c822665bb" />

Save the workflow and rerun it.

<img width="554" height="491" alt="image" src="https://github.com/user-attachments/assets/8ae6ef0e-95ee-40d8-a460-684d3757d24e" />

verify that the email is recieved with the expected alet details.

<img width="963" height="285" alt="image" src="https://github.com/user-attachments/assets/986dc50d-a695-4a2a-bf94-ec8f74599483" />
