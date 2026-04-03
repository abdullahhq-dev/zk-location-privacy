# ZK Location Proof (Noir)

A privacy-preserving system that allows a user to prove they are within a geographic region **without revealing their exact coordinates**, using Zero-Knowledge Proofs (ZKP).

---

## Overview

Current location-based services require users to share precise GPS coordinates, leading to privacy risks such as:

* Continuous tracking
* Location profiling
* Data misuse by third parties

This project demonstrates how Zero-Knowledge Proofs can eliminate unnecessary data sharing while ensuring data authenticity.

---

## Problem Statement

How can a user prove:

> "I am inside a specific region"

without revealing:

* exact latitude
* exact longitude

and ensuring the data used is authentic?

---

## Solution

We construct a Zero-Knowledge circuit (using Noir) that proves:

> There exists (lat, lon) such that:
>
> * (lat, lon) lies within a region
> * The location is authenticated using a signature

The verifier learns **only the truth of the statement**, and nothing else.

---


### Circuit Logic

We enforce:

```
lat_min ≤ lat ≤ lat_max  
lon_min ≤ lon ≤ lon_max
```

Where:

* (lat, lon) → private inputs
* bounds → public inputs

---

### Proof Flow

1. User inputs private location (signed)
2. The circuit verifies the signature before performing region checks
3. Circuit checks region constraints
4. Proof is generated
5. Verifier checks proof

---

## Use Cases

* Location-based access control
* Geo-restricted content
