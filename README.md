# -Fake-Product-Review-Detector
import pandas as pd
import numpy as np
import re
import string
from sklearn.model_selection import train_test_split
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, classification_report

data = {
    'review': [
        "This product is amazing! Best purchase ever!", 
        "Worst product ever. Total waste of money!", 
        "I love it! Highly recommended!", 
        "Fake product, do not buy!", 
        "Absolutely fantastic, will buy again!", 
        "Not as described, very disappointed!"
    ],
    'label': [1, 0, 1, 0, 1, 0] 
}

df = pd.DataFrame(data)


def preprocess_text(text):
    text = text.lower()
    text = re.sub(f"[{string.punctuation}]", "", text)
    text = re.sub(r"\d+", "", text)
    return text

df['cleaned_review'] = df['review'].apply(preprocess_text)

vectorizer = TfidfVectorizer()
X = vectorizer.fit_transform(df['cleaned_review'])
y = df['label']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

model = LogisticRegression()
model.fit(X_train, y_train)

y_pred = model.predict(X_test)

accuracy = accuracy_score(y_test, y_pred)
print("Accuracy:", accuracy)
print(classification_report(y_test, y_pred))
