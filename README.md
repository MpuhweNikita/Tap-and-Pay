# IoT RFID Tap-and-Pay System - Team Shield

A complete RFID-based payment system with real-time card management, transaction tracking, and a modern dashboard interface.

---

## 🌐 **LIVE DEMO**

<div align="center">

# 🚀 **[VIEW LIVE APPLICATION →](http://157.173.101.159:9202)**

## **Frontend:** http://157.173.101.159:9202  
## **Backend API:** http://157.173.101.159:8202

<br>

[![Live Demo](https://img.shields.io/badge/Live%20Demo-Online-success?style=for-the-badge&logo=google-chrome&logoColor=white&scale=2)](http://157.173.101.159:9202)
[![Backend API](https://img.shields.io/badge/Backend%20API-Running-blue?style=for-the-badge&logo=fastapi&logoColor=white&scale=2)](http://157.173.101.159:8202)
[![Status](https://img.shields.io/badge/Status-Deployed-brightgreen?style=for-the-badge&scale=2)](http://157.173.101.159:9202)

<br>

</div>

---

## 👁️ Features

- Real-time RFID card detection via MQTT
- Cumulative balance top-ups with persistent storage
- Cardholder name management
- Complete transaction history
- MongoDB persistence
- Modern glass-morphism dashboard
- System status monitoring
- Live statistics and analytics

## Team Information

- **Team ID**: `team_rdf`
- **Live Application**: http://157.173.101.159:9202
- **VPS Server**: 157.173.101.159
- **Backend Port**: 8208
- **Frontend Port**: 9208
- **MQTT Broker**: 157.173.101.159:1883


#### Manual Start:
```bash
# Terminal 1 - Backend
cd backend
npm install
npm run dev

# Terminal 2 - Frontend
cd frontend
npm install
npm start
```

**Access locally:**
- Frontend: http://localhost:9208
- Backend: http://localhost:8208

### VPS Deployment

1. **Upload to VPS:**
```bash
scp -r tap-to-pay root@157.173.101.159:/root/
```

2. **Deploy:**
```bash
ssh root@157.173.101.159
cd /root/tap-to-pay
chmod +x deploy.sh
./deploy.sh
```

3. **Access online:**
- Frontend: http://157.173.101.159:9202
- Backend: http://157.173.101.159:8202


## 📡 MQTT Topics

- `rfid/team_rdf/card/status`: ESP8266 publishes card UID and balance when detected
- `rfid/team_rdf/card/topup`: Backend publishes top-up commands
- `rfid/team_rdf/card/balance`: ESP8266 publishes confirmation of balance update
- `rfid/team_rdf/device/status`: MQTT Last Will (online/offline)
- `rfid/team_rdf/device/health`: Periodic health metrics (IP, RSSI, Memory)

## 🔌 HTTP API Endpoints

### Cards
- `GET /cards` - Get all cards
- `GET /card/:uid` - Get specific card details
- `POST /topup` - Top up a card (requires `uid`, `amount`, and `holderName` for new cards)

### Transactions
- `GET /transactions` - Get all transactions (optional `?limit=100`)
- `GET /transactions/:uid` - Get transaction history for specific card

### WebSocket Events
- `card-status` - Emitted when card is detected
- `card-balance` - Emitted when balance is updated

## 🛠️ Hardware Setup (ESP8266 + RC522)

| RC522 Pin | ESP8266 Pin (NodeMCU) | Function  |
| --------- | --------------------- | --------- |
| 3.3V      | 3V3                   | Power     |
| RST       | D3 (GPIO0)            | Reset     |
| GND       | GND                   | Ground    |
| MISO      | D6 (GPIO12)           | SPI MISO  |
| MOSI      | D7 (GPIO13)           | SPI MOSI  |
| SCK       | D5 (GPIO14)           | SPI Clock |
| SDA (SS)  | D4 (GPIO2)            | SPI SS    |

### Firmware Setup

1. Open `/firmware/iot_rfid_project.ino` in Arduino IDE
2. Update WiFi credentials (`ssid` and `password`)
3. Install required libraries:
   - MFRC522
   - PubSubClient
   - ArduinoJson
4. Upload to ESP8266

## 🎨 Dashboard Features

### Sidebar
- Navigation menu (Cards, Analytics, Settings)
- Real-time system status monitoring
  - MQTT Broker connection
  - Backend server status
  - Database connection
- Team info and uptime counter

### Main Content
- **Quick Stats Cards**
  - Total Cards
  - Today's Transactions
  - Total Volume
  - Average Transaction
- **Active Card Display** with glass-morphism effect
- **Top-up Form** with cardholder name support
- **Transaction History** with visual indicators

## 📊 Database Schema

### Card Collection
```javascript
{
  uid: String (unique),
  holderName: String,
  balance: Number,
  lastTopup: Number,
  createdAt: Date,
  updatedAt: Date
}
```

### Transaction Collection
```javascript
{
  uid: String,
  holderName: String,
  type: 'topup' | 'debit',
  amount: Number,
  balanceBefore: Number,
  balanceAfter: Number,
  description: String,
  timestamp: Date
}
```

## 🔧 Configuration

### Backend (.env)
```env
MONGODB_URI=your_mongodb_connection_string
PORT=8208
```

### Auto-Configuration
The frontend automatically detects the environment:
- **Local**: Uses `localhost:8208`
- **Production**: Uses `157.173.101.159:8208`

No manual configuration needed!

## 🛠️ Technology Stack

- **Backend**: Node.js, Express, Socket.IO, Mongoose, MQTT
- **Frontend**: HTML5, CSS3, JavaScript, Socket.IO Client
- **Database**: MongoDB Atlas
- **Hardware**: ESP8266, MFRC522 RFID Reader
- **Process Manager**: PM2 (production)

## 📝 PM2 Commands (Production)

```bash
pm2 status                          # View all processes
pm2 logs tap-to-pay-backend        # View backend logs
pm2 logs tap-to-pay-frontend       # View frontend logs
pm2 restart tap-to-pay-backend     # Restart backend
pm2 restart tap-to-pay-frontend    # Restart frontend
pm2 monit                          # Monitor resources
```

## 🐛 Troubleshooting

### Backend Issues
- Check MongoDB connection in `.env`
- Verify port 8208 is available: `lsof -i :8208`
- Check logs: `pm2 logs tap-to-pay-backend`

### Frontend Issues
- Verify backend is running
- Check browser console for errors
- Test backend: `curl http://localhost:8208/cards`

### MQTT Issues
- Verify MQTT broker is running on port 1883
- Check Arduino serial monitor for connection status
- Test MQTT: `telnet 157.173.101.159 1883`

## 📦 Project Structure

```
tap-to-pay/
├── backend/
│   ├── server.js          # Backend API server
│   ├── package.json
│   └── .env              # MongoDB connection
├── frontend/
│   ├── index.html        # Dashboard UI
│   ├── app.js           # Frontend logic
│   ├── style.css        # Styling
│   ├── config.js        # Auto environment config
│   ├── server.js        # Frontend server
│   └── package.json
├── firmware/
│   └── iot_rfid_project.ino  # Arduino code
├── DEPLOYMENT.md        # Detailed deployment guide
├── deploy.sh           # VPS deployment script
├── start-local.sh      # Local startup (Linux/Mac)
└── start-local.bat     # Local startup (Windows)
```

## 🔐 Security Notes

- MongoDB credentials stored in `.env` (gitignored)
- CORS enabled for development
- Use HTTPS in production (add reverse proxy like Nginx)
- Firewall configured for ports 9208, 8208, 1883

## 📄 License

MIT
