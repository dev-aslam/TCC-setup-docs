# TCC Blog Firebase Project Setup

## Navigation

- [Creating and Configuring a Firebase Project](#creating-and-configuring-a-firebase-project)

  - [1. Create a Firebase Project](#1-create-a-firebase-project)
  - [2. Register Your Web App with Firebase](#2-register-your-web-app-with-firebase)
  - [3. Firebase Configuration](#3-firebase-configuration)
  - [4. Enable Firebase Services](#4-enable-firebase-services)
    - [Firestore (Database)](#firestore-database)
    - [Firebase Authentication](#firebase-authentication)
    - [Firebase Storage](#firebase-storage)

- [Setting Up EmailJS](#setting-up-emailjs)

  - [1. Create an EmailJS Account](#1-create-an-emailjs-account)
  - [2. Create an Email Service](#2-create-an-email-service)
  - [3. Create an Email Template](#3-create-an-email-template)
  - [4. Obtain Your User ID](#4-obtain-your-user-id)

- [TCC Website Deployment Guide](#tcc-website-deployment-guide)
  - [1. Deploying to Cloudflare Pages](#1-deploying-to-cloudflare-pages)
  - [2. Connect Git Repository](#2-connect-git-repository)
  - [3. Configure Build Settings](#3-configure-build-settings)
  - [4. Deploy the Project](#4-deploy-the-project)
  - [5. Custom Domain](#5-custom-domain)

# TCC Blog Firebase Project Setup

## Creating and Configuring a Firebase Project

### 1. Create a Firebase Project

- Go to the [Firebase Console](https://console.firebase.google.com/).
- Click on **Add Project**.
- Enter a project name (e.g., `TCC Website`).
- Click **Continue** and turn off google analytics
- Click **Create Project** and wait for Firebase to set up your project.

### 2. Register Your Web App with Firebase

- Once the project is created, navigate to the **Project Overview** page.
- Click on the **Web** icon (`</>`) to register your app.
- Enter an app nickname (e.g., `TCC Website App` or `TCC Blogs`) and click **Register App**.

### 3. Firebase Configuration

- After registering the app, Firebase will provide you with a Firebase SDK configuration object. Copy this configuration, as you’ll need it for the website.

  Example:

  ```javascript
  const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "your-project-id.firebaseapp.com",
    projectId: "your-project-id",
    storageBucket: "your-project-id.appspot.com",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID",
  };
  ```

### 4. Enable Firebase Services

Now enable the following services

- **Firestore (Database)**:

  - In the Firebase console, on the left-side menu under **Build**, find **Firestore Database**, and click **Create Database**.
  - Select a location (for Dubai users, ideal locations are `asia-south1 (Mumbai)` or `europe-west2 (London)`), and click **Done**.
  - Once the setup is complete, navigate to the **Rules** tab and replace the existing rule with the following, then click **Publish**:
    ```bash
    rules_version = '2';
        service cloud.firestore {
        match /databases/{database}/documents {
                match /{document=**} {
                allow read;
                allow write: if request.auth != null;
                }
            }
        }
    ```

- **Firebase Authentication**:

  - In the Firebase console, on the left-side menu under **Build**, find **Authentication**, and click **Get Started**.
  - Go to the **Sign-in method** tab.
  - Select the **Email/Password** sign-in method, enable it, and click **Save**.
  - Once done, you can manually add users in the **Users** tab.

- **Firebase Storage**:
  - In the Firebase console, on the left-side menu under **Build**, find **Storage**, and click **Get Started**.
  - Select your storage location (for Dubai users, ideal locations are `asia-south1 (Mumbai)` or `europe-west2 (London)`), and wait for the configuration to complete.
  - Once set up, go to the **Rules** tab, replace the existing rule with the following, and click **Publish**:
    ```bash
    rules_version = '2';
        service firebase.storage {
        match /b/{bucket}/o {
                match /{allPaths=**} {
                allow read;
                allow write: if request.auth != null;
                }
            }
        }
    ```

# Setting Up EmailJS

## 1. Create an EmailJS Account

- Go to the [EmailJS website](https://www.emailjs.com/).
- Click on **Sign Up** to create a new account.
- You can sign up using your email or through third-party services like Google or GitHub.
- After signing up, confirm your email address if prompted.

## 2. Create an Email Service

- Once logged in, navigate to the **Email Services** section from the left-side menu.
- Click on **Add New Service**.
- From the configuration menu, copy the **Service ID**; this will be used later when setting up your website.
- Select your email provider from the list. If you are using a custom email address (like `info@tcc-uae.ae`), select **SMTP**.
- Enter the SMTP details:
  - **Service Name**: Give it a recognizable name (e.g., `TCC UAE SMTP`).
  - **SMTP Server**: This is usually provided by your email host (e.g., `smtp.tcc-uae.ae`).
  - **Port**: Use `587` for TLS or `465` for SSL.
  - **Username**: Enter your full email address (e.g., `info@tcc-uae.ae`).
  - **Password**: Enter the password for your email account.
- Click **Save** once you have filled in the required information.

## 3. Create an Email Template

- Navigate to the **Email Templates** section from the left-side menu.
- Click on **Create New Template**.
- Provide the subject of the email (e.g., `Contact Form Submission from {{companyName}}`).
- In the content section, click **Edit Content** and then select **Design Editor**.
- Copy and paste the following content into the editor:

  ```
  Company Name: {{companyName}}

  Contact Person: {{contactPerson}}

  Phone Number: {{phone}}

  Email: {{email}}

  Message:

  {{message}}
  ```

- On the right side, set the **Email To** field to the email address where you want to receive this email (e.g., `info@tcc-uae.ae`).
- Go to the **Settings** tab; here you can find the **Template ID**. Copy this, as it will be used later when setting up EmailJS.
- Click **Save** to store the template.

## 4. Obtain Your User ID

- Navigate to the **Account** section; from here, you can find your **Public Key**. This is your User ID.
- Copy your public key for later use.

## Summary of Information Needed

- **Service ID**: Obtain from the Email Service setup.
- **Template ID**: Obtain from the Email Template setup.
- **User ID**: Your Public Key from the Account section.

# TCC Website Deployment Guide

## Deploying to Cloudflare Pages

### 1. Access Cloudflare Pages

- Log in to your Cloudflare account.
- In the dashboard, navigate to **Workers and Pages** from the left-side menu.
- Click on the **Pages** tab.

### 2. Connect Git Repository

- Click **Connect to Git** and choose **GitHub**.
- Follow the prompts to connect your GitHub account.
- Once connected, select the repository containing your project and click **Begin Setup**.

### 3. Configure Build Settings

- After selecting your GitHub repository, under **Build Settings**

  - **Framework preset**: Select **Create React App**.
  - **Build command**: Ensure the build command is `npm run build`
  - **Build Output Directory**: Set this to `dist`.

- **Environment Variables**:

  - You will need to add the following environment variables during the build configuration. Replace the placeholder values with the actual keys you copied from Firebase SDK and EmailJS:

  **Note: You don’t need to add EmailJS variables for the admin side; only Firebase variables are required**

  ```Javascript
    //EmailJS
      VITE_EMAILJS_SERVICE_ID=your_emailjs_service_id
      VITE_EMAILJS_TEMPLATE_ID=your_emailjs_template_id
      VITE_EMAILJS_USER_ID=your_emailjs_user_id
  ```

  ```Javascript
    // Firebase
      VITE_API_KEY=your_firebase_api_key
      VITE_AUTH_DOMAIN=your_firebase_auth_domain
      VITE_PROJECT_ID=your_firebase_project_id
      VITE_STORAGE_BUCKET=your_firebase_storage_bucket
      VITE_MESSAGING_SENDER_ID=your_firebase_messaging_sender_id
      VITE_APP_ID=your_firebase_app_id
  ```

  Make sure all Firebase keys from the SDK configuration (like `apiKey`, `authDomain`, etc.) are replaced with the values from your Firebase console.

### 4. Deploy the Project

- After setting the build configuration, click **Save and Deploy**.
- Cloudflare will begin building the application. This may take a few minutes.

### 5. Custom Domain

- Once the build is complete, you can add the custom domain here
