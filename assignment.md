# Assignment

## Brief

Write the Python codes for the following questions.

## Instructions

Paste the answer as Python in the answer code section below each question.

### Question 1

Question: From the `movies` collection, return the documents with the `plot` that starts with `"war"` in acending order of released date, print only title, plot and released fields. Limit the result to 5.

Answer:

```python

results = movies.find({
                        "plot": {
                            "$regex": "^war", # The ^ symbol means the string must start with the pattern
                            "$options": "i"  # case-insensitive
                        }
                    }).sort('released', pymongo.ASCENDING).limit(5)

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
    print(f"Rating: {rating_summary['_id']} - Movies: {rating_summary['movie_count']}")


```

### Question 3

Question: Count the number of movies with 3 comments or more.

Answer:

```python

pipeline = [
    {
        "$lookup": {
            "from": "comments", # the collection to lookup and join with 
            "localField": "_id", # id in movies collection to use to match in comments collection
            "foreignField": "movie_id", # the movie id in the comments collection
            "as": "related_comments", # new field name to store the related comments in movies collection
        }
    },
    {
        "$addFields": {
            "comment_count": {"$size": "$related_comments"}
        }
    },
    {
        "$match": {
            "comment_count": {
                "$gte": 3,
            }
        }
    },
    {
        "$limit": 5 # have to limit or it will time out
    }
]

results = movies.aggregate(pipeline)
for movie in results:
   print(movie["title"])
   print("Comment count:", movie["comment_count"])

   for comment in movie["related_comments"][:5]:
         print(" * {name}: {text}".format(
            name=comment["name"],
            text=comment["text"]))
   print()

```

## Submission

- Submit the URL of the GitHub Repository that contains your work to NTU black board.
- Should you reference the work of your classmate(s) or online resources, give them credit by adding either the name of your classmate or URL.
