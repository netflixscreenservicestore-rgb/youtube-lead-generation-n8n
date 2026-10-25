# YouTube Lead Generation — n8n Workflow

An automated n8n workflow that searches YouTube by keyword, filters
channels by subscriber count and upload activity, extracts contact
and social media links, and saves qualified leads to Google Sheets.

---

## 🔧 What This Workflow Does

1. Takes a search keyword via an n8n Form Trigger
2. Searches YouTube for up to 100 matching channels
3. Pulls full channel stats (subscribers, views, video count, bio)
4. Filters channels by subscriber count range
5. Checks each channel's most recent upload date, filters out
   inactive channels (based on a configurable "posted within X days")
6. Extracts email addresses and social links from the channel bio
7. Scrapes the channel's About page for additional hidden links
   (Instagram, Facebook, Twitter/X, TikTok, LinkedIn, website)
8. Saves the final lead list to Google Sheets, avoiding duplicates

This repo includes **two versions** of the pipeline:
- A **simple version** (fixed subscriber range + fixed 10-day window)
- An **advanced version** (dropdown form lets you choose min/max
  subscribers and "posted within" days at runtime)

---

## 🔄 How It Works (Step by Step)
Form Trigger (Search word [+ optional filters])
|
v
HTTP Request — YouTube Search API (find channels)
|
v
Code — Extract channel IDs
|
v
HTTP Request — YouTube Channels API (get stats + bio)
|
v
Code — Filter by subscriber count, extract email/social from bio
|
v
HTTP Request — YouTube Playlist API (get latest video)
|
v
Code — Filter by upload recency
|
v
HTTP Request — Scrape channel's About page (extra social links)
|
v
Code — Parse scraped HTML for real social/website links
|
v
Append or Update row in Google Sheets (matched by channel name)

---

## ✅ What This Workflow CAN Do

- Search YouTube by any keyword
- Filter leads by subscriber count range
- Filter leads by channel activity/freshness
- Extract email + social links from bios automatically
- Scrape additional hidden social links from the About page
- Avoid duplicate rows in the output sheet

## ❌ What This Workflow CANNOT Do / Limitations

- Relies on scraping YouTube's HTML structure — may break if
  YouTube changes their page layout
- API quota limits apply (YouTube Data API has daily request caps)
- Both pipeline versions write to the same sheet/tab — running
  both back-to-back is redundant (though not harmful)
- No retry/backoff logic if YouTube rate-limits requests

---

## 🚀 Setup Instructions

1. Import `youtube-lead-generation.json` into n8n
2. Replace all instances of `YOUR_YOUTUBE_API_KEY_HERE` with your
   own YouTube Data API v3 key (free from Google Cloud Console)
3. Replace `YOUR_YOUTUBE_LEADS_SHEET_ID_HERE` and
   `YOUR_SHEET_GID_HERE` with your own Google Sheet's document ID
   and tab ID
4. Reconnect your Google Sheets credential
5. Test with one search keyword before running at scale

---

## 📁 Repo Structure
youtube-lead-generation-n8n/
├── README.md
├── youtube-lead-generation.json
└── images/
├── workflow-canvas.png
├── execution-success.png
└── sheet-output-sample.png
