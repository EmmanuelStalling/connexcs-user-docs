# Table of Contents

- [Table of Contents](#table-of-contents)
- [Billing Increments](#billing-increments)
  - [Minimum Call Durations/Pulse](#minimum-call-durations-pulse)
  - [Incremental Variable](#incremental-variable)

# Billing Increments

The following document explains the incremental billing protocols used in ConnexCS' default billing setup. In ConnexCS, billing increments (also called 'minimums') are used to establish several calculations:
* When to round up or down on a call duration to attain the even durations needed for billing calculations. Providers may choose to round up calls to a certain amount to keep calculations free of floating numbers that lead to errors, or down to incentivize users with packages that charge less for specific call types.
* Minimum call durations, or the amount of time that will be billed no matter the duration. This is a measure found in most VOIP systems, used to avoid missed charges for calls that take less than systems lowest unit of measurement, and to establish a baseline charge for all calls.
* The pulse, or the rate at which the per-time values are calculated, is used to determine how often a rounding up or down will occur if these measures are activated in the billing profile.

### Minumum Call Durations/Pulse

Within the system, billing increments are expressed as X/X. The first X is the minimum call duration (MCD), typically expressed in seconds.  The second X is the pulse, or the block of time that counts as a single unit in the system in the systems billing cycle. Both can affect per-minute rates.  

**Example:** A system with a billing increment of _30/5_ has a 30-second minimum, and is measured in 5-second increments thereafter. A 28-second call made through this system would be charged half the standard per-minute rate as a base, since the 28 seconds is rounded up to 30 to satisfy the minumum. A 33-second call in that same system would count as 35 seconds if the system is set to round up because of the 5-second pulse (or increments), or 30 seconds if its set to round down.  

### Incremental Variables
In ConnexCS, rounding increments up or down the previous example is a matter of the billing profile's confifuration. In this configuarion, providers can control this operation using system variables:
* **full up** and **full down** round call durations up or down to the nearest whole number, generally the next whole second.
* **half up** and **half down** round call durations up or down to the nearest half-number, e.g. the nearest 0.5 seconds.
* **precision** determines how many decimal points will be used for rounding once the call calculates. Adding more decimal places equals greater precision at the cost of less exact figures and more floating numbers that may require more operation for monetary translation.

