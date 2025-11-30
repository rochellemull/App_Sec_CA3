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
and expiry are properly checked. I also moved the JWT secret key into a 
.env file. This ensures tokens cannot be forged or modified and protects 
sensitive user data.