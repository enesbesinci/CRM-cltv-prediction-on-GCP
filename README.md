# Customer Life Time Value Prediction

Hello everyone, in this project we are going to calculate customer life time value (also knowns as CLTV or CLV) using GCP technologies like Google Cloud Storage, BigQuery and Vertex AI.

Customer lifetime value (CLV or CLTV) is a metric that measures the total revenue a customer is expected to generate for the business over the course of their relationship with the company. It is an important metric for businesses to understand as it can help them make better decisions about marketing, customer retention and product development.

Let's start.

## STEP 1: Upload the data to Google Cloud Stroage
First of all we should create a Google Cloud Stroage to upload and store the dataset. There are different kind of storage solutions in GCP but we will use the standart one because of the pricing. And after that we upload the data from our local PC to Google Cloud Stroage.

![Ekran görüntüsü 2023-11-25 192223](https://github.com/enesbesinci/cltv-prediction/assets/110482608/81d4c1d0-9768-4be8-b388-532d260e2bc6)

Our data is now in Google Cloud Stroage, in the next steps we will pull the data from here to BigQuery and Vertex Ai's notebook.

## STEP 2: Load the data from Bucket to BigQuery

In this step we pull the data from Bucket to BigQuery. We'll load the data into BigQuery because we'll be pulling data from BigQuery instead of Bucket when we work on the Workbench notebook.

![Ekran görüntüsü 2023-11-25 194606](https://github.com/enesbesinci/cltv-prediction/assets/110482608/daadaed1-6c24-44d9-bf80-bebb622bbcfd)

We loaded the data into BigQuery from Bucket. Now we can use SQL commands to prepare the data for CLTV calculation if we want but I will prepare the data on Vertex Ai Workbench.

## STEP 3: Preprocessing, Dataset Exploration and CLTV Calculation with BG-NBD and Gamma-Gamma Submodel on the Vertex AI Workbench

In this step, we will take a look at the dataset and complete the data preprocessing steps and then calculate the CLTV using BG-NBD and Gamma-Gamma Submodel on the workbench.

### !!! This is a real-world dataset from FLO, a Turkish shoe retailer.

#### A Summary of The Dataset:

The data set consists of information obtained from the past shopping behavior of customers who made their last purchases as OmniChannel (both online and offline shoppers) in 2020 - 2021.

We have some features that contain information about the customer, here are all of them:

#### ***********************************************************************************************************************

##### master_id: Unique customer identifier
##### order_channel: The channel used for the shopping transaction, belonging to the platform (Android, iOS, Desktop, Mobile, Offline)
##### last_order_channel: The channel used for the most recent shopping transaction
##### first_order_date: The date of the customer's first shopping transaction
##### last_order_date: The date of the customer's last shopping transaction
##### last_order_date_online: The date of the customer's last online shopping transaction
##### last_order_date_offline: The date of the customer's last offline shopping transaction
##### order_num_total_ever_online: The total number of transactions the customer has made online
##### order_num_total_ever_offline: The total number of transactions the customer has made offline
##### customer_value_total_ever_offline: The total amount paid by the customer in offline transactions
##### customer_value_total_ever_online: The total amount paid by the customer in online transactions
##### interested_in_categories_12: The list of categories in which the customer has made purchases in the last 12 months

#### ***********************************************************************************************************************

Now we can move on to data preprocessing and EDA.

We will use Vertex Ai Workbench for the calculations because of the high computing power capacity offered by Google. Now we create an instance to work on it, we don't need to use a lot of computing power as we will work with a dataset with about 20000 rows and 12 features, so a machine with 2vCPU and 8GB Ram is enough for us.

![Ekran görüntüsü 2023-11-26 222537](https://github.com/enesbesinci/cltv-prediction/assets/110482608/65d05a6e-fb3c-4fe0-81e1-2a5881f26c3f)


Now let's create a notebook in the instance we created.


![Ekran görüntüsü 2023-11-26 222628](https://github.com/enesbesinci/cltv-prediction/assets/110482608/af1b64d1-e268-4a9c-9e79-fdf945150d0d)

Now we can start writing code.

First of all we need to import the necessary libraries.

![Ekran görüntüsü 2023-11-26 223641](https://github.com/enesbesinci/cltv-prediction/assets/110482608/844ddfb1-57d8-4af3-b74a-feac49feb93b)

Then we create a BigQuery client object to pull the data into the workbench environment, and just like in the BigQuery environment, we pull our data with SQL commands and bring them into a dataframe format.
Let's look at the data.

![Ekran görüntüsü 2023-11-26 223917](https://github.com/enesbesinci/cltv-prediction/assets/110482608/219654ac-977d-45f4-9e0e-d82e4457168e)

Let's see how many columns and how many rows are in our data set.

![Ekran görüntüsü 2023-11-26 224239](https://github.com/enesbesinci/cltv-prediction/assets/110482608/7e9b9a7d-4883-4cba-8074-e20c5f30316e)


We have about 20.000 row and 12 feature

Let's see the data dtypes.

![Ekran görüntüsü 2023-11-26 224435](https://github.com/enesbesinci/cltv-prediction/assets/110482608/bf561c64-97a3-4cc3-be11-0eeddd06bef3)

We received the data from BigQuery, so the date columns seem to be of the wrong type, we have to fix them.

![Ekran görüntüsü 2023-11-26 224527](https://github.com/enesbesinci/cltv-prediction/assets/110482608/13649fee-827c-4a50-86c5-9a821c1e2e4d)

Now, to dive deeper into our dataset, let's make a simple summary of our dataset with Pandas Profiling, and save it as an html file.

![Ekran görüntüsü 2023-11-26 224623](https://github.com/enesbesinci/cltv-prediction/assets/110482608/ecbc1ab7-0817-4df6-952f-87616a69cf7e)

In order to observe the outlier values in the columns containing the number of orders and order amounts in both online and offline channels, let's simply visualize these columns with the boxplot.

![Ekran görüntüsü 2023-11-26 225118](https://github.com/enesbesinci/cltv-prediction/assets/110482608/ac549e9f-5a18-4749-be2b-10cd96a21307)

As you can see we seem to have some outliers.

![Ekran görüntüsü 2023-11-26 225325](https://github.com/enesbesinci/cltv-prediction/assets/110482608/ed76b6ce-c6c4-4165-b8bc-0cd8c3a781a8)

![Ekran görüntüsü 2023-11-26 225348](https://github.com/enesbesinci/cltv-prediction/assets/110482608/57ede107-ef74-4779-8142-3a8cd7440d40)

Now we write a simple function to see the missing values, this function will return the number of missing values and percentage of missing values.

![Ekran görüntüsü 2023-11-26 225512](https://github.com/enesbesinci/cltv-prediction/assets/110482608/2bc24c12-b5ec-4c9a-94f2-f2d8979d91c2)

The results:

![Ekran görüntüsü 2023-11-26 225618](https://github.com/enesbesinci/cltv-prediction/assets/110482608/01e8e5bb-47a9-460e-b5f9-b8e4fe272f04)

As you can see we have no missing values, this is a good news because each observation is important for us, now we will define the numeric variables and save it as a csv file. And after that we should check the outliers. There are different methods to identify outliers, in this project I will use the IQR method. Let's see these outliers, I have written a simple function to see outliers, in this function we need to write the values Q1 and Q3.

![Ekran görüntüsü 2023-11-26 225839](https://github.com/enesbesinci/cltv-prediction/assets/110482608/6c5c1d0f-aec7-49dc-808c-65734edec039)

The results:

![Ekran görüntüsü 2023-11-26 225928](https://github.com/enesbesinci/cltv-prediction/assets/110482608/f5348bf0-4956-4ed6-af92-cae4ef0a5552)

Now that we have seen our outliers, we can either discard these outliers from the dataset or suppress them according to the values we want, we said that each observation is important, so we will suppress the observations that contain these outliers.

![Ekran görüntüsü 2023-11-26 230019](https://github.com/enesbesinci/cltv-prediction/assets/110482608/af094344-9cd0-4563-b679-4a89cfd82223)

Let's check if our missing data is gone.

![Ekran görüntüsü 2023-11-26 230231](https://github.com/enesbesinci/cltv-prediction/assets/110482608/9fab4046-fe5b-4536-909c-dbbf3a9638e9)






















