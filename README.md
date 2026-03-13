# 🔐 VaultX — Secure Password Manager

A zero-knowledge password manager built with Python, Flask and MongoDB.

## Features
- AES-256 + RSA-2048 hybrid encryption
- Argon2id key derivation
- Honey password intrusion detection
- Brute-force lockout protection
- HaveIBeenPwned breach scanner (k-anonymity)
- Cyberpunk dark UI

## Tech Stack
- Backend: Python, Flask, MongoDB
- Encryption: cryptography, argon2-cffi
- Frontend: HTML, CSS, JavaScript

## Setup
1. Clone the repo
2. Create virtual environment: `python -m venv venv`
3. Activate: `venv\Scripts\activate`
4. Install: `pip install -r requirements.txt`
5. Create `.env` file with your SECRET_KEY and MONGO_URI
6. Run: `python run.py`