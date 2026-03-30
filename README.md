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
> * (optionally) the location is authenticated using a signature

The verifier learns **only the truth of the statement**, and nothing else.

---

## Key Idea

Instead of sharing location:

* Traditional system:

  ```
  (lat, lon) → server
  ```

* This system:

  ```
  ZK Proof → "Inside region"
  ```

---

## Features

* Private location verification
* Rectangle-based geofencing (MVP)
* Minimal data disclosure
* Noir-based circuit implementation
* Optional signed location support (anti-spoofing)

---

## Tech Stack

* Noir (ZK DSL)
* Nargo (build/prove/verify tool)
* Finite field arithmetic (no floating point)
* Digital signatures (optional, for authenticated inputs)

---

## Project Structure

```
zk-location-proof/
│
├── src/
│   └── main.nr        # ZK circuit
│
├── Prover.toml        # Private inputs
├── Nargo.toml         # Noir config
│
├── docs/
│   ├── design.md
│   ├── threat-model.md
│
└── scripts/
```

---

## How It Works

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

1. User inputs private location
2. Circuit checks region constraints
3. Proof is generated
4. Verifier checks proof

---

### Optional: Authenticated Location

To prevent spoofing, the system can be extended so that:

* A trusted authority signs `(lat, lon, timestamp)`
* The circuit verifies the signature before performing region checks

---

## Running the Project

### 1. Install Noir

Follow: https://noir-lang.org

---

### 2. Build

```
nargo compile
```

---

### 3. Generate Proof

```
nargo prove
```

---

### 4. Verify Proof

```
nargo verify
```

---

## Example Input

```
lat = 129716  
lon = 775946  

lat_min = 120000  
lat_max = 130000  
lon_min = 770000  
lon_max = 780000  
```

*(Fixed-point representation: scaled integers instead of floats)*

---

## Privacy Guarantees

This system ensures:

* Exact location is never revealed
* Only region membership is disclosed
* No unnecessary data leakage

---

## Comparison with Existing Systems

| Method                 | Privacy            |
| ---------------------- | ------------------ |
| Raw GPS sharing        | ❌ None             |
| Approximate location   | Partial            |
| Geofencing APIs        | Trust-based        |
| **ZKP (this project)** | Minimal disclosure |

---

## Threat Model

### Adversary

* Malicious user (fake location)
* Curious verifier

### Mitigations

* ZKP ensures correctness of computation
* (Optional) signed inputs help prevent spoofing

---

## Limitations

* Assumes honest location input (current version)
* Does not prevent OS-level tracking
* Rectangle region only (for now)

---

## Future Work

* Signature verification (anti-spoofing)
* Polygon region support
* Circle-based geofencing
* Integration with mobile devices
* TEE-based location attestation

---

## Use Cases

* Location-based access control
* Geo-restricted content
* Anonymous attendance systems

---

## Key Insight

> "Trust the proof, not the data."
