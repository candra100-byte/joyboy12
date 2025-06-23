# Panduan Kontribusi

Terima kasih atas minat Anda untuk berkontribusi pada Sistem Monitoring Imunisasi Lombok Tengah!

## Setup Development Environment

### Prerequisites
- Node.js (v18 atau lebih baru)
- PostgreSQL database
- Git
- Visual Studio Code (recommended)

### Setup Lokal

1. Fork repository ini
2. Clone fork Anda:
   ```bash
   git clone https://github.com/YOUR_USERNAME/immunization-monitoring-system.git
   cd immunization-monitoring-system
   ```

3. Install dependencies:
   ```bash
   npm install
   ```

4. Copy dan konfigurasi environment variables:
   ```bash
   cp .env.example .env
   ```

5. Setup database:
   ```bash
   npm run db:push
   ```

6. Start development server:
   ```bash
   npm run dev
   ```

## Development Guidelines

### Code Style
- Gunakan TypeScript untuk semua file baru
- Ikuti konvensi penamaan yang konsisten
- Gunakan Prettier untuk formatting
- Gunakan ESLint untuk linting

### Commit Messages
Gunakan format commit message yang deskriptif:
```
feat: menambah fitur notifikasi WhatsApp
fix: perbaiki bug pada kalender penjadwalan
docs: update README dengan instruksi deployment
style: format kode dengan prettier
refactor: restructure komponen dashboard
test: tambah unit test untuk patient service
```

### Branch Naming
- Feature branches: `feature/nama-fitur`
- Bug fixes: `fix/deskripsi-bug`
- Documentation: `docs/topik-dokumentasi`

### Pull Request Process

1. Pastikan semua tests pass
2. Update dokumentasi jika diperlukan
3. Buat PR dengan deskripsi yang jelas
4. Request review dari maintainer
5. Address semua feedback sebelum merge

### Testing
- Tulis unit tests untuk fungsi utility
- Test komponen React dengan React Testing Library
- Pastikan API endpoints memiliki integration tests
- Jalankan `npm run test` sebelum submit PR

### Database Changes
- Gunakan Drizzle migrations untuk perubahan schema
- Test migrations di environment lokal dulu
- Dokumentasikan perubahan schema di PR description

## Pelaporan Issues

### Bug Reports
Sertakan informasi berikut:
- Langkah untuk reproduce
- Expected vs actual behavior
- Screenshots jika relevan
- Environment details (OS, browser, Node version)

### Feature Requests
- Jelaskan use case yang jelas
- Berikan mockup atau wireframe jika memungkinkan
- Diskusikan implementation approach

## Code Review Process

### Sebagai Reviewer
- Review logic dan architecture
- Check for security vulnerabilities
- Verify tests coverage
- Ensure documentation is updated
- Test functionality locally

### Sebagai Author
- Respond to feedback constructively
- Make requested changes promptly
- Ask questions if feedback unclear
- Update PR description if scope changes

## Development Best Practices

### Frontend (React/TypeScript)
- Gunakan custom hooks untuk logic reuse
- Implement proper error boundaries
- Use React Query untuk data fetching
- Follow accessibility guidelines
- Optimize for performance (memo, useMemo, useCallback)

### Backend (Node.js/Express)
- Validate all inputs dengan Zod schemas
- Implement proper error handling
- Use structured logging
- Follow RESTful API conventions
- Implement rate limiting untuk public endpoints

### Database (PostgreSQL/Drizzle)
- Always use parameterized queries
- Implement proper indexes
- Follow normalization principles
- Use transactions for multi-step operations
- Regular backup procedures

## Security Guidelines

- Never commit secrets atau API keys
- Validate semua user inputs
- Implement proper authentication checks
- Use HTTPS di production
- Regular dependency updates
- Follow OWASP security guidelines

## Release Process

1. Update version di package.json
2. Update CHANGELOG.md
3. Create release branch
4. Test thoroughly di staging
5. Create GitHub release
6. Deploy ke production
7. Monitor untuk issues

## Getting Help

- Join discussion di GitHub Discussions
- Check existing issues dan documentation
- Reach out di communication channels
- Attend community meetings (jika ada)

## Recognition

Contributors akan diakui di:
- README.md contributors section
- Release notes
- Community showcases

Terima kasih atas kontribusi Anda untuk meningkatkan sistem kesehatan di Lombok Tengah!