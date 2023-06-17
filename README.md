# Datastreaming project Using AWS

Project Objective

Stream real-time data using cloud services for storage and analysis in real-time.

Introduction

In this project the aim is to simulate a real-time data streaming scenario where data is being Generated continuously and it needs to be transferred and processed instantly. It finds application in various domains where timely and up-to-date information is crucial like Internet of Things (IoT), financial services, social media analysis, online gaming and streaming, traffic management and logistics, cybersecurity and network monitoring, healthcare and remote monitoring, energy and utilities. The high-level architecture diagram of the project is as below.
The data source here is a csv file containing stock market data. In a real scenario the data will be fetched using an API. The Kafka producer and consumer have been running at the same time to simulate the real scenario. The Kafka server and Zookeeper instance have been deployed on the EC2 instance and are both running at the same time. The data is being streamed to an S3 container and a glue crawler is storing it in a table and the data can be queried in real-time using Athena.

<ins> Architecture Diagram </ins>


![image](https://github.com/DataCounsel/Datastreaming-project/assets/71335870/3aae44fa-5e63-4c23-ad38-b2f5360334e6)



Setting up the environment

The environment has to be setup in the EC2 instance for being able to run Kafka broker. The dependencies like JDK need to be installed Kafka has to be downloaded and unzipped. The Amazon EC2 instance in the free tier has only 1 GB memory so to run Kafka smoothly the variable needs to be  changed accordingly, KAFKA_HEAP_OPTS="-Xmx256M -Xms128M". Also, the ADVERTISED_LISTENERS variable has to be uncommented and changed to the public IP of the EC2 instance to be able to connect from the local machine. The AWS CLI needs to be installed and setup by generating the access token this will be required for communication with the kafka consumer.

 
 ![image](https://github.com/DataCounsel/Datastreaming-project/assets/71335870/2b49c289-e98f-4f4c-bde2-c135d6475427)
 
 ![image](https://github.com/DataCounsel/Datastreaming-project/assets/71335870/bf9d291b-4c7c-4b79-9247-b25caf3d40a2)


![image](https://github.com/DataCounsel/Datastreaming-project/assets/71335870/7b8b8052-cb02-4aac-b207-a76af0869388)

 

The Project Flow

<ins>Data source:</ins> The data source for this project was a csv file containing stock market data. It will be upgraded to data being fetched using an API as that would be the typical data source in a real-life scenario. The CSV dataset contains data like index (name of stock), date, open value, high, low, Adjusted close, Volume and close columns. 

![image](https://github.com/DataCounsel/Datastreaming-project/assets/71335870/1c7ffe05-7012-47e3-8ffd-60c084205ae9)
 

<ins>Kafka:</ins> Kafka is one of the most popular open-source tools for streaming data. The Kafka server was installed and run on the Amazon EC2 instance. 
Once the changes are made the Zookeeper instance can be started followed by the Kafka server. Now the Kafka server will be accessed from the local machine for creating the topics. There is a Kafka producer which received the input data stream and a topic is created. The Kafka consumer uses this topic and receives the input data which can then be stored. The dataset has a very large number of records so do not run the producer and consumer till all records are fetched as this may crash the kafka broker and Zookeeper. Adding a sleep duration of 1 sec helps process the data more smoothly.

![image](https://github.com/DataCounsel/Datastreaming-project/assets/71335870/c9a93e93-9ac5-4278-a3a9-2185ed21cc79)

<ins>Python:</ins> The python libraries used were kafka-python, Pandas, S3fs, time, JSON. The kafka producer and consumer codes are written using python in Jupyter notebook on the local machine.



<ins>S3 Bucket:</ins> The kafka consumer receives the data from the producer and stores the data in S3 bucket in JSON format. The data extracted from the CSV is stored in the JSON format in S3 bucket. The idea is if the data is fetched from the API chances are it will be in the JSON format. So, when the data is fetched using an API format this will be useful. Each entry in the dataset is stored as a separate file based on the entry number in the dataset. 

![image](https://github.com/DataCounsel/Datastreaming-project/assets/71335870/a65eabb3-9823-4786-b72f-54983d3b4af3)


<ins>Glue Crawler and Glue tables:</ins> The crawler infers the schema of the data, such as column names, data types, and relationships between tables. A crawler was created analysing and crawling the s3 bucket where the kafka consumer has written the data. The crawled data was stored in a table called stockmarket_project which is present in the database called realtime-de-db. The crawler can be run on a schedule but for the purpose of our project it was only triggered manually. 

 ![image](https://github.com/DataCounsel/Datastreaming-project/assets/71335870/6cae882a-0151-4882-aa18-0f2109f13b42)


 ![image](https://github.com/DataCounsel/Datastreaming-project/assets/71335870/39067148-cdc8-40b6-9342-fcadf4d64a21)


<ins>Amazon Athena:</ins> The crawled data is present in the Glue database. To view the data and run queries we need to use Amazon Athena. It is a serverless query service provided by AWS that allows you to analyze data directly from various data sources using standard SQL. Its specialty lies in its ability to perform ad hoc querying and analysis of large-scale datasets without the need for infrastructure provisioning or complex data management tasks.
 

![image](https://github.com/DataCounsel/Datastreaming-project/assets/71335870/5acb3665-8f75-4118-86ad-6bcb2615e2c2)




								


