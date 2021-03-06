# Table of Contents

- [Table of Contents](#table-of-contents)
- [Routing Engine](#routing-engine)
  - [Ingress and Egress.](#ingress-and-egress)
  - [Error Codes](#error-codes)

# Routing Engine
When a call first lands on the system it hits the routing engine. The routing engine follows the process:

## Ingress and Egress.

Ingress means inbound and egress means outbound. If this is describing a switch (for IP auth), the direction is relative to the switch
that you are currently describing. E.g if you add a customer's switch that will be sending traffic to terminate with a carrier, the customer's switch would be considered Egress as it is sending calls out. If it has a DID pointing to it, it would be Ingress as traffic would be flowing into it.

If a call bound for termination comes into the routing engine, after it passes authorisation it will go through ingress routing. This decides the profile of the call and where it is passed to next. There is no Egress routing section per se. The egress routing is built into the customer's rate card as this contains 1 or more carriers and, optionally, a routing strategy (default LCR).


## Error Codes
If your SIP Trace shows that an INVITE packet was received by the switch but not sent out to any providers, the fail will be in the ingress routing.



| SIP Code | SIP Reason                             | Details                                                                                                |
|:--------:|----------------------------------------|--------------------------------------------------------------------------------------------------------|
|    403   | IP Not Authorised                      | The IP Address does not match any account in the system.                                               |
|    500   | Unidentified Internal Switch           | This is an internal error, you should never see this. If you do please contact us.                     |
|    500   | Server not accepting calls (Paused)    | Your account with ConnexCS has been disabled or your server has been disabled.                         |
|    503   | Unknown User                           | Username & Passwords do not match to any known user account.                                           |
|    503   | Unable to perform LRN                  | You have selected LRN dipping for this route, however it is likely that you don't have credit with us. |
|    503   | LCR Unavailable                        | The system is unable to perform a LCR lookup.                                                          |
|    503   | Blocked by Dialplan                    | The prefix/number is not matched by the allowed dial plan.                                             |
|    503   | No Routes Available (Pre)              | There is no rate card rule to allow the call to progress.                                              |
|    503   | No Routes Available (U)                | No routes are available either due to: Lock, Profit Assurance, Routing Strategy or ScriptForge.        |
|    503   | No Routes Available (Lock)             | Locking your Ingress routing has left no routing options.                                              |
|    503   | No Routes Available (Profit Assurance) | Profit Assurance has left no routing options.                                                          |
|    503   | Dropping Call (Strategy)               | Strategic Routing has dropped the call.                                                                |
|    503   | Internal Strategic Routing Error       | There is an error with the config of Strategic Routing.                                                |
|    580   | No Route Available                     | The number dialled does not match any ingress routing profile.                                         |
|    580   | Switch IP Variable Not Provided        | This is an internal error, you should never see this. If you do please contact us.                     |
|    580   | To (oU) User Missing                   | This is an internal error, you should never see this. If you do please contact us.                     |
|    580   | To (fU) User Missing                   | This is an internal error, you should never see this. If you do please contact us.                     |

**Note:** When making changes, although we try to synchronise all endpoints as fast as possible, as this is a distributed system it can take up to 60 seconds for any changes to take affect.
