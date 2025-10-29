# 🚀 KAYEE01 - Complete Deployment Guide

## 📋 Pre-Deployment Checklist

### ✅ Required Items
- [ ] VPS server (Ubuntu 20.04/22.04, minimum 2GB RAM)
- [ ] Domain name configured and pointing to VPS IP
- [ ] SSH access to VPS
- [ ] Gmail account with App Password for SMTP
- [ ] At least ONE payment gateway account (Stripe OR Plisio recommended)

### ✅ API Keys & Credentials to Gather

#### Required:
1. **Domain & Email**
   - Domain name (e.g., kayee01.com)
   - Gmail SMTP credentials (email + app password)

2. **MongoDB**
   - Create a strong password (min 12 characters)

3. **Payment Gateway (Choose at least ONE)**
   - **Stripe** (Recommended for credit cards): https://dashboard.stripe.com/apikeys
   - **Plisio** (Recommended for crypto): https://plisio.net/account/api

#### Optional:
- **Google OAuth**: https://console.cloud.google.com/apis/credentials
- **PayPal**: https://developer.paypal.com/
- **Facebook OAuth**: https://developers.facebook.com/apps/
- **Google Analytics**: https://analytics.google.com/

---

## 🛠️ Step-by-Step Deployment

### Step 1: Prepare Your Environment Configuration

1. **Copy the template file**:
   ```bash
   cp env.template .env.template
   ```

2. **Edit `.env.template` with your credentials**:
   - Replace `your-gmail-app-password-here` with your actual Gmail app password
   - Replace `ChangeThisToASecurePassword123!` with a strong MongoDB password
   - Add your Stripe or Plisio API keys
   - Update `DOMAIN_NAME` with your actual domain

3. **Important**: The `.env` file will be created by the deployment script

### Step 2: Push Code to GitHub

```bash
# Ensure all files are committed
git add .
git commit -m "Ready for deployment"
git push origin main
```

### Step 3: Connect to Your VPS

```bash
ssh root@YOUR_VPS_IP
# Or if using non-root user:
ssh your-user@YOUR_VPS_IP
```

### Step 4: Download and Run Deployment Script

```bash
# Download the deployment script
wget https://raw.githubusercontent.com/YOUR-USERNAME/kayee01/main/VPS-FINAL-COMPLETE/DEPLOY.sh

# Make it executable
chmod +x DEPLOY.sh

# Run the deployment
./DEPLOY.sh
```

### Step 5: Follow the Interactive Prompts

The script will ask you for:
1. **GitHub Repository URL** - Your repository URL
2. **Domain Name** - Your domain (e.g., kayee01.com)
3. **MongoDB Password** - A secure password
4. **SMTP Password** - Your Gmail app password
5. **Stripe Secret Key** - Your Stripe API key
6. **Plisio API Key** - Your Plisio API key

### Step 6: Configure DNS (If Not Done)

Point your domain's A record to your VPS IP:
```
Type: A
Name: @
Value: YOUR_VPS_IP
TTL: 3600
```

For www subdomain:
```
Type: A
Name: www
Value: YOUR_VPS_IP
TTL: 3600
```

### Step 7: Setup SSL Certificate

After DNS is configured and propagated (can take 5-60 minutes):
```bash
cd /opt/kayee01/VPS-FINAL-COMPLETE
./setup-ssl.sh your-domain.com
```

---

## 🔐 Post-Deployment Security

### Immediate Actions (Critical!)

1. **Change Admin Password**
   - Login at: https://your-domain.com/admin/login
   - Email: kayicom509@gmail.com
   - Password: Admin123!
   - **Immediately change this password after first login!**

2. **Verify MongoDB Password**
   - Ensure you used a strong password during setup
   - Password is stored in `/opt/kayee01/VPS-FINAL-COMPLETE/.env`

3. **Secure Your SSH**
   ```bash
   # Disable root login (if using another user)
   sudo nano /etc/ssh/sshd_config
   # Set: PermitRootLogin no
   sudo systemctl restart sshd
   ```

4. **Setup Automatic Backups**
   ```bash
   # Add to crontab
   crontab -e
   # Add this line for weekly backups:
   0 2 * * 0 /opt/kayee01/VPS-FINAL-COMPLETE/backup-mongodb.sh
   ```

---

## 📊 Monitoring & Maintenance

### Check Service Status
```bash
cd /opt/kayee01/VPS-FINAL-COMPLETE
docker-compose ps
```

### View Logs
```bash
# All services
docker-compose logs -f

# Specific service
docker-compose logs -f backend
docker-compose logs -f frontend
docker-compose logs -f nginx
docker-compose logs -f mongodb
```

### Restart Services
```bash
# Restart all
docker-compose restart

# Restart specific service
docker-compose restart backend
```

### Update Application
```bash
cd /opt/kayee01/VPS-FINAL-COMPLETE
git pull origin main
docker-compose build --no-cache
docker-compose up -d
```

---

## 🐛 Troubleshooting

### Site Not Loading

1. **Check if containers are running**:
   ```bash
   docker-compose ps
   ```

2. **Check Nginx logs**:
   ```bash
   docker-compose logs nginx
   ```

3. **Verify DNS**:
   ```bash
   dig +short your-domain.com
   # Should return your VPS IP
   ```

### Backend Errors

1. **Check backend logs**:
   ```bash
   docker-compose logs backend
   ```

2. **Verify environment variables**:
   ```bash
   cat .env
   ```

3. **Test MongoDB connection**:
   ```bash
   docker exec -it kayee01-mongodb mongosh -u kayee01_admin -p
   ```

### SSL Issues

1. **Verify DNS is pointing to server**:
   ```bash
   dig +short your-domain.com
   ```

2. **Re-run SSL setup**:
   ```bash
   ./setup-ssl.sh your-domain.com
   ```

3. **Check certificate**:
   ```bash
   docker exec kayee01-nginx ls -la /etc/nginx/ssl/live/
   ```

### Email Not Sending

1. **Verify SMTP credentials in .env**
2. **Check if Gmail App Password is correct**
3. **Check backend logs for email errors**:
   ```bash
   docker-compose logs backend | grep -i email
   ```

### Payment Issues

1. **Verify API keys in .env**
2. **Check if payment service is in demo mode** (logs will show "Demo Mode")
3. **For Stripe**: Check webhook endpoint is configured
4. **For Plisio**: Verify callback URL is accessible

---

## 📞 Support & Resources

### Documentation Links
- **Stripe Docs**: https://docs.stripe.com/
- **Plisio Docs**: https://plisio.net/documentation/
- **MongoDB Docs**: https://www.mongodb.com/docs/
- **Docker Docs**: https://docs.docker.com/
- **Nginx Docs**: https://nginx.org/en/docs/

### Common Commands Reference

```bash
# Navigate to project
cd /opt/kayee01/VPS-FINAL-COMPLETE

# Check status
docker-compose ps

# View logs
docker-compose logs -f

# Restart services
docker-compose restart

# Stop all services
docker-compose down

# Start all services
docker-compose up -d

# Rebuild and restart
docker-compose build --no-cache && docker-compose up -d

# MongoDB backup
./backup-mongodb.sh

# Update SSL certificate
./setup-ssl.sh your-domain.com

# Check disk space
df -h

# Check memory usage
free -h

# Check system load
htop
```

---

## 🎯 Performance Optimization

### After Deployment

1. **Enable HTTP/2** (automatically enabled with SSL)

2. **Configure CDN** (Optional but recommended)
   - Cloudflare (Free tier available)
   - CloudFront
   - BunnyCDN

3. **Database Indexing**
   ```bash
   docker exec -it kayee01-mongodb mongosh -u kayee01_admin -p
   use kayee01_db
   db.products.createIndex({ name: "text", description: "text" })
   db.orders.createIndex({ order_number: 1 })
   ```

4. **Monitor Performance**
   - Use Google Analytics (configure in admin settings)
   - Monitor server resources with htop
   - Check Docker stats: `docker stats`

---

## 🔄 Backup & Recovery

### Manual Backup
```bash
./backup-mongodb.sh
```

### Automated Backups (Recommended)
```bash
# Add to crontab
crontab -e

# Daily backup at 2 AM
0 2 * * * /opt/kayee01/VPS-FINAL-COMPLETE/backup-mongodb.sh

# Weekly backup on Sunday at 3 AM
0 3 * * 0 /opt/kayee01/VPS-FINAL-COMPLETE/backup-mongodb.sh
```

### Restore from Backup
```bash
# Stop services
docker-compose down

# Restore MongoDB
docker exec kayee01-mongodb mongorestore --drop \
  -u kayee01_admin -p YOUR_PASSWORD \
  --authenticationDatabase admin \
  /data/backup/BACKUP_DATE

# Restart services
docker-compose up -d
```

---

## ✅ Deployment Complete!

Your Kayee01 e-commerce platform is now live! 🎉

**Access Points:**
- **Website**: https://your-domain.com
- **Admin Panel**: https://your-domain.com/admin/login
- **API Docs**: https://your-domain.com/api/docs

**Next Steps:**
1. Change admin password
2. Add your first products
3. Test checkout process
4. Configure payment gateways
5. Customize store settings
6. Set up Google Analytics (optional)

---

**Need Help?** Check the troubleshooting section or review the logs with `docker-compose logs -f`

**Happy Selling! 🚀**

