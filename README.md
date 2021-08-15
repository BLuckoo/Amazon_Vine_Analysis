<p align="center">
<image src="https://user-images.githubusercontent.com/82583576/129462078-923766d8-b765-4038-a86d-364b4faf586c.png"
</p>


## **Amazon Vine Analysis**
  
### Overview of the analysis of the Amazon Vine program ###  

Manufacturers pay a fee to Amazon and provides free products to Amazon Vine members so they can try the products and write reviews about these products.

This project aanalyzes product reviews written by Amazon customers, Vine members and non-Vine members to find out if there is any bias in the reviews from the Vine member reviews versus non-Vine member reviews.  
  
Datasets containing the reviews are made public by Amazon. For the project, the dataset containing reviews of "Home Entertainment products" was used. 
This dataset can be found [HERE](https://s3.amazonaws.com/amazon-reviews-pds/tsv/amazon_reviews_us_Home_Entertainment_v1_00.tsv.gz). *CAUTION - this is a BIG DATA file which requires a lot of space.*

The analysis has been done using an ETL (Extract, Transform, Load) process using Cloud-based tools such as Google Colab and an AWS RDS instance as well as PostGreSQL. 
PySpark was used to retrieve the reviews data using DataFrames. Filters, counts and statistical functions were then used to analyze the data.   
  

### Results of the analysis ###  

Using Google Colab and PySpark, the reviews for Home Entertainment were extracted and and loaded into a DataFrame as illustrated below.

https://github.com/BLuckoo/Amazon_Vine_Analysis/blob/main/Images/AWSData.PNG
  
<p align="center">
<image src="https://user-images.githubusercontent.com/82583576/129478139-2a41f3ae-0825-41e6-93a3-7f50b364558e.PNG"
</p>
  
**The dataset was then transformed into 4 DataFrames as shown below:**
 
1. The customer_id DataFrame was created by grouping the customer_id and counting the number of reviews by customer_id using the aggregate funstion in Python.

  
<p align="center">  
<image src="https://user-images.githubusercontent.com/82583576/129478243-b895595e-e764-4e77-9589-45025630cbd7.PNG"
</p>
  

  

  
2. The products DataFrame was created by selecting "product_id" and "product_title" from the reviews DataFrame and the "drop_duplicates" function was used to retrieve only unique product_ids from the reviews DataFrame.
  
<p align="center">   
<image src="https://user-images.githubusercontent.com/82583576/129478257-57a4c838-8264-4e85-9262-5a2ecbc3eef8.PNG"
</p>



  
3. The "select" function was used to create the "review_id" DataFrame. The "review_date" data was converted to a date by using the "to_date" function.
  
<p align="center">  
<image src="https://user-images.githubusercontent.com/82583576/129478263-7fdbec3c-d06e-433c-8526-8b2f064c2818.PNG"
</p>

  
 
  
4. The "vine" DataFrame containing data with regards to the reviews ratings, votes and whether the reviews are from Vine Members or not, was created using the "select" function on the reviews dataset. 
  
<p align="center">  
<image src="https://user-images.githubusercontent.com/82583576/129478270-e2758571-2ad0-48d9-8041-39cac48a41ab.PNG"
</p>

  
The 4 DataFrames were then loaded to an AWS RDS instance using PostGreSQL. The following code snippet illustrates how this was achieved.

<p align="center">  
<image src="https://user-images.githubusercontent.com/82583576/129479494-bcb7ba71-61e0-49af-bd59-83061e651a28.PNG"
</p>  

  
  
### Summary ###
  
There is approximately a 5% difference of 5-star reviews in Vine to non-Vine reviews (40.61% to 45.34%).

Although the number of Vine reviews is pretty low, so far it can still represent the product. However, the average rating from Vine customers is 4.38 with std deviation of 0.78, and this is much higher than the 3.77 from non-Vine customers.

I believe the Vine customers tend to give higher ratings and pretty focusing on the higher ratings too. So reviews from Vine customers are not that trustworthy for me.
