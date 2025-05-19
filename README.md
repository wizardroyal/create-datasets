# Creating A Movie Review Dataset

## Update Packages
<pre>sudo apt update</pre>

## Install Prerequisites
<pre>sudo apt install -y software-properties-common</pre>

## Add Deadsnakes PPA
<pre>sudo add-apt-repository ppa:deadsnakes/ppa</pre>

## Update again
<pre>sudo apt update</pre>

## Install Python 3.13
<pre>sudo apt install -y python3.13 python3.13-dev python3.13-venv</pre>

## Install Pip
<pre>sudo apt install -y python3-pip</pre>

## Verify Python and Pip Installation
<pre>python3.13 --version
python3.13 -m pip --version</pre>

should show Python and Pip versions

## Test Python
<pre>python3.13 -c "print('Hello, World!')"</pre>

should output Hello World

## Create a Virtual Environment
<pre>python3.13 -m venv datasets_env
source datasets_env/bin/activate</pre>

## Install dependencies
<pre>pip install pandas faker</pre>

## Verify dependencies 
<pre>pip list</pre>

should list pandas, faker, numpy

## Create directory
<pre>mkdir -p ~/dataset
cd ~/dataset</pre>

## Open text editor
<pre>nano generate_movie_reviews_dataset.py</pre>

## Input this code
<pre>import pandas as pd
import random
from faker import Faker
import os
import logging

# Set up logging
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')

# Initialize Faker
fake = Faker('en_US')

# Parameters
num_samples = 10000
output_file = "movie_reviews_dataset.csv"
labels = ["positive", "negative", "neutral"]
languages = ["English"]
movie_titles = [
    f"The {fake.word().capitalize()} Chronicles",
    f"{fake.word().capitalize()} of Destiny",
    f"{fake.word().capitalize()} Rising",
    f"Shadows of {fake.word().capitalize()}",
    f"{fake.name()}'s Adventure"
]
sample_reviews = {
    "positive": [
        f"Absolutely loved {movie_title}! The plot was thrilling and {fake.name()} was brilliant!",
        f"{movie_title} is a masterpiece, stunning visuals and a heartfelt story.",
        f"Best movie of the year! {fake.word().capitalize()} scenes were unforgettable.",
        f"{fake.name()} nailed it in {movie_title}, highly recommend!",
    ],
    "negative": [
        f"{movie_title} was a letdown, boring plot and weak acting by {fake.name()}.",
        f"Disappointed with {movie_title}, felt like a waste of time.",
        f"Terrible direction in {movie_title}, {fake.word()} scenes dragged on.",
        f"Expected more from {movie_title}, {fake.name()} was miscast.",
    ],
    "neutral": [
        f"{movie_title} was okay, decent story but nothing special from {fake.name()}.",
        f"{movie_title} had good moments, but the {fake.word()} parts were average.",
        f"Not bad, {movie_title} is watchable but forgettable.",
        f"{fake.name()}'s performance in {movie_title} was fine, nothing stood out.",
    ]
}

def generate_dataset():
    try:
        logging.info(f"Generating dataset with {num_samples} samples...")
        data = {
            "id": [],
            "review_text": [],
            "sentiment": [],
            "language": [],
            "movie_title": []
        }

        for i in range(num_samples):
            label = random.choice(labels)
            language = random.choice(languages)
            movie_title = random.choice(movie_titles)
            # Format review text with movie title
            review_text = random.choice(sample_reviews[label]).format(
                movie_title=movie_title, **{k: v() for k, v in fake.__dict__.items() if callable(v)}
            )

            data["id"].append(i + 1)
            data["review_text"].append(review_text)
            data["sentiment"].append(label)
            data["language"].append(language)
            data["movie_title"].append(movie_title)

        # Save to CSV
        df = pd.DataFrame(data)
        df.to_csv(output_file, index=False)
        logging.info(f"Dataset saved to {os.path.abspath(output_file)}")
    except Exception as e:
        logging.error(f"Error generating dataset: {str(e)}")
        raise

if __name__ == "__main__":
    generate_dataset()</pre>

press CTRL+X, CTRL+Y, and Enter to save

## Verify file is in directory
<pre>ls</pre>

## Generate dataset
<pre>python generate_movie_reviews_dataset.py</pre>

It should show something like this with the last line showing the location of the dataset:

2025-05-19 09:36:14,706 - INFO - Saved chunk 99

2025-05-19 09:36:14,710 - INFO - Generating chunk 100/101...

2025-05-19 09:36:15,909 - INFO - Saved chunk 100

2025-05-19 09:36:15,910 - INFO - Dataset saved to /home/wizard/nlp_dataset/large_nlp_dataset.csv

## Verify output
<pre>ls -lh movie_reviews_dataset.csv
wc -l movie_reviews_dataset.csv</pre>

## Show Preview
<pre>head movie_reviews_dataset.csv</pre>

You now have your dataset

Navigate to the folder, copy file and upload on Sahara

For windows users using WSL, click `\\wsl$` in the search bar, enter Ubuntu folder and navigate to the path shown in the last line after the dataset generation 
