# maTLS Tamarin Implementations

This repository contains various Tamarin implementations of the maTLS extension to the TLS protocol. This includes a formalisation of the security goals.

The maTLS protocol was accepted as part of a submission to NDSS'19 as a joint work by Lee et al. Tamarin is an automated symbolic verification tool for analysis of security protocols.

## Implementation Discussion

The maTLS protocol is somewhat complex from a modelling perspective, as it is at its core a multi-party protocol, where the number of participants can vary. This is expounded by issues related to agents forwarding modified payloads, which can lead to deducibility confusions for the Tamarin prover (also referred to as partial decompositions). The structure of the TLS protocol also does us no favours, as the handshake generally consists of a series of messages sent in plaintext, followed by a collection of hashed and encrypted transcripts.

As such, our analysis has consisted of a series of ever-improving approximations of the "true" protocol suite, with the intent of verifying that our desired goals continue to hold as our level of precision increases. Note that abstraction from the protocol is indeed an intrinsic part of symbolic analysis - cryptographic primitives are replaced by black box functions, certificate infrastructures with global public knowledge and so on.

This means that while our implementation cannot be a full guarantee of security - especially as implementations in the wild will never fully match the idealised specification - it has served as a valuable source for identifying possible attack vectors such that countermeasures can be developed.

We are making continuing progress on improving the accuracy of this implementation, which will be merged into the main branch when fully prepared.

## Security Goals

The core goals of maTLS differs somewhat from the traditional authentication protocols of old. In particular, an assumption of the middlebox-enabled TLS family of protocols is that each of the intermediate agents has access to the message payload. This means that security claims like secrecy are secondary.

Instead, the goals of the protocol are related to **integrity** and **accountability** of the message payload. Informally:

*Handshake Phase*
- A client should be confident that they are indeed interacting with their intended server
- Each middlebox should establish (unique) accountability keys to be used for tracking modifications
- The client is aware of the security parameters used in each TLS segment (e.g. ciphersuite used), allowing them to make an informed judgement on the security of the session
- Each segment in the full TLS session must use different encryption keys

*Record Phase*
- The client should be confident that the response they receive did indeed originate from the server (even if it was later modified en-route)
- The client should be able to tell whether a middlebox made (or didn't make) a modification to the message en-route
- The middleboxes involved must act precisely in the order that was established during the handshake subprotocol (e.g. it must not be possible to "skip" over a middlebox)

## Contact

This repository is currently maintained by Zach Smith. You can contact me at zach dot smith at uni dot lu.

For more information about the maTLS protocol, please refer to our [website](
https://middlebox-aware-tls.github.io "Middlebox Aware TLS").