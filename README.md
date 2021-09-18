# Infinitesimal Labs

Infitesimal Labs is doing NEAR ZERO knowledge proof.

## Goal

We're experimenting to build a zero knowledge proof SDK on NEAR. More accurately, it's a zk-SNARK toolkit for NEAR. And we apply it to solve prove of an HTTPS response is authentic problem. To be more specific, the HTTPS resonse is from Yahoo Finance and the use case is to prove a given stock price is equal to X, without require the verifier to have access of Internet. The prover is able to access Internet, make the HTTPS request and generate the proof. Prover is then submit (price X, proof) to the verifier. The verifier is a contract on chain that do not have access to Internet (so it has no knowledge of how this price is obtained), nor can it accept a lot of data or doing too expensive computation due to block gas limit. But the verifier can do a constant time, fit gas limit verification on the proof to see if it's valid. If it is, the verifier contract is confident that the prover has indeed made the HTTPS response, the response haven't been modified and the price is contained in the response body. This eliminate the need for an oracle and make external source of data (from any HTTPS response), to be accessible on chain in a totally permissionless and decentralized way. 

## Overall Status
The overall work is challenging and we cannot fully finish it in the hackathon, but we have figured out a lot of theoratical challenges and turn this problem into feasible engineering tasks. All the engineering tasks are considered doable without uncertainty, but just additional engineering time.

## Subtasks
 There are three subtasks for this goal:
 - ZCash-style ceremony implemented on chain
 - Verifier NEAR-contract generation, implemented as a new backend for a widely used zk-SNARK toolkit: SnarkJS.
 - Script to do HTTPS request and save required data for proof generation. And a zk-circuit to check if the required data (private witness) and the price info (public input) satisfy the circuit.

## Progress of Subtasks

### ZCash-style ceremony

The purpose is to obtain the secret lambda in a decentralize way. Yifang did some research and found both ZCash's Paper and its reference implementation provided a feasible approach. However it also need further discussion about how to implement it in the best way to fit nearcore.

### Verifier NEAR-contract generation

Nikolay ported SNARK-checking smart contract into Rust. The program needs more work to ensure correctness and performance optimizations to maximize the numbers of pairings that can be checked per smart contract call within Near gas limit. The repo is in: https://github.com/nikurt/factor_rs

### Script and Circuit
Bo Implemented the script to start a HTTP over TLS signature. Obtain all required information to be verified in the Circuit. The program is functional, yet more clean up required to make the script neat and friendly to user. The circuit is described in `note` in repo and also represented by the Python cryptography libs, which are theoratically convertible to a circuit language already, although actually port it will need more effort after hackathon. The repo is in: https://github.com/ailisp/zkphttps
