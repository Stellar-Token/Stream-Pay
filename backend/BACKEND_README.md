# StreamPay Backend

> Robust NestJS backend API for StreamPay micropayment streaming platform

[![NestJS](https://img.shields.io/badge/NestJS-10-red)](https://nestjs.com)
[![Node.js](https://img.shields.io/badge/Node.js-18+-green)](https://nodejs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-blue)](https://typescriptlang.org)

## 📋 Overview

The StreamPay backend is a NestJS-based REST API that handles authentication, content management, payment processing, and real-time streaming sessions. It integrates with the Stellar blockchain for payment settlements and provides comprehensive analytics.

## 🛠️ Technology Stack

- **Framework**: NestJS 10
- **Language**: TypeScript
- **Runtime**: Node.js 18+
- **Database**: PostgreSQL 14+
- **ORM**: TypeORM / Prisma
- **Cache**: Redis
- **Queue**: Bull (Redis-based)
- **Authentication**: Passport.js + JWT
- **Validation**: class-validator + class-transformer
- **Real-time**: Socket.io
- **Stellar SDK**: @stellar/stellar-sdk
- **File Storage**: AWS S3 / IPFS
- **API Documentation**: Swagger/OpenAPI
- **Testing**: Jest
- **Logging**: Winston

## 📁 Project Structure

```
backend/
├── src/
│   ├── modules/                 # Feature modules
│   │   ├── auth/               # Authentication module
│   │   │   ├── controllers/
│   │   │   ├── services/
│   │   │   ├── strategies/
│   │   │   ├── guards/
│   │   │   ├── dto/
│   │   │   └── auth.module.ts
│   │   │
│   │   ├── users/              # User management
│   │   │   ├── controllers/
│   │   │   ├── services/
│   │   │   ├── entities/
│   │   │   ├── dto/
│   │   │   └── users.module.ts
│   │   │
│   │   ├── content/            # Content management
│   │   │   ├── controllers/
│   │   │   ├── services/
│   │   │   ├── entities/
│   │   │   ├── dto/
│   │   │   └── content.module.ts
│   │   │
│   │   ├── payments/           # Payment processing
│   │   │   ├── controllers/
│   │   │   ├── services/
│   │   │   ├── entities/
│   │   │   ├── dto/
│   │   │   ├── processors/
│   │   │   └── payments.module.ts
│   │   │
│   │   ├── streaming/          # Streaming sessions
│   │   │   ├── controllers/
│   │   │   ├── services/
│   │   │   ├── gateways/
│   │   │   ├── entities/
│   │   │   ├── dto/
│   │   │   └── streaming.module.ts
│   │   │
│   │   ├── analytics/          # Analytics & reporting
│   │   │   ├── controllers/
│   │   │   ├── services/
│   │   │   ├── entities/
│   │   │   └── analytics.module.ts
│   │   │
│   │   └── stellar/            # Stellar integration
│   │       ├── services/
│   │       ├── contracts/
│   │       ├── dto/
│   │       └── stellar.module.ts
│   │
│   ├── common/                  # Shared utilities
│   │   ├── decorators/
│   │   ├── filters/
│   │   ├── guards/
│   │   ├── interceptors/
│   │   ├── pipes/
│   │   ├── middleware/
│   │   └── utils/
│   │
│   ├── config/                  # Configuration
│   │   ├── database.config.ts
│   │   ├── redis.config.ts
│   │   ├── jwt.config.ts
│   │   ├── stellar.config.ts
│   │   └── app.config.ts
│   │
│   ├── database/               # Database related
│   │   ├── migrations/
│   │   ├── seeds/
│   │   └── database.module.ts
│   │
│   ├── main.ts                 # Application entry point
│   └── app.module.ts           # Root module
│
├── test/                       # E2E tests
│   ├── auth.e2e-spec.ts
│   ├── content.e2e-spec.ts
│   └── payments.e2e-spec.ts
│
├── .env.example
├── .eslintrc.js
├── .prettierrc
├── nest-cli.json
├── tsconfig.json
└── package.json
```

## 🚀 Getting Started

### Prerequisites

- Node.js 18 or higher
- PostgreSQL 14 or higher
- Redis 7 or higher
- npm or yarn

### Installation

1. **Navigate to the backend directory**

```bash
cd backend
```

2. **Install dependencies**

```bash
npm install
# or
yarn install
```

3. **Set up environment variables**

Create a `.env` file in the backend root:

```env
# Application
NODE_ENV=development
PORT=3001
API_PREFIX=api/v1
CORS_ORIGIN=http://localhost:3000

# Database
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_USER=streampay
DATABASE_PASSWORD=your_secure_password
DATABASE_NAME=streampay_db
DATABASE_SYNC=false
DATABASE_LOGGING=true

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=
REDIS_DB=0

# JWT
JWT_SECRET=your_super_secret_jwt_key_change_this_in_production
JWT_EXPIRATION=7d
JWT_REFRESH_SECRET=your_refresh_secret_key
JWT_REFRESH_EXPIRATION=30d

# Stellar Network
STELLAR_NETWORK=testnet
STELLAR_HORIZON_URL=https://horizon-testnet.stellar.org
STELLAR_PASSPHRASE=Test SDF Network ; September 2015
STELLAR_MASTER_PUBLIC_KEY=
STELLAR_MASTER_SECRET_KEY=

# Smart Contracts
PAYMENT_STREAM_CONTRACT_ID=
ESCROW_CONTRACT_ID=

# AWS S3 (Optional)
AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
S3_BUCKET_NAME=streampay-content
S3_UPLOAD_EXPIRATION=3600

# IPFS (Optional)
IPFS_API_URL=https://ipfs.infura.io:5001
IPFS_PROJECT_ID=
IPFS_PROJECT_SECRET=

# Rate Limiting
THROTTLE_TTL=60
THROTTLE_LIMIT=100

# Email (Optional)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=
SMTP_PASSWORD=
EMAIL_FROM=noreply@streampay.io

# Monitoring
SENTRY_DSN=
LOG_LEVEL=debug
```

4. **Set up the database**

```bash
# Create database
createdb streampay_db

# Run migrations
npm run migration:run

# Seed database (optional)
npm run seed
```

5. **Start the development server**

```bash
npm run start:dev
```

6. **Access the API**

- API: http://localhost:3001
- API Docs: http://localhost:3001/api/docs

## 📦 Available Scripts

```bash
# Development
npm run start              # Start production server
npm run start:dev          # Start development server with watch
npm run start:debug        # Start with debugging
npm run start:prod         # Start production build

# Building
npm run build              # Build for production
npm run prebuild           # Clean build directory

# Database
npm run migration:create   # Create new migration
npm run migration:run      # Run migrations
npm run migration:revert   # Revert last migration
npm run seed               # Seed database

# Testing
npm run test               # Run unit tests
npm run test:watch         # Run tests in watch mode
npm run test:cov           # Run tests with coverage
npm run test:e2e           # Run E2E tests

# Code Quality
npm run lint               # Run ESLint
npm run format             # Format code with Prettier
npm run format:check       # Check code formatting
```

## 🏗️ Module Overview

### 1. Auth Module

Handles user authentication and authorization.

```typescript
// src/modules/auth/controllers/auth.controller.ts
@Controller('auth')
export class AuthController {
  constructor(private readonly authService: AuthService) {}

  @Post('register')
  async register(@Body() registerDto: RegisterDto) {
    return this.authService.register(registerDto);
  }

  @Post('login')
  async login(@Body() loginDto: LoginDto) {
    return this.authService.login(loginDto);
  }

  @UseGuards(JwtAuthGuard)
  @Get('profile')
  getProfile(@CurrentUser() user: User) {
    return user;
  }

  @Post('refresh')
  async refresh(@Body() refreshDto: RefreshTokenDto) {
    return this.authService.refreshToken(refreshDto);
  }
}
```

### 2. Content Module

Manages content creation, storage, and retrieval.

```typescript
// src/modules/content/services/content.service.ts
@Injectable()
export class ContentService {
  constructor(
    @InjectRepository(Content)
    private contentRepository: Repository<Content>,
    private s3Service: S3Service,
    private stellarService: StellarService,
  ) {}

  async create(createContentDto: CreateContentDto, file: Express.Multer.File) {
    // Upload to S3
    const fileUrl = await this.s3Service.upload(file);
    
    // Create content record
    const content = this.contentRepository.create({
      ...createContentDto,
      fileUrl,
      status: ContentStatus.PROCESSING,
    });
    
    await this.contentRepository.save(content);
    
    // Queue for processing
    await this.queueContent(content.id);
    
    return content;
  }

  async findAll(query: ContentQueryDto) {
    const { page = 1, limit = 20, category, search } = query;
    
    const qb = this.contentRepository.createQueryBuilder('content');
    
    if (category) {
      qb.andWhere('content.category = :category', { category });
    }
    
    if (search) {
      qb.andWhere('content.title ILIKE :search', { search: `%${search}%` });
    }
    
    qb.skip((page - 1) * limit)
      .take(limit)
      .orderBy('content.createdAt', 'DESC');
    
    return qb.getManyAndCount();
  }
}
```

### 3. Payments Module

Processes payments and manages transactions.

```typescript
// src/modules/payments/services/payment.service.ts
@Injectable()
export class PaymentService {
  constructor(
    @InjectRepository(Payment)
    private paymentRepository: Repository<Payment>,
    private stellarService: StellarService,
    @InjectQueue('payments')
    private paymentQueue: Queue,
  ) {}

  async initiateStream(
    userId: string,
    contentId: string,
    ratePerSecond: number,
  ) {
    // Create payment stream contract call
    const transaction = await this.stellarService.createStreamTransaction({
      userId,
      contentId,
      ratePerSecond,
    });
    
    // Store payment record
    const payment = this.paymentRepository.create({
      userId,
      contentId,
      ratePerSecond,
      status: PaymentStatus.PENDING,
      transactionHash: transaction.hash,
    });
    
    await this.paymentRepository.save(payment);
    
    // Queue for monitoring
    await this.paymentQueue.add('monitor-stream', {
      paymentId: payment.id,
    });
    
    return payment;
  }

  async processPayment(paymentId: string, duration: number) {
    const payment = await this.paymentRepository.findOne({
      where: { id: paymentId },
    });
    
    const amount = payment.ratePerSecond * duration;
    
    // Submit payment to Stellar
    await this.stellarService.submitPayment({
      from: payment.userId,
      to: payment.creatorId,
      amount,
    });
    
    // Update payment record
    payment.amount = amount;
    payment.duration = duration;
    payment.status = PaymentStatus.COMPLETED;
    
    await this.paymentRepository.save(payment);
    
    return payment;
  }
}
```

### 4. Streaming Module

Handles real-time streaming sessions via WebSocket.

```typescript
// src/modules/streaming/gateways/streaming.gateway.ts
@WebSocketGateway({
  cors: {
    origin: process.env.CORS_ORIGIN,
  },
})
export class StreamingGateway {
  @WebSocketServer()
  server: Server;

  constructor(
    private streamingService: StreamingService,
    private paymentService: PaymentService,
  ) {}

  @SubscribeMessage('startStream')
  async handleStartStream(
    @ConnectedSocket() client: Socket,
    @MessageBody() data: StartStreamDto,
  ) {
    const { contentId, userId } = data;
    
    // Create streaming session
    const session = await this.streamingService.createSession({
      contentId,
      userId,
      socketId: client.id,
    });
    
    // Initiate payment stream
    const payment = await this.paymentService.initiateStream(
      userId,
      contentId,
      data.ratePerSecond,
    );
    
    // Start tracking
    this.trackSession(session.id, client);
    
    return { sessionId: session.id, paymentId: payment.id };
  }

  @SubscribeMessage('stopStream')
  async handleStopStream(
    @ConnectedSocket() client: Socket,
    @MessageBody() data: StopStreamDto,
  ) {
    const { sessionId } = data;
    
    // Finalize session
    const session = await this.streamingService.finalizeSession(sessionId);
    
    // Process final payment
    await this.paymentService.processPayment(
      session.paymentId,
      session.duration,
    );
    
    return { success: true, duration: session.duration };
  }

  @SubscribeMessage('heartbeat')
  async handleHeartbeat(
    @ConnectedSocket() client: Socket,
    @MessageBody() data: HeartbeatDto,
  ) {
    await this.streamingService.updateHeartbeat(data.sessionId);
    return { timestamp: Date.now() };
  }
}
```

## 🌐 Stellar Integration

### Stellar Service

```typescript
// src/modules/stellar/services/stellar.service.ts
@Injectable()
export class StellarService {
  private server: Server;
  private masterKeypair: Keypair;

  constructor(private configService: ConfigService) {
    const horizonUrl = this.configService.get('STELLAR_HORIZON_URL');
    this.server = new Server(horizonUrl);
    
    const masterSecret = this.configService.get('STELLAR_MASTER_SECRET_KEY');
    this.masterKeypair = Keypair.fromSecret(masterSecret);
  }

  async createAccount(publicKey: string, startingBalance: string = '2') {
    const account = await this.server.loadAccount(
      this.masterKeypair.publicKey(),
    );

    const transaction = new TransactionBuilder(account, {
      fee: BASE_FEE,
      networkPassphrase: Networks.TESTNET,
    })
      .addOperation(
        Operation.createAccount({
          destination: publicKey,
          startingBalance,
        }),
      )
      .setTimeout(180)
      .build();

    transaction.sign(this.masterKeypair);
    return await this.server.submitTransaction(transaction);
  }

  async createStreamTransaction(params: CreateStreamParams) {
    const { userId, contentId, ratePerSecond } = params;
    
    const account = await this.server.loadAccount(userId);
    
    const contract = new Contract(
      this.configService.get('PAYMENT_STREAM_CONTRACT_ID'),
    );

    const transaction = new TransactionBuilder(account, {
      fee: BASE_FEE,
      networkPassphrase: Networks.TESTNET,
    })
      .addOperation(
        contract.call(
          'initialize_stream',
          xdr.ScVal.scvString(contentId),
          xdr.ScVal.scvU64(new BigNumber(ratePerSecond)),
        ),
      )
      .setTimeout(180)
      .build();

    return transaction;
  }

  async submitPayment(params: SubmitPaymentParams) {
    const { from, to, amount } = params;
    
    const sourceAccount = await this.server.loadAccount(from);
    
    const transaction = new TransactionBuilder(sourceAccount, {
      fee: BASE_FEE,
      networkPassphrase: Networks.TESTNET,
    })
      .addOperation(
        Operation.payment({
          destination: to,
          asset: Asset.native(),
          amount: amount.toString(),
        }),
      )
      .setTimeout(180)
      .build();

    return await this.server.submitTransaction(transaction);
  }

  async getBalance(publicKey: string) {
    const account = await this.server.loadAccount(publicKey);
    return account.balances;
  }

  async getTransactionHistory(publicKey: string, limit: number = 10) {
    const transactions = await this.server
      .transactions()
      .forAccount(publicKey)
      .limit(limit)
      .order('desc')
      .call();
    
    return transactions.records;
  }
}
```

## 🔐 Security Implementation

### JWT Strategy

```typescript
// src/modules/auth/strategies/jwt.strategy.ts
@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy) {
  constructor(
    private configService: ConfigService,
    private usersService: UsersService,
  ) {
    super({
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
      ignoreExpiration: false,
      secretOrKey: configService.get('JWT_SECRET'),
    });
  }

  async validate(payload: JwtPayload) {
    const user = await this.usersService.findById(payload.sub);
    
    if (!user) {
      throw new UnauthorizedException();
    }
    
    return user;
  }
}
```

### Guards

```typescript
// src/common/guards/roles.guard.ts
@Injectable()
export class RolesGuard implements CanActivate {
  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const requiredRoles = this.reflector.getAllAndOverride<Role[]>('roles', [
      context.getHandler(),
      context.getClass(),
    ]);

    if (!requiredRoles) {
      return true;
    }

    const { user } = context.switchToHttp().getRequest();
    return requiredRoles.some((role) => user.roles?.includes(role));
  }
}
```

## 📊 API Documentation

API documentation is automatically generated using Swagger and available at `/api/docs`.

### Example Swagger Decorator

```typescript
@ApiTags('content')
@Controller('content')
export class ContentController {
  @Post()
  @UseGuards(JwtAuthGuard)
  @ApiOperation({ summary: 'Create new content' })
  @ApiResponse({ status: 201, description: 'Content created successfully' })
  @ApiResponse({ status: 400, description: 'Bad request' })
  @ApiConsumes('multipart/form-data')
  @UseInterceptors(FileInterceptor('file'))
  async create(
    @Body() createContentDto: CreateContentDto,
    @UploadedFile() file: Express.Multer.File,
    @CurrentUser() user: User,
  ) {
    return this.contentService.create(createContentDto, file, user.id);
  }
}
```

## 🧪 Testing

### Unit Test Example

```typescript
// src/modules/payments/services/payment.service.spec.ts
describe('PaymentService', () => {
  let service: PaymentService;
  let repository: Repository<Payment>;
  let stellarService: StellarService;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        PaymentService,
        {
          provide: getRepositoryToken(Payment),
          useValue: mockRepository,
        },
        {
          provide: StellarService,
          useValue: mockStellarService,
        },
      ],
    }).compile();

    service = module.get<PaymentService>(PaymentService);
    repository = module.get(getRepositoryToken(Payment));
    stellarService = module.get<StellarService>(StellarService);
  });

  it('should initiate payment stream', async () => {
    const result = await service.initiateStream('user1', 'content1', 0.001);
    
    expect(result).toBeDefined();
    expect(result.status).toBe(PaymentStatus.PENDING);
    expect(stellarService.createStreamTransaction).toHaveBeenCalled();
  });
});
```

### E2E Test Example

```typescript
// test/auth.e2e-spec.ts
describe('AuthController (e2e)', () => {
  let app: INestApplication;

  beforeEach(async () => {
    const moduleFixture: TestingModule = await Test.createTestingModule({
      imports: [AppModule],
    }).compile();

    app = moduleFixture.createNestApplication();
    await app.init();
  });

  it('/auth/register (POST)', () => {
    return request(app.getHttpServer())
      .post('/auth/register')
      .send({
        email: 'test@example.com',
        password: 'Password123!',
        username: 'testuser',
      })
      .expect(201)
      .expect((res) => {
        expect(res.body).toHaveProperty('accessToken');
      });
  });
});
```

## 📈 Performance Optimization

### Caching Strategy

```typescript
// src/modules/content/services/content.service.ts
@Injectable()
export class ContentService {
  constructor(
    @Inject(CACHE_MANAGER)
    private cacheManager: Cache,
  ) {}

  async findById(id: string) {
    const cacheKey = `content:${id}`;
    
    // Try cache first
    const cached = await this.cacheManager.get(cacheKey);
    if (cached) {
      return cached;
    }
    
    // Fetch from database
    const content = await this.contentRepository.findOne({
      where: { id },
    });
    
    // Cache for 5 minutes
    await this.cacheManager.set(cacheKey, content, 300);
    
    return content;
  }
}
```

### Database Query Optimization

```typescript
// Use query builder for complex queries
const contents = await this.contentRepository
  .createQueryBuilder('content')
  .leftJoinAndSelect('content.creator', 'creator')
  .leftJoinAndSelect('content.category', 'category')
  .where('content.status = :status', { status: 'published' })
  .andWhere('content.views > :minViews', { minViews: 100 })
  .orderBy('content.createdAt', 'DESC')
  .take(10)
  .getMany();
```

## 🚀 Deployment

### Docker Deployment

```dockerfile
# Dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .
RUN npm run build

EXPOSE 3001

CMD ["node", "dist/main"]
```

### Environment Configuration

For production, ensure:
- Use strong JWT secrets
- Enable HTTPS
- Set NODE_ENV=production
- Configure proper CORS origins
- Use production Stellar network
- Enable rate limiting
- Set up monitoring and logging

## 🐛 Common Issues & Solutions

### Issue: Database connection fails
**Solution**: Verify PostgreSQL is running and credentials are correct

### Issue: Redis connection timeout
**Solution**: Check Redis server status and network connectivity

### Issue: Stellar transaction fails
**Solution**: Verify network is correct (testnet/mainnet) and account has sufficient XLM

## 📚 Additional Resources

- [NestJS Documentation](https://docs.nestjs.com)
- [TypeORM Documentation](https://typeorm.io)
- [Stellar SDK Documentation](https://stellar.github.io/js-stellar-sdk/)
- [Bull Queue Documentation](https://docs.bullmq.io)

## 🤝 Contributing

Please read the main [CONTRIBUTING.md](../CONTRIBUTING.md) for contribution guidelines.

## 📄 License

MIT License - See [LICENSE](../LICENSE)

---

**Backend Team** | StreamPay
