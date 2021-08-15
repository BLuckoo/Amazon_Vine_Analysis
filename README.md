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
  

## Results of the analysis ##  

### Part 1: Extracting the Amazon Reviews Dataset and loading the data into four different tables ###
  
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

 
### Part 2: Filter the Vine reviews and determine if there is a bias in the Vine Reviews ###  

  
1. Using only certain elements of the Amazon Dataset, a vine_review DataFrame was created as follows:

<p align="center">  
<image src="https://user-images.githubusercontent.com/82583576/129481294-ab0abc9c-4340-493f-866f-6f56055eb7d0.png"
</p>
 

 
2. The vine_review DataFrame was filtered for total_votes greater than or equal to 20.
  
<p align="center">
<image src="https://user-images.githubusercontent.com/82583576/129481371-19bf35f1-e27a-4733-95bb-884a26686db1.png"
</p>


3. The vine_review DataFrame was further filtered to include only reviews whereby (helpful_reviews/total_votes) were greater than or equal to 50%.
  
<p align="center">  
<image src="https://user-images.githubusercontent.com/82583576/129481465-96cbd75a-d7d4-4e85-be38-896dac805421.png"
</p>
  

4. The new vine_review DataFrame was then filtered for Vine members and non-Vine members by filtering on the vine variable, "Y" - vine member and "N" -non- vine member.
 
   *Vine members DataFrame*   
  
<p align="center">  
<image src="https://user-images.githubusercontent.com/82583576/129481876-920616a7-69f7-4266-a08f-62a3b7655ceb.png"
</p>

  
 
  
   *Non-Vine members DataFrame*
  
<p align="center">
<image src="https://user-images.githubusercontent.com/82583576/129481911-33a0983c-40fa-483d-8d52-e149ea5d6d61.png"
</p>
  
The Vine reviews and non-Vine reviews with 5-star ratings were then extracted from the "paid_vine_review" and "unpaid_vine_review" DataFrames by using a filter on the "star_rating" columns.
  
  *5-Star Ratings from Vine Members*
  
  <p align="center">
  <image src="https://user-images.githubusercontent.com/82583576/129484069-8f077093-d1c7-4fa9-895f-f1a64cb77818.png"
  </p>
  
    
  *5-Star Ratings from Non-Vine Members*
    
  <p align="center">
  <image src="https://user-images.githubusercontent.com/82583576/129484143-a66a0fc6-4ef7-4039-a13b-857f0781e819.png"
  </p>
 
    
##   
##    
    
##  The following table summarizes the number Vine and non-Vine reviews:  
 
  <p align="center">	
  <image src="https://user-images.githubusercontent.com/82583576/129485296-48273c78-3bd5-4035-a1c0-b3ef8c7aba59.png"
  </p>
    
    
  
  ***This was extracted using the following count functions on the multiple vine_review DataFrames as illustrated below:***
 
 ![image](https://user-images.githubusercontent.com/82583576/129484970-2489bce6-0b56-463f-b536-9f158dd76f5e.png)

    
 ![image](https://user-images.githubusercontent.com/82583576/129485026-9541d2a2-9efa-45a0-a525-1bccdc68b64e.png)
   
 
 ![image](https://user-images.githubusercontent.com/82583576/129485068-603394d0-ddec-4dbb-9fdf-7e0b0f0dcfe7.png)
  
 ##   
    
 ![image](https://user-images.githubusercontent.com/82583576/129484347-d92d2d5b-24b9-4caa-b8fd-2505a71122bc.png)

    
 ![image](https://user-images.githubusercontent.com/82583576/129485007-22d9cf20-0e3d-4d45-bfba-3e33f80feee9.png)
    
 
 ![image](https://user-images.githubusercontent.com/82583576/129485082-b1dd9d38-ff52-4b1e-94dd-5faeb7c99401.png)

 ##   
    
   
 ![image](https://user-images.githubusercontent.com/82583576/129484531-5f1d2d4f-e7d1-4aa4-99bd-6e48e65e4651.png)

    
 ![image](https://user-images.githubusercontent.com/82583576/129485118-a55e9def-4720-4857-b262-3ba14903c72a.png)

  
 ![image](https://user-images.githubusercontent.com/82583576/129485134-027eb4cf-c7af-4e6c-8c94-5aefe0a661fa.png)
 
    
    
##     
##    
### Summary ###
  
- There is approximately a 5% difference of 5-star reviews in Vine to non-Vine reviews (40.61% to 45.34%).
- The number of Vine reviews is quite low, 1.07% of the total number of reviews. And out of the total number of 5-star ratings reviews, only 0.44% is accounted for by the Vine reviews.
- Within the total sample of reviews, the small proportion (1.07%) of 5-star reviews from the Vine members will not create a bias.
- Amongst the Vine members, the 5-star ratings amount to 40.61% of the Vine ratings. This relatively high percentage could indicate some bias. 
  
- A statistical analysis of the reviews, looking at the mean and standard deviations, provides some more insight into the reviews.
    
<p align="center">  	
<image src="https://user-images.githubusercontent.com/82583576/129485925-64b65b05-00a7-4571-9748-ca6decaa5e12.png"
</p>  
  
The above statistical data were obtained using the "describe" function on the reviews DataFrames, as shown below:
  
<p align="center">
<image src="https://user-images.githubusercontent.com/82583576/129486999-829f1a70-0572-4162-8557-4d75b5f43cd3.png"
</p>
  

The average rating from Vine members is 4.11 with standard deviation of 0.95.
The average rating from non-Vine members is 3.61 with standard deviation of 1.58.

This is an indication that the Vine members tend to give higher ratings. So, the reviews indicate that there is a bias towards higher ratings from the Vine members.
