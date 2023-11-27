# Customer Life Time Value Prediction

Hello everyone, in this project we are going to calculate customer life time value (also knowns as CLTV or CLV) using GCP technologies like Google Cloud Storage, BigQuery, Vertex AI and Google Looker Studio.

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

## STEP 3: Data preprocessing, Data Exploration and Calculation of Metrics Needed to Calculate CLTV on Vertex Ai Workbench

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

Now that we no longer have any missing or outlier data, we can begin to create the metrics needed to calculate CLTV.

## STEP 4: How to Calculate the CLTV Prediction

First of all we create a new more simple dataframe to calculate the metrics to CLTV easily

![Ekran görüntüsü 2023-11-27 125337](https://github.com/enesbesinci/cltv-prediction/assets/110482608/4e463efc-3dd4-4d53-888e-0e4e7c9dd53c)

After that, we set the analysis date as 2 days after the last date in the dataset for this project.

![Ekran görüntüsü 2023-11-27 125456](https://github.com/enesbesinci/cltv-prediction/assets/110482608/53ddaebf-7897-4f74-906c-d0c842e0019c)

And now, we can calculate the Tenure, Recency, Frequency and Monetary values.

![Ekran görüntüsü 2023-11-27 132602](https://github.com/enesbesinci/cltv-prediction/assets/110482608/dbde85e6-b594-4104-952a-5eba394a93e1)

Yes, we have calculated the metrics, let's take a brief look at our dataset.

![Ekran görüntüsü 2023-11-27 132742](https://github.com/enesbesinci/cltv-prediction/assets/110482608/b09c8d62-39c3-4336-b786-0977904c0a7f)

Yes, now that we have our metrics to calculate CLTV, we can now predict customer purchase frequency and purchase amounts with BG-NBD and Gamma-Gamma Submodel.

First of all, let's try to predict the 3 and 6-month purchase frequency of customers with the BG-NGB model.

![Ekran görüntüsü 2023-11-27 133228](https://github.com/enesbesinci/cltv-prediction/assets/110482608/44739f54-e96b-4a3e-9d48-6750f2e19f31)

We then estimate the average purchase amounts of each customer with the Gamma-Gamma Sub Model and after that we can now predict the 6-month CLTV for each customer.

![Ekran görüntüsü 2023-11-27 133252](https://github.com/enesbesinci/cltv-prediction/assets/110482608/1c3e7cf5-f071-4454-9477-fabd9628dee9)

Let's look at the results, it seem consistent

![Ekran görüntüsü 2023-11-27 134416](https://github.com/enesbesinci/cltv-prediction/assets/110482608/89de4b88-e3f6-4ebb-9b1d-e34f65cf2fdd)

Now we need to segment our customers according to their Customer Lifetime Values. In this way, we can differentiate between the different customers we have and perform different sales, advertising and marketing activities for each different segment. In this section, customers can be divided into 3, 4, 5 or more segments. But in this project, I will divide the customers into 4 different segments and I will sort them as A-B-C-D from the highest to the lowest CLTV value.

![Ekran görüntüsü 2023-11-27 134947](https://github.com/enesbesinci/cltv-prediction/assets/110482608/b6a46067-0bf0-4aca-90da-59caad0a0dba)

Let's analyze the differences of each segment.

![Ekran görüntüsü 2023-11-27 140434](https://github.com/enesbesinci/cltv-prediction/assets/110482608/4d7dda5a-7fe9-4390-a212-87a883f5712c)

Yes, we have made our CLTV forecasts, now we can focus on different segments and develop different sales, advertising and marketing activities for each of them. Finally, let's save our customers in segment A, the most valuable ones for us, in a csv file. For customers in this segment we can develop reward programs or different strategies to make sure they keep buying.

![Ekran görüntüsü 2023-11-27 140654](https://github.com/enesbesinci/cltv-prediction/assets/110482608/9c119c8a-ed5f-406c-b197-3efcb193132d)

![Ekran görüntüsü 2023-11-27 141102](https://github.com/enesbesinci/cltv-prediction/assets/110482608/f35678e2-d9b1-486c-893e-2e4182f1f67e)

Then, let's upload our dataset containing all our customers and the segments they belong to back to the bucket we use as file storage.

![Ekran görüntüsü 2023-11-27 145548](https://github.com/enesbesinci/cltv-prediction/assets/110482608/17a52f84-ffa4-4672-be11-d31bc931bc99)

Yes, we have uploaded.

![Ekran görüntüsü 2023-11-27 145617](https://github.com/enesbesinci/cltv-prediction/assets/110482608/ccb47c72-382a-49cd-a80f-cecd9b656f32)

As an example, let's show 20 customers belonging to segment A using BigQuery.

![Screenshot 2023-11-27 162551](https://github.com/enesbesinci/cltv-prediction-on-GCP/assets/110482608/bcb67755-40fd-49f1-9dc5-048a9f3310eb)

So now that we know our customer segments and what they mean, we are going to create some charts about our customer segments with Google Looker Studio to present the results of these predictions in a more understandable way to our team or others.


Graph 1: Expected number of total purchases by customer segments for 3 and 6 months

![Screenshot 2023-11-27 173810](https://github.com/enesbesinci/cltv-prediction-on-GCP/assets/110482608/b1724f86-473f-4d11-8d24-73a11a5a97a5)

As you can see above The 3-month total number of purchases by customers in segment A is almost equal to the 6-month total number of purchases by customers in segment D.

Graph 2: Average expected purchase amount by customer segments

![Screenshot 2023-11-27 173857](https://github.com/enesbesinci/cltv-prediction-on-GCP/assets/110482608/530e6925-cbe8-4115-be2e-9842d860a0f7)

Yes, now that we know our different customer segments and the customers included in these segments, we should develop different sales, advertising and marketing strategies for each segment and communicate with different channels.

Thank you for reading.

As expected, the average purchase amount of customers in segment A is higher than other customer segments.





