# StreamPay Frontend

> Modern, responsive web application for StreamPay micropayment streaming platform

[![Next.js](https://img.shields.io/badge/Next.js-14-black)](https://nextjs.org)
[![React](https://img.shields.io/badge/React-18-blue)](https://reactjs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-blue)](https://typescriptlang.org)

## 📋 Overview

The StreamPay frontend is a Next.js-based web application that provides an intuitive interface for content creators and consumers. It features real-time payment streaming, content playback, and comprehensive analytics dashboards.

## 🛠️ Technology Stack

- **Framework**: Next.js 14 (App Router)
- **Language**: TypeScript
- **UI Library**: React 18
- **Styling**: Tailwind CSS
- **State Management**: Zustand
- **Form Handling**: React Hook Form + Zod
- **HTTP Client**: Axios
- **Real-time**: Socket.io-client
- **Wallet Integration**: @stellar/freighter-api
- **Media Player**: 
  - Video: Video.js
  - Audio: Howler.js
- **Charts**: Recharts
- **Animations**: Framer Motion
- **Testing**: Jest + React Testing Library
- **E2E Testing**: Playwright

## 📁 Project Structure

```
frontend/
├── src/
│   ├── app/                      # Next.js app router
│   │   ├── (auth)/              # Auth-related pages
│   │   │   ├── login/
│   │   │   └── register/
│   │   ├── (dashboard)/         # Dashboard pages
│   │   │   ├── creator/
│   │   │   ├── analytics/
│   │   │   └── earnings/
│   │   ├── (public)/            # Public pages
│   │   │   ├── explore/
│   │   │   ├── content/[id]/
│   │   │   └── creator/[id]/
│   │   ├── layout.tsx
│   │   └── page.tsx
│   │
│   ├── components/              # React components
│   │   ├── ui/                 # Base UI components
│   │   │   ├── Button.tsx
│   │   │   ├── Input.tsx
│   │   │   ├── Modal.tsx
│   │   │   └── Card.tsx
│   │   ├── layout/             # Layout components
│   │   │   ├── Header.tsx
│   │   │   ├── Sidebar.tsx
│   │   │   └── Footer.tsx
│   │   ├── content/            # Content-related components
│   │   │   ├── ContentPlayer.tsx
│   │   │   ├── ContentCard.tsx
│   │   │   └── ContentList.tsx
│   │   ├── payment/            # Payment components
│   │   │   ├── PaymentStream.tsx
│   │   │   ├── WalletConnect.tsx
│   │   │   └── TransactionHistory.tsx
│   │   └── dashboard/          # Dashboard components
│   │       ├── EarningsChart.tsx
│   │       ├── ViewsAnalytics.tsx
│   │       └── CreatorStats.tsx
│   │
│   ├── hooks/                   # Custom React hooks
│   │   ├── useAuth.ts
│   │   ├── useWallet.ts
│   │   ├── usePaymentStream.ts
│   │   ├── useContent.ts
│   │   └── useWebSocket.ts
│   │
│   ├── lib/                     # Utility libraries
│   │   ├── api/                # API client
│   │   │   ├── client.ts
│   │   │   ├── auth.ts
│   │   │   ├── content.ts
│   │   │   └── payments.ts
│   │   ├── stellar/            # Stellar utilities
│   │   │   ├── wallet.ts
│   │   │   ├── transactions.ts
│   │   │   └── contracts.ts
│   │   ├── utils/              # Helper functions
│   │   │   ├── formatters.ts
│   │   │   ├── validators.ts
│   │   │   └── helpers.ts
│   │   └── constants.ts
│   │
│   ├── stores/                  # State management
│   │   ├── authStore.ts
│   │   ├── walletStore.ts
│   │   ├── contentStore.ts
│   │   └── streamingStore.ts
│   │
│   ├── types/                   # TypeScript types
│   │   ├── auth.ts
│   │   ├── content.ts
│   │   ├── payment.ts
│   │   └── user.ts
│   │
│   └── styles/                  # Global styles
│       ├── globals.css
│       └── components.css
│
├── public/                      # Static assets
│   ├── images/
│   ├── icons/
│   └── fonts/
│
├── __tests__/                   # Test files
│   ├── unit/
│   ├── integration/
│   └── e2e/
│
├── .env.local.example
├── .eslintrc.json
├── .prettierrc
├── next.config.js
├── tailwind.config.js
├── tsconfig.json
└── package.json
```

## 🚀 Getting Started

### Prerequisites

- Node.js 18 or higher
- npm, yarn, or pnpm
- A Stellar wallet (Freighter recommended)

### Installation

1. **Navigate to the frontend directory**

```bash
cd frontend
```

2. **Install dependencies**

```bash
npm install
# or
yarn install
# or
pnpm install
```

3. **Set up environment variables**

Create a `.env.local` file in the frontend root:

```env
# API Configuration
NEXT_PUBLIC_API_URL=http://localhost:3001
NEXT_PUBLIC_API_PREFIX=api/v1

# WebSocket Configuration
NEXT_PUBLIC_WS_URL=ws://localhost:3001

# Stellar Configuration
NEXT_PUBLIC_STELLAR_NETWORK=testnet
NEXT_PUBLIC_HORIZON_URL=https://horizon-testnet.stellar.org
NEXT_PUBLIC_PASSPHRASE=Test SDF Network ; September 2015

# Contract Addresses (Update after deployment)
NEXT_PUBLIC_PAYMENT_STREAM_CONTRACT=
NEXT_PUBLIC_ESCROW_CONTRACT=

# Application Configuration
NEXT_PUBLIC_APP_NAME=StreamPay
NEXT_PUBLIC_APP_URL=http://localhost:3000

# Feature Flags
NEXT_PUBLIC_ENABLE_ANALYTICS=true
NEXT_PUBLIC_ENABLE_NOTIFICATIONS=true

# Media Configuration
NEXT_PUBLIC_MAX_UPLOAD_SIZE=100MB
NEXT_PUBLIC_ALLOWED_VIDEO_FORMATS=mp4,webm,mov
NEXT_PUBLIC_ALLOWED_AUDIO_FORMATS=mp3,wav,ogg
```

4. **Run the development server**

```bash
npm run dev
# or
yarn dev
# or
pnpm dev
```

5. **Open your browser**

Navigate to [http://localhost:3000](http://localhost:3000)

## 📦 Available Scripts

```bash
# Development
npm run dev              # Start development server
npm run build            # Build for production
npm run start            # Start production server
npm run lint             # Run ESLint
npm run format           # Format code with Prettier
npm run type-check       # Run TypeScript compiler check

# Testing
npm run test             # Run unit tests
npm run test:watch       # Run tests in watch mode
npm run test:coverage    # Run tests with coverage
npm run test:e2e         # Run E2E tests

# Code Quality
npm run lint:fix         # Fix ESLint errors
npm run format:check     # Check code formatting
npm run analyze          # Analyze bundle size
```

## 🎨 Key Features

### 1. Content Player
```typescript
// components/content/ContentPlayer.tsx
- Real-time payment streaming
- Adaptive bitrate streaming
- Picture-in-picture mode
- Playback speed control
- Keyboard shortcuts
- Progress tracking
```

### 2. Wallet Integration
```typescript
// hooks/useWallet.ts
- Freighter wallet connection
- Account balance display
- Transaction signing
- Multi-asset support
- Wallet switching
```

### 3. Payment Streaming
```typescript
// hooks/usePaymentStream.ts
- Real-time micropayments
- Payment rate calculation
- Stream status tracking
- Automatic settlements
- Transaction history
```

### 4. Creator Dashboard
```typescript
// app/(dashboard)/creator/page.tsx
- Revenue analytics
- Content performance
- Viewer demographics
- Earnings projections
- Payout management
```

## 🔌 API Integration

### API Client Setup

```typescript
// lib/api/client.ts
import axios from 'axios';

const apiClient = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Request interceptor
apiClient.interceptors.request.use((config) => {
  const token = localStorage.getItem('accessToken');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

export default apiClient;
```

### Using API Hooks

```typescript
// Example: Fetching content
import { useContent } from '@/hooks/useContent';

function ContentList() {
  const { contents, loading, error } = useContent();
  
  if (loading) return <Spinner />;
  if (error) return <Error message={error} />;
  
  return <ContentGrid contents={contents} />;
}
```

## 🌟 Stellar Integration

### Connecting Wallet

```typescript
// lib/stellar/wallet.ts
import { isConnected, getPublicKey } from '@stellar/freighter-api';

export async function connectWallet() {
  if (await isConnected()) {
    const publicKey = await getPublicKey();
    return publicKey;
  }
  throw new Error('Freighter wallet not found');
}
```

### Payment Stream

```typescript
// hooks/usePaymentStream.ts
import { useState, useEffect } from 'react';
import { useWallet } from './useWallet';

export function usePaymentStream(contentId: string) {
  const { publicKey, signTransaction } = useWallet();
  const [streamActive, setStreamActive] = useState(false);
  const [amountPaid, setAmountPaid] = useState(0);

  const startStream = async (ratePerSecond: number) => {
    // Initialize payment stream contract
    const tx = await createStreamTransaction({
      from: publicKey,
      contentId,
      ratePerSecond,
    });
    
    const signedTx = await signTransaction(tx);
    await submitTransaction(signedTx);
    
    setStreamActive(true);
  };

  const stopStream = async () => {
    // Finalize payment stream
    await finalizeStream(contentId);
    setStreamActive(false);
  };

  return { streamActive, amountPaid, startStream, stopStream };
}
```

## 🎭 Component Examples

### Content Card Component

```typescript
// components/content/ContentCard.tsx
import Image from 'next/image';
import Link from 'next/link';
import { Card } from '@/components/ui/Card';

interface ContentCardProps {
  id: string;
  title: string;
  thumbnail: string;
  creator: string;
  price: number;
  views: number;
}

export function ContentCard({ 
  id, 
  title, 
  thumbnail, 
  creator, 
  price, 
  views 
}: ContentCardProps) {
  return (
    <Link href={`/content/${id}`}>
      <Card className="hover:shadow-lg transition-shadow">
        <div className="relative aspect-video">
          <Image
            src={thumbnail}
            alt={title}
            fill
            className="object-cover rounded-t-lg"
          />
        </div>
        <div className="p-4">
          <h3 className="font-semibold text-lg truncate">{title}</h3>
          <p className="text-sm text-gray-600">{creator}</p>
          <div className="flex justify-between items-center mt-2">
            <span className="text-sm font-medium">
              {price} XLM/min
            </span>
            <span className="text-sm text-gray-500">
              {views.toLocaleString()} views
            </span>
          </div>
        </div>
      </Card>
    </Link>
  );
}
```

## 🧪 Testing

### Unit Test Example

```typescript
// __tests__/unit/components/ContentCard.test.tsx
import { render, screen } from '@testing-library/react';
import { ContentCard } from '@/components/content/ContentCard';

describe('ContentCard', () => {
  it('renders content information correctly', () => {
    render(
      <ContentCard
        id="1"
        title="Test Video"
        thumbnail="/test.jpg"
        creator="Test Creator"
        price={0.5}
        views={1000}
      />
    );

    expect(screen.getByText('Test Video')).toBeInTheDocument();
    expect(screen.getByText('Test Creator')).toBeInTheDocument();
    expect(screen.getByText('0.5 XLM/min')).toBeInTheDocument();
  });
});
```

### E2E Test Example

```typescript
// __tests__/e2e/content-playback.spec.ts
import { test, expect } from '@playwright/test';

test('should play content and process payment', async ({ page }) => {
  await page.goto('/content/123');
  
  // Connect wallet
  await page.click('[data-testid="connect-wallet"]');
  await page.click('[data-testid="freighter-option"]');
  
  // Start playback
  await page.click('[data-testid="play-button"]');
  
  // Wait for payment stream to start
  await expect(page.locator('[data-testid="stream-active"]'))
    .toBeVisible();
  
  // Verify payment updates
  await expect(page.locator('[data-testid="amount-paid"]'))
    .not.toHaveText('0 XLM');
});
```

## 🔒 Security Best Practices

1. **Never expose private keys**
   - Always use Freighter or similar wallet extensions
   - Never store keys in localStorage or code

2. **Validate user input**
   - Use Zod schemas for form validation
   - Sanitize all user-generated content

3. **Implement rate limiting**
   - Limit API calls from frontend
   - Handle 429 responses gracefully

4. **Secure API communication**
   - Always use HTTPS in production
   - Implement CSRF protection
   - Use secure HTTP-only cookies for tokens

## 🎨 Styling Guidelines

### Tailwind CSS Convention

```typescript
// Use consistent spacing and sizing
<div className="p-4 space-y-4">
  <h1 className="text-2xl font-bold">Title</h1>
  <p className="text-gray-600">Description</p>
</div>

// Responsive design
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
  {/* Content */}
</div>

// Dark mode support
<div className="bg-white dark:bg-gray-900 text-gray-900 dark:text-white">
  {/* Content */}
</div>
```

## 📱 Responsive Design

The application is fully responsive with breakpoints:
- **Mobile**: < 640px
- **Tablet**: 640px - 1024px
- **Desktop**: > 1024px

## 🚀 Deployment

### Build for Production

```bash
npm run build
```

### Deploy to Vercel (Recommended)

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel --prod
```

### Environment Variables for Production

Set these in your deployment platform:

```env
NEXT_PUBLIC_API_URL=https://api.streampay.io
NEXT_PUBLIC_WS_URL=wss://api.streampay.io
NEXT_PUBLIC_STELLAR_NETWORK=public
NEXT_PUBLIC_HORIZON_URL=https://horizon.stellar.org
```

## 🐛 Common Issues & Solutions

### Issue: Wallet connection fails
**Solution**: Ensure Freighter extension is installed and user is on correct network

### Issue: WebSocket disconnects frequently
**Solution**: Implement reconnection logic with exponential backoff

### Issue: Slow content loading
**Solution**: Implement lazy loading and optimize images with Next.js Image component

## 📚 Additional Resources

- [Next.js Documentation](https://nextjs.org/docs)
- [Stellar Documentation](https://developers.stellar.org)
- [Freighter Wallet API](https://docs.freighter.app)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)

## 🤝 Contributing

Please read the main [CONTRIBUTING.md](../CONTRIBUTING.md) for contribution guidelines.

## 📄 License

MIT License - See [LICENSE](../LICENSE)

---

**Frontend Team** | StreamPay
