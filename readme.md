# Data Engineering Nanodegree
## Project: Data Modeling with Cassandra
## Table of Contents
* **Definition**
    * **Project Overview** :
    Sparkify is a music app, they wants to analyze the data they've been collecting on songs and user activity on their new music streaming app.
   Currently, there is no easy way to query the data to generate the results, since the data reside in a directory of CSV files on user activity on the app.
    
    * **Problem Statement** : 
       Sparkify Analytics team is particularly interested in understanding what songs users are listening to. 

* **Cassandra Data Modeling**
    
    **Overview** : 
    Cassandra is a NoSQL database that provides high availability and horizontal scalability without compromising performance.
    To get the best performance out of Cassandra, I need to carefully design the schema around query patterns specific to the business problem at hand.
    
    **Partition Key** : 
    Cassandra is a distributed database in which data is partitioned and stored across multiple nodes within a cluster.
    The partition key is made up of one or more data fields and is used by the partitioner to generate a token via hashing to distribute the data uniformly across a cluster.
    
    **Clustering Key** : 
    A clustering key is made up of one or more fields and helps in clustering or grouping together rows with same partition key and storing them in sorted order.
    Let’s say that I want to store time-series data in Cassandra and I want to retrieve the data in chronological order. A clustering key that includes time-series data fields will be very helpful for efficient retrieval of data for this use case.
    Note: The combination of partition key and clustering key makes up the primary key and uniquely identifies any record in the Cassandra cluster.
    
    **Guidelines Around Query Patterns** : 
    Before starting with data modeling in Cassandra, I should identify the query patterns and ensure that they adhere to the following guidelines:
    Each query should fetch data from a single partition
    I should keep track of how much data is getting stored in a partition, as Cassandra has limits around the number of columns that can be stored in a single partition
    It is OK to denormalize and duplicate the data to support different kinds of query patterns over the same data
    Based on the above guidelines, I have come up data model using Cassandra for Sparkify music app.
    
    **Data Modeling On Sparkify music app** : 
    Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. Apache Cassandra database which can create queries on song play data to answer the below questions
 
     1. Give me the artist, song title and song's length in the music app history that was heard during sessionId = 338, and itemInSession = 4
     1. Give me only the following: name of artist, song (sorted by itemInSession) and user (first and last name) for userid = 10, sessionid = 182
     1. Give me every user name (first and last) in my music app history who listened to the song 'All Hands Against His Own'

    I need to introduce a fair amount of denormalization to support the above queries as shown below data model which has been designed based on query approach
     ![Cassandra Data Model](/images/Cassandra_Data_Model.jpg)  
    **Inefficient Query Patterns** : Due to the way that Cassandra stores data, some query patterns are not at all efficient, including the following:
         1. Fetching data from multiple partitions – this will require a coordinator to fetch the data from multiple nodes, store it temporarily in heap, and then aggregate the data before returning results to the user
         1. Join-based queries – due to its distributed nature, Cassandra does not support table joins in queries the same way a relational database does, and as a result, queries with joins will be slower and can also lead to inconsistency and availability issues

* **ETL Design** 
    * **ETL Pipeline** :
    Once you run the pipeline, it will collect, process, and store csv file into cassandra tables. Once it is processed, you can get timely insights and react quickly to new information. Below is three step process
        1. Implemented the logic in section Part I of the notebook template to iterate through each event file in event_data to process and create a new CSV file in Python and stored as .csv file.
        1. In the Part II of the Jupiter notebook, created all the three tales for three queries and insertv statement to load processed records into respective tables .
        1. Run the SELECT statments for each query to see the output.

* **How to Run** : Open the Jupiter notebook, either one you can run
    1. Run Selected cell using SHIFT + ENTER
    1. Run ALL 
    
* **Final Result / Analysis** : Now Sparkify Analytics team can run multiple queries with optimal performance by quering individual tables.     
     1. Give me the artist, song title and song's length in the music app history that was heard during sessionId = 338, and itemInSession = 4
     1. Give me only the following: name of artist, song (sorted by itemInSession) and user (first and last name) for userid = 10, sessionid = 182
     1. Give me every user name (first and last) in my music app history who listened to the song 'All Hands Against His Own'
* **Software Requirements** : This project uses the following software and Python libraries:
        1. Python 3.0
        1. cassandra        
        
    You will also need to have software installed to run and execute a Jupyter Notebook.
    If you do not have Python installed yet, it is highly recommended that you install the Anaconda distribution of Python, which already has the above packages and more included.    

* **Acknowledgement** : Must give credit to Udacity for the project. You can't use this for you Udacity capstone project. Otherwise, feel free to use the code here as you would like!

