# Complete Google Drive Setup Guide

This guide will walk you through setting up Google Drive integration for your photo and document management application. Follow these steps carefully to ensure proper authentication and file storage.

## 📋 Prerequisites

- Google account with access to Google Cloud Console
- Basic understanding of API keys and OAuth
- Admin access to your Google Drive

---

## 🔧 Step 1: Create Google Cloud Project

### 1.1 Access Google Cloud Console

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Sign in with your Google account

### 1.2 Create New Project

1. Click on the **project dropdown** (top-left, next to Google Cloud logo)
2. Click **"New Project"**
3. Enter project details:
   - **Project name**: `Family Photos App` (or your preferred name)
   - **Organization**: Leave as default or select your organization
   - **Location**: Leave as default
4. Click **"Create"**
5. **Wait** for project creation (may take a few minutes)
6. **Select** your new project from the dropdown

---

## 🚀 Step 2: Enable Required APIs

### 2.1 Navigate to APIs & Services

1. In the Google Cloud Console, click the **☰ hamburger menu**
2. Go to **"APIs & Services"** → **"Library"**

### 2.2 Enable Google Drive API

1. In the API Library, search for **"Google Drive API"**
2. Click on **"Google Drive API"** from the results
3. Click **"Enable"** button
4. Wait for enablement confirmation

### 2.3 Enable Google People API (for profile info)

1. Search for **"Google People API"** in the library
2. Click on **"Google People API"**
3. Click **"Enable"** button

### 2.4 Verify APIs are Enabled

1. Go to **"APIs & Services"** → **"Enabled APIs"**
2. Confirm you see:
   - ✅ Google Drive API
   - ✅ Google People API

---

## 🔐 Step 3: Create Service Account

The Service Account allows your application to access Google Drive programmatically without user interaction.

### 3.1 Navigate to Service Accounts

1. Go to **"APIs & Services"** → **"Credentials"**
2. Click **"+ Create Credentials"** → **"Service Account"**

### 3.2 Create Service Account

1. **Service account details**:
   - **Service account name**: `family-photos-service`
   - **Service account ID**: `family-photos-service` (auto-generated)
   - **Description**: `Service account for family photos app file management`
2. Click **"Create and Continue"**

### 3.3 Grant Permissions (Optional)

1. **Grant this service account access to project**: Skip this step
2. Click **"Continue"**

### 3.4 Generate Service Account Key

1. **Grant users access to this service account**: Skip this step
2. Click **"Done"**
3. You'll be redirected to the credentials page
4. Find your service account in the list
5. Click on the **service account email** (e.g., `family-photos-service@your-project.iam.gserviceaccount.com`)
6. Go to the **"Keys"** tab
7. Click **"Add Key"** → **"Create new key"**
8. Select **"JSON"** format
9. Click **"Create"**
10. **Download** the JSON file (keep it secure!)

### 3.5 Extract Credentials

From the downloaded JSON file, you'll need these values for your `.env.local`:

```json
{
  "type": "service_account",
  "project_id": "your-project-id",
  "private_key_id": "your-private-key-id",
  "private_key": "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n",
  "client_email": "family-photos-service@your-project.iam.gserviceaccount.com",
  "client_id": "your-client-id",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "your-cert-url"
}
```

---

## 👤 Step 4: Set Up OAuth 2.0 for User Authentication

OAuth 2.0 allows users to sign in with their Google accounts.

### 4.1 Configure OAuth Consent Screen

1. Go to **"APIs & Services"** → **"OAuth consent screen"**
2. Select **"External"** user type
3. Click **"Create"**

### 4.2 Fill OAuth Consent Screen Details

**App information**:
- **App name**: `Family Photos & Document Manager`
- **User support email**: Your email address
- **App logo**: Upload a logo (optional)

**App domain** (for production):
- **Application home page**: `https://your-domain.com`
- **Application privacy policy link**: `https://your-domain.com/privacy`
- **Application terms of service link**: `https://your-domain.com/terms`

**Developer contact information**:
- **Email addresses**: Your email address

Click **"Save and Continue"**

### 4.3 Configure Scopes

1. Click **"Add or Remove Scopes"**
2. Add these scopes:
   - `../auth/userinfo.email`
   - `../auth/userinfo.profile`
   - `../auth/drive.file` (for Google Drive access)
3. Click **"Update"**
4. Click **"Save and Continue"**

### 4.4 Add Test Users

1. Click **"+ Add Users"**
2. Add email addresses of users who should access the app:
   - Your email address
   - Any other team members
3. Click **"Save and Continue"**

### 4.5 Review and Submit

1. Review your OAuth consent screen configuration
2. Click **"Back to Dashboard"**

### 4.6 Create OAuth 2.0 Credentials

1. Go to **"APIs & Services"** → **"Credentials"**
2. Click **"+ Create Credentials"** → **"OAuth 2.0 Client IDs"**
3. Select **"Web application"**
4. **Name**: `Family Photos Web Client`
5. **Authorized JavaScript origins**:
   - `http://localhost:3000` (for development)
   - `https://your-domain.com` (for production)
6. **Authorized redirect URIs**:
   - `http://localhost:3000/api/auth/callback/google` (development)
   - `https://your-domain.com/api/auth/callback/google` (production)
7. Click **"Create"**
8. **Copy** the Client ID and Client Secret

---

## 📁 Step 5: Create Google Drive Upload Folder

### 5.1 Create Shared Folder

1. Go to [Google Drive](https://drive.google.com/)
2. Click **"+ New"** → **"Folder"**
3. Name the folder: `Family Photos Upload` (or your preferred name)
4. Click **"Create"**

### 5.2 Share Folder with Service Account

1. **Right-click** on the newly created folder
2. Select **"Share"**
3. In the "Add people and groups" field, enter your **service account email**:
   ```
   family-photos-service@your-project.iam.gserviceaccount.com
   ```
4. Set permission to **"Editor"** (allows upload, edit, delete)
5. **Uncheck** "Notify people" (service accounts don't need notifications)
6. Click **"Share"**

### 5.3 Get Folder ID

1. **Open** the shared folder in Google Drive
2. Look at the **URL** in your browser address bar:
   ```
   https://drive.google.com/drive/folders/1ABCDEfghijklmnopQRSTuvwxyz123456789
   ```
3. **Copy** the folder ID (the long string after `/folders/`):
   ```
   1ABCDEfghijklmnopQRSTuvwxyz123456789
   ```
4. **Save** this folder ID for your environment variables

---

## ⚙️ Step 6: Configure Environment Variables

Create or update your `.env.local` file with all the credentials:

```env
# Database
DATABASE_URL="postgresql://username:password@localhost:5432/database_name"

# NextAuth Configuration
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="your-nextauth-secret-key-here"

# Google OAuth 2.0 (from Step 4.6)
GOOGLE_CLIENT_ID="1234567890-abcdefghijklmnopqrstuvwxyz.apps.googleusercontent.com"
GOOGLE_CLIENT_SECRET="GOCSPX-abcdefghijklmnopqrstuvwxyz"

# Google Service Account (from Step 3.5 JSON file)
TYPE="service_account"
PROJECT_ID="your-project-id"
PRIVATE_KEY_ID="abcdefghijklmnopqrstuvwxyz123456789"
PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\nMIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQC7VJTUt9Us8cKB\n...(your full private key)...\n-----END PRIVATE KEY-----\n"
CLIENT_EMAIL="family-photos-service@your-project.iam.gserviceaccount.com"
CLIENT_ID="123456789012345678901"
CLIENT_X509_CERT_URL="https://www.googleapis.com/robot/v1/metadata/x509/family-photos-service%40your-project.iam.gserviceaccount.com"

# Google Drive Folder (from Step 5.3)
GOOGLE_DRIVE_SHARED_FOLDER_ID="1ABCDEfghijklmnopQRSTuvwxyz123456789"
```

### 6.1 Important Notes for Environment Variables

- **PRIVATE_KEY**: Keep the `\n` characters in the private key
- **Don't commit** `.env.local` to version control
- **Use different keys** for development and production
- **Regenerate keys** regularly for security

---

## 🧪 Step 7: Test Your Setup

### 7.1 Test Service Account Access

1. Start your development server: `npm run dev`
2. Check the terminal for any authentication errors
3. Try uploading a test file through your app
4. Verify the file appears in your Google Drive folder

### 7.2 Test OAuth Authentication

1. Go to `http://localhost:3000`
2. Try signing in with Google
3. Check that your profile information loads correctly
4. Verify you can access the dashboard

### 7.3 Test Image Gallery

1. Upload some test images through your app
2. Navigate to the "Image Gallery" tab
3. Verify images display correctly
4. Test the search functionality

---

## 🔧 Troubleshooting

### Common Issues and Solutions

#### "The incoming JSON object does not contain a client_email field"
- **Solution**: Check that all service account environment variables are set correctly
- **Verify**: The `CLIENT_EMAIL` variable matches the service account email

#### "Access denied" or "403 Forbidden"
- **Solution**: Ensure the shared folder is properly shared with the service account email
- **Check**: Service account has "Editor" permissions on the folder

#### "redirect_uri_mismatch" OAuth error
- **Solution**: Add the correct redirect URI to your OAuth 2.0 client
- **Format**: `http://localhost:3000/api/auth/callback/google`

#### "Files not appearing in gallery"
- **Solution**: Check that images are being uploaded to the correct folder ID
- **Verify**: `GOOGLE_DRIVE_SHARED_FOLDER_ID` matches your actual folder ID

#### Private key format errors
- **Solution**: Ensure the private key includes proper line breaks (`\n`)
- **Check**: Key starts with `-----BEGIN PRIVATE KEY-----` and ends with `-----END PRIVATE KEY-----`

### Debug Steps

1. **Check environment variables** are loaded correctly
2. **Verify API quotas** in Google Cloud Console
3. **Review service account permissions** in Google Drive
4. **Test API calls** manually using Google's API Explorer
5. **Check application logs** for detailed error messages

---

## 🔒 Security Best Practices

### Service Account Security

- **Never commit** service account JSON files to version control
- **Use environment variables** for all sensitive data
- **Rotate keys regularly** (every 90 days recommended)
- **Monitor API usage** in Google Cloud Console
- **Limit service account permissions** to minimum required

### OAuth Security

- **Use HTTPS** in production
- **Set up proper domains** in OAuth consent screen
- **Regular review** of authorized applications
- **Monitor signin activity** in Google Account settings

### File Storage Security

- **Set appropriate folder permissions**
- **Regular audit** of shared folders
- **Monitor file access logs**
- **Implement file type validation**
- **Set reasonable file size limits**

---

## 📚 Additional Resources

- [Google Drive API Documentation](https://developers.google.com/drive/api)
- [Google OAuth 2.0 Guide](https://developers.google.com/identity/protocols/oauth2)
- [Google Cloud Console](https://console.cloud.google.com/)
- [NextAuth.js Documentation](https://next-auth.js.org/)
- [Google Workspace Admin Help](https://support.google.com/a/answer/6002043)

---

**🎉 Congratulations!** You've successfully set up Google Drive integration for your application. Users can now authenticate with Google and upload files directly to your managed Google Drive folder.
