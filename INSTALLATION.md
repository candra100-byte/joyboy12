# Panduan Instalasi

Panduan langkah demi langkah untuk menginstal dan menjalankan Sistem Monitoring Imunisasi Lombok Tengah di Visual Studio Code.

## Prasyarat Sistem

### Software yang Diperlukan
- **Node.js 18.x** atau lebih baru
- **PostgreSQL 12.x** atau lebih baru  
- **Git** untuk version control
- **Visual Studio Code** (recommended)

### Ekstensi VS Code yang Direkomendasikan
Ekstensi ini akan terinstal otomatis ketika Anda membuka proyek:
- TypeScript and JavaScript Language Features
- Tailwind CSS IntelliSense
- Prettier - Code formatter
- ESLint
- Auto Rename Tag
- Path Intellisense

## Instalasi

### 1. Clone Repository

```bash
git clone https://github.com/your-username/immunization-monitoring-system.git
cd immunization-monitoring-system
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Setup Database

#### Install PostgreSQL (jika belum ada)

**Windows:**
```bash
# Download dari https://www.postgresql.org/download/windows/
# Atau gunakan Chocolatey
choco install postgresql
```

**macOS:**
```bash
# Menggunakan Homebrew
brew install postgresql
brew services start postgresql
```

**Linux (Ubuntu/Debian):**
```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

#### Konfigurasi Database

```bash
# Login ke PostgreSQL
sudo -u postgres psql

# Buat database dan user
CREATE DATABASE immunization_db;
CREATE USER immunization_user WITH PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE immunization_db TO immunization_user;
ALTER USER immunization_user CREATEDB;
\q
```

### 4. Konfigurasi Environment Variables

```bash
# Copy file template
cp .env.example .env
```

Edit file `.env` dengan konfigurasi yang sesuai:

```env
# Database Configuration
DATABASE_URL=postgresql://immunization_user:your_password@localhost:5432/immunization_db
PGHOST=localhost
PGPORT=5432
PGUSER=immunization_user
PGPASSWORD=your_password
PGDATABASE=immunization_db

# Session Configuration (generate random string untuk production)
SESSION_SECRET=your-super-secret-session-key-here-change-this-in-production

# Replit OAuth Configuration (untuk authentication)
REPL_ID=your-replit-app-id
ISSUER_URL=https://replit.com/oidc
REPLIT_DOMAINS=localhost:5000

# Development Configuration
NODE_ENV=development
PORT=5000
```

### 5. Setup Database Schema

```bash
# Push schema ke database
npm run db:push
```

### 6. Verifikasi Instalasi

```bash
# Jalankan type checking
npm run check

# Start development server
npm run dev
```

Aplikasi akan berjalan di `http://localhost:5000`

## Development di Visual Studio Code

### 1. Buka Proyek

```bash
# Buka dengan VS Code
code .
```

### 2. Install Ekstensi yang Direkomendasikan

VS Code akan otomatis menampilkan notifikasi untuk menginstal ekstensi yang direkomendasikan. Klik "Install All" untuk menginstal semua ekstensi.

### 3. Konfigurasi VS Code

Proyek sudah dikonfigurasi dengan:
- **Settings**: Formatting otomatis, TypeScript preferences
- **Tasks**: Build, type check, database operations
- **Launch**: Debug configuration untuk server
- **Extensions**: Rekomendasi ekstensi yang diperlukan

### 4. Debugging

1. Tekan `F5` atau pilih "Run and Debug" dari sidebar
2. Pilih "Debug Server" configuration
3. Set breakpoints di kode TypeScript
4. Debug dengan fitur penuh VS Code

### 5. Tasks yang Tersedia

Tekan `Ctrl+Shift+P` dan ketik "Tasks: Run Task":

- **Start Development Server**: Menjalankan server development
- **Build Project**: Build aplikasi untuk production
- **Type Check**: Cek TypeScript errors
- **Database Push**: Update database schema

## Struktur Proyek

```
immunization-monitoring-system/
├── .github/                    # GitHub workflows dan templates
│   ├── workflows/
│   │   └── ci.yml             # CI/CD pipeline
│   ├── ISSUE_TEMPLATE/        # Template untuk issues
│   └── pull_request_template.md
├── .vscode/                   # VS Code configuration
│   ├── settings.json          # Editor settings
│   ├── tasks.json             # Build tasks
│   ├── launch.json            # Debug configuration
│   └── extensions.json        # Recommended extensions
├── client/                    # Frontend React application
│   ├── src/
│   │   ├── components/        # Reusable UI components
│   │   ├── pages/             # Page components
│   │   ├── hooks/             # Custom React hooks
│   │   ├── lib/               # Utility functions
│   │   └── main.tsx           # Entry point
│   └── index.html
├── server/                    # Backend Express application
│   ├── index.ts               # Server entry point
│   ├── routes.ts              # API routes
│   ├── storage.ts             # Database operations
│   ├── db.ts                  # Database connection
│   └── replitAuth.ts          # Authentication setup
├── shared/                    # Shared types and schemas
│   └── schema.ts              # Database schema dan types
├── .env.example               # Environment variables template
├── .gitignore                 # Git ignore rules
├── .prettierrc                # Code formatting rules
├── package.json               # Dependencies and scripts
├── tsconfig.json              # TypeScript configuration
├── tailwind.config.ts         # Tailwind CSS configuration
├── vite.config.ts             # Vite build configuration
├── drizzle.config.ts          # Database ORM configuration
├── README.md                  # Project documentation
├── INSTALLATION.md            # Installation guide
├── DEPLOYMENT.md              # Deployment guide
├── CONTRIBUTING.md            # Contribution guidelines
├── CHANGELOG.md               # Version history
└── LICENSE                    # MIT License
```

## Scripts Package.json

Setelah instalasi, Anda dapat menggunakan script berikut:

```bash
# Development
npm run dev          # Start development server dengan hot reload

# Building
npm run build        # Build aplikasi untuk production
npm run start        # Start production server

# Database
npm run db:push      # Push schema changes ke database
npm run db:generate  # Generate migration files
npm run db:migrate   # Run database migrations

# Quality
npm run check        # TypeScript type checking
npm run format       # Format code dengan Prettier
npm run format:check # Check code formatting
npm run lint         # Run linting
npm run test         # Run tests

# Utilities
npm run clean        # Clean build artifacts
```

## Troubleshooting

### Database Connection Issues

**Error**: `connection refused`
```bash
# Check PostgreSQL status
sudo systemctl status postgresql

# Start PostgreSQL jika belum running
sudo systemctl start postgresql
```

**Error**: `authentication failed`
```bash
# Reset password
sudo -u postgres psql -c "ALTER USER immunization_user PASSWORD 'new_password';"

# Update .env file dengan password baru
```

### Port Already in Use

**Error**: `EADDRINUSE: address already in use`
```bash
# Find process using port 5000
lsof -i :5000

# Kill process
kill -9 <PID>

# Atau gunakan port yang berbeda
PORT=3000 npm run dev
```

### TypeScript Errors

```bash
# Clear TypeScript cache
rm -rf node_modules/.cache

# Reinstall dependencies
rm -rf node_modules package-lock.json
npm install

# Run type checking
npm run check
```

### Build Errors

```bash
# Clear build cache
npm run clean

# Check for syntax errors
npm run check

# Rebuild
npm run build
```

### VS Code Issues

**Extensions tidak loading:**
1. Restart VS Code
2. Check VS Code version (minimal 1.85.0)
3. Manual install extensions dari Extensions marketplace

**IntelliSense tidak bekerja:**
1. Reload VS Code window (`Ctrl+Shift+P` → "Developer: Reload Window")
2. Check TypeScript version: `Ctrl+Shift+P` → "TypeScript: Select TypeScript Version"
3. Restart TypeScript server: `Ctrl+Shift+P` → "TypeScript: Restart TS Server"

## Tips Development

### Hot Reload
Server otomatis restart ketika ada perubahan di file `.ts`. Frontend akan reload otomatis untuk perubahan React components.

### Database Schema Changes
Setelah mengubah schema di `shared/schema.ts`:
```bash
npm run db:push
```

### Code Formatting
Format otomatis saat save file, atau manual:
```bash
npm run format
```

### Environment Variables
Gunakan VS Code untuk edit `.env` dengan syntax highlighting dan autocomplete.

## Bantuan dan Support

### Documentation
- [README.md](README.md) - Overview dan fitur
- [DEPLOYMENT.md](DEPLOYMENT.md) - Panduan deployment
- [CONTRIBUTING.md](CONTRIBUTING.md) - Panduan kontribusi

### Issues dan Questions
- Buat issue di GitHub repository
- Check existing issues untuk solusi yang sudah ada
- Gunakan template yang sesuai untuk bug reports atau feature requests

### Community
- GitHub Discussions untuk pertanyaan umum
- Pull requests welcome untuk improvements
- Follow coding standards dan guidelines

Selamat coding! 🚀