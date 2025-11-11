# Assignment

## Brief

Write the Python codes for the following questions.

## Instructions

Paste the answer as Python in the answer code section below each question.

### Question 1

Question: From the `movies` collection, return the documents with the `plot` that starts with `"war"` in acending order of released date, print only title, plot and released fields. Limit the result to 5.

Answer:

```python
for m in movies.find({"plot": {"$regex": "war"}}).sort('released', pymongo.DESCENDING).limit(5):
    print(f"{m['title']} ({m['plot']}) was released on {m['released']}")
```

### Question 2

Question: Group by `rated` and count the number of movies in each.

Answer:

```python
stage_group_rated = {
   "$group": {
         "_id": "$rated",
         # Count the number of movies in the group:
         "movie_count": { "$sum": 1 }, 
   }
}

pipeline = [
   stage_group_rated
]
results = movies.aggregate(pipeline)

# Loop through the 'rated-summary' documents:
for rated_summary in results:
   print(rated_summary)
```

### Question 3

Question: Count the number of movies with 3 comments or more.

Answer:

```python
stage_match_with_comments = {
   "$match": {
         "num_mflix_comments": {
            "$gte": 3
         }
   } 
}

stage_group_count = {
   "$group": {
         "_id": None,
         "movies_count": { "$sum": 1 }
   }
}

pipeline = [
   stage_match_with_comments,
   stage_group_count
]

results = movies.aggregate(pipeline)

for result in results:
   print(f"Number of movies with 3 comments or more: {result['movies_count']}")
```

## Submission

- Submit the URL of the GitHub Repository that contains your work to NTU black board.
- Should you reference the work of your classmate(s) or online resources, give them credit by adding either the name of your classmate or URL.
