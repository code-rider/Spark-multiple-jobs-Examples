Spark multiple jobs Examples
============================
<h2>Dependencies</h2>

<a href="http://sourceforge.net/projects/simplevoronoi/">simplevoronoi</a><br>
<p>Download simplevoronoi and place simplevoronoi-version-SNAPSHOT.jar in lib directory</p>

<h2>Create jar file</h2>

<p>Download Spark-multiple-jobs-Examples</p> 
<p>cd Spark-multiple-jobs-Examples and run</p>
>sbt
> > package

<p>After successful complete in directory target/scala-2.10/spark_multiple_job_examples_2.10-SNAPSHOT-0.1.jar should be created 
now we will run classes from this jar as spark jobs</p>

<h2>Spark dependencies</h2>

<a href="http://mvnrepository.com/artifact/org.twitter4j/twitter4j-core/3.0.3">twitter4j-core</a><br>
<a href="http://mvnrepository.com/artifact/org.twitter4j/twitter4j-stream/3.0.3">Twitter4j Stream</a><br>
<a href="http://mvnrepository.com/artifact/org.apache.spark/spark-streaming_2.10/1.3.0">spark-streaming</a><br>
<a href="http://mvnrepository.com/artifact/org.apache.spark/spark-streaming-twitter_2.10/1.4.1">spark-streaming-twitter</a><br>
<a href="http://sourceforge.net/projects/simplevoronoi/">simplevoronoi</a><br>

<p>Download listed libraries some where on disc</p>
<p>and add these in SPARK_CLASSPATH in you user user .bashrc file or spark-env.sh file</p>
<p>in this tutorial we export these in spark-env.sh so add these lines in you SparkHome/conf/spark-env.sh</p>
<br>
>export SPARK_CLASSPATH=PathToFile/twitter4j-core-3.0.3.jar:$SPARK_CLASSPATH<br>
>export SPARK_CLASSPATH=PathToFile/twitter4j-stream-3.0.3.jar:$SPARK_CLASSPATH<br>
>export SPARK_CLASSPATH=PathToFile/spark-streaming-twitter_2.10-1.4.1.jar:$SPARK_CLASSPATH<br>
>export SPARK_CLASSPATH=PathToFile/simplevoronoi-0.2-SNAPSHOT.jar:$SPARK_CLASSPATH<br>

<h2>Examples</h2>

<h2>write tweets from twitter live stream in CSV file</h2>
<br>
<p><strong>1.1:</strong> we need twitter API credentials if you dont have create frist <a href="https://apps.twitter.com/">here</a></p>
<p><strong>1.2:</strong> create a file with Twitter API credentiale like name twitter-credentials.txt</p>
<p><strong>1.3:</strong> enter credential</p>
    
>TWITTER_API_KEY=ApiKey
>TWITTER_API_SECRET=ApiSecret
>TWITTER_ACCESS_TOKEN=AccessToken
>TWITTER_ACCESS_TOKEN_SECRET=AccessTokenSecret

<p><strong>1.4:</strong> run sbt job to write live stream from twitter in a csv file</p>
>sbt "run-main xulu.FetchTweets twitter-credentials.txt tweets.csv"
<p>This job write tweets in fomate <Longitude,Latitude,Text> to change modifie FetchTweets.scala and run again</p>
<br>		
<p>shell output</p> 
> -56.544541,-29.089541,Por que ni estamos jugando, son más pajeros estos locos! <span class="wp-smiley wp-emoji wp-emoji-neutral" title=":|">:|</span>
> -69.922686,18.462675,Aprenda hablar amigo
> -118.565107,34.280215,today a boy told me I'm pretty and he loved me. he's six years old so that's good.
> 121.039399,14.72272,@Kringgelss labuyoo. Hahaha
> -34.875339,-7.158832@keithmeneses_ oi td bem? sdds 😔💚
> 103.766123,1.380696,Xian Lim on iShine 3 2
> ......

<h2>spark live Twitter stream</h2>

<p><strong>2.1:</strong> we already create a jar file from our repostory in step one so just run stream class on spark</p>
<p>cd to spark home</p>
>bin/spark-submit --class xulu.TwitterLiveStreaming --master spark://sparkMasterIP:7077 PathTo/spark_multiple_job_examples_2.10-SNAPSHOT-0.1.jar (Optional kyeWords separated by space)
<br>		 
<p>This example should only print tweet text from Twitter live stream on shell</p> 
<p>you are able to do write it any where or change the query </p>
<p>modifie TwitterLiveStreaming.scala and rebluild you jar  and run again</p>
<br>		 
<h2>Segmenting Audience with KMeans and Voronoi Diagram using Spark and MLlib</h2>
<p>In this example, we will be using the <a href="http://en.wikipedia.org/wiki/K-means_clustering"k-means clustering</a> algorithm implemented in <a href="https://spark.apache.org/mllib/">Spark Machine Learning Library</a>(MLLib) to segment the dataset by geolocation.</p>
<br>
<p><strong>3.1:</strong> we need twitter data to perform this example.</p>
<p>in example one we show how you write Twitter data in a file so run example one to fetch some data fron Twitter.</p>
<p>For your convenience, we provide the file tweets_drink.csv</p>
		 
<p><strong>3.2:</strong> upload Twitter data tweets_drink.csv in to Hadoop</p>

<p><strong>3.3:</strong> run the job</p>
>bin/spark-submit --class xulu.KMeansApp --master spark://SparkMasterIP:7077 PathTo/spark_multiple_job_examples_2.10-SNAPSHOT-0.1.jar hdfs://HDFS_IP:9000/UploadedLocation/tweets_drink.csv outputFileName.png 

<p>result</p>

<h2>Analyzing your audience location with Twitter Streams and Heat Maps</h2>

<p><strong>4.1:</strong> Downlaod Twitter data about Drink </p>
<p>You can run the Example one With Keywords about Drink to get the tweets having a word related to a drink:</p>

>sbt "run-main xulu.FetchTweets twitter-credentials.txt tweets_drink.csv \
>redbull schweppes coke cola pepsi fanta orangina soda \
>coffee cafe expresso latte tea \
>alcohol booze alcoholic whiskey tequila vodka booze cognac baccardi \
>drink beer rhum liquor gin ouzo brandy mescal alcoholic wine drink"	
		
<p>When you have enough tweets, stop the program by pressing CTRL + C</p>
		
<p>Unfortunately only a fraction of all the tweets have geolocation information(the publisher has to tweet from a phone and has to opt in to send its position). So you might need to wait several hours (even days if the words are not popular) to get enough tweets to draw on a map. For your convenience, we provide the file tweets_drink.csv which already contains the tweets that we collected using those keywords.</p>

<p>The tweet file contains on each line, the tweet longitude ,latitude, and message:</p>

<p><strong>4.2:</strong> upload Twitter data tweets_drink.csv in to Hadoop</p>
<p><strong>4.3:</strong> run the job</p>
>bin/spark-submit --class xulu.HeatMap --master spark://SparkMasterIP:7077 PathTo/spark_multiple_job_examples_2.10-SNAPSHOT-0.1.jar hdfs://HDFS_IP:9000/UploadedLocation/tweets_drink.csv outputFileName.png 0.5 coke pepsi	
		 
<p>result</p> 
<br>		 
<p>On this map, we can clearly see that the word 'coke'(in green) is used much more than the word 'pepsi'(in red) in the tweets. There are some places which are yellow, that means that there are tweets on coke and tweets on pepsi (yellow = red + green). Interestingly enough, we can see that coke is not used much in South America unlike pepsi which is used in Brazil and in Argentina.</p>
		 
<h2>Following</h2>
https://chimpler.wordpress.com/2014/07/11/segmenting-audience-with-kmeans-and-voronoi-diagram-using-spark-and-mllib/
https://chimpler.wordpress.com/2014/06/26/analyzing-your-audience-location-with-twitter-streams-and-heat-maps/
		
			      

