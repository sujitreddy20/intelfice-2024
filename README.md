# DHCP SERVER IN UBUNTU
### OVERVIEW OF THE PROJECT

#### ABOUT THE PROJECT 

Project Summary: Setting Up a DHCP Server on Ubuntu
In this project, you configure and deploy a DHCP server on an Ubuntu system using the ISC DHCP server software. The DHCP server dynamically assigns IP addresses and other network settings (like DNS servers, subnet masks, and gateways) to devices on a local network. The setup includes defining a custom subnet, configuring the DHCP server to listen on specific network interfaces (Wi-Fi or Ethernet), and verifying that IP addresses are properly leased to clients. The project provides a streamlined approach to automating IP address management, simplifying network administration in small to medium-sized networks.

---
#### ABOUT DHCP SERVER

A DHCP server (Dynamic Host Configuration Protocol) automatically assigns IP addresses to devices on a network. This eliminates the need for manual configuration, allowing devices to quickly connect and communicate. It also provides other settings like subnet mask, gateway, and DNS server addresses. The DHCP server ensures each device gets a unique IP, preventing conflicts. Essentially, it's like a network manager that hands out addresses and connection details as needed


___


### **Understanding DHCP and Its Role in Networking**

DHCP (Dynamic Host Configuration Protocol) automates the assignment of IP addresses to devices on a network, ensuring they can communicate without manual configuration. When a device (client) connects to a network, it begins a **four-step process** to obtain an IP address. This process is called **DORA**: 

1. **Discover**: The client broadcasts a "discover" message, asking if any DHCP servers are available.
2. **Offer**: A DHCP server responds with an "offer," proposing an available IP address.
3. **Request**: The client accepts the offer by sending a "request" message, confirming it wants that IP.
4. **Acknowledge**: The DHCP server sends an "acknowledge" message, finalizing the IP assignment.

DHCP also provides additional configuration like subnet masks, DNS servers, and gateways. By automating these tasks, DHCP simplifies network management, especially in environments with many devices, reducing the risk of IP conflicts and manual errors.


___
## Subnetting and Network Configuration:
Explanation of how to define and configure subnets (e.g., 192.168.10.0/24), along with netmask, gateway, and broadcast settings. Include a discussion on IP ranges and how they affect network performance. 
#### Subnetting and Network Configuration**

Subnetting divides a large network into smaller, more manageable segments called **subnets**, each with its own IP address range. For example, in `192.168.10.0/24`, the **/24** (subnet mask) means the first 24 bits are reserved for the network, leaving 8 bits for host addresses. This creates a subnet with **254 usable IP addresses** (192.168.10.1 to 192.168.10.254), where `192.168.10.0` is the network address and `192.168.10.255` is the broadcast address.

The **subnet mask** (`255.255.255.0` for /24) helps devices understand which part of the IP address represents the network and which part identifies the host. The **gateway** (e.g., `192.168.10.1`) directs traffic between the subnet and external networks. Proper subnet configuration helps avoid IP conflicts and improves network performance by minimizing broadcast traffic, while also organizing devices logically, making management easier. The size of the subnet impacts network performance, with larger subnets handling more devices but increasing broadcast traffic.
___
### **Configuring DHCP for Multiple Interfaces**

In a network with both **wired (Ethernet)** and **wireless (Wi-Fi)** connections, a DHCP server can manage IP address allocation separately for each interface, ensuring that devices on both networks receive appropriate IP addresses. Configuring DHCP for multiple interfaces allows for better network segmentation and management, preventing IP conflicts between wired and wireless clients.

#### 1. **Identify Network Interfaces**
First, determine the network interface names for both Ethernet and Wi-Fi. Typically, Ethernet interfaces are labeled with names like `eth0`, and Wi-Fi interfaces may be labeled `wlp3s0`. These names are important because you'll need to assign separate IP address pools to each interface.

#### 2. **Plan Subnets for Each Interface**
To properly segment the network, plan different subnets for the wired and wireless segments. For instance, you might assign the **Ethernet network** the subnet `192.168.10.0/24` and the **Wi-Fi network** the subnet `192.168.20.0/24`. This ensures that devices connected via Ethernet and Wi-Fi will get IP addresses from different pools, preventing IP address conflicts.

#### 3. **Subnet Settings**
For each subnet, define:
- **IP Range**: Decide which range of IP addresses the DHCP server will assign to devices. For example, Ethernet devices might receive addresses from `192.168.10.100` to `192.168.10.200`, while Wi-Fi devices might get `192.168.20.100` to `192.168.20.200`.
- **Gateway**: Set the appropriate default gateway for each subnet. Typically, this is the IP address of the router for that subnet.
- **DNS Servers**: Ensure the devices receive correct DNS server addresses, such as Google’s DNS (8.8.8.8) or your own internal DNS.

#### 4. **Assign Interfaces to the DHCP Server**
Tell the DHCP server which network interfaces (Ethernet and Wi-Fi) it should manage. This ensures the DHCP server listens for connection requests on both the wired and wireless networks and assigns IPs accordingly.

#### 5. **Restart the DHCP Server**
After configuring the subnets and assigning the interfaces, restart the DHCP server to apply the changes. This will ensure the server is ready to handle IP assignments for both network segments.

#### 6. **Monitor IP Assignments**
Once the server is up and running, you can monitor its logs to verify that devices on both the Ethernet and Wi-Fi networks are receiving correct IP addresses. You can also check lease times and see which devices are connected to which subnet, ensuring the setup works as expected.

By handling multiple interfaces in a DHCP configuration, you create a clear separation between wired and wireless networks, which improves network organization and efficiency. This approach is useful in environments where both types of connections are used, such as offices, schools, or large home networks.

### DHCP Lease Management and Monitoring
DHCP Lease Times
Dynamic Host Configuration Protocol (DHCP) lease times define the duration a device can use an IP address assigned by the DHCP server. When a device connects to the network, it requests an IP address from the DHCP server. The server allocates an IP address for a specific period, known as the lease time. After the lease time expires, the device must renew the lease to continue using that IP address.
Lease times can be configured based on network requirements. Short lease times (e.g., 1 hour) are beneficial in environments with many transient devices, while longer lease times (e.g., 24 hours or more) suit stable networks with few changes. Configuration typically occurs in the DHCP server settings, allowing administrators to set global lease times or individual settings for specific address pools.
Monitoring DHCP Leases
Monitoring DHCP leases helps ensure that the IP address allocation is efficient and that devices remain connected. Network administrators can track lease usage and identify potential issues by analyzing DHCP logs. In Linux-based systems, DHCP logs are usually found in /var/log/syslog, which records events related to DHCP lease assignments, renewals, and expirations.
To monitor leases, administrators can use command-line tools or network management software to view the current lease table, which displays the assigned IP addresses, corresponding MAC addresses, and lease expiration times. Troubleshooting DHCP issues may involve analyzing logs for errors, such as failed lease requests or address conflicts, which can indicate connectivity problems or misconfigurations.

### Security Considerations in DHCP
Common DHCP-Related Security Risks
DHCP is susceptible to various security threats, particularly DHCP spoofing, where a rogue server impersonates a legitimate DHCP server, potentially distributing malicious configurations or redirecting traffic. Other risks include DHCP starvation attacks, which can exhaust the available IP address pool, preventing legitimate devices from obtaining an IP address.
Mitigation Strategies
To mitigate DHCP-related risks, organizations can implement DHCP snooping, a security feature on switches that prevents untrusted DHCP servers from sending responses to DHCP clients. This allows only authorized DHCP servers to provide IP addresses.
Firewalls can also be configured to restrict DHCP traffic, allowing only necessary DHCP messages to flow through. Additionally, proper access control lists (ACLs) can ensure that only authorized devices connect to the network, reducing the risk of unauthorized DHCP requests.
Furthermore, network segmentation can limit the impact of attacks, ensuring that only trusted devices can communicate with the DHCP server. Regular audits of network devices and DHCP configurations help maintain security and identify potential vulnerabilities in the network environment.

### Creating a DHCP server 
Creating a DHCP server involves several steps, including installing the DHCP server software, configuring its settings, and ensuring proper network integration. Below is a detailed breakdown of each step, along with corresponding code examples typically found in a Linux environment using `isc-dhcp-server` as the DHCP server software.

#### Step 1: Install DHCP Server Software

**Detailing:**  
To start, you need to install the DHCP server package on your system. In most Linux distributions, this can be done using the package manager.

**Code:**
```bash
# For Debian/Ubuntu-based systems
sudo apt update
sudo apt install isc-dhcp-server

# For Red Hat/CentOS-based systems
sudo yum install dhcp
```

#### Step 2: Configure DHCP Server Settings

**Detailing:**  
The main configuration file for the ISC DHCP server is located at `/etc/dhcp/dhcpd.conf`. This file contains the settings for the DHCP server, including the IP address range, subnet mask, and options for clients.

**Code:**
```bash
# Open the DHCP configuration file
sudo nano /etc/dhcp/dhcpd.conf
```

#### Step 3: Define Subnet and Range

**Detailing:**  
In the configuration file, define the subnet and the range of IP addresses that the DHCP server will allocate to clients.

**Code:**
```plaintext
# Example DHCP configuration

subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.100 192.168.1.200;  # Range of IPs to be assigned
    option routers 192.168.1.1;         # Default gateway
    option domain-name "example.com";   # Domain name
    option domain-name-servers 192.168.1.1;  # DNS server
}
```

#### Step 4: Set DHCP Lease Time

**Detailing:**  
You can specify lease time settings, which determine how long a device can use the assigned IP address.

**Code:**
```plaintext
# Lease time settings
default-lease-time 600;    # 10 minutes
max-lease-time 7200;       # 2 hours
```

#### Step 5: Specify Hardware Type

**Detailing:**  
You may want to specify the hardware type for which the server is providing addresses. This is usually Ethernet.

**Code:**
```plaintext
# Specify hardware type
option hardware-type 1;     # 1 indicates Ethernet
```

#### Step 6: Add Host Entries (Optional)

**Detailing:**  
You can create specific entries for known devices, assigning them fixed IP addresses based on their MAC addresses.

**Code:**
```plaintext
# Example of static IP assignment
host mydevice {
    hardware ethernet 00:11:22:33:44:55;  # Device MAC address
    fixed-address 192.168.1.150;           # Fixed IP address
}
```

#### Step 7: Specify Interfaces

**Detailing:**  
In some systems, you need to specify which network interface the DHCP server should listen to. This can be done in the DHCP server's default configuration file.

**Code:**
```bash
# Specify the interface in /etc/default/isc-dhcp-server
# Edit this file
sudo nano /etc/default/isc-dhcp-server

# Change the INTERFACES line to your network interface
INTERFACES="eth0"  # Replace eth0 with your network interface name
```

#### Step 8: Start and Enable DHCP Server

**Detailing:**  
After configuring the DHCP server, you need to start the service and enable it to run on boot.

**Code:**
```bash
# Start the DHCP service
sudo systemctl start isc-dhcp-server

# Enable the service to start on boot
sudo systemctl enable isc-dhcp-server
```

#### Step 9: Check Status and Logs

**Detailing:**  
To ensure that the DHCP server is running correctly, you can check its status and review logs for any errors or confirmations of DHCP leases.

**Code:**
```bash
# Check the status of the DHCP server
sudo systemctl status isc-dhcp-server

# View DHCP logs (typically found in syslog)
sudo tail -f /var/log/syslog
```

#### Step 10: Test DHCP Server Functionality

**Detailing:**  
After setting up the DHCP server, you can test it by connecting a device to the network and checking if it receives an IP address within the specified range.

**Code:**
- On the client device (Linux):
```bash
# Release any existing IP address
sudo dhclient -r

# Request a new IP address from the DHCP server
sudo dhclient
```

### Conclusion

Following these steps, you will successfully create a DHCP server that allocates IP addresses to clients on your network. Ensure that the configurations are tailored to your specific network requirements for optimal performance and security.

