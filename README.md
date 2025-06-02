# Family Photos & Document Management System

A secure, role-based document and photo management application built with Next.js and Google Drive integration. Perfect for families, small teams, or organizations who need to organize, search, and share files with granular permission controls.

![Next.js](https://img.shields.io/badge/Next.js-15.3.3-black)
![TypeScript](https://img.shields.io/badge/TypeScript-5-blue)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-4-38B2AC)
![Google Drive](https://img.shields.io/badge/Google_Drive-API-4285F4)

## ✨ Features

### 🔐 **Role-Based Access Control**
- **Admin**: Full system access, user management
- **Editor**: Upload files, edit metadata, search documents
- **Viewer**: Search and view files (read-only access)
- **Guest**: Limited folder access

### 📁 **Document Management**
- Upload PDFs and images (JPEG, PNG, WebP, GIF)
- Add keywords and descriptions for searchability
- Advanced search with keyword filtering
- File organization with Google Drive integration
- Automatic file validation (type, size limits)

### 🖼️ **Image Gallery**
- Beautiful responsive image gallery
- Grid and list view modes
- Image thumbnails with lazy loading
- Direct view and Google Drive integration
- Search images by name and description

### 🔍 **Smart Search**
- Keyword-based file search
- Description and metadata search
- Role-based search results
- Fast file discovery

### 🚀 **Modern Technology Stack**
- **Frontend**: Next.js 15 with App Router, TypeScript, Tailwind CSS
- **Authentication**: NextAuth.js with Google OAuth
- **Storage**: Google Drive API with service account
- **Database**: PostgreSQL with Drizzle ORM
- **UI Components**: Lucide React icons, responsive design

## 🚀 Quick Start

### Prerequisites

- Node.js 18+ and npm
- PostgreSQL database
- Google Cloud Platform account
- Google Drive API credentials

### 1. Clone the Repository

```bash
git clone https://github.com/phsphd/familyphotos.git
cd familyphotos
npm install
```

### 2. Environment Setup

Create a `.env.local` file in the root directory:

```env
# Database
DATABASE_URL="postgresql://username:password@localhost:5432/database_name"

# NextAuth
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="your-nextauth-secret-key"

# Google OAuth
GOOGLE_CLIENT_ID="your-google-client-id"
GOOGLE_CLIENT_SECRET="your-google-client-secret"

# Google Service Account (for Drive API)
TYPE="service_account"
PROJECT_ID="your-project-id"
PRIVATE_KEY_ID="your-private-key-id"
PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"
CLIENT_EMAIL="your-service-account@project.iam.gserviceaccount.com"
CLIENT_ID="your-client-id"
CLIENT_X509_CERT_URL="your-cert-url"

# Google Drive
GOOGLE_DRIVE_SHARED_FOLDER_ID="your-shared-folder-id"
```

### 3. Google Cloud Setup

#### Enable APIs
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Enable the following APIs:
   - Google Drive API
   - Google People API

#### Create Service Account
1. Navigate to **IAM & Admin** → **Service Accounts**
2. Create a new service account
3. Generate and download the JSON key file
4. Extract the credentials to your `.env.local`

#### Create OAuth Application
1. Go to **APIs & Services** → **Credentials**
2. Create OAuth 2.0 Client IDs
3. Add authorized redirect URI: `http://localhost:3000/api/auth/callback/google`

#### Set Up Google Drive Folder
1. Create a shared folder in Google Drive
2. Share it with your service account email
3. Copy the folder ID from the URL

### 4. Database Setup

```bash
# Install Drizzle CLI
npm install -g drizzle-kit

# Push database schema
npm run db:push

# Optional: Open database studio
npm run db:studio
```

### 5. Start Development Server

```bash
npm run dev
```

Visit `http://localhost:3000` to access the application.

## 📋 Usage

### First Time Setup

1. **Sign in** with your Google account
2. **Admin** will need to configure user permissions
3. **Start uploading** documents and images
4. **Add keywords** and descriptions for better searchability

### User Roles

- **New users** start with limited access
- **Contact admin** to upgrade permissions
- **Role-based features** automatically adjust interface

### File Management

- **Upload files** via drag & drop or file picker
- **Add keywords** separated by commas
- **Search files** using the search interface
- **View images** in the responsive gallery

## 🏗️ Project Structure

```
src/
├── app/                          # Next.js App Router
│   ├── api/                      # API routes
│   │   ├── auth/                 # NextAuth configuration
│   │   ├── images/               # Image gallery API
│   │   └── upload/               # File upload API
│   ├── dashboard/                # Main dashboard page
│   └── login/                    # Authentication pages
├── components/                   # React components
│   ├── ImageGallery.tsx          # Image gallery component
│   ├── ImageUpload.tsx           # File upload component
│   ├── ProtectedRoute.tsx        # Route protection
│   └── EnhancedHeader.tsx        # Navigation header
├── lib/                          # Utility libraries
│   ├── database/                 # Database schema & queries
│   ├── services/                 # Business logic services
│   └── google-drive.ts           # Google Drive integration
├── hooks/                        # Custom React hooks
│   └── useUserPermissions.ts     # Permission management
└── types/                        # TypeScript definitions
    └── index.ts                  # Shared types
```

## 🔧 Configuration

### User Permissions

Edit user permissions in your permission management system:

```typescript
// Example permission structure
{
  "user@example.com": {
    "role": "editor",
    "permissions": {
      "upload": true,
      "search": true,
      "edit": true
    }
  }
}
```

### File Types

Supported file types:
- **Documents**: PDF
- **Images**: JPEG, PNG, WebP, GIF
- **Size Limit**: 10MB per file

## 🚀 Deployment

### Vercel Deployment

1. **Connect your GitHub repository** to Vercel
2. **Add environment variables** in Vercel dashboard
3. **Deploy** automatically on git push

### Environment Variables for Production

Update your production environment variables:
- Set `NEXTAUTH_URL` to your production domain
- Use production database URL
- Ensure Google OAuth redirect URIs include production domain

## 🛠️ Development

### Available Scripts

```bash
npm run dev          # Start development server
npm run build        # Build for production
npm run start        # Start production server
npm run lint         # Run ESLint
npm run db:generate  # Generate database migrations
npm run db:push      # Push schema to database
npm run db:studio    # Open Drizzle Studio
```

### Adding New Features

1. **Create feature branch**: `git checkout -b feature/new-feature`
2. **Implement changes** with proper TypeScript types
3. **Test thoroughly** with different user roles
4. **Update documentation** if needed
5. **Submit pull request**

## 🔒 Security

### Best Practices Implemented

- **Role-based access control** with granular permissions
- **Secure file validation** and size limits
- **Service account authentication** for Google Drive
- **Environment variable protection** for sensitive data
- **CSRF protection** with NextAuth

### Security Considerations

- Regularly rotate Google service account keys
- Monitor file uploads and access patterns
- Keep dependencies updated
- Use HTTPS in production
- Implement proper backup strategies

## 🤝 Contributing

1. **Fork the repository**
2. **Create your feature branch** (`git checkout -b feature/AmazingFeature`)
3. **Commit your changes** (`git commit -m 'Add some AmazingFeature'`)
4. **Push to the branch** (`git push origin feature/AmazingFeature`)
5. **Open a Pull Request**

### Development Guidelines

- Follow TypeScript best practices
- Use Tailwind CSS for styling
- Implement proper error handling
- Add comments for complex logic
- Test with different user permission levels

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **Next.js team** for the excellent framework
- **Google Drive API** for reliable file storage
- **Tailwind CSS** for rapid UI development
- **Drizzle ORM** for type-safe database operations
- **NextAuth.js** for authentication solution

## 📞 Support

- **Create an issue** for bug reports
- **Start a discussion** for feature requests
- **Check existing issues** before creating new ones

---

**Built with ❤️ for families and teams who value organized, secure file management.**
