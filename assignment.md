# Assignment

## Brief

Write the Python codes for the following questions.

## Instructions

Paste the answer as Python in the answer code section below each question.

### Question 1

Question: From the `movies` collection, return the documents with the `plot` that starts with `"war"` in acending order of released date, print only title, plot and released fields. Limit the result to 5.

Answer:

```python

# find first 5 movies with plot starting with 'war', then sort by released date in ascending order
results = movies.find({
                        "plot": {
                            "$regex": "^war", # The ^ symbol means the string must start with the pattern
                            "$options": "i"  # case-insensitive
                        }
                    }).sort('released', pymongo.ASCENDING).limit(5) #alternative use 1 for ascending and -1 for descending

# print the results line by line with a separator
for movie in results:
    print(f"ID: {movie['_id']}")
    print(f"Title: {movie['title']}")
    print(f"Plot: {movie['plot']}")
    print(f"Released: {movie['released']}")
    print("-" * 50)

```

### Question 2

Question: Group by `rated` and count the number of movies in each.

Answer:

```python
# Group by 'rated' field and count movies in each category
pipeline = [
    {
        "$group": {
            "_id": "$rated",           # Group by the 'rated' field
            "movie_count": {"$sum": 1} # Count movies in each group
        }
    }
]

results = movies.aggregate(pipeline)

# Print the results
for rating_summary in results:
    print(f"Rating: {rating_summary['_id']}")
    print(f"Movies: {rating_summary['movie_count']}")
    print("-" * 20)

```

### Question 3

Question: Count the number of movies with 3 comments or more.

Answer:

```python

count = movies.count_documents({"num_mflix_comments": {"$gte": 3}})
print(count)

```



## Submission

- Submit the URL of the GitHub Repository that contains your work to NTU black board.
- Should you reference the work of your classmate(s) or online resources, give them credit by adding either the name of your classmate or URL.
