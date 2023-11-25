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





