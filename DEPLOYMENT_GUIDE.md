# Deployment Guide - Adaptive AI Sales Agent

## 📋 Project Structure

```
hackindia-hack/
├── backend/                          # Python FastAPI Backend
│   ├── main.py                       # FastAPI entry point
│   ├── ai_service.py                 # AI service logic
│   ├── pattern_detector.py           # Memory pattern detection
│   ├── memory_engine.py              # Database operations
│   ├── models.py                     # Pydantic models
│   └── requirements.txt              # Python dependencies
├── frontend/                         # React Frontend
│   ├── app.jsx                       # Main React component
│   ├── index.html                    # HTML entry point
│   └── [build output]/               # Production build
├── .env.example                      # Environment template
├── Dockerfile                        # Container image
├── docker-compose.yml                # Local development
├── vercel.json                       # Vercel deployment
└── requirements.txt                  # Root-level dependencies
```

## 🚀 Deployment Options

### Option 1: Local Development

```bash
# 1. Setup environment
cp .env.example .env
# Edit .env and add your GROQ_API_KEY

# 2. Install dependencies
pip install -r backend/requirements.txt

# 3. Run backend
cd backend
python -m uvicorn main:app --reload --host 0.0.0.0 --port 8000

# 4. Access at http://localhost:8000
```

### Option 2: Docker (Local)

```bash
# 1. Build and run with Docker Compose
cp .env.example .env
# Edit .env with your GROQ_API_KEY

docker-compose up --build

# 2. Access at http://localhost:8000
```

### Option 3: Vercel (Production)

```bash
# 1. Install Vercel CLI
npm i -g vercel

# 2. Deploy
vercel --env-file .env

# 3. Set environment variables in Vercel dashboard
# Add GROQ_API_KEY in Settings > Environment Variables
```

### Option 4: Railway / Render

```bash
# 1. Push code to GitHub
git push origin deploy-ready

# 2. Connect repository to Railway/Render
# - Select this repository
# - Set Build Command: pip install -r backend/requirements.txt
# - Set Start Command: python -m uvicorn backend.main:app --host 0.0.0.0 --port 8000
# - Add environment variable: GROQ_API_KEY

# 3. Deploy automatically
```

### Option 5: AWS / Google Cloud / Azure

```bash
# 1. Using Dockerfile
docker build -t hackindia-backend .
docker run -p 8000:8000 -e GROQ_API_KEY=your_key hackindia-backend

# 2. Push to container registry and deploy
```

## 🔧 Environment Variables

Required variables (see `.env.example`):

- `GROQ_API_KEY`: Your Groq API key (required)
- `BACKEND_PORT`: Port for backend (default: 8000)
- `BACKEND_HOST`: Host binding (default: 0.0.0.0)
- `FRONTEND_PORT`: Port for frontend (default: 3000)
- `DATABASE_PATH`: Path to SQLite database

## 📦 Key Dependencies

**Backend (Python)**
- `fastapi==0.110.0` - Web framework
- `uvicorn==0.29.0` - ASGI server
- `groq==0.4.2` - Groq API client
- `pydantic==2.6.4` - Data validation
- `python-dotenv==1.0.1` - Environment management

**Frontend (React)**
- React 18
- TailwindCSS
- (See frontend dependencies in package.json)

## 🔐 Security Checklist

- [ ] Never commit `.env` file to version control
- [ ] Use `.env.example` as template
- [ ] Enable HTTPS in production
- [ ] Set CORS properly for frontend domain
- [ ] Use secrets management for API keys
- [ ] Implement rate limiting on endpoints
- [ ] Validate all user inputs
- [ ] Use environment-specific configurations

## 📊 Monitoring & Logs

```bash
# Docker logs
docker-compose logs -f backend

# Local logs
# Check console output when running locally

# Production
# Use provider's logging dashboard (Vercel, Railway, etc.)
```

## 🐛 Troubleshooting

### Port already in use
```bash
# Change port in command
python -m uvicorn backend.main:app --port 8001
```

### GROQ_API_KEY not found
```bash
# Verify .env file exists and has correct key
cat .env

# For Docker
docker-compose config | grep GROQ
```

### Database errors
```bash
# Clear database and restart
rm backend/memories.db backend/hindsight_memory.db
# Restart application
```

### CORS errors
- Check backend CORS configuration in `main.py`
- Verify frontend is accessing correct API URL

## 🚢 Production Best Practices

1. **Use environment-specific configs**
   - Separate development and production settings
   - Use feature flags for testing

2. **Database**
   - Regular backups for production database
   - Consider upgrading to PostgreSQL for production

3. **API Rate Limiting**
   - Implement rate limiting for Groq API calls
   - Add caching where possible

4. **Frontend Build**
   - Minify and optimize assets
   - Use CDN for static files

5. **Monitoring**
   - Set up error tracking (Sentry, etc.)
   - Monitor API response times
   - Track Groq API usage and costs

## 📝 Notes

- Current homepage is configured at: https://hackindia-hack.vercel.app
- Backend serves frontend static files natively
- SQLite database is persistent in container volumes

## Support

For issues or questions, check the main README.md or create an issue on GitHub.
