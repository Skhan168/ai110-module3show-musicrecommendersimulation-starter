# 🎧 Model Card: Music Recommender Simulation

## 1. Model Name  

Give your model a short, descriptive name.  
Example: **VibeFinder 1.0**  

---

## 2. Intended Use  

Describe what your recommender is designed to do and who it is for. 

Prompts:  

- What kind of recommendations does it generate  
- What assumptions does it make about the user  
- Is this for real users or classroom exploration  

---

## 3. How the Model Works  

Explain your scoring approach in simple language.  

Prompts:  

- What features of each song are used (genre, energy, mood, etc.)  
- What user preferences are considered  
- How does the model turn those into a score  
- What changes did you make from the starter logic  

Avoid code here. Pretend you are explaining the idea to a friend who does not program.

---

## 4. Data  

Describe the dataset the model uses.  

Prompts:  

- How many songs are in the catalog  
- What genres or moods are represented  
- Did you add or remove data  
- Are there parts of musical taste missing in the dataset  

---

## 5. Strengths  

Where does your system seem to work well  

Prompts:  

- User types for which it gives reasonable results  
- Any patterns you think your scoring captures correctly  
- Cases where the recommendations matched your intuition  

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

Ideas for how you would improve the model next.  

Prompts:  

- Additional features or preferences  
- Better ways to explain recommendations  
- Improving diversity among the top results  
- Handling more complex user tastes  

---

## 9. Personal Reflection  

A few sentences about your experience.  

Prompts:  

- What you learned about recommender systems  
- Something unexpected or interesting you discovered  
- How this changed the way you think about music recommendation apps  
