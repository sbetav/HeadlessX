# 🚀 HeadlessX Deployment Guide for Render

This guide will help you deploy HeadlessX to Render using Docker.

## 📋 Prerequisites

1. **GitHub Repository**: Your HeadlessX code should be in a GitHub repository
2. **Render Account**: Sign up at [render.com](https://render.com)
3. **Domain (Optional)**: You can use Render's provided domain or connect your own

## 🚀 Step-by-Step Deployment

### Step 1: Prepare Your Repository

1. **Fork or Clone** the HeadlessX repository to your GitHub account
2. **Ensure** you have the following files in your repository root:
   - `Dockerfile.render` (created for Render optimization)
   - `render.yaml` (Render configuration)
   - `package.json`
   - `src/` directory with all source code

### Step 2: Create Render Account and Connect GitHub

1. Go to [render.com](https://render.com) and sign up
2. Connect your GitHub account
3. Authorize Render to access your repositories

### Step 3: Deploy to Render

#### Option A: Using render.yaml (Recommended)

1. **Push your code** to GitHub with the `render.yaml` file
2. **Go to Render Dashboard** → "New" → "Blueprint"
3. **Connect your repository** containing the `render.yaml` file
4. **Render will automatically** detect the configuration and deploy

#### Option B: Manual Web Service Setup

1. **Go to Render Dashboard** → "New" → "Web Service"
2. **Connect your GitHub repository**
3. **Configure the service:**
   - **Name**: `headlessx` (or your preferred name)
   - **Environment**: `Docker`
   - **Dockerfile Path**: `./Dockerfile.render`
   - **Docker Context**: `.` (root directory)
   - **Plan**: `Starter` (free tier) or `Standard` (recommended for production)

### Step 4: Configure Environment Variables

In your Render service dashboard, go to "Environment" tab and add:

```
NODE_ENV=production
PORT=3000
AUTH_TOKEN=your_secure_random_token_here
DOMAIN=your-app-name.onrender.com
SUBDOMAIN=headlessx
UV_THREADPOOL_SIZE=128
NODE_OPTIONS=--max-old-space-size=4096
```

**Important**: 
- Generate a secure `AUTH_TOKEN` (use a password generator)
- The `DOMAIN` will be provided by Render (e.g., `your-app-name.onrender.com`)

### Step 5: Deploy and Test

1. **Click "Deploy"** in your Render dashboard
2. **Wait for deployment** to complete (usually 5-10 minutes)
3. **Check the logs** for any errors
4. **Test your deployment**:

```bash
# Health check (no auth required)
curl https://your-app-name.onrender.com/api/health

# Test with authentication
curl "https://your-app-name.onrender.com/api/status?token=YOUR_AUTH_TOKEN"
```

## 🔧 Configuration Details

### Environment Variables Explained

| Variable | Description | Required | Default |
|----------|-------------|----------|---------|
| `AUTH_TOKEN` | API authentication token | ✅ | - |
| `DOMAIN` | Your domain (auto-set by Render) | ✅ | - |
| `SUBDOMAIN` | Subdomain for the service | ❌ | `headlessx` |
| `PORT` | Application port | ❌ | `3000` |
| `NODE_ENV` | Environment mode | ❌ | `production` |
| `UV_THREADPOOL_SIZE` | Thread pool size | ❌ | `128` |
| `NODE_OPTIONS` | Node.js options | ❌ | `--max-old-space-size=4096` |

### Render Service Configuration

- **Plan**: Start with `Starter` (free) for testing, upgrade to `Standard` for production
- **Region**: Choose closest to your users
- **Auto-Deploy**: Enable to automatically deploy on git push
- **Health Check**: `/api/health` endpoint

## 🌐 Accessing Your Deployed Service

Once deployed, your HeadlessX service will be available at:

- **Website**: `https://your-app-name.onrender.com`
- **API Health**: `https://your-app-name.onrender.com/api/health`
- **API Endpoints**: `https://your-app-name.onrender.com/api/*`

## 🧪 Testing Your Deployment

### Basic Health Check
```bash
curl https://your-app-name.onrender.com/api/health
```

### API Test with Authentication
```bash
curl -X POST "https://your-app-name.onrender.com/api/render?token=YOUR_AUTH_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com",
    "timeout": 30000
  }'
```

### Test Anti-Detection Features
```bash
curl -X POST "https://your-app-name.onrender.com/api/test-fingerprint?token=YOUR_AUTH_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "profile": "desktop-chrome",
    "testCanvas": true,
    "testWebGL": true
  }'
```

## 🔧 Troubleshooting

### Common Issues

1. **Build Fails**: Check that all files are in the repository
2. **Service Won't Start**: Check environment variables are set correctly
3. **Health Check Fails**: Ensure `AUTH_TOKEN` is set and valid
4. **Out of Memory**: Upgrade to a higher plan or adjust `NODE_OPTIONS`

### Checking Logs

1. Go to your service dashboard on Render
2. Click on "Logs" tab
3. Look for error messages or warnings

### Performance Optimization

For production use, consider:
- **Upgrading to Standard plan** for better performance
- **Setting up custom domain** for better branding
- **Configuring CDN** for static assets
- **Monitoring** with Render's built-in metrics

## 🔄 Updates and Maintenance

### Updating Your Service

1. **Push changes** to your GitHub repository
2. **Render will auto-deploy** if auto-deploy is enabled
3. **Or manually deploy** from the Render dashboard

### Monitoring

- **Health Checks**: Monitor `/api/health` endpoint
- **Logs**: Check Render dashboard logs regularly
- **Metrics**: Use Render's built-in monitoring

## 🎯 Next Steps

After successful deployment:

1. **Test all API endpoints** to ensure functionality
2. **Set up monitoring** and alerting
3. **Configure custom domain** (optional)
4. **Set up CI/CD** for automated deployments
5. **Scale up** if needed for production use

## 📞 Support

- **Render Documentation**: [render.com/docs](https://render.com/docs)
- **HeadlessX Issues**: [GitHub Issues](https://github.com/saifyxpro/headlessx/issues)
- **Community**: [GitHub Discussions](https://github.com/saifyxpro/headlessx/discussions)

---

**🎉 Congratulations!** Your HeadlessX service is now deployed on Render and ready to use!
