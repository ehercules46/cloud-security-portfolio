# Cracking MD5 hashes with Hashcat

A hands-on test of dictionary-based password recovery, generating MD5 hashes, attacking them with Hashcat, and working around a memory limit that blocked the intended approach halfway through.

## Scenario

This mimics what an attacker finds after breaching a database of hashed credentials: a list of hashes with no context, and a guess at what tool cracked them fastest. MD5 was used deliberately, not because it shows up in real systems anymore, but because it's fast to compute and easy to demonstrate why that speed is exactly the problem. A production system would use something slow and salted, like bcrypt, Argon2, or PBKDF2, specifically to make this kind of attack impractical.

## Environment and tools

- Operating system: Kali Linux (VM)
- Tool: Hashcat v7.1.2
- Hash type: MD5 (`-m 0`)
- Attack mode: dictionary / straight (`-a 0`)

## Step 1: confirm Hashcat is installed

```
hashcat --version
```

![Hashcat v7.1.2 confirmed](screenshots/01-hashcat-version.png)

## Step 2: find the wordlist

Kali ships with a few wordlists pre-installed. `locate` finds files by name without having to know the exact path in advance.

```
locate rockyou.txt
```

This returned `/usr/share/wordlists/rockyou.txt.gz`, the standard wordlist for this kind of exercise, still compressed.

![rockyou.txt.gz located](screenshots/02-locate-wordlist.png)

## Step 3: generate target hashes

Five words were hashed with MD5 using an online generator and saved to `target_hashes.txt`, one hash per line. This is the only step in the whole lab worth a real caveat: pasting real passwords into a third-party hashing site would be a bad idea outside a throwaway exercise like this one, since it means a stranger's server saw the plaintext.

```
cat target_hashes.txt
```

![Five MD5 hashes to crack](screenshots/03-target-hashes.png)

## Step 4: run the dictionary attack, and hit a wall

The plan was to run Hashcat directly against the full `rockyou.txt.gz` wordlist:

```
hashcat -m 0 -a 0 -o cracked.txt target_hashes.txt /usr/share/wordlists/rockyou.txt.gz
```

That's around 14 million candidate passwords, and the VM couldn't handle it. Two attempts, both minutes apart, failed the same way:

```
Not enough allocatable device memory or free host memory for mapping.
```

The first run also flagged that Hashcat had fallen back to unoptimized ("pure") kernels, which trade speed for compatibility, but that wasn't the actual blocker. The real issue was the VM's allocated memory being too small to map a wordlist that size at all, not a speed problem.

![First attempt, memory error](screenshots/04-memory-error-attempt1.png)
![Second attempt, same error](screenshots/05-memory-error-attempt2.png)

**The workaround:** rather than fight the VM's memory allocation, a small custom wordlist (`custom_wordlist.txt`) was built with a handful of candidate words instead of the full 14 million. Much smaller attack surface, but small enough for the VM to actually load into memory.

```
hashcat -m 0 -a 0 -o cracked.txt target_hashes.txt custom_wordlist.txt --force
```

- `-m 0`: hash type is MD5
- `-a 0`: straight dictionary attack
- `-o cracked.txt`: write recovered passwords here
- `--force`: proceed past Hashcat's driver and runtime warnings

## Step 5: check what got cracked

```
cat cracked.txt
```

![Two of five hashes cracked](screenshots/06-cracked-output.png)

Two of the five hashes came back: `chicken` and `taco`. The other three weren't in the small custom wordlist, so they stayed uncracked. This is the honest result of the workaround, not the full rockyou.txt run, which almost certainly would have caught all five, given they were simple dictionary words to begin with.

## The decision that mattered

The interesting part of this lab wasn't the successful crack, it was the memory error. A dictionary attack's real bottleneck is often infrastructure, not password strength: a full-size wordlist needs somewhere to actually live in memory while Hashcat works through it, and a constrained VM can fail on that alone regardless of how weak the passwords are. Scaling down the wordlist got a result, but it also quietly weakens the test, three hashes went unchecked not because they were strong, but because the tool never got to try them.
