# 📱 n8n Multi-Platform Social Media Poster

An automated workflow built with [n8n](https://n8n.io) that allows you to post images and captions to **Instagram** and **Facebook** simultaneously through a simple web form.

![n8n](https://img.shields.io/badge/n8n-workflow-FF6D5A?style=flat-square&logo=n8n)
![Docker](https://img.shields.io/badge/Docker-compose-2496ED?style=flat-square&logo=docker)
![Meta](https://img.shields.io/badge/Meta-Graph%20API-0866FF?style=flat-square&logo=meta)

---

## ✨ Features

- 📝 **Web Form** — Simple form to upload image, write caption, and select platforms
- 📸 **Instagram Posting** — Automatically posts image and caption to Instagram Business account
- 👍 **Facebook Posting** — Simultaneously posts to your Facebook Page
- ⏱️ **Delayed Posting** — Schedule posts to go out after a set number of minutes
- ♿ **Alt Text Support** — Accessibility-friendly alt text for images on all platforms
- ✅ **Input Validation** — Validates caption, image, and platform selection before posting
- 📊 **Google Sheets Logging** — Logs every post (success or failure) to a Google Sheet
- 📲 **Telegram Notifications** — Instant Telegram alerts when a post fails
- 🔒 **Environment Variables** — All secrets stored securely via Docker environment variables
- 🖼️ **imgbb Image Hosting** — Automatically uploads images to imgbb to get a public URL

---

## 🗺️ Workflow Overview

```
[Form Trigger]
      ↓
[Input Validation]
      ↓
[Delay / Immediate Check]
      ↓
[imgbb Upload] → get public image URL
      ↓
[Set Node] → normalize data
      ↓
[Switch Node] → route to platforms
      ↓                    ↓
[Instagram]           [Facebook]
[Create Container]    [Post Photo]
      ↓                    ↓
[Instagram]         [Google Sheets]
[Publish]            [Log Success]
      ↓
[Google Sheets]
[Log Success]
```

---

## 🛠️ Prerequisites

- [Docker](https://docs.docker.com/get-docker/) installed on your machine
- A **Meta Developer App** with the following:
  - Instagram Business Account
  - Facebook Page
  - System User Token with required permissions
- An **imgbb** account (free) for image hosting
- A **Telegram Bot** for notifications
- A **Google Cloud Project** with Sheets API enabled (service account)
- A **Google Sheet** for logging

---

## 📦 Setup

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/n8n-social-poster.git
cd n8n-social-poster
```

### 2. Set up environment variables

Copy the example env file:

```bash
cp .env.example .env
```

Fill in your actual values in `.env`:

```env
IMGBB_API_KEY=your_imgbb_api_key
FB_PAGE_TOKEN=your_facebook_page_token
FB_PAGE_ID=your_facebook_page_id
IG_USER_ID=your_instagram_business_account_id
TELEGRAM_CHAT_ID=your_telegram_chat_id
```

### 3. Start n8n with Docker

```bash
docker compose up -d
```

### 4. Import the workflow

1. Open n8n at `http://localhost:5678`
2. Go to **Workflows → Import**
3. Import `workflow.json` from this repository

### 5. Set up credentials in n8n

You'll need to add these credentials in n8n:

| Credential | Used For |
|---|---|
| Facebook Graph API | Instagram & Facebook posting |
| Google Service Account | Google Sheets logging |
| Telegram API | Error notifications |

> 📖 See the full step-by-step credentials setup guide in [CREDENTIALS.md](./CREDENTIALS.md)

### 6. Share Google Sheet with service account

1. Open your Google Sheet
2. Click **Share**
3. Add your service account email as **Editor**

### 7. Activate the workflow

1. Open the imported workflow
2. Click the **Active** toggle in the top right
3. Your form URL will be shown in the Form Trigger node

---

## 🔐 Required Meta Permissions

Your Meta System User Token needs these permissions:

- `instagram_basic`
- `instagram_content_publish`
- `pages_read_engagement`
- `pages_show_list`
- `pages_manage_posts`

---

## 🌍 Environment Variables

| Variable | Description |
|---|---|
| `IMGBB_API_KEY` | imgbb API key for image hosting |
| `FB_PAGE_TOKEN` | Facebook Page Access Token |
| `FB_PAGE_ID` | Facebook Page ID |
| `IG_USER_ID` | Instagram Business Account ID |
| `TELEGRAM_CHAT_ID` | Telegram Chat ID for notifications |

---

## 📋 Google Sheets Log Structure

The workflow logs every post to a Google Sheet with these columns:

| Date | Caption | Platforms | Image URL | Status | Error |
|---|---|---|---|---|---|
| 24/02/2026 14:30 | My caption | instagram | https://... | ✅ Success | - |
| 24/02/2026 15:00 | My caption | facebook | https://... | ❌ Failed | Error message |

---

## 📱 How to Use

1. Open the form URL from the Form Trigger node
2. Fill in:
   - **Caption** — your post caption (max 2,200 characters)
   - **Image** — upload your image
   - **Platforms** — select Instagram, Facebook, or both
   - **Alt Text** — optional accessibility description
   - **Delay Minutes** — optional delay before posting
3. Click **Submit**
4. Your post will be automatically published to the selected platforms

---

## 🐳 Docker Compose

```yaml
services:
  n8n:
    image: docker.n8n.io/n8nio/n8n:latest
    ports:
      - "5678:5678"
    volumes:
      - ./n8n_data/_data:/home/node/.n8n
    environment:
      - N8N_BLOCK_ENV_ACCESS_IN_NODE=false
      - IMGBB_API_KEY=${IMGBB_API_KEY}
      - FB_PAGE_TOKEN=${FB_PAGE_TOKEN}
      - FB_PAGE_ID=${FB_PAGE_ID}
      - IG_USER_ID=${IG_USER_ID}
      - TELEGRAM_CHAT_ID=${TELEGRAM_CHAT_ID}
    restart: unless-stopped
```

---

## 🤝 Contributing

Feel free to open issues or submit pull requests if you have ideas for improvements!

---

## 📄 License

MIT License — feel free to use this project however you like.

---

## 👤 Author

Built with persistence, a lot of API debugging, and ☕
