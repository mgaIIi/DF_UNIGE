# Network Forensic Analysis

## Assignment

```
The Potenzio company requests that you analyze the attached PCAP file and 
create a written report (suggested length: 2 pages) detailing whether any 
malicious activities occurred and providing a timeline of the related events.

Please organize the report using the 5W1H method (Who, What, Where, When,
Why, How) and specify the timestamp and hashes of related artifacts for 
each event.

Note the following hints:
	
	The IP address 203.0.113.2 is used by Potenzio as a
	masquerade address.
    
	The IP address 203.0.113.113 is the provider’s DNS
	address.

Both of the above addresses can be considered trusted.
Please ignore Layer 2 information.
```

## Who?

The given pcap file displayed normal network activity of different actors but among them there is an attacker, **libre0ffice.com**, acting under the IP address **58.16.123.111** and attacking **potenzio.com**.

## What happened?

At **10:27:33** the attacker sent an email to a Potenzio employee disguising himself for a society called **harmonic.com**  with some *"exclusive offers"* just for him as we can read in the message body:

```
Content-Type: text/html; charset=UTF-8
Content-Transfer-Encoding: 7bit

<html>
  <head>
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
  </head>
  <body>
    <p>Dear Claudio,</p>
    <p>Please find attached <b>exclusive offers</b> for you.</p>
    <p>Cheers</p>
    <p>Jan Bauer<br>
    </p>
  </body>
</html>
```

Taking a general look to the pcap file with Network Miner we can see some infos about the message itself

![](./assets/Network_Assignment_NetworkMiner.png)

Taking a closer look it's possible to see that the mail was sent from a pretty suspicious address ( **smtp.harmonic.com (mx.lockermaster.lol \[58.16.123.111]**) and contained an attchment called **offers.odt**.






