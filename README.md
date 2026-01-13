# freemusic

This project is a music downloader utility built to explore **asynchronous Python**, **web scraping**, and **API integration**. It fetches metadata from Spotify, retrieves audio via YouTube, and injects ID3 tags into the resulting MP3 files.

**Note:** This is an educational project for portfolio purposes. It is not intended for deployment or piracy.
---

https://github.com/user-attachments/assets/f0cc0e8f-cb39-48b8-9ee4-34d705812d02
---

## Tech Stack

- **Backend:** Django
    
- **APIs:** Spotipy (Spotify Web API)
    
- **Audio Engine:** `yt-dlp`
    
- **Metadata:** `eyed3`
    
- **Concurrency:** `asyncio` & `asgiref`
    
- **Frontend** Django Templates + Tailwind
---

## Technical Logic

### 1. Asynchronous API Wrapper

Since the standard Spotify library is synchronous, I implemented an `AsyncSpotify` class. By wrapping calls in `sync_to_async`, the application handles data fetching (playlists, tracks, top artists) without blocking the event loop.

### 2. Authentication Flow

The app utilizes Spotify OAuth to access user-specific data. Session tokens are managed to allow browsing of private libraries, recently played tracks, and personalized playlists.

### 3. Download & Metadata Pipeline

The core logic resides in the `download` view, which follows a strict pipeline:

- **Metadata Extraction:** Pulls track titles, artists, and high-res album art from Spotify.
    
- **Sanitization:** Cleans strings to ensure filesystem compatibility across different OS environments.

- **Download** Uses fetched metadata from spotify to query youtube and download audio with yt-dlp. FFMPEG turns audio to mp3.
    
- **Concurrent Execution:** Uses `asyncio.gather` with defined chunk sizes to download audio while managing rate limits and system resources.
    
- **Tagging:** Files are processed via `eyed3` to embed cover art and metadata directly into the MP3.
    
- **Cleanup:** The system bundles files into a ZIP archive for the user and performs an immediate cleanup of the local server storage.
    

---

## Future Improvements

- **Robust Error Handling:** Replacing generic `try/except` blocks with specific exception logging to better track failed downloads or tagging errors.
    
- **Task Queuing:** Migrating long-running download tasks from the request-response cycle to a worker-based system like **Celery**.
    
- **Dynamic Throttling:** Implementing a more sophisticated rate-limiting algorithm to better navigate external site protections.
