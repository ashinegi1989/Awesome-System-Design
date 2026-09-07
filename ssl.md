



========================================================
       CERTIFICATE + PRIVATE KEY WORKFLOW
========================================================

STEP 1: Generate Key Pair
--------------------------

Your system generates TWO keys:

    🔑 PRIVATE KEY
       - Secret
       - Never share it
       - Used to SIGN

    🔓 PUBLIC KEY
       - Can be shared
       - Used to VERIFY signatures


STEP 2: Create CSR
------------------

You create a CSR containing:

    Your Identity Information
             +
        🔓 Public Key
             |
             v
            CSR

You send CSR to the CA.

IMPORTANT:
The Private Key is NOT sent to the CA.


STEP 3: CA Issues Certificate
-----------------------------

CA verifies your identity.

CA says:

"I confirm that this Public Key
belongs to this identity."

CA creates:

    📜 CERTIFICATE
       |
       ├── Your Identity
       ├── 🔓 Your Public Key
       └── CA's Digital Signature

CA gives the certificate to you.


STEP 4: Store in JKS
--------------------

Your JKS/Java KeyStore can contain:

        JKS 👜
       /      \
      /        \
 🔑 Private    📜 Certificate
    Key             |
                    └── 🔓 Public Key

Private Key = SECRET
Certificate = Can be shared


STEP 5: You Connect to Server
-----------------------------

Server says:

    "Prove who you are."


Your application uses:

    🔑 Private Key
          |
          v
       SIGN ✍️
          |
          v
    Digital Signature


STEP 6: Send to Server
----------------------

Your application sends:

    📜 Certificate
           +
    ✍️ Digital Signature
           |
           v
        SERVER


STEP 7: Server Gets Public Key
------------------------------

Server reads your certificate.

Certificate contains:

    🔓 Public Key

So server now has your Public Key.


STEP 8: Server Verifies
-----------------------

Server uses:

    🔓 Public Key
          +
    ✍️ Digital Signature
          |
          v
       VERIFY
          |
          v
        ✅ VALID

This proves:

    "The sender possesses the
     corresponding Private Key."


========================================================
             SIMPLE REAL-LIFE ANALOGY
========================================================

🔑 Private Key   = Your secret key
🔓 Public Key    = Lock that you can show everyone
📜 Certificate   = ID card saying
                   "This lock belongs to you"
👜 JKS           = Safe wallet holding your key + ID
✍️ Signature     = Proof that you used your secret key
🏢 Server        = Security guard
🏢 CA            = Trusted authority confirming your identity


========================================================
             MOST IMPORTANT RULE
========================================================

🔑 PRIVATE KEY
      |
      └── SIGN

🔓 PUBLIC KEY
      |
      └── VERIFY


CA:
    "This Public Key belongs to this Identity."

Server:
    "The signature proves that you possess
     the corresponding Private Key."


========================================================

========================================================
              HTTPS / TLS — SIMPLE WORKFLOW
========================================================

HTTPS
  |
  └── Uses TLS
       |
       ├── 1. TLS HANDSHAKE 🤝
       |
       |      A. IDENTITY CHECK 🪪
       |         |
       |         └── Server sends Certificate
       |              |
       |              └── Client checks:
       |                   "Is this really the server?"
       |
       |      B. SESSION KEY 🔑
       |         |
       |         └── Client + Server establish
       |              session keys
       |
       |
       └── 2. SECURE COMMUNICATION 🔐
              |
              └── Actual data is encrypted
                  using the session keys


========================================================
                  CERTIFICATE PART
========================================================

Server
  |
  | Certificate 📜
  | contains Public Key 🔓
  v
Client
  |
  | Checks certificate
  | and trusts the CA
  v
✅ "This is the correct server"


CA = Certificate Authority
  |
  └── Confirms:
      "This certificate/public key belongs
       to this server."


========================================================
             PRIVATE / PUBLIC KEY
========================================================

🔑 Private Key
   |
   └── Kept SECRET by the owner

🔓 Public Key
   |
   └── Can be shared


Certificate 📜
   |
   └── Contains the Public Key
       + Identity information
       + CA's signature


========================================================
              ENCRYPTION PART
========================================================

During TLS handshake:

Client + Server
      |
      └── Establish session key(s) 🔑
                    |
                    v

Actual communication:

Client
  |
  | "Hello Server"
  |
  | 🔐 Encrypt using session key
  |
  | "8x#Kp92@..."
  |
  v
Server
  |
  | 🔓 Decrypt using session key
  |
  v
"Hello Server"


========================================================
                 EASY MEMORY
========================================================

🪪 Certificate
      =
"WHO IS THE SERVER?"

🤝 TLS Handshake
      =
"Let's verify identity AND establish keys."

🔑 Session Key
      =
"Secret key for our conversation."

🔐 TLS Encryption
      =
"Keep our conversation private."


========================================================
                ONE-LINE FLOW
========================================================

Client
  ↓
TLS Handshake
  ↓
🪪 Verify Server Identity
  ↓
🔑 Establish Session Keys
  ↓
🔐 Encrypted Communication
  ↓
Server


IMPORTANT:
--------------------------------------------------------

❌ Certificate does NOT normally encrypt every message.

❌ Public/private keys are NOT normally used to encrypt
   every piece of HTTPS data.

✅ They are used during the TLS handshake.

✅ Session keys are then used for the actual
   encrypted communication.
========================================================

========================================================
          CLIENT → IDAHO → JWT SIMPLE WORKFLOW
========================================================

CLIENT has JKS
----------------

        JKS 👜
       /      \
      /        \
🔑 Private     📜 Certificate
   Key              |
                    └── 🔓 Public Key


========================================================
STEP 1 — CLIENT IDENTIFIES ITSELF
========================================================

CLIENT
   |
   | "I am Client ABC"
   | + Certificate 📜
   |
   v
IDAHO


========================================================
STEP 2 — IDAHO GETS PUBLIC KEY
========================================================

Idaho receives the Certificate 📜

Certificate
     |
     └── 🔓 Public Key

Idaho uses the Public Key to verify
the client's identity/proof.


========================================================
STEP 3 — PROVE PRIVATE KEY OWNERSHIP
========================================================

IDAHO
   |
   | Random Challenge
   | "123456"
   v
CLIENT

CLIENT uses:

   123456
      +
🔑 Private Key
      |
      v
✍️ Digital Signature


========================================================
STEP 4 — CLIENT SENDS SIGNATURE
========================================================

CLIENT
   |
   | Digital Signature ✍️
   v
IDAHO

Idaho uses:

🔓 Public Key
      +
✍️ Signature
      |
      v
   VERIFY
      |
      v
     ✅


Meaning:

"Client really has the Private Key
associated with this Certificate."


========================================================
STEP 5 — IDENTITY IS VERIFIED
========================================================

Idaho:

    Certificate trusted?       ✅
    Identity valid?            ✅
    Private key proven?        ✅

             |
             v

       CLIENT AUTHENTICATED
             ✅


========================================================
STEP 6 — IDAHO ISSUES JWT
========================================================

IDAHO
   |
   | Create JWT
   | Add client information
   | Sign JWT ✍️
   |
   v
🎟️ JWT TOKEN
   |
   v
CLIENT


========================================================
STEP 7 — CLIENT CALLS API
========================================================

CLIENT
   |
   | 🎟️ JWT
   | 🔐 HTTPS/TLS
   v
API SERVER
   |
   | Verify JWT
   | Check expiry
   | Check permissions/scope
   |
   v
✅ ACCESS


========================================================
          WHO DOES WHAT?
========================================================

🔑 Private Key
      ↓
Client uses it to SIGN / prove ownership

🔓 Public Key
      ↓
Idaho uses it to VERIFY

📜 Certificate
      ↓
Binds identity + Public Key
and is trusted through the CA

🛡️ Idaho
      ↓
Authenticates client
and issues JWT

🎟️ JWT
      ↓
Temporary token used to access APIs

🔐 HTTPS/TLS
      ↓
Protects communication while traveling


========================================================
              EASY MEMORY
========================================================

JKS
 ↓
"I have my secret Private Key 🔑"

        ↓

Idaho
 ↓
"Prove you own it."

        ↓

Client
 ↓
"Here is my Signature ✍️"

        ↓

Idaho
 ↓
"Verified ✅"

        ↓

Idaho
 ↓
"Here is your JWT 🎟️"

        ↓

Client
 ↓
"Use JWT to call API"

        ↓

API
 ↓
"JWT valid? ✅"
========================================================

NOTE:
The random-challenge step above is a simple
illustration of proving possession of a private key.
The exact JPMC/Idaho implementation may use a
different standard authentication flow.
