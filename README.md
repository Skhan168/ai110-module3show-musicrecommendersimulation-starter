# 🎵 Music Recommender Simulation

## Project Summary

In this project you will build and explain a small music recommender system.

Your goal is to:

- Represent songs and a user "taste profile" as data
- Design a scoring rule that turns that data into recommendations
- Evaluate what your system gets right and wrong
- Reflect on how this mirrors real world AI recommenders

Replace this paragraph with your own summary of what your version does.

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



