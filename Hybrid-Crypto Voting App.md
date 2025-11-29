# Hybrid-Crypto Voting App (Java)

A secure local **GUI voting system** that demonstrates a **hybrid cryptographic scheme** using:

- **AES-256-GCM** to encrypt each vote (fast, authenticated encryption)
- **RSA-2048-OAEP** to encrypt (“wrap”) the random AES keys (secure key distribution)
- **Salted SHA-256** password hashing for voter/admin authentication

The app lets users log in, cast votes through a Swing GUI, store those votes in an **encrypted ledger**, and then lets an admin securely decrypt and tally the results.

---

## Features

- **Local GUI voting app** (Java Swing)
- **Hybrid encryption**
  - AES-256-GCM for vote confidentiality + integrity (ciphertext + tag)
  - RSA-2048-OAEP for wrapping the AES session key per ballot
- **Authentication**
  - Voter/Admin login with salted SHA-256 password hashes (no plaintext passwords)
  - `hasVoted` flag to prevent multiple votes from the same ID
- **Encrypted vote ledger**
  - `ballots.csv` stores only IV, tag, ciphertext, and RSA-wrapped AES key
  - Admin uses RSA private key to decrypt and tally
- **Admin tools**
  - Decrypt & tally all ballots
  - Add new voters via GUI
- **Configurable candidates**
  - `candidates.json` defines the list of options (e.g., names, proposals)


