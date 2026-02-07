# InterVlan-routing-multilayer-switch

What is Multilayer Switching?

Multilayer switching is a method of performing inter-VLAN routing directly on a Layer 3 switch using SVIs (Switched Virtual Interfaces) instead of sending traffic to an external router.

Each VLAN gets:

an SVI (virtual interface) on the multilayer switch

an IP address that acts as the default gateway for that VLAN

Routing happens in hardware (ASIC) inside the switch, making it much faster and more scalable than router-based routing.

Why use Multilayer Switching instead of ROAS?

 Problems with ROAS (Router-on-a-Stick)

In ROAS:

All inter-VLAN traffic goes through one physical router link

Router must process every packet

Creates a bottleneck

Not ideal for enterprise or data-center networks

Why Multilayer Switching is Better

With multilayer switching:

Inter-VLAN routing happens inside the switch

No trunk bottleneck to a router

Much lower latency

High throughput

Industry standard in enterprise and data centers

Key real-world rule

ROAS = learning + small networks
Multilayer switch = production + enterprise networks

My diagram reflects this:

SW2 (3650-24PS) is now the Layer 3 device

R1 is used only for WAN/Internet routing

VLAN gateways moved from router → switch

Multilayer Switching Lab – Step-by-Step 

 VLAN & Subnet Information
 
VLAN	Subnet	Last Usable (Gateway)

VLAN10	10.0.0.0/26	10.0.0.62

VLAN20	10.0.0.64/26	10.0.0.126

VLAN30	10.0.0.128/26	10.0.0.190

Mask for all: 255.255.255.192

here i have used all the configuration of thr ROAS labs i have just replaced the SW2 with the layer 3 SW

<img width="1587" height="1130" alt="image" src="https://github.com/user-attachments/assets/08c30795-842a-4773-85c0-884c548c9866" />

if we did  this from the first we would do

first, assigning ip addresses to pc and vlan creation on the each switch

after that creating the trunk port b/w SW1 and SW2 and also b/w SW2 & R1

after that we will remove all the ROAS SUB interface configuration on the R1
<img width="2382" height="1058" alt="image" src="https://github.com/user-attachments/assets/989003a3-bd9e-4055-ae69-161cbcdc80ab" />
<img width="2402" height="562" alt="image" src="https://github.com/user-attachments/assets/cab989af-1327-42c0-9634-5bd96f09237a" />
after that we will configure the g0/0 with the gateway address of 10.0.0.194/30
<img width="2385" height="799" alt="image" src="https://github.com/user-attachments/assets/370d7a3b-cddf-4944-a3f6-16ba89f232fa" />
now on the  multilayer SW2 it has configured trunk port with the R1 we need to remove it
<img width="1943" height="863" alt="image" src="https://github.com/user-attachments/assets/307000d2-147e-4d00-a873-00a8950c1a16" />
<img width="2173" height="448" alt="image" src="https://github.com/user-attachments/assets/eacb56d0-39bb-40c5-a1e8-299908e63330" />
To configure routing on the Layer 3 switch, I first enabled routing globally using the "ip routing" command. This allows the switch to route traffic between VLANs. Then, I converted the interface connected to the router into a routed port using "no switchport", allowing it to have an IP address and function as a Layer 3 point-to-point link.

<img width="2169" height="1062" alt="image" src="https://github.com/user-attachments/assets/b333e1cd-5674-436c-a74e-9f00af8ba86f" />
and i also configure the gateway for the G1/0/2 for the SW2

here before configuring the SVI we will configure the default route on the G1/0/2 on SW2
<img width="2187" height="1143" alt="image" src="https://github.com/user-attachments/assets/572b654a-e6ef-4564-b3c8-52bf6d64ef3d" />

A Switched Virtual Interface (SVI) is a logical Layer 3 interface created on a multilayer switch to represent a specific VLAN.

The SVI provides an IP address that acts as the default gateway for all devices within that VLAN, enabling inter-VLAN routing directly on the switch.

Unlike physical switch interfaces, which are used to connect individual devices and operate at Layer 2, an SVI represents the entire VLAN as a single routed interface. 

Assigning an IP address to a VLAN interface instead of a physical switch port allows the switch to route traffic for all hosts in that VLAN without tying the gateway function to a specific port.

This design is scalable, efficient, and aligns with enterprise network architecture, where VLAN membership can change without affecting routing configuration. 

Using SVIs also allows routing to occur in hardware on the switch, resulting in lower latency and higher throughput compared to router-based solutions. Overall, SVIs provide a clean, flexible, and high-performance method for implementing inter-VLAN routing on multilayer switches.

now we will assign the gateway address to the vlans
<img width="2716" height="1300" alt="image" src="https://github.com/user-attachments/assets/296bb127-4431-49ad-82fa-9db1bd5ffbf0" />

he we can see the ip address is assigned to the vlans
<img width="2625" height="236" alt="image" src="https://github.com/user-attachments/assets/6d3e27ea-f9d8-4244-aa1c-61e17547c9b8" />

Test Same-VLAN Connectivity 

Purpose: Confirms VLANs and access ports are configured correctly.

Test from:

PC1 (VLAN 10) → ping PC2 (VLAN 10)

ping 10.0.0.2
<img width="1663" height="950" alt="image" src="https://github.com/user-attachments/assets/5add403b-f4c0-4088-be41-66293cdc9c62" />

Switch Layer 2 forwarding works

 Test Inter-VLAN Routing (SVI routing check)

Purpose: Confirms multilayer switch is routing between VLANs.

Test from one VLAN to the others:

From PC1 (VLAN 10):

ping 10.0.0.65    # VLAN 20 PC
<img width="1660" height="893" alt="image" src="https://github.com/user-attachments/assets/05bbb1d0-0f2e-4b01-a75d-06addf2a7a75" />

ping 10.0.0.129   # VLAN 30 PC
<img width="1572" height="946" alt="image" src="https://github.com/user-attachments/assets/e77d3f87-4296-4bad-b795-5c188d199bce" />



 Reverse Direction Test (no one-way routing issues)

Purpose: Confirms routing works both directions.

From PC5 (VLAN 20):

ping 10.0.0.1     # VLAN 10 PC
<img width="1941" height="1291" alt="image" src="https://github.com/user-attachments/assets/a64d43bb-5014-4954-bb2b-1f1b95e27678" />

ping 10.0.0.129   # VLAN 30 PC
<img width="1838" height="858" alt="image" src="https://github.com/user-attachments/assets/b0f8db5c-70e1-4186-94ea-ae48b65d36f5" />





 Gateway Reachability Test (SVI validation)

Purpose: Confirms each VLAN can reach its SVI.

From one PC per VLAN:

VLAN10 PC → ping 10.0.0.62
VLAN20 PC → ping 10.0.0.126
<img width="1823" height="1399" alt="image" src="https://github.com/user-attachments/assets/ed18fcec-5b22-4d29-8e98-d8077537af0f" />

VLAN30 PC → ping 10.0.0.190
<img width="1951" height="876" alt="image" src="https://github.com/user-attachments/assets/c23cb0d5-f068-4176-9422-c14bfffb2553" />



 WAN / Internet Reachability 

Purpose: Confirms Layer 3 uplink + default route.

From any PC (example PC1):

ping 1.1.1.1
<img width="1886" height="925" alt="image" src="https://github.com/user-attachments/assets/aa104098-a8e3-4fee-a8cd-51f5f3cb2782" />







