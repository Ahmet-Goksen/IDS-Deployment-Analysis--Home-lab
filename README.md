# Intrusion Detection System (IDS) Deployment with Snort

## Project Summary

This project demonstrates the deployment and configuration of a basic Intrusion Detection System (IDS) using **Snort** on an **Ubuntu Server** virtual machine. The objective was to monitor live network traffic within a home lab environment and generate alerts for specific activity, specifically ICMP (ping) requests.

The project covers the full setup process, including virtual machine creation, network configuration, Snort installation, rule customization, and alert validation.

---

## Environment

- **Virtualization Platform:** Oracle VirtualBox  
- **Operating System:** Ubuntu Server 24.04.3 LTS  
- **VM Resources:**  
  - 2 CPU cores  
  - 4 GB RAM  
  - 25 GB dynamically allocated disk  
- **Network Mode:** Bridged Adapter with Promiscuous Mode enabled  

---

## Key Implementation Steps

### 1. Virtual Machine Configuration
A new Ubuntu Server VM was created in VirtualBox with appropriate system resources for running Snort.

### 2. Network Bridging & Promiscuous Mode
The VM network adapter was switched from NAT to **Bridged Adapter**, and **Promiscuous Mode** was set to *Allow All*.  
This allowed the VM to capture live traffic from the local network, which is essential for IDS functionality.

### 3. Snort Installation & Configuration
Snort was installed from Ubuntu’s default repositories.  
The `HOME_NET` and `EXTERNAL_NET` variables were configured in `snort.conf` to properly define internal and external traffic boundaries.

Configuration validation was performed using:

```bash
sudo snort -T -c /etc/snort/snort.conf
```

---

### 4. Custom Rule Creation
A local detection rule was added to trigger alerts on ICMP (ping) traffic:

```text
alert icmp any any -> $HOME_NET any (msg:"ALARM: Ping Detected!"; sid:1000001; rev:1;)
```

This rule was added to:

```
/etc/snort/rules/local.rules
```

And confirmed active in `snort.conf`.

---

### 5. Testing & Alert Verification
Snort was executed in console mode:

```bash
sudo snort -A console -q -c /etc/snort/snort.conf -i enp0s3
```

When the Ubuntu VM was pinged from another device on the same network, Snort successfully generated a real-time alert, confirming correct IDS operation.

---

## Outcome

The project successfully demonstrates:

- Deployment of Snort in a virtualized environment  
- Configuration of network monitoring via bridging and promiscuous mode  
- Creation and activation of a custom detection rule  
- Real-time alert generation for monitored traffic  

This setup provides a foundational IDS implementation that can be expanded with advanced rule sets, logging mechanisms, and integration with SIEM platforms for deeper network security analysis.

---

## Future Improvements

- Enable packet logging and alert file output  
- Integrate with a SIEM solution  
- Implement additional custom detection rules  
- Deploy Snort inline as an IPS (Intrusion Prevention System)  
- Add automated alert notifications  
