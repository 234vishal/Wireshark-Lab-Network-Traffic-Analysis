# Wireshark-Lab-Network-Traffic-Analysis
This lab will help you practice packet capturing and protocol analysis using Wireshark.
## 1. Install Wireshark:
### For Linux:
Open the terminal and type the below command.

```
sudo apt update
sudo apt install wireshark -y
```
### For Windows / macOS:
Download the installer from: `https://www.wireshark.org/download.html`

## 2. Start Capturing on Your Active Network Interface

* Open Wireshark.
* Select your active network interface `(usually Ethernet or Wi-Fi)`.
* Click the blue shark fin icon **🦈** to begin capturing.
* Open a browser and open and website.
* In Wireshark, click the red square button to stop capturing after few minuts.
* Use Wiresharks display filters to filter the packets eg `Http`, `DNS`, `Tcp`, `ICMP`.

## Filter Captured Packets by Protocol :

### HTTP Packet Analysis:
 * **Screenshot:**
  <img width="1920" height="887" alt="http" src="https://github.com/user-attachments/assets/038360ff-c10d-487e-b49d-029c8c94b57b" />

* **Packet Details:**

  * **Source IP:** 192.168.248.128 `(local machine)`.

  * **Destination IP:** 172.217.24.67 `(Google server)`.

  * **Protocol:** HTTP `(with OCSP response over HTTP)`.

  * **Length:** 1156 bytes (example packet).

* **Packet Report:**
This HTTP packet shows a 200 OK response from a Google server delivering an `OCSP certificate validation response`. It demonstrates how multiple layers (Ethernet → IP → TCP → HTTP) encapsulate data.


### TCP Packet Analysis:

* **Screenshot:**
   <img width="1884" height="857" alt="tcp" src="https://github.com/user-attachments/assets/59177144-2817-44e1-89c1-0b16e08aa437" />
* **Packet Details**
  Based on a careful analysis of the packet capture screenshot, the following points explain the TCP Three-Way Handshake establishing a new secure connection (observed in packets 13, 14, and 15):

  * **Client Initiates Connection (SYN):** The handshake begins with the client, identified by the private IP address 192.168.1.248, sending a SYN (Synchronize) flag packet to the destination server, `34.128.158.37` (Packet 13).

    * **Source Port:** 54245 .

    * **Destination Port:** 443 .
    * This packet synchronizes the client's initial sequence number, proposing a new connection.

  * **Server Acknowledges and Synchronizes (SYN-ACK):** The server, `34.128.158.37`, responds immediately with a SYN, ACK (Synchronize, Acknowledge) flag packet (Packet 14).

    * **Source Port:** 443.

    * **Destination Port:** 54245.

    * This response acknowledges the client’s request (ACK) and provides the server's own initial sequence number (SYN), which is necessary for ordered data transfer.

  * **Client Finalizes Connection (ACK):** The client, `192.168.1.248`, sends the final ACK (Acknowledge) flag packet back to the server.

    * This final packet confirms the receipt of the server's SYN-ACK, completing the three-way handshake.

* **Successful Connection:** The entire process occurs extremely rapidly (within milliseconds, visible in the time column) and confirms that a connection was successfully established between the two endpoints using TCP port 443. Immediately following the handshake (Packet 16), the client sends the TLSv1.3 Client Hello, demonstrating that the reliable TCP connection has been successfully handed over to the application layer for secure communication.

### DNS Packet Analysis:

* **Screenshot:**
 <img width="1920" height="848" alt="DNS" src="https://github.com/user-attachments/assets/32213430-4815-4cfb-b7ef-e85dee098d35" />


* **Key Points from the DNS Packet Analysis:**
   * **Protocol Exclusivity:** The entire capture window is dedicated to DNS traffic, which operates at the Application Layer (Layer 7) and is primarily responsible for resolving domain names to IP addresses.

   * **Internal Resolution:** All communications are occurring within a local network.

  * **Client Host IP:**` 192.168.248.128` (The machine initiating the queries).

  * **DNS Server IPs:** `192.168.248.1` and `192.168.248.2` (The local DNS resolvers being queried).

  * **DNS Ports:** The selected packet detail (Frame 4279) confirms the use of the standard DNS port 53 as the source port and an ephemeral destination port (57264) on the client machine.

  * **Traffic Direction and Flow:** The majority of the visible packets are categorized as Standard query response, confirming that the local DNS server is actively fulfilling name resolution requests and sending the resulting IP address back to the client.

  * **Diversity of Queried Domains:** The client is accessing a variety of public services, including:

    * Major search and video platforms `(www.google.co.in, www.youtube.com)`.

    * Content and information services `(www.wikipedia.org, upload.wikimedia.org)`.
   
## Conclusion:
This exercise demonstrated how Wireshark can capture live traffic, filter by protocol, and reveal details of communication across layers (Ethernet → IP → TCP → HTTP). The analysis confirmed that different protocols coexist in a session and that HTTP responses may contain important security controls like certificate validation and headers to mitigate attacks.
