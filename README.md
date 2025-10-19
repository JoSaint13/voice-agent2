# VoiceFlow AI

> Empowering professionals with intelligent, privacy-first conversational productivity

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Build Status](https://img.shields.io/badge/build-passing-brightgreen.svg)](https://github.com/voiceflow-ai/agent2)

## Overview

VoiceFlow AI is a cutting-edge conversational AI platform designed to transform how knowledge workers interact with digital workflows. By providing intelligent, context-aware voice interactions, we help professionals streamline communication, automate tasks, and boost productivity.

## Features

- ✨ Privacy-first voice interaction
- 🚀 Contextual understanding of professional workflows
- 💡 Intelligent meeting note automation
- 🔒 Enterprise-grade security and data protection

## Tech Stack

**Frontend:**
- React 18
- TypeScript
- Tailwind CSS
- Zustand
- React Router v6

**Backend:**
- Next.js 14
- Node.js 20 LTS
- PostgreSQL
- Prisma ORM
- tRPC

**Deployment:**
- Vercel
- Supabase
- AWS S3

## Quick Start

### Prerequisites

```bash
node >= 18.0.0
npm >= 9.0.0
```

### Installation

```bash
# Clone the repository
git clone https://github.com/voiceflow-ai/agent2.git

# Install dependencies
cd agent2
npm install

# Set up environment variables
cp .env.example .env
# Edit .env with your configuration

# Run development server
npm run dev
```

## Project Structure

```
/
├── src/
│   ├── components/     # React components
│   ├── pages/          # Next.js pages
│   ├── utils/          # Utility functions
│   └── styles/         # CSS/styling
├── public/             # Static assets
├── tests/              # Test files
└── docs/               # Documentation
```

## Development

### Available Scripts

```bash
npm run dev         # Start development server
npm run build       # Build for production
npm run test        # Run tests
npm run lint        # Lint code
```

### Environment Variables

Required environment variables:

```env
DATABASE_URL=your_postgresql_connection_string
NEXTAUTH_SECRET=your_jwt_secret
```

## Testing

```bash
# Run unit tests
npm run test

# Run with coverage
npm run test:coverage

# Run E2E tests
npm run test:e2e
```

## Deployment

### Vercel (Recommended)

```bash
npm run build
vercel --prod
```

## Contributing

We welcome contributions! Please read our [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct and development process.

### How to Contribute

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

For support, please open an issue in the GitHub repository or email support@voiceflowai.com.

---

**Made with ❤️ by VoiceFlow AI Team**