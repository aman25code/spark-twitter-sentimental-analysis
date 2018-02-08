# spark-twitter-sentimental-analysis
This project is about Sentiment Analysis of a desired Twitter topic with Apache Spark Structured Streaming, Apache Kafka, Python and AFINN Module. You can learn sentiment status of a topic that is desired.
For example; you might be curious about Game of Thrones’s new episode and you might get someone’s opinions about this new episode previously. Answer can be NEGATIVE, NEUTRAL or POSITIVE according to opinions.

# Code Explanation
 - Authentication operations were completed with Tweepy module of Python. You must take keys from Twitter API.
 - StreamListener named TweetListener was create for Twitter Streaming. StreamListener produces data for Kafka Topic named 'twitter'.
 - StreamListener also calculates Tweets' sentiment value with AFINN module and sends this value to 'twitter' topic.
 - Producing data was filtered about including desired topic.
 - Kafka Consumer that consumes data from 'twitter' topic was created.
 - Also, it converts streaming data to structured data. This structured data is placed into a SQL table named 'data'.
 - Data table has 2 columns named 'text' and 'senti_val'.
 - Average of sentiment values of senti_val column is calculated by pyspark.sql.functions.
 - Also, user defined function named fun is created for status column.
 - Status column has POSITIVE, NEUTRAL or NEGATIVE that change according to avg(senti_val) column in real-time.
 
# Running
Create Twitter API account and get keys for twitter_config.py

Start Apache Kafka
/bin/kafka-server-start.sh /config/server.properties

Run tweet_listener.py with Python version 3 and desired topic name. 
PYSPARK_PYTHON=python3 bin/spark-submit tweet_listener.py "Game of Thrones"

Run twitter_topic_avg_sentiment_val.py with Python version 3.
PYSPARK_PYTHON=python3 bin/spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.11:2.1.1 twitter_topic_avg_sentiment_val.py
