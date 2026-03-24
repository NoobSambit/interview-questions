# ArmyVerse (Non-Negotiable) - AI Recommendation / Playlist Engine

These answers are based on the implemented ArmyVerse project, but rewritten in simple interview language so they are easier to remember and explain.

---

## 1) Explain how your AI music recommendation system works.

### Short intro
In ArmyVerse, the AI system is mainly an AI playlist generator. The idea is simple: the user describes a vibe or goal, the LLM suggests tracks in a strict JSON format, and then the backend validates and enriches those suggestions using real data sources.

### Step-by-step explanation
1. The user gives a prompt, such as mood, era, members, playlist length, or music preferences.
2. The backend builds a structured prompt with clear rules, like only using official BTS or solo songs and returning a fixed output format.
3. The AI model generates a candidate playlist in JSON.
4. The backend checks whether that JSON is valid. If parsing fails, it returns an error instead of blindly using bad output.
5. In the enhanced flow, each suggested track is matched against the local track database first.
6. If a good local match is not found, the backend uses Spotify search as a fallback.
7. The final response includes real metadata such as Spotify IDs, links, artwork, and sometimes audio features.
8. The result can then be shown to the user, saved in MongoDB, or exported to Spotify.

### Design reasoning
I use the LLM for creativity and curation, but I do not trust it as the final source of truth. Real metadata still comes from the database and Spotify.

### Possible improvement
I would add a stricter mode where only tracks verified in the database are allowed.

### Hard Terms Explained Simply
- `LLM` = the AI model that generates text and suggestions.
- `Enrichment` = adding real metadata after the AI gives its initial suggestions.
- `Source of truth` = the place whose data is treated as the reliable final version.

---

## 2) What is prompt engineering?

### Short intro
Prompt engineering means carefully writing the instructions given to the AI so it produces the kind of answer I actually want.

### Step-by-step explanation
1. I clearly define the task for the AI, such as creating a BTS playlist with specific constraints.
2. I add rules like only using official tracks and returning a fixed number of songs.
3. I force the response into strict JSON so the backend can parse it safely.
4. I also include user preferences like mood, era, member focus, or length.
5. In playlist evolution, I send the current playlist plus a new instruction so the AI can modify the list instead of starting from zero.

### Design reasoning
Without a well-designed prompt, the model can add extra text, break JSON formatting, or suggest tracks that do not fit the product rules.

### Possible improvement
I would version the prompts so I can compare prompt changes safely over time.

### Hard Terms Explained Simply
- `Prompt` = the instruction text sent to the AI.
- `Constraint` = a rule the AI must follow.
- `Version` = a tracked variation of the prompt so changes can be compared later.

---

## 3) What acoustic parameters did you use?

### Short intro
I used Spotify-style audio features like danceability, energy, valence, and tempo to help the playlist feel closer to the requested vibe.

### Step-by-step explanation
1. The track model stores audio-related features for songs.
2. These include values like danceability, energy, valence, tempo, acousticness, and more.
3. When possible, user preferences or seed tracks are translated into these vibe signals.
4. The backend then reflects those signals in the prompt in simple language, like highly danceable or low-energy emotional tracks.
5. In some flows, the response can also include values like BPM so the UI has richer information to show.

### Design reasoning
These features are useful because they connect technical music metadata to something the user can actually feel, like mood or intensity.

### Possible improvement
I would use those audio features more strongly in post-processing and ranking, not only in the prompt.

### Hard Terms Explained Simply
- `Valence` = how happy or sad a song feels.
- `Danceability` = how suitable a song feels for dancing.
- `Tempo` = the speed of the track, usually measured in beats per minute.

---

## 4) What is temperature in LLM?

### Short intro
Temperature controls how random or creative the AI behaves. Higher temperature means more variety, while lower temperature means more stable and predictable output.

### Step-by-step explanation
1. For playlist generation, I use a moderate temperature so the model stays creative but still follows structure.
2. If the temperature is too low, the output may become repetitive or too rigid.
3. If it is too high, the output may become inconsistent and JSON reliability may drop.
4. For more poetic or creative text, I can use a higher temperature than I would for strict structured playlist JSON.

### Design reasoning
Different AI tasks need different levels of creativity. Structured playlist generation needs reliability more than pure imagination.

### Possible improvement
I would tune temperature separately for each endpoint based on user feedback and parse-failure rate.

### Hard Terms Explained Simply
- `Temperature` = the creativity setting for the model.
- `Parse-failure rate` = how often the AI output fails to match the expected format.

---

## 5) What is hallucination?

### Short intro
Hallucination is when the AI confidently returns something that is wrong, such as a song title that does not actually exist on Spotify.

### Step-by-step explanation
1. The model generates track suggestions.
2. Some of those suggestions may be inaccurate, unofficial, or completely made up.
3. If the backend trusted them blindly, users would see wrong songs or exports would fail.
4. So hallucination is a real practical issue in playlist generation, not just a theory question.

### Design reasoning
Music recommendation is especially vulnerable to this because similar track names, unofficial uploads, and remembered-but-wrong titles are common.

### Possible improvement
I would add a replacement step that forces the system to swap any invalid track for a verified one.

### Hard Terms Explained Simply
- `Hallucination` = when the AI makes something up or gets it wrong while sounding confident.
- `Verified` = checked against a real trusted source.

---

## 6) How did you reduce hallucination?

### Short intro
I reduced hallucination by combining prompt constraints with real-world validation after generation. In short, I do not rely on the model alone.

### Step-by-step explanation
1. The prompt clearly says to use only official songs that actually exist on Spotify.
2. The output is forced into JSON so there is less extra noise.
3. After generation, the backend checks each suggested track against the local track database if possible.
4. If the local database does not confirm it, the system tries Spotify search.
5. This means the model can suggest creatively, but the final result is still checked against real sources.
6. If exact matches are weak, the system can still return a safer fallback like a search link instead of pretending everything is certain.

### Design reasoning
Prompting helps, but prompting alone is not enough. The real reduction in hallucination comes from validating the AI output against external truth.

### Possible improvement
I would track how often suggestions fail validation and automatically retry replacement for bad tracks.

### Hard Terms Explained Simply
- `Ground truth` = the real trusted data source, like Spotify or the local DB.
- `Fallback` = backup behavior used when the preferred path does not work.
- `Validation` = checking whether output is correct before trusting it.

---

## 7) What is context window?

### Short intro
The context window is the amount of text the model can consider in one request. If I send too much, quality can drop or important parts may be ignored.

### Step-by-step explanation
1. The AI can only read a limited amount of input at once.
2. In playlist evolution, I cannot keep sending huge playlist histories forever.
3. So I cap how much playlist information goes into the prompt.
4. For example, I may include only the most relevant tracks and summarize the rest.
5. This keeps the prompt focused and helps the AI respond more reliably.

### Design reasoning
Compact prompts are easier for the model to handle and usually give more stable results.

### Possible improvement
I would add a dedicated summarizer that turns large playlists into a stable vibe summary before sending them back to the AI.

### Hard Terms Explained Simply
- `Context window` = how much text the AI can read and use in one request.
- `Summarize` = reduce a lot of text into a smaller but meaningful version.

---

## 8) What is token limit?

### Short intro
The token limit is the maximum amount of output text I allow the model to generate in one call.

### Step-by-step explanation
1. AI models break text into smaller units called tokens.
2. I set `max_tokens` so the response cannot grow too large.
3. Smaller endpoints use lower limits.
4. More detailed endpoints can use higher limits.
5. This keeps output size under control and makes JSON parsing safer.

### Design reasoning
Bounded outputs make the system more stable. Without output limits, the model can become too verbose and create parsing or UX problems.

### Possible improvement
I would compute the output limit dynamically based on playlist size and expected JSON complexity.

### Hard Terms Explained Simply
- `Token` = a small text unit used by AI models.
- `max_tokens` = the cap on how much the model is allowed to generate.

---

## 9) Why Groq instead of OpenAI?

### Short intro
In this project, I used Groq because the implementation was already built around it, and it gave a practical balance of speed and cost for a student-scale AI feature.

### Step-by-step explanation
1. The playlist routes create a Groq client using the project API key.
2. They call the configured LLM with endpoint-specific settings like temperature and token limits.
3. Fast responses matter in a user-facing playlist feature, because long waits make the experience feel weak.
4. So Groq was a practical choice for the current project stage.

### Design reasoning
This was mainly an implementation and cost-speed decision, not a claim that one provider is universally best.

### Possible improvement
I would wrap the provider behind an adapter so switching models later becomes easier.

### Hard Terms Explained Simply
- `Inference` = the moment when the model generates the answer.
- `Provider` = the company or API that hosts the model.
- `Adapter` = a wrapper that hides provider-specific details behind one common interface.
