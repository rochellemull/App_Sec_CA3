This is our Application Security CA3   project

Fix 1: Input Sanitisation (Command Injection)(Mira)

The original code passed user input directly into child_process.exec(),
which allowed attackers to inject shell commands. I fixed this by adding
strict sanitisation to remove unsafe characters and by replacing exec() with
execFile(), which does not use the system shell. 
This prevents command injection attacks and makes the input handling secure.

Fix 2: Secure JWT Verification (Sensitive Data Protection)(Mira)

The app was using jwt.decode(), which only reads the token without checking 
if it is valid. I replaced this with jwt.verify() so the token’s signature
and expiry are properly checked. This ensures tokens cannot be forged or modified and protects 
sensitive user data.

Fix 3: Hardcoded Secret (Habiba)

At the start of server.js, the JWT_secret was showing hardcoded, and according to (snyk,2025), attackers are doing scans constantly, and this issue could cause a data breach, service abuse, or total system compromise.
Fixing this issue by creating a .env file to store the hardcoded secret in there. Implementing this is done by installing dotenv, which will then create a .env file. Then, in server.js, load the environment variables from the .env file, which is placed at the start, and change the hardcoded secret to process.env.JWT_SECRET.

Fix 4: Use of Hardcoded Passwords (Habiba)

In the api/info endpoint there was a hardcoded password detected by the Snyk scan, and according to (owasp.org,2025), it is increasing the possibility for attackers to guess the passwords, and if the attacker found it, then they would be able to gain access and use that for malicious activities.
Fixing this issue is done by removing the password from server.js and putting it in a safe, isolated file, which in this case is the .env file.

Fix 5: Origin Validation Error
In the server.js for the header origin was not set to be from local host only as there was an or *. This part was removed to ensure the validation vulnerability was removed.

Fix 6: Path traversal
For this part of the file path for to download an image include input from user. To ensure only files had were used and input was sanitized an allow list was created and used to verify that the file gotten was an approved file. As well as this the input from user was santized by replace the characters ../ <> with nothing to also ensure traversal cannot occur through filename. To replace them the original filename variable was converted to a string
