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
 
  

**Hybrid flow (per vote):**

1. Voter chooses a candidate and clicks **Cast Vote**.
2. App generates a random 256-bit AES key and 96-bit IV (`SecureRandom`).
3. Vote text is encrypted with AES-256-GCM → ciphertext + tag.
4. AES key is wrapped with RSA-2048-OAEP using `authority_pub.pem`.
5. App appends a row to `ballots.csv`:
6. Admin later uses authority_priv.pem to unwrap the AES key, decrypts ciphertext with AES-GCM, and tallies votes.

   ```text
   voterId, iv, tag, ciphertext, wrappedKey
   
<img width="500" height="500" alt="Screenshot 2025-11-28 194117" src="https://github.com/user-attachments/assets/9fcd2717-f07a-41b8-885f-b8662c1e7cc7" />


**Needed to Run App**

Java 21+
Use either JAR or javac -d out src/app/*.java & java -cp out app.Main
Default Accounts 
 - Voter 1: voter1 / voter1pass & Voter 2: voter2 / voter2pass
 - Admin: admin / adminpass
 - You can add more voters from the Admin → Add Voter dialog.


Resetting Data:

- **Soft reset** clear ballots; keep users and keys. Delete rows from ballots.csv, leaving only the header. Set hasVoted to FALSE in voters.csv
- **Medium reset** new RSA keypair + clear ballots. Delete authority_pub.pem and authority_priv.pem. Then perform a soft reset. Next run will generate a fresh RSA keypair
- **Hard reset** factory defaults. Delete voters.csv, ballots.csv, and authority_*.pem. Next run re-creates default users and keys

- **User Login** 
<img width="300" height="300" alt="Screenshot 2025-11-28 194021" src="https://github.com/user-attachments/assets/0aeeb52b-32cf-44f9-868b-245eca987b6a" />

- **Admin Login**
<img width="300" height="300" alt="Screenshot 2025-11-28 194055" src="https://github.com/user-attachments/assets/c636eb7e-f878-4a39-ad6f-1d3dceaa54ed" />

- **Voting Status**
<img width="300" height="300" alt="Screenshot 2025-11-28 194134" src="https://github.com/user-attachments/assets/729362e5-76cc-4cd5-b9ab-2478be02102e" />

- **Files**
<img width="400" height="400" alt="Screenshot 2025-11-28 194209" src="https://github.com/user-attachments/assets/a1e5b06c-5682-4e63-b59a-89db6eab224c" />

- **Data Output**
<img width="400" height="400" alt="Screenshot 2025-11-28 194230" src="https://github.com/user-attachments/assets/f58c87f2-3e5a-4265-91e4-0219876ac469" />


   
