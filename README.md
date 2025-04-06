# BitDrop - Bitcoin Donation Extension

BitDrop is an open-source Bitcoin donation platform designed as a browser extension for **Chrome** and **Safari**. It enables content creators to easily generate Bitcoin donation addresses, display QR codes, and track incoming transactions. BitDrop supports both Bitcoin **mainnet** and **testnet** for development and testing.

---

## Motivation & Vision

**Empowering content creators and communities with seamless, private, and borderless Bitcoin donations.**

BitDrop makes it effortless to accept Bitcoin tips and donations directly in your browser, without relying on centralized platforms or exposing sensitive wallet details. Whether you're a developer, artist, writer, or activist, BitDrop helps you connect with your supporters through the power of Bitcoin.

Built with privacy, simplicity, and open-source principles at its core.


---

## Features

- **Generate unique Bitcoin donation addresses** (testnet or mainnet)
- **Display QR codes** for easy donations
- **View wallet balance** in satoshis or BTC
- **Track recent transactions** to donation addresses
- **Testnet faucet integration** for easy test coins
- **Chrome Extension** with backend API integration
- **Safari Extension** fully client-side, no backend required
- **Native Safari App Extension** container for macOS
- **Supports SegWit and legacy address formats**

---

## Tech Stack

- **Backend:** Python (Flask), bitcoinlib, bit
- **Frontend:** JavaScript, HTML, CSS
- **Browser APIs:** Chrome Manifest V3, Safari Web Extensions
- **Native:** Swift (Safari App Extension)
- **Bitcoin Libraries:** bitcoinjs-lib (client-side address generation)
- **APIs:** BlockCypher, Mempool.space, QRServer
- **Build Tools:** Webpack, npm
- **Others:** qrcode.js, axios


---

## Directory Layout

```plaintext
BitDrop/
├── Chrome_Extension/
│   ├── bitcoin-donation-api/          # Flask backend API (Python)
│   │   ├── app.py                     # Backend using bitcoinlib
│   │   ├── app2.py                    # Alternative backend using bit library
│   │   ├── requirements.txt           # Python dependencies
│   │   ├── addresses.json             # Stored addresses (bitcoinlib)
│   │   ├── addresses_bit.json         # Stored addresses (bit library)
│   │   └── templates/
│   │       └── dashboard.html         # Simple dashboard page
│   └── bitcoin-donation-extension/    # Chrome extension source
│       ├── manifest.json              # Chrome extension manifest
│       ├── popup.html                 # Popup UI
│       ├── popup.js                   # Popup logic
│       ├── qrcode.min.js              # QR code generation library
├── Safari_extension/
│   ├── BitcoinDonationExtension/      # Native Safari App Extension (Swift)
│   │   ├── AppDelegate.swift
│   │   ├── ViewController.swift
│   │   ├── Resources/                 # HTML, JS, CSS for native app UI
│   │   └── Assets.xcassets/           # App icons and assets
│   ├── src/                           # Safari Web Extension (Manifest V3)
│   │   ├── manifest.json              # Safari extension manifest
│   │   ├── popup.html                 # Popup UI
│   │   ├── popup-direct.js            # Popup logic (client-side)
│   │   ├── popup.js                   # (Possibly unused)
│   │   ├── popup.css                  # Popup styling
│   │   ├── background.js              # Background service worker
│   │   ├── content.js                 # Content script injected into pages
│   │   ├── lib/                       # Libraries (axios, qrcode)
│   │   └── images/                    # Extension icons
│   ├── build.sh                       # Build script for Safari extension
│   ├── package.json                   # NPM package info
│   ├── webpack.config.js              # Webpack build config
│   └── README.md                      # Safari extension specific README
```


---

## How It Works

1. **User installs the extension** in Chrome or Safari.
2. **Generates a unique Bitcoin address** (client-side or via backend).
3. **Displays a QR code** for easy donations.
4. **Donors scan and send Bitcoin** to the address.
5. **Extension fetches balance and transactions** from backend or public APIs.
6. **User tracks donations** in real-time, privately and securely.

---

## Architecture Overview

### 1. Chrome Extension (`Chrome_Extension/bitcoin-donation-extension/`)

- **Popup UI** (`popup.html`, `popup.js`): 
  - Connects to a local Flask backend API (default `http://127.0.0.1:5001`)
  - Generates new donation addresses
  - Displays QR code and wallet info
- **Backend API** (`Chrome_Extension/bitcoin-donation-api/`):
  - Two implementations:
    - `app.py` (using `bitcoinlib`)
    - `app2.py` (using `bit` library, default port 5001)
  - Manages wallet, generates addresses, tracks balance and transactions
  - Stores addresses in JSON files (`addresses.json`, `addresses_bit.json`)
  - Provides REST API endpoints:
    - `/generate-address`
    - `/wallet-balance`
    - `/transactions`
    - `/addresses`
    - `/dashboard` (simple HTML dashboard)

### 2. Safari Extension (`Safari_extension/src/`)

- **Manifest V3** extension, fully client-side
- Generates Bitcoin addresses in-browser using `bitcoinjs-lib`
- Displays QR codes via external API (`api.qrserver.com`)
- Fetches balance and transactions from public APIs (BlockCypher, Mempool.space)
- Supports toggling between testnet and mainnet
- Saves generated addresses in browser storage
- Includes faucet links for testnet coins
- No backend server dependency

### 3. Native Safari App Extension (`Safari_extension/BitcoinDonationExtension/`)

- macOS container app hosting the web extension
- Loads local HTML (`Main.html`) in a `WKWebView`
- Manages extension permissions and preferences
- Written in Swift

---

## Setup Instructions

### Prerequisites

- **Python 3.7+** (for backend API)
- **pip** (Python package manager)
- **Google Chrome** or **Safari 14+**
- **Xcode** (for building Safari App Extension, optional)

### Backend API (for Chrome Extension)

1. Navigate to the API directory:

```bash
cd Chrome_Extension/bitcoin-donation-api
```

2. Install dependencies:

For `app.py` (bitcoinlib):

```bash
pip install flask bitcoinlib
```

For `app2.py` (bit library):

```bash
pip install flask bit
```

3. Run the backend server (default: `app2.py` on port 5001):

```bash
python app2.py
```

Or run `app.py` (default port 5000):

```bash
python app.py
```

4. Ensure the server is running at `http://127.0.0.1:5001` (or update the Chrome extension manifest if needed).

### Chrome Extension

1. Open Chrome and navigate to `chrome://extensions/`
2. Enable **Developer mode**
3. Click **Load unpacked**
4. Select the folder:

```
Chrome_Extension/bitcoin-donation-extension
```

5. The extension popup will connect to the backend API to generate addresses and display info.

### Safari Extension

1. Open Safari and navigate to **Preferences > Extensions**
2. Enable **Bitcoin Donation Extension**
3. The extension works fully client-side, no backend needed.
4. Use the popup to generate addresses, view QR codes, and track transactions.
5. Toggle between testnet and mainnet as needed.

### Safari App Extension (macOS)

- Open `Safari_extension/BitcoinDonationExtension.xcodeproj` in Xcode
- Build and run the app to install the native container
- This hosts the web extension and manages permissions

---

## Usage

- **Generate Address:** Click "Generate New Address" to create a unique Bitcoin address.
- **Copy Address:** Use the "Copy" button to copy the address for sharing.
- **QR Code:** Scan the QR code with any Bitcoin wallet app.
- **Wallet Balance:** View total received balance.
- **Transactions:** See recent incoming transactions.
- **Testnet Mode:** Use testnet for development; get coins from provided faucets.
- **Mainnet Mode:** Switch to mainnet for real donations.

---

## Development Notes

- The Chrome extension **requires the backend API** running locally.
- The Safari extension is **fully client-side** and uses public APIs.
- You can customize API endpoints or add new features as needed.
- The backend supports Bitcoin **testnet** by default for safe testing.
- The project uses **Manifest V3** for both extensions.
- QR codes are generated using `qrcode.min.js` (Chrome) or external API (Safari).
- The backend stores addresses in JSON files; consider using a database for production.
- The Safari App Extension is optional but recommended for full macOS integration.

---

## Demo

[BitDrop Chrome and Safari Demo](https://www.youtube.com/watch?v=cBowsZzl6Go)

---


---

## Credits

- [bitcoinlib](https://bitcoinlib.readthedocs.io/)
- [bit](https://ofek.dev/bit/)
- [bitcoinjs-lib](https://github.com/bitcoinjs/bitcoinjs-lib)
- [BlockCypher API](https://www.blockcypher.com/dev/bitcoin/)
- [QR Server API](https://goqr.me/api/)
- Bitcoin logo and colors © respective owners

---
