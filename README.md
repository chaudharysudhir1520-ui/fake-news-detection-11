# Fake News Detection Web Application

This project is a complete fake news detection system built with:

- Frontend: HTML, CSS, JavaScript
- Backend: Node.js, Express
- Machine Learning: Python, TF-IDF, Logistic Regression

The app allows a user to paste news text into a web interface and receive a prediction of `Fake` or `Real` with a confidence score.

## Project Structure

```text
fake-news-detector/
|-- backend/
|   |-- controllers/
|   |   `-- predictController.js
|   |-- routes/
|   |   `-- predictRoutes.js
|   `-- server.js
|-- dataset/
|   `-- sample_news.csv
|-- frontend/
|   |-- index.html
|   |-- script.js
|   `-- style.css
|-- ml-model/
|   |-- predict.py
|   |-- preprocess.py
|   `-- train_model.py
|-- package.json
|-- README.md
`-- requirements.txt
```

## How the ML Model Works

1. The dataset contains labeled news examples with `fake` and `real` classes.
2. The text is preprocessed:
   - convert to lowercase
   - remove punctuation and non-letter characters
   - remove stopwords
   - apply lightweight stemming
3. The cleaned text is transformed into numbers using `TF-IDF Vectorizer`.
4. A `Logistic Regression` classifier learns patterns from the vectorized training data.
5. During prediction, the same preprocessing and vectorization steps are applied before the saved model makes a classification.

## How Node.js Communicates with Python

1. The frontend sends a `POST` request to `/predict`.
2. The Express backend receives the text.
3. The backend uses Node.js `child_process.spawn()` to run `ml-model/predict.py`.
4. The Python script loads `model.pkl` and `vectorizer.pkl`, predicts the label, and prints JSON.
5. Node.js reads the JSON output and sends it back to the frontend.

## Setup Instructions

### 1. Install Node.js dependencies

```bash
npm install
```

### 2. Install Python dependencies

```bash
pip install -r requirements.txt
```

### 3. Train the ML model

```bash
python ml-model/train_model.py
```

This creates:

- `ml-model/model.pkl`
- `ml-model/vectorizer.pkl`
- `ml-model/metrics.json`

### 4. Start the backend server

```bash
npm start
```

Open `http://localhost:5000` in your browser.

## API Endpoint

### `POST /predict`

Request body:

```json
{
  "text": "Paste your news text here"
}
```

Response body:

```json
{
  "prediction": "Fake",
  "confidence": "92.15%"
}
```

## Model Evaluation

After training, the script prints and saves:

- Accuracy
- Precision
- Recall
- F1-score
- Classification report

## Deployment Plan

### Frontend

- Vercel
- Netlify

### Backend

- Render
- Railway

### Python ML

- Bundle the Python scripts and saved model with the backend service
- Or deploy Python inference as a separate microservice if you want to scale it independently

## Notes About the Dataset

- A small starter dataset is included at `dataset/sample_news.csv` so the project runs immediately.
- For better real-world accuracy, replace it with a much larger dataset containing labeled fake and real news samples.
- Your CSV file should contain exactly two columns: `text` and `label`.
- The training script avoids heavy dataframe and NLP dependencies so it runs more reliably in local environments.

## Viva Questions and Answers

### 1. What is fake news detection?
Fake news detection is a classification task where a model predicts whether a news article is misleading or legitimate based on text patterns.

### 2. Why do we preprocess text?
Preprocessing removes noise and standardizes the text so the model can learn meaningful patterns more effectively.

### 3. What is TF-IDF?
TF-IDF converts text into numeric vectors by measuring how important a word is in one document relative to the full dataset.

### 4. Why use Logistic Regression here?
It is fast, interpretable, performs well for text classification baselines, and supports probability scores for confidence output.

### 5. Why save the model and vectorizer separately?
The vectorizer transforms raw text into features, and the model uses those features to predict labels. Both are needed at inference time.

### 6. How does Express call Python?
Express uses `child_process.spawn()` to run the Python script as a separate process and capture its output.

### 7. Why is the same preprocessing required during inference?
If training and inference use different text cleaning steps, the vectorized features will not match what the model learned.

### 8. What are accuracy, precision, recall, and F1-score?
- Accuracy: overall correct predictions
- Precision: how many predicted positives were correct
- Recall: how many actual positives were found
- F1-score: balance between precision and recall

### 9. What are the limitations of this project?
The included sample dataset is intentionally small, so predictions are for demonstration and learning purposes rather than production-grade fact checking.

### 10. How can this project be improved?
- use a larger dataset
- try advanced models such as Linear SVM or transformers
- add article source analysis and metadata features
- deploy Python inference behind a queue or API service for scale

## Future Improvements

- Add user authentication
- Store prediction history in a database
- Support headline-only and long-article modes
- Add charting for model metrics
- Replace the baseline classifier with a transformer-based model
