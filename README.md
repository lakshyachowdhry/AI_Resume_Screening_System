## AI Resume Screening System

This project is a complete, end-to-end AI-powered resume screening system suitable for a final-year college project. It demonstrates PDF parsing, NLP preprocessing, TF-IDF similarity, skill extraction and gap analysis, supervised learning, and a fully interactive Streamlit UI.

### Features

- **Multi-resume upload**: Upload multiple PDF resumes at once.
- **PDF text extraction**: Uses `PyPDF2` and handles pages with no extractable text.
- **NLP preprocessing**: Lowercasing, tokenization, stopword removal, and punctuation removal with NLTK.
- **Skill extraction**: Dictionary-based detection of technical and soft skills from both resumes and job descriptions.
- **TF-IDF + cosine similarity**: Compares resumes against the job description.
- **Supervised learning model**: Logistic Regression trained on a small labeled dataset.
- **Score breakdown**: Similarity score, skill match score, optional model probability, and final weighted score.
- **Skill gap analysis**: Required vs. detected skills with missing skills listed.
- **Explanation engine**: Human-readable explanation of why each candidate is scored as they are.
- **Duplicate detection**: Resume-to-resume similarity to flag near duplicates.
- **Top candidate highlight**: Best candidate is prominently displayed.
- **Logging**: Key events are logged to `outputs/logs.txt`.

### Project Structure

- `app.py` — Streamlit UI for end-to-end demo.
- `parser.py` — PDF text extraction using `PyPDF2`.
- `preprocess.py` — Text preprocessing and tokenization.
- `skills.py` — Skill extraction and gap analysis.
- `similarity.py` — TF-IDF vectorization and cosine similarity.
- `model.py` — Training pipeline for Logistic Regression and model persistence.
- `evaluator.py` — Accuracy, precision, recall, and F1 evaluation.
- `utils.py` — Helper utilities and score combination logic.
- `data/dataset.csv` — Example dataset for model training.
- `model/` — Saved model and vectorizer files (`model.pkl`, `vectorizer.pkl`).
- `outputs/logs.txt` — Plain-text logs of training and UI actions.

### Installation

1. Create and activate a virtual environment (recommended):

```bash
python -m venv .venv
.venv\Scripts\activate  # On Windows
```

2. Install dependencies:

```bash
pip install -r requirements.txt
```

### Training the Model

The model is a Logistic Regression classifier that predicts whether a resume matches a job description.

- **Input format** (`data/dataset.csv`):
  - `Resume_Text`: Raw resume text.
  - `Job_Description`: Raw job description text.
  - `Label`: `1` for good match, `0` for poor match.

To train and save the model and vectorizer:

```bash
python model.py
```

After training:

- `model/model.pkl` — Trained Logistic Regression model.
- `model/vectorizer.pkl` — TF-IDF vectorizer used at training time.

You can evaluate the model using:

```bash
python evaluator.py
```

or via the Streamlit sidebar button ("Run evaluation on training dataset").

### Running the Streamlit App

Start the UI with:

```bash
streamlit run app.py
```

Then:

1. Open the web browser link shown in the terminal.
2. In the sidebar, paste or select a job description.
3. Upload multiple resume PDFs in the main area.
4. Click **Analyze Candidates**.

You will see:

- A highlighted **Top Candidate** with explanation.
- A **ranking table** with similarity, skills, model probability, and final score.
- A **bar chart** of final scores.
- An expandable **detail section** per candidate with:
  - Skills found.
  - Missing skills (gaps).
  - Score breakdown.
  - Short text summary.
  - Natural language explanation.
- A **duplicate resumes** section if near-duplicates are detected.
- A **Download results as CSV** button.

### Deployment

The application can be deployed in several ways:

#### Option 1: Streamlit Cloud (Recommended for quick deployment)

1. Push your code to a GitHub repository
2. Go to [share.streamlit.io](https://share.streamlit.io)
3. Connect your GitHub account and select the repository
4. Set the main file path to `app.py`
5. Add your `GEMINI_API_KEY` in the secrets section
6. Deploy!

#### Option 2: Docker Deployment

1. Build the Docker image:
   ```bash
   docker build -t ai-resume-screening .
   ```

2. Run with Docker Compose:
   ```bash
   export GEMINI_API_KEY="your-api-key-here"
   docker-compose up -d
   ```

3. Access the app at `http://localhost:8501`

#### Option 3: Heroku Deployment

1. Install Heroku CLI
2. Create a Heroku app:
   ```bash
   heroku create your-app-name
   ```

3. Set environment variables:
   ```bash
   heroku config:set GEMINI_API_KEY="your-api-key-here"
   ```

4. Deploy:
   ```bash
   git push heroku main
   ```

#### Option 4: Other Cloud Platforms

The app can also be deployed to:
- AWS EC2
- Google Cloud Run
- Azure Container Instances
- Railway
- Render

For these platforms, use the provided Dockerfile and set the `GEMINI_API_KEY` environment variable.

### Environment Variables

- `GEMINI_API_KEY`: Your Google Gemini API key (required for AI features)

### Model Files

The app works without trained models (using only similarity and skills), but for full functionality:

1. Ensure you have `data/dataset.csv` with training data
2. Run `python model.py` to train and save the model
3. The `model.pkl` and `vectorizer.pkl` files will be created

### Demo Preparation Tips

- Prepare 3–5 PDF resumes and at least 2 job descriptions (e.g., Data Scientist, AI/ML Engineer).
- Run `python model.py` once before your demo so that the model files exist.
- Show the evaluation metrics in the sidebar to demonstrate accuracy, precision, recall, and F1.
- Walk through the code modules:
  - `parser.py` for PDF extraction.
  - `preprocess.py` for NLP preprocessing.
  - `skills.py` for skill extraction and gap analysis.
  - `similarity.py` for TF-IDF and cosine similarity.
  - `model.py` and `evaluator.py` for the supervised learning part.
  - `app.py` for the UI and integration logic.

