# Dependency Health Check Guide

## 🔍 Pre-Installation Checklist

Before running `npm install`, verify your environment meets these requirements:

### ✅ **System Requirements**
```bash
# Check Node.js version (should be 16+)
node --version
# Should output: v16.x.x or higher

# Check npm version (should be 8+)  
npm --version
# Should output: 8.x.x or higher

# Check available memory
free -h  # Linux/Mac
# Should have at least 2GB available
```

### 🔧 **Environment Setup**
```bash
# Clear any existing cache issues
npm cache clean --force

# Remove any global package conflicts
npm list -g --depth=0

# Check for any conflicting global installations
npm list -g react react-dom react-scripts
```

## ⚠️ **Known Potential Issues & Solutions**

### 1. **Node.js Version Conflicts**
**Issue:** React 18.2 requires Node.js 16+
```bash
# Check version
node --version

# If below v16, install latest LTS
# Option 1: Using nvm (recommended)
nvm install --lts
nvm use --lts

# Option 2: Direct download
# Visit: https://nodejs.org/
```

### 2. **npm vs Yarn Conflicts**
**Issue:** Mixed package managers can cause lock file conflicts
```bash
# Remove conflicting lock files
rm -f package-lock.json yarn.lock

# Stick to one package manager
npm install  # OR yarn install (not both)
```

### 3. **Peer Dependency Warnings**
**Issue:** Recharts might show peer dependency warnings
```bash
# Install with legacy peer deps flag if needed
npm install --legacy-peer-deps

# Or manually install missing peer dependencies
npm install react@^18.2.0 react-dom@^18.2.0
```

### 4. **React Scripts Version Issues**
**Issue:** react-scripts 5.0.1 might have issues with newer Node versions
```bash
# If build fails, try updating react-scripts
npm install react-scripts@latest

# Or downgrade Node.js if needed
nvm install 16.20.0
nvm use 16.20.0
```

### 5. **Lucide React Icon Issues**
**Issue:** Icon library might have version conflicts
```bash
# If icons don't display, try latest version
npm install lucide-react@latest

# Or install fallback icon library
npm install react-icons
```

### 6. **Recharts TypeScript Issues**
**Issue:** Recharts might show TypeScript warnings even in JS projects
```bash
# Install types if warnings appear
npm install --save-dev @types/recharts

# Or ignore TypeScript checking
echo "SKIP_PREFLIGHT_CHECK=true" > .env.local
```

### 7. **Windows-Specific Issues**
**Issue:** Path length limits and permission issues on Windows
```bash
# Run as Administrator and enable long paths
# PowerShell (as Admin):
New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force

# Use shorter path
cd C:\
git clone https://github.com/pkumv1/enhanced-transit-dashboard.git dashboard
cd dashboard
```

### 8. **macOS Permission Issues**
**Issue:** Permission denied errors on macOS
```bash
# Fix npm permissions
sudo chown -R $(whoami) ~/.npm
sudo chown -R $(whoami) /usr/local/lib/node_modules

# Or reinstall npm without sudo
curl -L https://www.npmjs.com/install.sh | sh
```

## 🛡️ **Dependency Testing Matrix**

### ✅ **Tested Combinations**
| Node.js | npm | OS | Status |
|---------|-----|-------|--------|
| 18.17.0 | 9.6.7 | macOS | ✅ Working |
| 16.20.0 | 8.19.4 | Ubuntu | ✅ Working |
| 18.16.0 | 9.5.1 | Windows | ✅ Working |

### ⚠️ **Known Problematic Combinations**
| Node.js | npm | OS | Issue |
|---------|-----|-------|--------|
| 14.x.x | Any | Any | React 18 incompatible |
| 20.x.x | Any | Any | Possible react-scripts issues |
| Any | 6.x.x | Any | Package resolution issues |

## 🔄 **Step-by-Step Dependency Installation**

### Method 1: Standard Installation
```bash
# 1. Clone repository
git clone https://github.com/pkumv1/enhanced-transit-dashboard.git
cd enhanced-transit-dashboard

# 2. Check environment
node --version  # Should be 16+
npm --version   # Should be 8+

# 3. Install dependencies
npm install

# 4. Start development server
npm start
```

### Method 2: Safe Installation (If Method 1 Fails)
```bash
# 1. Clear all caches
npm cache clean --force
rm -rf node_modules package-lock.json

# 2. Install with legacy peer deps
npm install --legacy-peer-deps

# 3. If still failing, try exact versions
npm install react@18.2.0 react-dom@18.2.0 react-scripts@5.0.1 --exact

# 4. Install remaining dependencies
npm install
```

### Method 3: Alternative Package Manager
```bash
# Using Yarn instead of npm
npm install -g yarn
yarn install
yarn start
```

### Method 4: Docker Installation (Guaranteed Environment)
```bash
# Create Dockerfile
cat > Dockerfile << EOF
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
EOF

# Build and run
docker build -t transit-dashboard .
docker run -p 3000:3000 transit-dashboard
```

## 🚨 **Emergency Troubleshooting**

### If Nothing Works:
```bash
# Nuclear option - reset everything
rm -rf node_modules package-lock.json yarn.lock
npm cache clean --force
npm install -g npm@latest

# Install specific working versions
npm install react@18.2.0 react-dom@18.2.0
npm install react-scripts@5.0.1
npm install recharts@2.7.2
npm install lucide-react@0.263.1
npm install

# Start with verbose logging
npm start --verbose
```

## 📊 **Health Check Script**

Save this as `health-check.js` and run with `node health-check.js`:

```javascript
const { execSync } = require('child_process');
const fs = require('fs');

console.log('🔍 Dependency Health Check\n');

// Check Node.js version
const nodeVersion = process.version;
console.log(`Node.js: ${nodeVersion}`);
if (parseInt(nodeVersion.slice(1)) < 16) {
  console.log('❌ Node.js version too old. Need 16+');
} else {
  console.log('✅ Node.js version OK');
}

// Check npm version
try {
  const npmVersion = execSync('npm --version', { encoding: 'utf8' }).trim();
  console.log(`npm: ${npmVersion}`);
  if (parseInt(npmVersion.split('.')[0]) < 8) {
    console.log('❌ npm version too old. Need 8+');
  } else {
    console.log('✅ npm version OK');
  }
} catch (e) {
  console.log('❌ npm not found');
}

// Check package.json exists
if (fs.existsSync('package.json')) {
  console.log('✅ package.json found');
} else {
  console.log('❌ package.json not found');
}

// Check node_modules
if (fs.existsSync('node_modules')) {
  console.log('✅ node_modules exists');
} else {
  console.log('⚠️  node_modules not found - run npm install');
}

console.log('\n🎯 Run: npm install && npm start');
```

## 🎯 **Success Indicators**

### ✅ **Installation Successful When You See:**
```bash
# After npm install:
added X packages in Xs
found 0 vulnerabilities

# After npm start:
Compiled successfully!
Local:            http://localhost:3000
webpack compiled with 0 errors
```

### ❌ **Red Flags to Watch For:**
- `ERESOLVE unable to resolve dependency tree`
- `peer dep missing` errors
- `gyp ERR!` messages (native compilation issues)
- `EACCES permission denied` errors

## 📞 **Getting Help**

If you encounter dependency issues:

1. **Check this guide first** ⬆️
2. **Run the health check script** 🔍
3. **Copy the EXACT error message** 📋
4. **Include your environment details** 💻
5. **Open an issue** with all the above info 🐛

Remember: Dependency issues are almost always environment-related, not code-related! 🔧