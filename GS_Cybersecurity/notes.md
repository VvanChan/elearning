# Cybersecurity: Crack leaked password database by Goldman Sachs

## 1 Cryptographic Hash Function (CHF)
An algorithm that takes an input and produces a fixed-size string (hash) that appears random.

- **Key Properties**:
  - Deterministic, fast to compute.
  - Pre-image and collision resistance.
  - Small changes in input cause large changes in output.
- **Applications**: Used in digital signatures, data integrity checks, password storage, blockchain, etc.

| Algorithm | Purpose | Salting | Configurable | Memory-Hard | Speed | Use Case | Security  | Weaknesses |
|-----------|---------|------|-------|------|-----------|-------------|--------|-------|
| MD5       | General-purpose hash function          | No            | No                    | No          | Fast                | Not recommended for passwords       | Low          | Fast, vulnerable to collisions and precomputation attacks |
| SHA       | General-purpose hash function (e.g., SHA-1, SHA-256) | No            | No                    | No          | Fast                | Not recommended for passwords       | Medium       | Fast, susceptible to brute-force attacks, SHA-1 is deprecated |
| bcrypt    | Password hashing with adaptive complexity | Yes           | Yes (work factor)     | No          | Slow (configurable) | Secure password storage             | High         | Slower performance with higher work factors |
| scrypt    | Password hashing with high security, resistant to hardware attacks | Yes           | Yes (CPU/memory cost) | Yes         | Slow (configurable) | Highly secure password storage      | High         | High resource usage, can be overkill for some applications |
| PBKDF2    | Password-based key derivation function | Yes           | Yes (iteration count) | No          | Slow (configurable) | Secure password storage             | High         | Slower performance with high iteration counts |


## 2 Salt

**Salting** is a technique that adds a random string (salt) to a password before hashing to improve security. but it does not fully prevent cracking, especially when dealing with small datasets or targeted attacks.

- **Purpose**: Protects against rainbow table attacks and makes brute-force attacks more difficult.
- **How It Works**:
  1. Generate a unique, random salt for each password.
  2. Combine the salt with the password, then hash it.
  3. Store both the salt and the hash in the database.
- **Benefits**: Ensures unique hashes for identical passwords, increases security.
- **Best Practices**: Use unique, random, and long salts; combine with key stretching techniques.

## 3 Password-Cracking Tools

| Tool | Key Features | Usage Scenarios | How to Use |
|-------------------|-----------------|----------------|-----------------------|
| **Aircrack-ng**   | Cracks WEP, WPA, WPA2; packet capture and analysis                            | Wireless network security testing                          | Capture packets (`airodump-ng`), then crack (`aircrack-ng`)                                    |
| **Cain & Abel**   | Windows-based, supports various password types and sniffing                   | Password recovery, network analysis                        | Install on Windows, select recovery method, start attack                                       |
| **John the Ripper**| Open-source, supports many hash types                                        | Cracking Unix/Linux passwords, security auditing            | Run `john` with target password file, use wordlist or brute-force                               |
| [**Hashcat**](https://github.com/hashcat/hashcat)       | GPU-accelerated, multi-hash support                                           | Cracking hashes, penetration testing                       | Command line: `hashcat -m [mode] -a [attack mode] [hash file] [wordlist]`                      |
| **Hydra**         | Fast network logon cracker; supports multiple protocols                       | Brute-forcing network services credentials                 | Run `hydra` with target and service options (e.g., `hydra -l user -P wordlist.txt ftp://host`) |
| **DaveGrohl**     | Focuses on "pass-the-hash" attacks                                            | Windows environment testing, privilege escalation           | Load NTLM hashes, execute attack                                                               |
| **ElcomSoft**     | Commercial, supports GPU acceleration; covers many applications               | Forensics, corporate security, file and document recovery   | Install tool, load file/hash, start recovery process                                           |

## 4 Cracking Techniques
| **Technique**          | **Key Points** |
|------------------------|----------------------------------------|
| **Brute-Force Attack**, Mask Attack | - Tests all combinations<br>- Effective for short passwords (1-6 char) |
| **Dictionary Attack**  | - Uses common words<br>- Effective against predictable passwords<br>- Enhanced with rules for variations    |
| **Hybrid Attack**      | - Combines dictionary and brute-force<br>- Appends characters to words<br>- Cracks passwords with variations |
| **Rule-Based Attack**  | - Applies rules to dictionary words<br>- Substitutes letters with numbers/symbols<br>- Targets patterns      |
| **Cold Boot Attack**   | - Exploits residual RAM data<br>- Reveals sensitive info if RAM not cleared or encrypted                    |
| **Smudge Attack**      | - Residual fingerprints on touchscreens<br>- Reveals input pattern                                          |
| **Markov Chain Attack**| - Uses statistical models<br>- Predicts likely characters/positions<br>- Reduces keyspace                    |


## 5 Practice
```bash
cat rockyou.txt >> wordlist
hashcat --potfile-path hashcat.potfile -m 0 hashfile wordlist
```
first 13 hashes were cracked by [rockyou](https://github.com/josuamarcelc/common-password-list), the rest were cracked [online](https://hashes.com/en/decrypt/hash).

### Analysis
| Username       | Hash                             | Word           | Hash Length | hashing algorithm | level of protection | password policy           |
|----------------|----------------------------------|----------------|-------------|-------------------|---------------------|---------------------------|
| experthead     | e10adc3949ba59abbe56e057f20f883e | 123456         | 32          | MD5 + Salt        | Medium              | Short Len, Small Keyspace |
| interestec     | 25f9e794323b453885f5181f1b624d0b | 123456789      | 32          | MD5               | Low                 | Small Keyspace            |
| ortspoon       | d8578edf8458ce06fbc5bb76a58c5ca4 | qwerty         | 32          | MD5 + Salt        | Medium              | Short Len, Small Keyspace |
| reallychel     | 5f4dcc3b5aa765d61d8327deb882cf99 | password       | 32          | MD5               | Low                 | Small Keyspace            |
| simmson56      | 96e79218965eb72c92a549dd5a330112 | 111111         | 32          | MD5 + Salt        | Medium              | Short Len, Small Keyspace |
| bookma         | 25d55ad283aa400af464c76d713c07ad | 12345678       | 32          | MD5 + Salt        | Medium              | Small Keyspace            |
| popularkiya7   | e99a18c428cb38d5f260853678922e03 | abc123         | 32          | MD5 + Salt        | Medium              | Short Len, Small Keyspace |
| eatingcake1994 | fcea920f7412b5da7be0cf42b8c93759 | 1234567        | 32          | MD5 + Salt        | Medium              | Small Keyspace            |
| heroanhart     | 7c6a180b36896a0a8c02787eeafb0e4c | password1      | 32          | MD5 + Salt        | Medium              | Small Keyspace            |
| edi_tesla89    | 6c569aabbf7775ef8fc570e228c16b98 | password!      | 32          | MD5 + Salt        | Medium              | Small Keyspace            |
| liveltekah     | 3f230640b78d7e71ac5514e57935eb69 | qazxsw         | 32          | MD5 + Salt        | Medium              | Short Len, Small Keyspace |
| blikimore      | 917eb5e9d6d6bca820922a0c6f7cc28b | Pa$$word1      | 32          | MD5 + Salt        | Medium              | Big Keyspace              |
| johnwick007    | f6a0cb102c62879d397b12b62c092c06 | bluered        | 32          | MD5 + Salt        | Medium              | Small Keyspace            |
| flamesbria2001 | 9b3b269ad0a208090309f091b3aba9db | flamesbria2001 | 32          | MD5 + Salt        | Medium              | Small Keyspace            |
| oranolio       | 16ced47d3fc931483e24933665cded6d | Oranolio1994   | 32          | MD5 + Salt        | Medium              | Small Keyspace            |
| spuffyffet     | 1f5c5683982d7c3814d4d9e6d749b21e | Spuffyffet12   | 32          | MD5 + Salt        | Medium              | Small Keyspace            |
| moodie         | 8d763385e0476ae208f21bc63956f748 | moodie00       | 32          | MD5 + Salt        | Medium              | Small Keyspace            |
| nabox          | defebde7b6ab6f24d5824682a16c3ae4 | nAbox!1        | 32          | MD5               | Low                 | Big Keyspace              |
| bandalls       | bdda5f03128bcbdfa78d8934529048cf | Banda11s       | 32          | MD5               | Low                 | Small Keyspace            |

### Memo
19 leaked passwords was examined, the level of protection was low in general.
1. Current Issues
     1. System - Outdated Hashing Algorithm: all encrypted in MD5, 15 with salting.
     2. User - Weak Password Policy: 
        1. 5 were shorter or equal to 6 characters, 17 consisted of few or equal to 2 types of characters out of lowercase letters, uppercase letters, numbers and special symbols.
        2. 13 were in common password list, 6 were adapted from username.
2. Proposed Uplifts
    1. System
       1. Upgrade Hashing Algorithm: Use SHA, bcrypt, scrypt, PBKDF2 or mix.
       2. Implement Salting: Add unique salts to prevent rainbow table attacks.
    2. User
       1. Increase Password Length: Set a minimum length of 6-8 characters.
       2. Password Restrictions: Prevent passwords from being identical to usernames or containing usernames.
       3. Prevent Common Passwords: Reject passwords like "password" or "12345678".
       4. Pattern Restrictions: Force combo of at least 3 of a-z, A-Z, 0-9, signs.
       5. User Education: Teach the use of passphrases (e.g., combining random words). Advise on the use of password managers to store complex passwords securely.
      
      
