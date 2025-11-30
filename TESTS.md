##1. Manual Testing for Fixes(Mira)

Fix 1: Input Sanitisation
- Send a safe command → should work
- Send a dangerous command (like `test; rm -rf /`) → should NOT execute

To test the command injection fix,
try sending a normal command and confirm it works. 
Then try sending a malicious command with symbols
like ;, &&, or |. The app should block these 
characters and refuse to execute anything unsafe.

Before:
const { exec } = require('child_process');
exec(cmd, (err, output) => {
After:
const { execFile } = require('child_process');
execFile (cmd, (err, output) => {
Fix 2: JWT Verification
- Send a valid token → request succeeds
- Send an invalid or modified token → request fails with "Invalid or expired token"
 
To test the JWT fix, send a request with a valid 
JWT—this should work normally. Then try modifying the
token or using an expired one. The server should
reject it with an “Invalid or expired token” message,
confirming the verification is working correctly.

Before:
const decoded = jwt.decode(token); // only decodes, does not verify
req.user = decoded;
After:
const verified = jwt.verify(token, process.env.JWT_SECRET);
req.user = verified;