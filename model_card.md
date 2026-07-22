# 🎧 Model Card: Music Recommender Simulation

## 1. Model Name  

VibeMatch 1.0 

---

## 2. Intended Use  

This recommender is designed for classroom exploration of how content-based music recommendation works. It takes a simple user taste profile (favorite genre, favorite mood, target energy) and suggests songs from a small fixed catalog that match those preferences. It assumes the user's taste can be reasonably described using just genre, mood, and energy, and it is not meant for real-world deployment with real users or a large-scale music catalog.

---

## 3. How the Model Works  

Each song has attributes like genre, mood, and energy. A user profile states a favorite genre, favorite mood, and target energy level. The system compares each song to the user's profile: it awards 2 points if the genre matches exactly, 1 point if the mood matches exactly, and up to 1 point based on how close the song's energy is to the user's target energy (the closer the match, the more points). Every song in the catalog is scored this way, then the songs are sorted from highest to lowest score, and the top few are shown to the user along with the specific reasons for each score.

---

## 4. Data  

The catalog contains 10 songs with the following attributes: title, artist, genre, mood, energy, tempo_bpm, valence, danceability, and acousticness. Genres represented include pop, lofi, rock, ambient, jazz, synthwave, and indie pop. Moods represented include happy, chill, intense, relaxed, moody, and focused. No songs were added or removed from the starter dataset. Musical taste factors that are missing include lyrical content, vocal style, instrumentation, and cultural or regional genre distinctions, none of which the current features capture.

---

## 5. Strengths  

The system gives clear, sensible results for users whose stated genre and mood are well represented in the catalog, such as a High-Energy Pop or Chill Lofi profile, where multiple close matches exist. The scoring correctly rewards exact genre and mood matches while still giving partial credit to songs that are close in energy even without a full match, which matched my own intuition about how "vibe" works. The explanation output (the list of reasons) makes it easy to see exactly why a song was recommended, which is more transparent than a black-box score alone.

---

## 6. Limitations and Bias 

The dataset is small (10 songs) and pop is the most represented genre, appearing 3 times, while genres like jazz and ambient each appear only once. This means a user who prefers pop gets more variety in their recommendations, while a user who prefers jazz or ambient will always see the same 1-2 songs at the top regardless of their mood or energy preference, since there simply aren't more options in that genre to rank against.

Because genre match is weighted the heaviest (+2.0), the system tends to overfit to whichever genre the user names, even if a song from a different genre would be a closer overall match on mood and energy. This creates a small-scale filter bubble: users are rarely shown songs outside their stated genre, even when those songs might match their mood or energy more closely.

The system also does not consider features like tempo, valence, danceability, or acousticness in scoring, even though they are present in the dataset. This means two songs with wildly different tempos or acoustic qualities could receive the same score if their genre, mood, and energy happen to match, which may not reflect a real user's full sense of "vibe."

---

## 7. Evaluation  

I tested the recommender with three distinct user profiles to see how the scoring logic responds to different tastes: High-Energy Pop, Chill Lofi, and Deep Intense Rock.

### Stress Test Results

=== High-Energy Pop ===
Top recommendations:
Sunrise City - Score: 3.98
Because: genre match (+2.0), mood match (+1.0), energy closeness (+0.98)
Gym Hero - Score: 2.87
Because: genre match (+2.0), energy closeness (+0.87)
Rooftop Lights - Score: 1.96
Because: mood match (+1.0), energy closeness (+0.96)
Night Drive Loop - Score: 0.95
Because: energy closeness (+0.95)
Storm Runner - Score: 0.89
Because: energy closeness (+0.89)

=== Chill Lofi ===
Top recommendations:
Library Rain - Score: 3.95
Because: genre match (+2.0), mood match (+1.0), energy closeness (+0.95)
Midnight Coding - Score: 3.88
Because: genre match (+2.0), mood match (+1.0), energy closeness (+0.88)
Focus Flow - Score: 2.90
Because: genre match (+2.0), energy closeness (+0.9)
Spacewalk Thoughts - Score: 1.98
Because: mood match (+1.0), energy closeness (+0.98)
Coffee Shop Stories - Score: 0.93
Because: energy closeness (+0.93)

=== Deep Intense Rock ===
Top recommendations:
Storm Runner - Score: 3.99
Because: genre match (+2.0), mood match (+1.0), energy closeness (+0.99)
Gym Hero - Score: 1.97
Because: mood match (+1.0), energy closeness (+0.97)
Sunrise City - Score: 0.92
Because: energy closeness (+0.92)
Rooftop Lights - Score: 0.86
Because: energy closeness (+0.86)
Night Drive Loop - Score: 0.85
Because: energy closeness (+0.85)

For each profile, the top result was always a song matching both genre and mood exactly, confirming the +2.0/+1.0 weighting works as intended. What I looked for was whether songs with only partial matches (e.g., mood match but no genre match) would still rank reasonably, and they did — they consistently landed in the middle of the top 5 rather than at the very top or bottom.

One thing worth comparing directly: "Gym Hero" (pop/intense/0.93 energy) ranked #2 for High-Energy Pop (genre match, no mood match) and #2 again for Deep Intense Rock (mood match, no genre match). This makes sense — its high energy and "intense" attributes give it broad appeal across profiles that value intensity, even without an exact genre match. Comparing Chill Lofi to Deep Intense Rock, the two profiles produced almost entirely non-overlapping top 5 lists, which is expected since lofi/chill songs sit at the opposite end of the energy scale from rock/intense songs.

No numeric error metrics were used — evaluation here is qualitative, based on whether the ranked lists match musical intuition.

---

## 8. Future Work  

If I continued developing this, I would add tempo and valence as secondary scoring features with smaller weights, since they carry some signal but shouldn't outweigh genre, mood, and energy. I would also expand the catalog so that underrepresented genres like jazz and ambient have more songs to compete for a top spot, reducing the small-scale filter bubble effect. Finally, I would add a diversity mechanism to the ranking step so the top 5 doesn't always cluster around the same genre when a user's taste is narrow.

---

## 9. Personal Reflection  

My biggest learning moment was seeing how a small change in weighting (doubling energy, halving genre) shifted the middle of the rankings without changing the top pick, which showed me how sensitive recommendation systems can be to arbitrary weight choices. AI tools helped me quickly generate working code for loading CSV data and scoring songs, but I had to double check the exact key names being used (like "genre" vs "favorite_genre") since mismatches there caused real bugs. What surprised me most was how "smart" the recommendations felt even though the underlying logic is just simple arithmetic on a few numeric and string comparisons — it made me think that a lot of the "personalization" feeling in real apps like Spotify might come from similarly simple rules applied at a much larger scale.
