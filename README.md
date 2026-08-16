# Vigilante — Personalized Email Importance Classifier

Connects to a Gmail inbox (read-only) and classifies each email as
**IMPORTANT** or **UNIMPORTANT** using a machine-learning model trained on
6,000 real emails.

**Author:** Aryan Sharma · TIET - Computer Engg - 1024030679

---

## What it does

The model is a **logistic regression** classifier using:
- Engineered features — sender type, unsubscribe headers, reply markers, subject length
- **TF-IDF** features on email subject lines

Training labels were derived via an **intent-based bucketing** method
(transactional / personal mail → important; marketing / newsletters /
promotions → unimportant). Achieves **~0.90 recall** on the important class.

> **Privacy:** Access is **read-only**. The app can only read email metadata
> (sender, subject, headers) — it cannot modify, send, or delete any email,
> and it never reads message bodies.

---

## Repository contents

| File | Description |
|------|-------------|
| `VigilanteStage0` | The notebook to run |
| `models/vigilante_model2.pkl` | Trained model |
| `models/vigilante_vectorizer2.pkl` | TF-IDF vectorizer |
| `models/vigilante_scaler2.pkl` | Feature scaler |
| `models/vigilante_metadata2.json` | Model configuration |
| `README.md` | This file |

> **Note:** `credentials.json` (the Google OAuth client) is **not** included
> in this repo for security reasons. See "Getting access" below.

---

## Getting access to run it

Because this app uses Gmail OAuth and is kept in Google's **testing mode**
(not publicly published), running it requires two things from me:

1. Your Google email added as an **authorized test user**
2. The `credentials.json` OAuth file

**If you want to test it on your own inbox — just contact me
([your email / contact]).** I'll add your email as a test user and send you
`credentials.json`, and you'll be able to run it in a couple of minutes.



---

## How to run (once you have credentials.json)

   1. Open `VigilanteStage0` in
   [Google Colab](https://colab.research.google.com).
2. Run the cells in order (**Runtime → Run all**).
3. When prompted, **upload the 4 model files** from the `models/` folder
   (select all 4 together).
4. When prompted, **upload `credentials.json`** (the file I sent you).
5. **Authorize Gmail access:**
   - Open the printed sign-in URL, sign in with your Gmail.
   - You'll see **"Google hasn't verified this app"** — this is expected for
     an academic project and is safe (read-only). Click
     **Advanced → Go to Vigilante Assignment (unsafe) → Continue → Allow**.
   - Copy the authorization code shown, paste it into the notebook, press Enter.
6. The notebook fetches your inbox, classifies each email, prints a summary +
   results table, and downloads a CSV.

---

## What you'll see

- The connected email account
- A **classification summary** — counts and percentages of important vs
  unimportant, average confidence scores, and top important emails
- A **full table** of every email with its verdict and score
- A downloadable **CSV** of results

---

## Notes

- Change `MAX_EMAILS` in the fetch cell to classify more/fewer emails (default 50).
- The model reflects its training inbox: strongest on financial/transactional
  and personal mail, and intentionally conservative (prioritizes not hiding
  important mail).

---

## How it was built

The model was trained through an iterative process:
1. A baseline was first trained on the public **SpamAssassin** corpus, which
   revealed limitations on modern email (dated vocabulary, weak personal-mail
   signal).
2. A second model was then trained on **6,000 real emails** (3,000 personal +
   3,000 institutional), labeled via intent-based bucketing.
3. Recall on the important class improved across data-scaling stages
   (1,200 → 3,000 → 6,000 emails): **0.72 → 0.87 → 0.90**.
