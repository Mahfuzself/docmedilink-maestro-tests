# Environment Management Guide

This guide explains how to manage different environments in your Maestro test suite for DocMediaLink.

## 📁 File Structure

```
docmedilink-maestro-tests/
├── .env                          # Your local environment (not committed)
├── .env.example                  # Template for .env
├── envs/
│   ├── dev.env                   # Development config
│   ├── staging.env               # Staging config
│   └── production.env            # Production config
├── scripts/
│   ├── run-tests.sh              # Linux/Mac test runner
│   └── run-tests.bat             # Windows test runner
└── flows/
    └── login-flow.yaml           # Sample flow with env vars
```

## 🚀 Quick Start

### 1. Setup Local Environment

```bash
# Copy the example file
cp .env.example .env

# Edit with your local values
nano .env
```

### 2. Run Tests

**On Mac/Linux:**
```bash
# Make script executable (first time only)
chmod +x scripts/run-tests.sh

# Run on dev
./scripts/run-tests.sh dev

# Run on staging
./scripts/run-tests.sh staging

# Run on production
./scripts/run-tests.sh production
```

**On Windows:**
```batch
REM Run on dev
scripts\run-tests.bat dev

REM Run on staging
scripts\run-tests.bat staging

REM Run on production
scripts\run-tests.bat production
```

## 🔧 Environment Variables

### Available Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `API_BASE_URL` | API endpoint | `http://localhost:8080` |
| `TEST_USERNAME` | Test user email | `test@docmedilink.com` |
| `TEST_PASSWORD` | Test user password | `password123` |
| `TEST_ENV` | Environment name | `dev`, `staging`, `production` |
| `APP_ID` | Application package ID | `com.docmedilink.mobile` |
| `API_TIMEOUT` | API timeout in seconds | `30` |

### Using in Flows

Reference variables in your YAML flows with `${VARIABLE_NAME}`:

```yaml
appId: ${APP_ID}
---
- launchApp

- tapOn:
    text: "Email"

- inputText: ${TEST_USERNAME}

- tapOn:
    text: "Password"

- inputText: ${TEST_PASSWORD}

- tapOn:
    text: "Login"
```

## 🌍 Environment Configurations

### Development (`envs/dev.env`)
- Local or dev server
- Dev credentials
- Longer timeouts for debugging
- Used for local testing

### Staging (`envs/staging.env`)
- Staging server URL
- Staging test credentials
- Production-like setup
- Used for pre-release testing

### Production (`envs/production.env`)
- Production API
- Real test account
- Standard timeouts
- Used for production verification

## 📝 Adding New Environments

1. Create a new file in `envs/`:
```bash
echo "API_BASE_URL=https://qa.docmedilink.com" > envs/qa.env
echo "TEST_USERNAME=qauser@docmedilink.com" >> envs/qa.env
echo "TEST_PASSWORD=qapass123" >> envs/qa.env
```

2. Use the runner script:
```bash
./scripts/run-tests.sh qa
```

## 🔐 Security Best Practices

### DO ✅
- Keep `.env` in `.gitignore` (already done)
- Use environment-specific credentials
- Rotate credentials regularly
- Use strong passwords

### DON'T ❌
- Commit `.env` to git
- Hardcode credentials in flows
- Use production credentials locally
- Share credentials in chat/email

## 🔄 CI/CD Integration

For GitHub Actions, set secrets in:
**Settings → Secrets and variables → Actions**

```
MAESTRO_CLOUD_API_KEY = xxx
TEST_USERNAME = xxx
TEST_PASSWORD = xxx
API_BASE_URL = xxx
```

Then in workflows:
```yaml
env:
  API_BASE_URL: ${{ secrets.API_BASE_URL }}
  TEST_USERNAME: ${{ secrets.TEST_USERNAME }}
  TEST_PASSWORD: ${{ secrets.TEST_PASSWORD }}
```

## 🛠️ Troubleshooting

### Variables not loading
```bash
# Check env file syntax
cat envs/dev.env

# Verify export works
source envs/dev.env
echo $API_BASE_URL
```

### Script permission denied (Mac/Linux)
```bash
chmod +x scripts/run-tests.sh
```

### Windows batch not working
- Use Command Prompt (cmd.exe) or PowerShell
- Ensure script is in correct path

## 📚 More Resources

- [Maestro Documentation](https://maestro.mobile/docs)
- [Environment Variables in Maestro](https://maestro.mobile/docs/environment-variables)
- [Flow Syntax](https://maestro.mobile/docs/flow-syntax)

## 💡 Tips

- **For local development:** Use `dev.env`
- **Before pull request:** Test on `staging.env`
- **Before release:** Verify on `production.env`
- **Create test-specific flows:** One for each major feature

## 🎯 Next Steps

1. Edit `.env` with your local configuration
2. Add your app binary to `apps/`
3. Run `./scripts/run-tests.sh dev`
4. Check `maestro-results-*.json` for results
