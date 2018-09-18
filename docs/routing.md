# Table of Contents

* [Table of Contents](#table-of-contents)
* [Routing](#routing)

# Routing

Routing in ConnexCS Cloudswitch happens in the following stages:

![alt text][call-flow]

* Incoming call is identified as belonging to a customer, this is checked by source IP or User/Pass Authentication
* Ingress Routing will check for prefixes first, then against available rate cards. There can be multiple ingress routes per customer
* Customer Rate Card is built up from carriers above it, so the call can be presented to any carrier who is the parent of the customer rate card.
* The Carriers Rate Card links into the specific carrier
* The Carrier selected can have multiple IP addresses, the call will be sent randomly, but carrier CPS and Channel limits will be obeyed.

A rate card in **_ConnexCS_** is not an isolated item, it represents a call path. If the rate card is incoming it identifies the carrier which provided that rate card. If the rate card is a customer rate card it will identify which carriers rate cards were used in the construction of this rate card.

[call-flow]: https://raw.githubusercontent.com/digipigeon/connexcs-user-docs/master/img/call-flow.jpg "Call-Flow"