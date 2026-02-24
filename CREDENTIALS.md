# 🔐 Credentials Setup Guide

This guide walks you through setting up all the credentials required for the n8n Social Media Poster workflow.

---

## 📋 Table of Contents

1. [Meta Developer App Setup](#1-meta-developer-app-setup)
2. [Instagram & Facebook Tokens](#2-instagram--facebook-tokens)
3. [imgbb API Key](#3-imgbb-api-key)
4. [Telegram Bot Setup](#4-telegram-bot-setup)
5. [Google Cloud & Sheets Setup](#5-google-cloud--sheets-setup)
6. [Adding Credentials to n8n](#6-adding-credentials-to-n8n)

---

## 1. Meta Developer App Setup

### Create a Meta Developer Account
1. Go to **developers.facebook.com**
2. Log in with your Facebook account
3. Click **"My Apps" → "Create App"**

### Configure Use Cases
1. Inside your app go to **"Use Cases"**
2. Make sure these use cases are added:
   - **Manage everything on your page**
   - **Manage messaging and content on Instagram**
3. For each use case click **"Edit"** and make sure these permissions are enabled:

| Permission | Required For |
|---|---|
| `instagram_basic` | Instagram posting |
| `instagram_content_publish` | Instagram posting |
| `pages_read_engagement` | Facebook page access |
| `pages_show_list` | Facebook page access |
| `pages_manage_posts` | Facebook posting |

---

## 2. Instagram & Facebook Tokens

### Create a Meta Business Account
1. Go to **business.facebook.com**
2. Create a Business account if you don't have one
3. Connect your **Instagram Business Account** and **Facebook Page**

### Create a System User
1. Go to **business.facebook.com → Settings → Users → System Users**
2. Click **"Add"** → give it a name e.g. `n8n-poster`
3. Set role to **Admin**
4. Click **"Create System User"**

### Add Assets to System User
1. Click on your System User
2. Click **"Add Assets"**
3. Add:
   - **Instagram Accounts** → select your account → Full Control
   - **Pages** → select your Facebook Page → Full Control
4. Click **"Save"**

### Generate System User Token
1. Click **"Generate New Token"**
2. Select your Meta app
3. Check these permissions:
   - `instagram_basic`
   - `instagram_content_publish`
   - `pages_read_engagement`
   - `pages_show_list`
   - `pages_manage_posts`
   - `business_management`
4. Click **"Generate Token"**
5. **Copy and save this token** — this is your `FB_SYSTEM_USER_TOKEN` for Instagram

### Get your Instagram Business Account ID
1. Go to **business.facebook.com → Settings → Accounts → Instagram Accounts**
2. Click on your Instagram account
3. Copy the **Account ID** — this is your `IG_USER_ID`

### Get your Facebook Page Token
1. Go to **developers.facebook.com/tools/explorer**
2. Select your app in the top right
3. In the query box type:
```
YOUR_PAGE_ID?fields=id,name,access_token
```
4. Click **Submit**
5. Copy the `access_token` from the response — this is your `FB_PAGE_TOKEN`
6. **Verify it's a Page token** at **developers.facebook.com/tools/debug/accesstoken** — Type should show **"Page"**

### Get your Facebook Page ID
1. Go to **facebook.com** → open your Facebook Page
2. Click **"About"**
3. Scroll down to find **"Page ID"** — this is your `FB_PAGE_ID`

> ⚠️ **Important:** The Page Token expires. Regenerate it periodically using the same steps above. The System User Token lasts much longer and is used for Instagram.

---

## 3. imgbb API Key

1. Go to **imgbb.com** and create a free account
2. Once logged in, go to **api.imgbb.com**
3. Copy your API key — this is your `IMGBB_API_KEY`

---

## 4. Telegram Bot Setup

### Create a Bot
1. Open **Telegram** and search for **"BotFather"**
2. Send `/newbot`
3. Give your bot a name e.g. `My n8n Bot`
4. Give it a username e.g. `myn8n_bot`
5. BotFather will give you a **Bot Token** — copy it

### Get your Chat ID
1. Search for **"userinfobot"** on Telegram
2. Send `/start`
3. It returns your **Chat ID** — copy it — this is your `TELEGRAM_CHAT_ID`

---

## 5. Google Cloud & Sheets Setup

### Create a Google Cloud Project
1. Go to **console.cloud.google.com**
2. Click the project dropdown → **"New Project"**
3. Name it e.g. `n8n-social-poster`
4. Click **"Create"**

### Enable Required APIs
1. Go to **"APIs & Services" → "Library"**
2. Search and enable:
   - **Google Sheets API**
   - **Google Drive API**

### Create a Service Account
1. Go to **"IAM & Admin" → "Service Accounts"**
2. Click **"Create Service Account"**
3. Name it e.g. `n8n-social-poster`
4. Click **"Create and Continue"** → **"Done"**

### Create a Service Account Key
1. Click on your service account
2. Go to the **"Keys"** tab
3. Click **"Add Key" → "Create New Key"**
4. Select **"JSON"**
5. Click **"Create"** — a JSON file downloads automatically
6. Open the file and note:
   - `client_email` — your service account email
   - `private_key` — your private key

### Create your Google Sheet
1. Go to **sheets.google.com**
2. Create a new sheet named `Social Media Post Log`
3. Add these headers in row 1:

| A | B | C | D | E | F |
|---|---|---|---|---|---|
| Date | Caption | Platforms | Image URL | Status | Error |

### Share Sheet with Service Account
1. Click **"Share"** in your Google Sheet
2. Add your service account email (from `client_email` in the JSON file)
3. Give it **"Editor"** access
4. Uncheck **"Notify people"**
5. Click **"Share"**

---

## 6. Adding Credentials to n8n

### Facebook Graph API Credential
1. Go to **n8n → Credentials → New**
2. Search **"Facebook Graph API"**
3. Paste your **System User Token** (for Instagram nodes)
4. Click **"Save"**

### Google Service Account Credential
1. Go to **Credentials → New**
2. Search **"Google Service Account API"**
3. Fill in:
   - **Service Account Email** — from `client_email` in JSON file
   - **Private Key** — from `private_key` in JSON file (include the `-----BEGIN PRIVATE KEY-----` and `-----END PRIVATE KEY-----` parts)
4. Click **"Save"**

### Telegram Credential
1. Go to **Credentials → New**
2. Search **"Telegram"**
3. Paste your **Bot Token**
4. Click **"Save"**

---

## 🔄 Token Refresh Guide

| Token | Expiry | How to Refresh |
|---|---|---|
| System User Token | Long-lived | Regenerate from business.facebook.com |
| Facebook Page Token | Short-lived | Re-run query in Graph API Explorer |
| imgbb API Key | Never expires | No action needed |
| Telegram Bot Token | Never expires | No action needed |
| Google Service Account | Never expires | No action needed |

> 💡 **Tip:** When the Facebook Page Token expires you'll get a `(#200) permission denied` error on the Facebook node. Just regenerate it using the Graph API Explorer steps above.
