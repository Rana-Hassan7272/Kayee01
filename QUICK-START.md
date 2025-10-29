# ⚡ KAYEE01 - Quick Start Guide

## 🚀 Deploy in 5 Minutes

### Prerequisites
- Ubuntu VPS (2GB+ RAM)
- Domain name pointing to VPS
- Gmail with App Password
- Stripe OR Plisio account

---

## 📝 Step 1: Prepare API Keys (2 minutes)

### Required:
```bash
# Your Information
DOMAIN: ___________________ (e.g., kayee01.com)
VPS IP: ___________________ (e.g., 93.127.217.2)

# Email (Gmail App Password)
Gmail: kayicom509@gmail.com
App Password: ___________________ (Generate at: https://myaccount.google.com/apppasswords)

# MongoDB
Password: ___________________ (Create strong password, min 12 chars)

# Payment Gateway (Choose ONE minimum)
□ Stripe Key: ___________________ (Get at: https://dashboard.stripe.com/apikeys)
□ Plisio Key: ___________________ (Get at: https://plisio.net/account/api)
```

---

## 🖥️ Step 2: Configure DNS (1 minute)

Set your domain's A records:
```
Type: A | Name: @ | Value: YOUR_VPS_IP | TTL: 3600
Type: A | Name: www | Value: YOUR_VPS_IP | TTL: 3600
```

---

## 🔧 Step 3: Deploy (2 minutes)

```bash
# 1. SSH into your VPS
ssh root@YOUR_VPS_IP

# 2. Download deployment script
wget https://raw.githubusercontent.com/YOUR-USERNAME/kayee01/main/VPS-FINAL-COMPLETE/DEPLOY.sh

# 3. Run deployment
chmod +x DEPLOY.sh && ./DEPLOY.sh
```

The script will prompt you for:
- GitHub repo URL
- Domain name
- MongoDB password
- SMTP password
- Stripe/Plisio keys

---

## ✅ Step 4: Verify (30 seconds)

Visit your site:
```
http://your-domain.com
```

Login to admin:
```
URL: http://your-domain.com/admin/login
Email: kayicom509@gmail.com
Password: Admin123!
```

**⚠️ IMPORTANT: Change admin password immediately!**

---

## 🔐 Step 5: Setup SSL (Optional, 1 minute)

After DNS propagates (5-60 minutes):
```bash
cd /opt/kayee01/VPS-FINAL-COMPLETE
./setup-ssl.sh your-domain.com
```

---

## 🎉 Done!

Your store is live at `https://your-domain.com`

### Quick Commands:
```bash
# Check status
docker-compose ps

# View logs
docker-compose logs -f

# Restart
docker-compose restart
```

### Next Steps:
1. ✅ Change admin password
2. ✅ Add products
3. ✅ Test checkout
4. ✅ Configure payment webhooks (if using Stripe)

---

## 🆘 Need Help?

**Site not loading?**
```bash
docker-compose ps  # Check if services are running
docker-compose logs nginx  # Check logs
```

**Can't login?**
- Default: kayicom509@gmail.com / Admin123!
- Check backend logs: `docker-compose logs backend`

**Payment issues?**
- Verify API keys in `/opt/kayee01/VPS-FINAL-COMPLETE/.env`
- Check logs: `docker-compose logs backend | grep -i payment`

---

**📚 Full Guide:** See `DEPLOYMENT-GUIDE.md` for detailed instructions

