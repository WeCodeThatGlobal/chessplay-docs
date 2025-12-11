# External API Documentation

API documentation for clients to integrate with our platform.

## What's Inside

- Authentication guide
- Quick start tutorial
- 19 API endpoints (Students, Classrooms, Batches)
- Error handling guide
- Code examples (Python, JavaScript, cURL)

## Preview Locally

```bash
npm install -g mintlify
cd docs
mintlify dev
```

Opens at `http://localhost:3000`

## Deploy

1. Push to GitHub
2. Sign up at [mintlify.com](https://mintlify.com)
3. Connect repo, select `docs` folder
4. Deploy

Live at: `https://your-org.mintlify.app`

See [DEPLOYMENT.md](./DEPLOYMENT.md) for details.

## API Endpoints

**Students** (5 endpoints)
- List, Create, Get, Update, Delete

**Classrooms** (7 endpoints)
- List, Create, Get, Update, Delete, Add Student, Remove Student

**Batches** (7 endpoints)
- List, Create, Get, Update, Delete, Add Student, Remove Student

## Authentication

All endpoints need JWT token:

```bash
# Get token
curl -X POST https://api.chessplay.io/api/token/ \
  -d '{"username": "user", "password": "pass"}'

# Use token
curl -X GET https://api.chessplay.io/api/v1/students/ \
  -H "Authorization: Bearer YOUR_TOKEN"
```

## Files

```
docs/
├── mint.json              # Config
├── introduction.mdx       # Home
├── authentication.mdx     # Auth guide
├── quickstart.mdx        # Tutorial
├── errors.mdx            # Errors
└── api-reference/        # All endpoints
```

## Support

Email: support@wecodethat.com
