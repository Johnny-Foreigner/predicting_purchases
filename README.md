# Business Understanding

In the last decade, e-commerce has fundamentally changed how we live our lives through how we shop. Companies such as Sears have gone bankrupt over the years, making the transition from brick and mortar to an online e-commerce marketplace, however other companies such as Chewy, have been able to exploit e-commerce to become a market leader in their category.

A study by emarketer.com found that the pandemic has had beneficial effects on US e-commerce. Sales will reach $794.50 billion this year, up 32.4% year-over-year. That’s a much higher growth rate than the 18.0% predicted in our Q2 forecast, as consumers continue to avoid stores and opt for online shopping amid the pandemic. By the end of the year e-commerce sales will reach 14.4% of all US retail spending for the year and 19.2% by 2024. If you further dig into the data and exclude gas and auto sales (categories sold almost exclusively offline), ecommerce penetration jumps to 20.6%.(1)

With e-commerce growing at such an unprecedented rate, many companies are to capitalise on this change in consumer behaviour. It comes as no surprise to many that purchasing items online is a different process from buying an item in a store. While in a store an employee can help guide a customer to items they are both looking for and items they may want to consider purchasing, many e-commerce marketplaces dont have the same leverage; it's much easier to close out of a "You should buy" popup, rather than to ignore the advice of an instore expert. 

This has presented a serious challenge to e-commerce stores in the form of cart abandonment. Cart abandonment is when a customer leaves without buying, after adding an item to their cart. It is a trend that has remained steady since e-commerce entered the mainstream, as seen in the below chart from statista.

<a href="https://www.statista.com/statistics/477804/online-shopping-cart-abandonment-rate-worldwide/" rel="nofollow"><img src="https://www.statista.com/graphic/1/477804/online-shopping-cart-abandonment-rate-worldwide.jpg" alt="Statistic: Online shopping cart abandonment rate worldwide from 2006 to 2019 | Statista" style="width: 100%; height: auto !important; max-width:1000px;-ms-interpolation-mode: bicubic;"/></a><br />Find more statistics at  <a href="https://www.statista.com" rel="nofollow">Statista</a>

The good news for data scientists is that during an in store purchase a customer can have a large degree of privacy, while almost every move an online customer makes is tracked and stored in a series of databases. This data can be used to produce insights into customer purchasing behaviour, and spending patterns. Using the "eCommerce Events History in Cosmetics Shop" on kaggle (3) https://www.kaggle.com/mkechinov/ecommerce-events-history-in-cosmetics-shop we plan to analyse the purchasing patterns in the five month history the dataset provides and predict wether a customer is going to remove an item from their cart. With this knowledge a company can then provide incentives, such as free shipping or discounts to turn that removal into a purchase.

# Data Understanding

The kaggle e-commerce dataset contains behavior data for five months (Oct 2019 – Feb 2020) from a medium sized unnamed cosmetics online store. Each row in the file represents an online event or action. All events are related to products and users.

The dataset contains the following features:
1. `event_time`: Time when event happened at (in UTC).
2. `event_type`: What action a customer took. For more information please see below.
3. `product_id`: ID of a product
4. `category_id`: Product's category ID
5. `category_code`: Product's category taxonomy (code name) if it was possible to make it. Usually present for meaningful categories and skipped for different kinds of accessories.
6. `brand`: Downcased string of brand name. Can be a nan value, if it was missed.
7. `price`: Float price of a product. Present.
8. `user_id`: Permanent user ID.
9. `user_session` :Temporary user's session ID. Same for each user's session. This value is changed every time user come back to online store from a long pause.


`event_type` is further broken up into four components, these are:
1. `view` - a user viewed a product
2. `cart` - a user added a product to shopping cart
3. `remove_from_cart` - a user removed a product from shopping cart
4. `purchase` - a user purchased a product

An example of a purchase funnel, may be three chronological rows, with the rows sharing `user_session` and `user_id` values. Where the first row is a view, the second is cart, and the final is a purchase.

https://www.kaggle.com/mkechinov/ecommerce-events-history-in-cosmetics-shop

# Data Preparation

First: After importing our data, I first converted the CSV files to a Pandas DataFrame, and dropped rows containing `cart` or `view` as they are irrelevant to our prediction. You must first add an item to the cart before you can purchase or remove the item.

Second: I dropped the redundant feature `category_id` and filled in the NA's within `brand` as `unknown` as well as dropping the remaining NAs from the DataFrame. We also dropped any rows with a negative value in `price`.

Third: As our DataFrame doesn't have many feautres, we will have to build out our own features to help our models perform better. As we have a timestamp for every event, we will start with Feature Engineering using time.

Fourth: I then used LabelBinarizer and OneHotEncoder on several feature in the natives dataset, and several of the features we built above to create many new features.

Fifth: Now that are data had been cleaned up and the new features we wanted to add have been built, we then needed to establish what our target column is and set it to y, with our predictors being set to X. We set y equal to `event_time`

Finally: We saw we had roughly 2:1 class imbalance in our dataset, so before we could train_test_split our data we SMOTEd the data to balance our targets.

# Model

# Evalutaiton and Deployment

# Summary and Next Steps
I have addressed the problem of items being removed from the cart by creating a predictive model to identify events that will likely result in a item being `removed_from_cart`. This will allow e-commerce executives and key stakeholders to push incentives on those that are at risk of not purchasing an item. These could be:
1. Free shipping
2. Discounts
3. Bundles

We studied the problem and the data available, iterated through several model prototypes, and developed a model successfully predicts almost XYZ of removed from cart events. I hope my predictive model will assist companies and organizations in targeting those at high risk of removing an item from their cart. You've done the hard work getting customers to visit your site, don't loose easy money by not closing the sale.


# Citations

(1) https://www.emarketer.com/content/us-ecommerce-growth-jumps-more-than-30-accelerating-online-shopping-shift-by-nearly-2-years
https://www.barilliance.com/cart-abandonment-rate-statistics/#:~:text=Cart%20Abandonment%20Rate%20Trends%3A%202006%2D%202020,-We%20compared%20abandonment&text=In%20one%20year%2C%20stores%20on,leaving%20without%20completing%20their%20purchase.&text=In%202006%2C%2059.8%25%20of%20shoppers,%25%2C%20a%2015.79%25%20increase.
(statista) https://www.statista.com/statistics/477804/online-shopping-cart-abandonment-rate-worldwide/
(3) https://www.kaggle.com/mkechinov/ecommerce-events-history-in-cosmetics-shop
