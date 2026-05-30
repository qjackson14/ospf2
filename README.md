<p align="center">




<h1>Video Demonstration</h1>

- ### [YouTube: Day 27 OSPF Pt. 2](https://www.youtube.com/watch?v=ffm-TWGWSDI&t=3s)

<h2>Environments and Technologies Used</h2>

- Jeremy's IT Lab Youtube Channel
- Cisco Packet Tracer
  

  
<h2>Operating Systems Used </h2>

- Cisco IOS


<h2>Step-by-Step</h2>

- 🧩 <b>Step 1 — Configure Hostnames</b>

🔧 Example (on each router)

ENABLE

CONF T

HOSTNAME R1

👉 Do this for all routers:

R1

R2

R3

R4

- 🧩 <b>Step 2 — Configure IP Addresses</b>

This is where you:

Assign IPs to interfaces

Bring interfaces up

🔧 Example (Router Interface)

CONF T

INTERFACE G0/0

IP ADDRESS 10.0.12.1 255.255.255.0

NO SHUTDOWN

🔧 Example (Loopback Interface)

INTERFACE LOOPBACK0

IP ADDRESS 1.1.1.1 255.255.255.255

- 🧩 <b>Step 3 — Enable OSPF on Interfaces</b>

✅ Key Idea:

Instead of using network statements, you enable OSPF per interface

🔧 Commands (R1 Example)

ENABLE

CONF T

INTERFACE RANGE G0/0, F1/0, L0

IP OSPF 1 AREA 0

🛑 Passive Interface (Loopback)

ROUTER OSPF 1

PASSIVE-INTERFACE L0

💡 Why passive?

Stops OSPF neighbor formation

Still advertises the network

⚡ Alternative Method (R4 Example)

ROUTER OSPF 1

PASSIVE-INTERFACE DEFAULT

NO PASSIVE-INTERFACE F1/0

NO PASSIVE-INTERFACE F2/0

👉 Meaning:

Everything is passive except specific interfaces

- 🧩 <b>Step 4 — Change OSPF Cost (IMPORTANT)</b>

🧠 Formula:

OSPF cost = Reference Bandwidth ÷ Interface Bandwidth

Cost=

Interface Bandwidth

Reference Bandwidth
	​
🎯 Goal:

Make FastEthernet (100 Mbps) have cost = 100

🔍 Solve:

100 = Reference / 100

Reference = 10,000

🔧 Command:

ROUTER OSPF 1

AUTO-COST REFERENCE-BANDWIDTH 10000

📊 Result:

Interface Type	Cost

FastEthernet (100 Mbps)	100

Gigabit (1 Gbps)	10

10 Gigabit	1

- 🧩 <b>Step 5 — Default Route + Advertisement</b>

1. Configure Default Route (R1)

IP ROUTE 0.0.0.0 0.0.0.0 203.0.113.2

2. Advertise it in OSPF

ROUTER OSPF 1

DEFAULT-INFORMATION ORIGINATE

🧠 What Happens:

Other routers learn a default route via OSPF

Appears as:

O*E2 0.0.0.0/0

- 🧩 <b>Step 6 — Advanced (But Important Insight)</b>

🔍 Why cost shows as 1 (E2 route)

E2 = External Type 2 route

It ignores internal OSPF path cost

👉 So even if paths differ:

Cost still = 1

⚠️ This is more CCNP-level, but good to recognize.

- 🧩 <b>Step 7 — OSPF Hello Packet Basics</b>

📡 Multicast Address:

224.0.0.5

👉 Used for all OSPF routers
