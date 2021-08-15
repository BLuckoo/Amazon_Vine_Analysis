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

  
  
  
### Summary ###
  
There is approximately a 5% difference of 5-star reviews in Vine to non-Vine reviews (40.61% to 45.34%).

Although the number of Vine reviews is pretty low, so far it can still represent the product. However, the average rating from Vine customers is 4.38 with std deviation of 0.78, and this is much higher than the 3.77 from non-Vine customers.

I believe the Vine customers tend to give higher ratings and pretty focusing on the higher ratings too. So reviews from Vine customers are not that trustworthy for me.
