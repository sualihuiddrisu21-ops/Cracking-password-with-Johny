# Password Auditing & Cryptographic Hash Recovery Lab
**W3-PM-FINAL | CYBERSECURITY | NETWORKWALKS**

---

## Project Metadata

| Field | Details |
| :--- | :--- |
| **Analyst Name** | Iddrisu Sualihu |
| **Program / Batch** | B082 - Networkwalks |
| **Date** | 20 November 2026 |
| **Modules Completed** | Module 1: Offline Password Auditing with John the Ripper (Johnny GUI)<br>Module 2: Hash Extraction & Recovery via Networkwalks Auditing Suite |
| **Scope / Target** | 3 Encrypted CTF Challenge PDFs (`My Locked PDF1.pdf`, `PDF2`, `PDF3`) |
| **Authorization** | Authorized educational lab exercises provided by Networkwalks |

---

## 1. Liability Disclaimer

All password auditing and hash recovery exercises documented in this repository were conducted exclusively on authorized Capture-The-Flag (CTF) challenge files provided within the Networkwalks Cybersecurity internship curriculum. These techniques are documented strictly for educational awareness, defensive security auditing, and understanding cryptographic resilience. Unauthorized access or password recovery against third-party systems or files is illegal and unethical.

---

## 2. Project Overview

This repository documents the Week 3 practical cybersecurity exercises focused on **document encryption assessment and offline password auditing**. The objective was to evaluate password strength across three encrypted PDF challenge files using two complementary workflows:

1. **Module 1 (John the Ripper / Johnny GUI):** Extracting PDF encryption metadata into standard `$pdf$` hash strings and performing dictionary-based password recovery using John the Ripper (`1.9.0-jumbo-1`).
2. **Module 2 (Networkwalks Hash & Auditing Utilities):** Verifying the extracted hashes and recovered credentials using the Networkwalks Hash Calculator and Password Auditing toolset to validate consistency across auditing platforms.

---

## 3. Tools & Utilities Used

| Tool / Utility | Purpose |
| :--- | :--- |
| **Online HashCrack (`pdf2john` converter)** | Extracts the cryptographic metadata and salt parameters from encrypted PDFs into John-compatible hash format (`$pdf$...`) |
| **Johnny (John the Ripper GUI v1.9.0-jumbo-1)** | Graphical frontend for John the Ripper used to load extracted hashes and execute dictionary/wordlist auditing[cite: 22, 23] |
| **Windows Notepad** | Used to store extracted `$pdf$` hash strings as plain-text input files for auditing |
| **Networkwalks Hash Calculator** | Extracts and computes cryptographic hash representations from target PDF files |
| **Networkwalks Password Cracker** | Validates and recovers plaintext passwords from extracted PDF hashes |

---

## 4. Methodology & Execution

### Module 1: Hash Extraction & Auditing with John the Ripper (Johnny)

1. **Cryptographic Hash Extraction:**  
   Each encrypted challenge PDF (`My Locked PDF1.pdf`, `PDF2`, and `PDF3`) was processed through the `pdf2john` hash extraction interface to isolate its encryption dictionary parameters (revision, key length, salt, and validation strings)[cite: 21].
   * *Sample Extracted Hash Format (PDF 1):*
```text
$pdf$4*4*128*-1028*1*16*ca7f72f11459cba469f1005a8765ed51*32*...[cite: 21]
```
2. **Hash File Preparation:**  
   The extracted hash strings were saved cleanly into plain-text `.txt` files using Windows Notepad so they could be parsed by John the Ripper.
3. **Configuration & Dictionary Auditing in Johnny:**  
   * Launched **Johnny** and linked the backend executable (`john.exe` — `1.9.0-jumbo-1 OMP`).
   * Loaded the saved hash text files via **Open password file**[cite: 22, 23].
   * Executed a wordlist-based audit (`Start new attack`), successfully recovering the plaintext passwords (e.g., `good-luck` for PDF 1) and unlocking all three documents to retrieve the embedded CTF flags[cite: 23, 24, 25, 26].

---

### Module 2: Cross-Verification with Networkwalks Auditing Tools

To validate findings across different auditing utilities, the three challenge PDFs were also evaluated using the Networkwalks lab toolset:

1. **Hash Generation:** Uploaded each locked PDF into the **Networkwalks Hash Calculator** to compute and extract the corresponding hash string.
2. **Credential Recovery:** Copied the generated hash into the **Networkwalks Password Cracker** utility, confirming identical password recovery results across all three files.

---

## 5. Summary of Results & Captured Flags

| Challenge File | Encryption Standard | Recovered Password | Captured CTF Flag |
| :--- | :--- | :--- | :--- |
| **PDF 1 (`My Locked PDF1.pdf`)** | PDF 1.7 / 128-bit (`$pdf$4*4*128*`)[cite: 21] | `good-luck` | `nw{cybersecurity_flag_captured_2608}` |
| **PDF 2** | 128-bit PDF Encryption | *(Recovered via JtR & NW Tool)* | `nw{networkwalks_persistence_jtr_270521}` |
| **PDF 3** | 128-bit PDF Encryption | *(Recovered via JtR & NW Tool)* | `nw{networkwalks_flag_260821_1}` |

---

## 6. Security Findings & Defensive Recommendations

### Key Observations
* **Vulnerability to Dictionary Attacks:** Even when files are encrypted with 128-bit algorithms, weak or common dictionary words (`good-luck`) allow offline auditing tools to derive the decryption key rapidly[cite: 21, 23].
* **Offline Auditability:** Because PDF encryption headers contain self-contained validation hashes, an attacker who obtains a copy of an encrypted file can test millions of password candidates offline without triggering account lockouts.

### Remediation & Hardening Best Practices
1. **Use Strong, High-Entropy Passphrases:** Protect sensitive documents with randomly generated passphrases of at least 14–16 characters mixing uppercase, lowercase, numbers, and special symbols rather than dictionary words.
2. **Enforce Modern AES-256 Encryption:** Configure PDF creation tools to use **AES-256 (PDF 2.0)** encryption, which utilizes high-iteration SHA-256 key derivation to significantly slow down offline brute-force attempts.
3. **Avoid Relying Solely on Owner/Permissions Passwords:** Always enforce a **User (Open) Password** when document confidentiality is required, as owner permission flags alone do not encrypt file contents.

---

## 7. Lab Evidence & Screenshots

### 1. PDF Hash Extraction (`pdf2john` Format)
<p align="center">
 <img width="1024" height="515" alt="image" src="https://github.com/user-attachments/assets/6d042c08-0bae-4f2d-b6d1-d6956493da24" />

### 2. Johnny GUI Configuration (`John the Ripper 1.9.0-jumbo-1`)
<p align="center">
 <img width="1024" height="515" alt="image" src="https://github.com/user-attachments/assets/3ef9c991-4e55-4ecf-bffd-f7dae5cc9697" />


### 3. Password Recovery Result in Johnny
<img width="1600" height="589" alt="image" src="https://github.com/user-attachments/assets/8dbb5d87-e1ed-4062-b51a-cf2cfc394513" />

##Hash calculator
| <img width="962" height="631" alt="image" src="https://github.com/user-attachments/assets/a62ed61e-d806-4926-b542-8e19551ff584" />
 |  
 ## password crack in networkwalks tool
 <img width="926" height="539" alt="Screenshot 2026-11-20 213023" src="https://github.com/user-attachments/assets/a9617aea-d20c-4bcd-aa56-f186c0cec378" />


### 4. Unlocked Challenge Documents & Captured Flags

<img width="614" height="823" alt="image" src="https://github.com/user-attachments/assets/86a8abb9-17dc-4823-860a-5a062eca5680" />

| <img width="394" height="529" alt="image" src="https://github.com/user-attachments/assets/ca53641f-2488-4e9a-aafb-7e1f18a58cfb" />   <img width="572" height="538" alt="image" src="https://github.com/user-attachments/assets/d664dd4a-b22a-4151-a06b-c4ba8d281385" />

 

---

## Author & Project Information

* **Author:** Iddrisu Sualihu  
* **Program:** Cybersecurity Program — Networkwalks (Batch B084)
* **Week:** Week 03 (PDF Encryption & Password Auditing Lab)  
* **Repository:** GitHub
