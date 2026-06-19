# fabula — Phrase Gate Specification

**Version:** 1.0  
**Status:** Public  
**Engine implementation:** proprietary (foxtrot42)

---

## What is fabula?

fabula is a phrase-based authentication gate. Instead of a password, the user enters
a human-readable phrase. The gate validates the phrase and grants or denies access.

The phrase is:
- Human-readable — can be spoken, written, remembered, or shared as a story
- Deterministic — the same phrase always maps to the same identity hash
- Context-bearing — the phrase proves the user knows something, not just has something

---

## Hello World



The gate receives the phrase, hashes it to a player identity, and transitions to the
correct environment. Wrong phrase → denied scene. Correct phrase → access scene.

---

## Gate protocol

### Entry



The engine:
1. Hashes the phrase to a deterministic player_id
2. Validates against known players (portal API call)
3. On success: transitions to 
4. On failure: transitions to 

### Response



---

## Identity model



The phrase is normalized (stripped, lowercased) before hashing.
Two people with the same phrase get the same identity — phrase is the shared secret.

---

## Gate transitions

| Condition | Destination scene |
|-----------|------------------|
| Valid phrase |  |
| Invalid phrase |  |
| Portal unreachable |  (fail closed) |

---

## Design principles

**Phrases over passwords.** A phrase is memorable, speakable, and shareable by design.
The gate treats it as a story element, not a credential.

**Fail closed.** Any validation error → denied. The gate never guesses.

**No username field.** Identity is derived from the phrase alone.

**Portable.** The gate protocol is independent of transport — HTTP, MCP, CLI.

---


## Identity model — detail

The phrase is normalized (stripped, lowercased) and hashed with SHA-256.
The resulting 256-bit digest serves as the player_id.

Two users presenting the same phrase receive the same identity.
This is by design: the phrase is a shared secret, not a personal token.
If unique per-user identity is required, salt the phrase with a username at enrollment.

## Enrollment

The gate does not manage enrollment directly. A phrase becomes valid by registering
its SHA-256 digest in the portal (the backing player registry).

1. Author/operator calls the portal API: 
2. The portal associates the player_id with a scene graph entry.
3. The gate never sees the phrase — only the hash produced client-side.

## Threat model

Protects against: password database breach (only hashes stored), credential stuffing
(phrase-space is continuous, not a fixed credential set), phishing text fields (none exist).

Does not protect against: observer who knows the phrase and presents it verbatim,
brute-force of short/common phrases (use 4+ uncommon words), server-side portal breach.

## Reference

- Engine: https://foxtrot42.org/f42quest  
- Contact: hello@foxtrot42.org
