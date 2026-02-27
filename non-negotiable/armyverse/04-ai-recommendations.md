# ArmyVerse (Non‑Negotiable) — AI Recommendation / Playlist Engine

These answers are based on implemented files like:
- `app/api/playlist/generate/route.ts`
- `app/api/playlist/generate-enhanced/route.ts`
- `app/api/playlist/evolve/route.ts`
- `lib/models/Track.ts`, `lib/models/Playlist.ts`

---

## 1) Explain how your AI music recommendation system works.

### Short intro
In ArmyVerse, the AI feature is an AI playlist generator. The core idea is: user describes a vibe, the LLM returns songs in a strict JSON format, and then the backend enriches/validates tracks using the database and Spotify search.

### Step-by-step explanation
1. User enters a prompt (vibe / intent) and optional constraints (mood, era, members, length, audio feature sliders).
2. Backend builds a strong prompt with constraints like:
   - only official BTS/solo songs
   - return exact number of tracks
   - output strict JSON
3. Backend calls Groq `llama-3.3-70b-versatile` with parameters like `temperature` and `max_tokens`.
4. Backend parses JSON response. If parsing fails, it returns an error instead of silently showing wrong data.
5. In the enhanced generator, backend tries to match each AI suggestion with:
   - track in MongoDB `Track` collection (best case)
   - else Spotify search fallback
6. Backend returns a final list that includes `spotifyId`, `spotifyUrl`, album art, and sometimes audio features.
7. Optionally, it can save the playlist in MongoDB `Playlist` model and later export it to Spotify.

### Design reasoning (why I did it)
- The LLM is good at “creative selection” but not always perfect on exact IDs.
- So I use the LLM for *suggestion*, and then use DB/Spotify for *ground truth enrichment*.
- Strict JSON output makes the system predictable for UI.

### Possible improvement (1–2 lines)
Add a stricter “only allow songs that exist in DB” mode to reduce Spotify search errors and reduce hallucinated titles.

### Hard Terms Explained Simply
- `LLM` = AI model that generates text.
- `Enrichment` = adding real metadata (IDs, URLs) after AI output.

---

## 2) What is prompt engineering?

### Short intro
Prompt engineering is how I write the instructions so the model gives the output I want (format + constraints), not random chat text.

### Step-by-step explanation
1. I clearly tell the model what the task is (“curate a BTS playlist”).
2. I add strict rules (“ONLY officially released songs on Spotify”).
3. I force output format (“Return ONLY JSON in this shape”).
4. I add extra constraints from the user (moods, eras, member bias, length).
5. In the evolve endpoint, I include the current playlist and the new instruction so the model can modify the list.

### Design reasoning (why I did it)
- Without strict prompts, models add extra text and break parsing.
- Constraints reduce wrong/imaginary tracks.

### Possible improvement (1–2 lines)
Store prompt versions so I can compare quality changes safely over time.

### Hard Terms Explained Simply
- `Prompt` = instruction text you send to the AI.
- `Constraint` = a rule the model must follow.

---

## 3) What acoustic parameters did you use?

### Short intro
I use Spotify-style audio features stored in the `Track` model. In UI, users mainly control danceability and valence (mood), and the system can also track energy and tempo.

### Step-by-step explanation
1. Track model supports audio features like:
   - `danceability`, `energy`, `valence`, `tempo`, `acousticness`, `instrumentalness`, etc.
2. When seed tracks exist, the prompt includes their energy/danceability to help the model match vibe.
3. When the user sets audio feature preferences, the prompt translates it into simple language (example: “highly danceable”, “melancholic”).
4. For some flows, the response includes BPM (`tempo`) so UI can show it.

### Design reasoning (why I did it)
- These features are easy to explain and map nicely to “vibe”.
- They also help make results feel less random.

### Possible improvement (1–2 lines)
Use audio features more strongly in post-processing (ranking/reordering) instead of only using them inside prompts.

### Hard Terms Explained Simply
- `Valence` = how happy/sad the track feels.
- `Danceability` = how suitable the track is for dancing.

---

## 4) What is temperature in LLM?

### Short intro
Temperature controls randomness. Higher temperature = more creative but less consistent. Lower temperature = more predictable.

### Step-by-step explanation
1. In playlist generation routes, I send `temperature` (often around 0.7).
2. That gives a balance: not too boring, but still stable enough for JSON formatting.
3. In the “inspiration” endpoint (short poetic text), temperature is higher (more creative).

### Design reasoning (why I did it)
- For structured JSON outputs, I need consistency.
- For creative short descriptions, I want more variety.

### Possible improvement (1–2 lines)
Tune temperature per endpoint based on real user feedback and parse-failure rates.

### Hard Terms Explained Simply
- `Temperature` = creativity knob.
- `Consistent output` = same prompt gives similar structure each time.

---

## 5) What is hallucination?

### Short intro
Hallucination is when the model confidently outputs something false, like a track name that does not exist on Spotify.

### Step-by-step explanation
1. Model suggests tracks.
2. Some suggestions may be wrong or non-existent.
3. If we trust them blindly, export/search fails or user sees wrong results.

### Design reasoning (why I did it)
- This is a real issue in music lists because many unofficial names exist.

### Possible improvement (1–2 lines)
Add a “verification stage” that rejects any track not found in DB/Spotify and asks model to replace it.

### Hard Terms Explained Simply
- `Hallucination` = AI made-up output.
- `Verification` = checking output against real data.

---

## 6) How did you reduce hallucination?

### Short intro
I reduce hallucination mainly by constraints in the prompt and by validating/enriching the output using DB and Spotify search.

### Step-by-step explanation
1. Prompt says “ONLY include songs that actually exist on Spotify and are officially released”.
2. Output must be JSON, so extra chat text is reduced.
3. Enhanced generator tries to match AI suggestions with the `Track` database first.
4. If not found, it uses Spotify search fallback and still returns a safe link (search URL) if exact track isn’t found.

### Design reasoning (why I did it)
- Prompt-only control is not enough.
- DB/Spotify lookup acts like reality-check.

### Possible improvement (1–2 lines)
Track “not found” rate and automatically re-run generation to replace missing songs.

### Hard Terms Explained Simply
- `Ground truth` = the real source of truth (Spotify/DB).
- `Fallback` = backup behavior when primary lookup fails.

---

## 7) What is context window?

### Short intro
Context window is how much text the model can consider at once. If you send too much, some part is ignored.

### Step-by-step explanation
1. In “evolve playlist”, I include up to 20 tracks in the prompt and summarize the rest.
2. This keeps prompt size reasonable so the model focuses on key information.
3. If you send the whole world, quality drops.

### Design reasoning (why I did it)
- Keeping prompts compact improves reliability and speed.

### Possible improvement (1–2 lines)
Add a smarter summarizer to compress long playlists into a stable “vibe summary” instead of listing titles.

### Hard Terms Explained Simply
- `Context window` = how much the AI can read at once.
- `Summarize` = compress big text into smaller meaning.

---

## 8) What is token limit?

### Short intro
Token limit controls how long the model’s response can be. I set `max_tokens` in Groq calls.

### Step-by-step explanation
1. For simple playlist generation, I use a smaller `max_tokens` (example: 1024).
2. For more detailed routes, I allow more (example: 2048).
3. This keeps responses bounded and avoids huge unexpected outputs.

### Design reasoning (why I did it)
- Predictable outputs make parsing and UI stable.

### Possible improvement (1–2 lines)
Calculate `max_tokens` dynamically based on playlist length and expected JSON size.

### Hard Terms Explained Simply
- `Token` = chunk of text used by AI models.
- `max_tokens` = cap on output length.

---

## 9) Why Groq instead of OpenAI?

### Short intro
In this repo, the implementation uses Groq SDK. It was a practical choice for speed and cost at student scale.

### Step-by-step explanation
1. Playlist routes create a Groq client using `GROQ_API_KEY`.
2. They call `llama-3.3-70b-versatile`.
3. I tune temperature and token caps per endpoint.

### Design reasoning (why I did it)
- Fast inference helps UX.
- It keeps the project workable without high recurring cost.

### Possible improvement (1–2 lines)
Wrap the AI provider behind an adapter interface (like in DocBuilder) so switching providers is easier.

### Hard Terms Explained Simply
- `Inference` = model generating an output.
- `Provider` = company/API hosting the model.
