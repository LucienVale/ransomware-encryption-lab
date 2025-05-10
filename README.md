# ransomware-encryption-lab
# Ransomware Encryption Payload – Ethical Hacking Book (Chapter 5)

This report documents my hands-on implementation of a custom Python-based ransomware payload, developed while working through **Chapter 5: Cryptography and Ransomware** from the book *Ethical Hacking: A Hands-On Introduction to Breaking In* by Daniel G. Graham.

The lab focuses on generating an encryption key, building a payload that encrypts a target file, and attempting to deploy that payload on a vulnerable system (Metasploitable). It reflects the kind of experimentation, failures, and breakthroughs that happen during real-world red team development.

---

## Summary

**Main Objective:**  
- Follow the exercise from the book and create a working ransomware payload using Python and the `cryptography` library.

**What I accomplished:**  
- Successfully generated a Fernet symmetric key  
- Wrote and ran a functional encryption payload on my local Kali machine  
- Attempted to deploy the same payload on Metasploitable  
- Discovered that Metasploitable can’t run modern Python code (Python 2.4 limitation)

---

## Phase 1 – Kali Linux: Successful Payload Execution

**Scripts Developed:**

- `key_server.py` – Generates and stores a Fernet encryption key (`keyfile.key`)
- `encrypt_payload.py` – Encrypts the contents of a target file using that key
<img width="651" alt="Screenshot 2025-05-09 at 9 09 59 PM" src="https://github.com/user-attachments/assets/6c9b7022-a83a-49c8-97a9-ccd0f6a17bf5" />
<img width="882" alt="Screenshot 2025-05-09 at 10 19 55 PM" src="https://github.com/user-attachments/assets/d37011a8-fabc-4a7b-859c-c77a2a2aa703" />

**Test File Used:**
- `dummyindex.html`

**Result:**
- File encrypted successfully  
- New file `dummyindex.html.enc` was generated  
- Simulated real ransomware behavior with potential to overwrite or delete original files

**Screenshot of successful encryption:**  
<img width="687" alt="Screenshot 2025-05-09 at 10 17 08 PM" src="https://github.com/user-attachments/assets/2dd23e17-7d07-403b-ace1-560cbc55909a" />


---

## Phase 2 – Attempted Deployment on Metasploitable2

I transferred the scripts using a simple HTTP server from Kali:
```bash
 python3 -m http.server 8080
```
And used wget inside Metasploitable to pull the files into /tmp.

File Created on Victim Machine:
	•	important.txt (to simulate sensitive data)

Result:
	•	Metasploitable runs Python 2.4
	•	My payload uses with open(...) and Python’s cryptography module — incompatible with old systems
	•	Payload failed to run, throwing a syntax error

Screenshot of error:
<img width="715" alt="Screenshot 2025-05-09 at 10 59 10 PM" src="https://github.com/user-attachments/assets/16ea1a40-0450-463c-a290-04812450f29d" />

⸻

Reflection

This wasn’t a waste of time — it was exactly what offensive security is about:
	•	Things won’t always work
	•	You adapt
	•	You sharpen your tools

I completed the exercise from the book and learned something real about target environments:

You can’t drop modern weapons into ancient machines and expect them to fire.

⸻

Next Mission: Covert Exfiltration Payload

My next objective is to build a more advanced version of the payload that will:
	•	Encrypt a specific file on a remote system
	•	Send it back to my Kali machine using an HTTP POST request (covert channel)
	•	Optionally delete the original to simulate stealth
	•	Document and upload this as a separate lab

⸻

Author

Luis aka Lucien Vale
Red team mindset in the making.
Working toward OSCP.
Hands-on, never theoretical.
