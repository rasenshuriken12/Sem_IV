 # Introduction to Computer Networks

 **Contents**
 - Uses Of Computer Networks
 - Reference Models: OSI, TCP/IP,
 - Protocol Layering
 - Comparison of OSI & TCP/IP,
 - Network Devices
   - Network Hardware
   - Network Software

## Use of Computer Networks
- Communication
  - Email: betn individuals & organizations globally
  - Instant Messaging: allows users to chat and share information quickly
- Resource Sharing
  - File Sharing: across connected devices
  - Printers and Peripherals: can be accessed & used by multiple users on a network
- Internet Access
  - Web Browsing: 
  - Online Services: 
- Remote Access and Control
- Data Storage and Backup


### What is a "Reference Model"?
In networking, a "reference model" describes **what** needs to be done at each stage of communication, but it doesn't prescribe **how** to do it. It serves as a standard blueprint that all networking technologies should follow.

### The Core Philosophy: Peer-to-Peer Communication
The most important concept in the OSI Reference Model is that each layer on one computer communicates logically with the same layer on another computer. They are "peers."

### Detailed Layer Functions (The "Reference" Perspective)
```
email, file transfer -> data syntax, encryption -> who talks when -> retransmission if lost, flow control -> finds the best path -> moves data from one device to the another -> transmits raw bits
```
| Layer | Name | Responsibility (What it does) | Data Unit | Real-World Analogy |
| :--- | :--- | :--- | :--- | :--- |
| **7** | **Application** | **The Interface:** Provides network services to the user's applications (email, file transfer). It is not the app itself, but the network access part of the app. | Data | **The User:** You writing a letter. |
| **6** | **Presentation** | **The Translator:** Handles data syntax, encryption, and compression. It ensures data sent from one system's Application layer can be read by another's (e.g., converting EBCDIC to ASCII). | Data | **The Translator:** Turning your thoughts into words on paper. Encrypting the letter. |
| **5** | **Session** | **The Manager:** Controls the dialogue between devices. It sets up, manages, and tears down connections. It decides who talks when (half-duplex or full-duplex) and inserts checkpoints to resume transfer if interrupted. | Data | **The Coordinator:** Putting the letter in an envelope, addressing it, and deciding to send it via courier. |
| **4** | **Transport** | **The Lifeline:** Ensures reliable delivery. It segments data, provides error checking (retransmission if lost), and flow control (prevents a fast sender from overwhelming a slow receiver). | Segment | **The Dispatcher:** Ensuring the courier guarantees delivery. Splitting a large document into multiple envelopes. |
| **3** | **Network** | **The Post Office:** Handles logical addressing (IP addresses) and routing. It finds the best path through the internetwork to get the data to the destination. | Packet | **The Sorter:** Reading the zip code and determining which truck needs to take the envelope to the correct city. |
| **2** | **Data Link** | **The Direct Delivery:** Handles physical addressing (MAC addresses). It moves data from one device to the *next* device on the same network. It detects (and sometimes corrects) physical layer errors. | Frame | **The Truck Driver:** Delivering the envelope to the specific house on the street, not just the city. |
| **1** | **Physical** | **The Hardware:** Defines the electrical, mechanical, and procedural specifications. It transmits raw bits (0s and 1s) over a physical medium (cable, air). | Bits | **The Road/Truck:** The actual physical movement of the envelope. |

