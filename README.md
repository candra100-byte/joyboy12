# Sistem Monitoring Imunisasi Lombok Tengah

Sistem monitoring imunisasi komprehensif untuk Lombok Tengah dengan autentikasi multi-peran, manajemen pasien, penjadwalan, dan kemampuan pelaporan.

## Fitur Utama

### 🔐 Autentikasi Multi-Peran
- Sistem login dengan Replit OAuth
- Peran pengguna: Admin, Dokter, Staff, Pasien
- Sesi aman dengan penyimpanan database

### 👥 Manajemen Pasien
- CRUD operasi lengkap untuk data pasien
- Pencarian dan filter berdasarkan nama, NIK, status
- Riwayat imunisasi per pasien
- Export data pasien ke format Excel/PDF

### 📊 Dashboard Real-time
- Statistik cakupan imunisasi
- Grafik progress harian/bulanan
- Aktivitas terbaru sistem
- Kartu statistik interaktif

### 📅 Sistem Penjadwalan
- Kalender interaktif untuk jadwal imunisasi
- Penjadwalan otomatis berdasarkan jenis vaksin
- Reminder untuk jadwal yang akan datang
- Manajemen slot waktu dokter

### 🔔 Sistem Notifikasi
- Dukungan SMS, WhatsApp, dan Email
- Template notifikasi yang dapat disesuaikan
- Notifikasi otomatis untuk jadwal imunisasi
- Status pengiriman notifikasi

### 📋 Survei Kepuasan
- Survei kepuasan pasien dengan rating bintang
- Analisis feedback dan komentar
- Laporan tingkat kepuasan layanan
- Dashboard analitik survei

### 📄 Pelaporan
- Generator laporan otomatis
- Export ke PDF dan Excel
- Laporan cakupan imunisasi
- Laporan aktivitas bulanan

## Teknologi yang Digunakan

### Frontend
- **React 18** - Framework UI modern
- **TypeScript** - Type-safe JavaScript
- **Tailwind CSS** - Utility-first CSS framework
- **Shadcn/UI** - Komponen UI yang dapat digunakan kembali
- **Wouter** - Router ringan untuk React
- **TanStack Query** - State management untuk server state
- **React Hook Form** - Form handling yang performant
- **Recharts** - Library charting untuk React
- **Lucide React** - Icon library modern

### Backend
- **Node.js** - Runtime JavaScript
- **Express.js** - Web framework untuk Node.js
- **TypeScript** - Type-safe JavaScript
- **Drizzle ORM** - Modern TypeScript ORM
- **PostgreSQL** - Database relasional
- **Passport.js** - Authentication middleware
- **OpenID Connect** - Authentication protocol

### Tools & Utilities
- **Vite** - Build tool yang cepat
- **ESBuild** - JavaScript bundler yang cepat
- **Drizzle Kit** - Database migrations
- **Zod** - Schema validation
- **Date-fns** - Library manipulasi tanggal

## Instalasi dan Setup

### Prasyarat
- Node.js (versi 18 atau lebih baru)
- PostgreSQL database
- Visual Studio Code (recommended)

### Langkah Instalasi

1. **Clone repository**
   ```bash
   git clone <repository-url>
   cd immunization-monitoring-system
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Setup environment variables**
   ```bash
   cp .env.example .env
   ```

   Edit file `.env` dengan konfigurasi yang sesuai:
   ```
   DATABASE_URL=postgresql://username:password@localhost:5432/immunization_db
   SESSION_SECRET=your-session-secret-key
   REPL_ID=your-replit-app-id
   ISSUER_URL=https://replit.com/oidc
   REPLIT_DOMAINS=your-domain.replit.app
   ```

4. **Setup database**
   ```bash
   npm run db:push
   ```

5. **Start development server**
   ```bash
   npm run dev
   ```

6. **Akses aplikasi**
   Buka browser dan navigasi ke `http://localhost:5000`

## Struktur Proyek

```
├── client/                 # Frontend React application
│   ├── src/
│   │   ├── components/    # Reusable UI components
│   │   ├── pages/         # Page components
│   │   ├── hooks/         # Custom React hooks
│   │   ├── lib/           # Utility functions
│   │   └── main.tsx       # Entry point
│   └── index.html
├── server/                 # Backend Express application
│   ├── index.ts           # Server entry point
│   ├── routes.ts          # API routes
│   ├── storage.ts         # Database operations
│   ├── db.ts              # Database connection
│   └── replitAuth.ts      # Authentication setup
├── shared/                 # Shared types and schemas
│   └── schema.ts          # Database schema and types
├── .vscode/               # VS Code configuration
├── package.json
├── tsconfig.json
├── tailwind.config.ts
└── vite.config.ts
```

## Scripts yang Tersedia

- `npm run dev` - Start development server
- `npm run build` - Build untuk production
- `npm run start` - Start production server
- `npm run check` - Type checking dengan TypeScript
- `npm run db:push` - Push schema changes ke database

## Development di Visual Studio Code

Proyek ini telah dikonfigurasi untuk bekerja optimal dengan Visual Studio Code:

### Extensions yang Direkomendasikan
- TypeScript and JavaScript Language Features
- Tailwind CSS IntelliSense
- Prettier - Code formatter
- ESLint
- Auto Rename Tag
- Path Intellisense

### Debug Configuration
Gunakan `F5` untuk mulai debugging server dengan breakpoints.

### Tasks
- `Ctrl+Shift+P` → "Tasks: Run Task" untuk menjalankan task yang tersedia
- Build, type check, dan database operations tersedia sebagai tasks

## Deployment

### Build untuk Production
```bash
npm run build
```

### Environment Variables untuk Production
Pastikan semua environment variables telah di-set dengan benar:
- `DATABASE_URL`
- `SESSION_SECRET`
- `REPL_ID`
- `ISSUER_URL`
- `REPLIT_DOMAINS`

## Kontribusi

1. Fork repository
2. Buat feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push ke branch (`git push origin feature/amazing-feature`)
5. Buat Pull Request

## Troubleshooting

### Database Connection Issues
- Pastikan PostgreSQL running
- Verify DATABASE_URL di environment variables
- Jalankan `npm run db:push` untuk create/update tables

### Authentication Issues
- Pastikan REPL_ID dan ISSUER_URL benar
- Verify REPLIT_DOMAINS sesuai dengan domain deployment

### Build Issues
- Clear node_modules dan install ulang
- Check TypeScript errors dengan `npm run check`
- Pastikan semua dependencies ter-install dengan benar

## License

MIT License - lihat file [LICENSE](LICENSE) untuk detail lengkap.

## Support

Untuk bantuan dan pertanyaan:
- Buat issue di GitHub repository
- Contact: [your-email@domain.com]

---

Dikembangkan dengan ❤️ untuk Sistem Kesehatan Lombok Tengah