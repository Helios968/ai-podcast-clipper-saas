# AI Podcast Clipper Setup Guide

## Prerequisites

1. **Python 3.12** - Download from [python.org](https://www.python.org/downloads/)
2. **Node.js 18+** - Download from [nodejs.org](https://nodejs.org/)
3. **PostgreSQL** - For the database
4. **AWS Account** - For S3 storage
5. **Stripe Account** - For payments
6. **Google AI Studio** - For Gemini API
7. **Modal Account** - For serverless GPU processing

## Quick Setup

### 1. Install Frontend Dependencies
```bash
cd ai-podcast-clipper-frontend
npm install
```

### 2. Setup Backend
```bash
cd ai-podcast-clipper-backend
pip install -r requirements.txt
```

### 3. Clone LR-ASD Repository
```bash
cd ai-podcast-clipper-backend
git clone https://github.com/Junhua-Liao/LR-ASD.git asd
```

### 4. Configure Environment Variables

Copy the `.env` file in the frontend directory and fill in your credentials:

- **Database**: Set up PostgreSQL and update `DATABASE_URL`
- **AWS S3**: Create bucket and IAM user with proper permissions
- **Stripe**: Create products and get API keys
- **Gemini**: Get API key from Google AI Studio
- **Modal**: Setup account and get endpoint URL

### 5. Database Setup
```bash
cd ai-podcast-clipper-frontend
npx prisma db push
```

### 6. Modal Setup
```bash
cd ai-podcast-clipper-backend
modal setup
modal deploy main.py
```

### 7. Start Development Servers

Terminal 1 - Frontend:
```bash
cd ai-podcast-clipper-frontend
npm run dev
```

Terminal 2 - Queue Processing:
```bash
cd ai-podcast-clipper-frontend
npm run inngest-dev
```

## AWS S3 Configuration

### CORS Policy
```json
[
    {
        "AllowedHeaders": [
            "Content-Type",
            "Content-Length",
            "Authorization"
        ],
        "AllowedMethods": [
            "PUT"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": [
            "ETag"
        ],
        "MaxAgeSeconds": 3600
    }
]
```

### IAM Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": "arn:aws:s3:::your-bucket-name"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::your-bucket-name/*"
        }
    ]
}
```

## Features

- 🎬 Auto-detection of viral moments in podcasts
- 🔊 Automatically added subtitles on clips
- 📝 Transcription with WhisperX
- 🎯 Active speaker detection for video cropping
- 📱 Clips optimized for vertical platforms
- 🧠 LLM-powered viral moment identification
- 💳 Credit-based payment system
- 👤 User authentication
- 📊 Queue system for handling user load

## Architecture

- **Frontend**: Next.js 15, React, TypeScript, Tailwind CSS, ShadCN
- **Backend**: Python, FastAPI, Modal (serverless GPU)
- **Database**: PostgreSQL with Prisma ORM
- **Queue**: Inngest for background processing
- **Storage**: AWS S3
- **Payments**: Stripe
- **AI**: WhisperX, Gemini 2.5, LR-ASD

## Troubleshooting

1. **Database Connection**: Ensure PostgreSQL is running and connection string is correct
2. **Modal Issues**: Make sure you're authenticated with `modal setup`
3. **S3 Permissions**: Verify CORS and IAM policies are correctly configured
4. **Stripe Webhooks**: Use ngrok or similar for local webhook testing

For more details, see the main README.md file.