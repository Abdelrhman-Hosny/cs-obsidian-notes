## Reference Information

Link: https://www.infoq.com/presentations/Twitter-Timeline-Scalability/

---
## Scaling Twitter Newsfeed

The first thing you would think of to show a Newsfeed would be a query like this
```
Select *
from Tweets t
where t.tweet_writer in users_i_follow
SORT BY t.creation_date
ORDER DESC
```

But this doesn't work well at scale, here the read is a very heavy operation, while the write is a relatively simple one where you insert a tweet to a column.

What twitter did is that it created a cache for each user, and whenever a user tweeted, we add the tweet to the cache owned by each one of his followers.

![[Fanout]]


We have now changed read to be a simple read of the users cache, but the write is now more expensive
```
write_cost = O(# followers)
read_cost = O(1)
```

We only keep a cache entry for active users (User logged in last 30 days), if an inactive user comes back, we reconstruct the cache using the SQL query above, and then use that instead.

The cache also has a limit of 800 total tweets to keep the size of the Redis cluster that stores the data manageable

---

![[twitter-search-vs-timeline.webp]]
Earlybird is a modified version of Lucene for twitter, and it is used in search.

Twitter made search fairly slow as it relies on a scatter and gather to get results.

But less people use search so they can get away with that.

---
## Dealing with Celebrities

Since tweeting is an `O(# followers)`, if someone like Taylor Swift tweets, we would need to do a write for each one of her followers, which will take minutes.

Some people might see the tweet and respond to it before others, so other people would see responses to the tweet without being able to see the tweet itself


Twitter decided to use a mix of both approaches, Fanout and the select.

So we use fanout for people below a certain threshold of followers, and for others we use the  select approach and then combine the two timelines generated.

---

