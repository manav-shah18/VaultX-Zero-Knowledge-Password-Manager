# VaultX — Zero-Knowledge Password Manager

A secure, zero-knowledge password manager built from scratch using Python,
Flask, and MongoDB. Developed as a Cyber Security mini project at NMIMS
School of Technology Management & Engineering, Navi Mumbai.

> **Zero-knowledge** means the server never sees your passwords — not during
> storage, not during login, not ever. Even a complete database breach
> reveals nothing without the master password.

---

## Tech Stack

| Layer | Technology | Purpose |
|---|---|---|
| Backend | Python 3 + Flask | Web server and API routing |
| Database | MongoDB (NoSQL) | Stores users, vault, alerts |
| Encryption | AES-256-CFB + RSA-2048 | Hybrid encryption of all passwords |
| Key Derivation | Argon2id + PBKDF2 | Master password hashing and key generation |
| Intrusion Detection | Honey Passwords | Trap-based attack detection |
| Breach Detection | HaveIBeenPwned API | K-anonymity password leak checking |
| Frontend | HTML + CSS + JavaScript | Cyberpunk dashboard UI |

---

## Security Features

| Feature | Implementation | Standard |
|---|---|---|
| Password hashing | Argon2id (64MB RAM, 3 iterations) | OWASP 2024 recommended |
| Key derivation | PBKDF2-HMAC-SHA256 (600,000 iterations) | NIST SP 800-132 |
| Data encryption | AES-256-CFB | FIPS 197 |
| Key exchange | RSA-2048 OAEP SHA-256 | PKCS#1 v2.2 |
| Intrusion detection | Honey password traps | Canary trap model |
| Breach detection | HIBP k-anonymity API | Troy Hunt model |
| Brute-force defense | 5-attempt lockout (300 seconds) | NIST SP 800-63B |

---

## How It Works

```
User logs in with master password
        ↓
Argon2id verifies master password hash
        ↓
PBKDF2 derives 256-bit AES key
        ↓
AES key decrypts RSA private key (stored encrypted in DB)
        ↓
RSA private key lives only in session memory
        ↓
Each vault entry: RSA decrypts its unique AES key → AES decrypts password
```

The server stores only ciphertext. The master password never touches the
database — only its Argon2id hash does.

---

## Project Structure

```
vaultx-password-manager/
│
├── .env                  ← Secret keys (create from .env.example)
├── .env.example          ← Template — copy this to .env
├── config.py             ← Flask configuration loader
├── requirements.txt      ← Python dependencies
├── run.py                ← Entry point — starts Flask on port 5000
│
└── app/
    ├── __init__.py       ← Creates Flask app + connects MongoDB
    ├── auth.py           ← Register, login, key derivation, lockout
    ├── encryption.py     ← AES-256 + RSA-2048 encrypt/decrypt engine
    ├── honey.py          ← Honey password creation + intrusion alerts
    ├── password_gen.py   ← Cryptographic password generator + analyser
    ├── breach.py         ← HaveIBeenPwned k-anonymity breach checker
    ├── routes.py         ← All 14 Flask API endpoints
    ├── vault.py          ← Vault CRUD operations
    └── templates/
        ├── index.html    ← Login and register page
        └── dashboard.html← Main vault dashboard (cyberpunk theme)
```

---

## Setup Instructions

### Prerequisites
- Python 3.10+
- MongoDB running locally on port 27017
- Git

### Installation

**1. Clone the repository**
```bash
git clone https://github.com/manav-shah18/VaultX-Zero-Knowledge-Password-Manager.git
cd VaultX-Zero-Knowledge-Password-Manager
```

**2. Create a virtual environment and activate it**
```bash
python -m venv venv

# Windows
venv\Scripts\activate

# Mac/Linux
source venv/bin/activate
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

**4. Create your `.env` file**
```bash
# Windows
copy .env.example .env

# Mac/Linux
cp .env.example .env
```

Then open `.env` and fill in your values:
```
SECRET_KEY=any-long-random-string-you-choose
MONGO_URI=mongodb://localhost:27017
DB_NAME=password_manager
```

**5. Start MongoDB**

Make sure MongoDB is running locally. If installed as a Windows service
it starts automatically. Otherwise:
```bash
mongod
```

**6. Run VaultX**
```bash
python run.py
```

Open your browser at `http://127.0.0.1:5000`

---

## API Endpoints

| Method | Endpoint | Auth | Purpose |
|---|---|---|---|
| GET | / | No | Login and register page |
| GET | /dashboard | Yes | Main vault dashboard |
| POST | /register | No | Create account |
| POST | /login | No | Login and start session |
| POST | /logout | Yes | Clear session |
| GET | /vault | Yes | List vault entries |
| POST | /vault/add | Yes | Add encrypted entry |
| POST | /vault/get | Yes | Decrypt one entry |
| POST | /vault/update | Yes | Update entry |
| POST | /vault/delete | Yes | Delete entry |
| POST | /generate | No | Generate secure password |
| POST | /analyze | No | Analyze password strength |
| GET | /alerts | Yes | Get intrusion alerts |
| POST | /breach/check | No | Check password against HIBP |
| POST | /breach/scan | Yes | Scan entire vault for breaches |

---

## Attack Simulator

This project has a companion attack simulator that demonstrates all
security defenses working in real time.

**Repository:** [VaultX Attack Simulator](https://github.com/manav-shah18/vaultx-attack-simulator)

The simulator runs 6 attacks against VaultX and shows each one being
blocked or detected live on a Blood Red and Black dashboard.

---

## Academic Context

This project was built as a mini project for the subject **Cyber Security**
in Semester VI of B.Tech Computer Engineering at NMIMS STME, Navi Mumbai.
It demonstrates practical implementation of:
- Zero-knowledge architecture
- Hybrid cryptography (AES + RSA)
- Memory-hard password hashing (Argon2id)
- Honey trap intrusion detection
- K-anonymity breach detection (HaveIBeenPwned)
```