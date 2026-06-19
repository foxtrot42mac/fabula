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

## Reference

- Engine: https://foxtrot42.org/f42quest  
- Contact: hello@foxtrot42.org
