# 🎵 Music Recommender Simulation

## Project Summary

This project simulates a simplified content-based music recommender. It represents each song using genre, mood, and energy attributes, compares them against a user's stated taste profile, and produces a ranked list of the top-k best-matching songs along with plain-language reasons for each recommendation.

I tested a weight-shift experiment where I halved the genre match weight (2.0 → 1.0) and doubled the maximum energy closeness score (1.0 → 2.0). The top recommendation for each of my three test profiles stayed the same, but the middle of the rankings shifted — for example, in the High-Energy Pop profile, a song matching only mood (Rooftop Lights) jumped ahead of a song matching only genre (Gym Hero) once energy and mood were weighted more heavily relative to genre. This showed that the ranking is sensitive to weight choices even when the top pick is stable. Full details and side-by-side output are in `model_card.md` under Evaluation.

This project showed me that recommenders don't need to be complicated to feel personalized — a handful of weighted comparisons between song attributes and a user profile was enough to produce results that matched my own musical intuition. It also made clear where bias can creep in: because the dataset isn't evenly distributed across genres, and because genre carries the most weight in scoring, users with less common tastes get less varied, more repetitive recommendations. That's a small-scale version of the same filter-bubble problem real platforms deal with at a much larger scale.
---

## How The System Works

Real music platforms like Spotify use two main approaches to recommend songs. Collaborative filtering predicts what a user will like based on the listening behavior of other similar users, without needing to know anything about the songs themselves. Content-based filtering instead looks at the actual attributes of a song (genre, mood, energy, tempo, etc.) and compares them to a user's stated preferences. This project builds a content-based recommender.

My system represents each song using the following features from `songs.csv`: `genre`, `mood`, and `energy`. These three were chosen because they showed the strongest, most distinct signal across the dataset, while features like `valence` and `danceability` overlapped heavily with `mood` and `energy`, and `tempo_bpm` would need separate normalization to combine with the other 0-1 scaled features.

The `UserProfile` stores a target `genre`, target `mood`, and target `energy` value. The `Recommender` scores each song by awarding +2.0 points for an exact genre match, +1.0 point for an exact mood match, and additional points based on how close the song's energy is to the user's target energy (closer = more points, rather than simply rewarding higher energy values). All songs are scored this way and then ranked from highest to lowest score to produce the top-k recommendations.

---

## Getting Started

### Setup

1. Create a virtual environment (optional but recommended):

   ```bash
   python -m venv .venv
   source .venv/bin/activate      # Mac or Linux
   .venv\Scripts\activate         # Windows

2. Install dependencies

```bash
pip install -r requirements.txt
```

3. Run the app:

```bash
python -m src.main
```

### Running Tests

Run the starter tests with:

```bash
pytest
```

You can add more tests in `tests/test_recommender.py`.

---

## Sample Recommendation Output

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
```

**Screenshot or video** *(optional)*: <!-- Insert a screenshot or demo video link here -->

---

## Experiments You Tried

Use this section to document the experiments you ran. For example:

- What happened when you changed the weight on genre from 2.0 to 0.5
- What happened when you added tempo or valence to the score
- How did your system behave for different types of users

---

## Limitations and Risks

Summarize some limitations of your recommender.

Examples:

- It only works on a tiny catalog
- It does not understand lyrics or language
- It might over favor one genre or mood

You will go deeper on this in your model card.

---

## Reflection

Read and complete `model_card.md`:

[**Model Card**](model_card.md)

Write 1 to 2 paragraphs here about what you learned:

- about how recommenders turn data into predictions
- about where bias or unfairness could show up in systems like this



