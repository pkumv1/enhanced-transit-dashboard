# Troubleshooting Guide

## Common Issues and Solutions

### 🚨 URIError: Failed to decode param '/%PUBLIC_URL%/favicon.ico'

**Problem:** The `%PUBLIC_URL%` placeholder is not being replaced during development.

**Solution:**
```bash
# 1. Clear node_modules and reinstall
rm -rf node_modules package-lock.json
npm install

# 2. Clear React Scripts cache
npm start -- --reset-cache

# 3. If still having issues, try:
export PUBLIC_URL=""
npm start
```

**Root Cause:** This happens when Create React App's environment variable replacement isn't working properly.

---

### 🔧 Development Server Won't Start

**Problem:** `npm start` fails or shows errors.

**Solutions:**
```bash
# Check Node.js version (needs 16+)
node --version

# Clear cache and reinstall
rm -rf node_modules package-lock.json
npm cache clean --force
npm install

# Try different port
npm start -- --port 3001
```

---

### 🎨 Tailwind Styles Not Loading

**Problem:** Components appear unstyled.

**Solutions:**
1. Check browser console for CDN errors
2. Verify internet connection (Tailwind loads from CDN)
3. Try refreshing the page (Ctrl+F5)

**Alternative:** Install Tailwind locally:
```bash
npm install -D tailwindcss
npx tailwindcss init
```

---

### 📱 Icons Not Displaying

**Problem:** Lucide icons show as squares or don't appear.

**Solution:**
```bash
# Reinstall lucide-react
npm uninstall lucide-react
npm install lucide-react@latest
```

---

### 🔄 Real-time Data Not Updating

**Problem:** Dashboard shows static data.

**Check:**
1. Connection status (green/red indicator in header)
2. Browser console for JavaScript errors
3. Click "Live/Offline" toggle to reconnect

---

### 🖥️ Performance Issues

**Problem:** Dashboard is slow or laggy.

**Solutions:**
1. Close other browser tabs
2. Check browser developer tools for memory usage
3. Disable browser extensions temporarily
4. Try in incognito/private mode

---

### 📊 Charts Not Rendering

**Problem:** Recharts components appear blank.

**Solutions:**
```bash
# Reinstall recharts
npm uninstall recharts
npm install recharts@latest

# Clear browser cache
# Chrome: Ctrl+Shift+R
# Firefox: Ctrl+F5
```

---

### 🔒 CORS Errors (Future API Integration)

**Problem:** API calls blocked by CORS policy.

**Solutions:**
```bash
# For development - add proxy to package.json
{
  "proxy": "http://localhost:5000"
}

# Or use environment variables
REACT_APP_API_URL=http://localhost:5000
```

---

### 🌐 Build/Deployment Issues

**Problem:** `npm run build` fails or deployment doesn't work.

**Solutions:**
```bash
# Check for TypeScript errors (if migrating)
npm run build 2>&1 | grep -i error

# Increase Node.js memory limit
export NODE_OPTIONS="--max-old-space-size=4096"
npm run build

# Clean build folder
rm -rf build
npm run build
```

---

### 📝 Missing Environment Variables

**Problem:** App references undefined environment variables.

**Solution:**
```bash
# Create .env file in root directory
echo "REACT_APP_VERSION=1.0.0" > .env
echo "REACT_APP_ENVIRONMENT=development" >> .env

# Restart development server
npm start
```

---

## 🛠️ General Debugging Steps

### 1. Check Browser Console
- Open Developer Tools (F12)
- Look for red error messages
- Check Network tab for failed requests

### 2. Verify Dependencies
```bash
npm list --depth=0
npm outdated
npm audit
```

### 3. Clear All Caches
```bash
# Clear npm cache
npm cache clean --force

# Clear React cache
rm -rf node_modules/.cache

# Clear browser cache
# Chrome: Settings > Privacy > Clear browsing data
```

### 4. Check System Requirements
- **Node.js:** Version 16 or higher
- **npm:** Version 8 or higher  
- **RAM:** At least 4GB available
- **Browser:** Chrome 90+, Firefox 88+, Safari 14+

---

## 🆘 Getting Help

### Before Asking for Help

1. **Check this troubleshooting guide**
2. **Search existing GitHub issues**
3. **Try the debugging steps above**

### When Reporting Issues

Include the following information:

```bash
# System Information
node --version
npm --version
npm list react react-dom

# Error Information
npm start 2>&1 | tee debug.log
```

**Template for GitHub Issues:**
```markdown
## 🐛 Bug Description
Brief description of the issue

## 🔄 Steps to Reproduce
1. Clone the repository
2. Run npm install
3. Run npm start
4. ...

## 💻 Environment
- OS: [Windows 11/macOS 13/Ubuntu 22.04]
- Node.js: [version]
- Browser: [Chrome 120/Firefox 115/Safari 16]

## 📋 Error Logs
```
Paste error logs here
```

## 🔍 Expected vs Actual
- **Expected:** Dashboard loads without errors
- **Actual:** Error message about favicon
```

---

## 🎯 Quick Fixes for Common Scenarios

### Fresh Install
```bash
git clone https://github.com/pkumv1/enhanced-transit-dashboard.git
cd enhanced-transit-dashboard
npm install
npm start
```

### Reset Everything
```bash
rm -rf node_modules package-lock.json
npm cache clean --force
npm install
npm start
```

### Alternative Package Manager
```bash
# Using Yarn instead of npm
npm install -g yarn
yarn install
yarn start
```

### Docker Setup (Advanced)
```bash
# Create Dockerfile
echo "FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD [\"npm\", \"start\"]" > Dockerfile

# Build and run
docker build -t transit-dashboard .
docker run -p 3000:3000 transit-dashboard
```

---

## 📞 Support Channels

- 🐛 **Bug Reports:** [GitHub Issues](https://github.com/pkumv1/enhanced-transit-dashboard/issues)
- 💬 **Discussions:** [GitHub Discussions](https://github.com/pkumv1/enhanced-transit-dashboard/discussions)  
- 📚 **Documentation:** [Project Wiki](https://github.com/pkumv1/enhanced-transit-dashboard/wiki)

---

*Last Updated: June 2025*