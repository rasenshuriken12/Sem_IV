# Open Systems Interconnection (OSI) Model

The OSI model is a 7-layer reference model that standardizes how data is transmitted, received, and interpreted across a network.

Data moves from Layer 7 (Application) down to Layer 1 (Physical) for transmission, then reverses at the destination. 

*Mnemonics:*
**A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing (Top-to-Bottom)
**P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way (Bottom-to-Top) 


The 7 Layers of OSI Model are:

1. Application Layer : (Layer 7)

- This layer serves as a window for the
application services to access the network andfor displaying the received information to theuser.

> It provides network services directly to user applications and acts as the interface between the user software and the network.

- The Application Layer determines how much data is generated, because it decides:
  - Message size
  - Request frequency
  - Response type (text, image, video, JSON, binary)
  - Compression usage

- This directly affects:
  - Bandwidth requirement
  - Network congestion
  - Server load
  - Latency



- Eg. Web Browser (HTTP) , Email (SMTP), etc.



