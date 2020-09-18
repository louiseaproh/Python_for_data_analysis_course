# TD1_Hotel Reviews Data in Europe

The csv file contains 17 fields. The description of each field is as below:

- Hotel_Address: Address of hotel.
- Review_Date: Date when reviewer posted the corresponding review.
- Average_Score: Average Score of the hotel, calculated based on the latest comment in the last year.
- Hotel_Name: Name of Hotel
- Reviewer_Nationality: Nationality of Reviewer
- Negative_Review: Negative Review the reviewer gave to the hotel. If the reviewer does not give the negative review, then it should be: 'No Negative'
- ReviewTotalNegativeWordCounts: Total number of words in the negative review.
- Positive_Review: Positive Review the reviewer gave to the hotel. If the reviewer does not give the negative review, then it should be: 'No Positive'
- ReviewTotalPositiveWordCounts: Total number of words in the positive review.
- Reviewer_Score: Score the reviewer has given to the hotel, based on his/her experience
- TotalNumberofReviewsReviewerHasGiven: Number of Reviews the reviewers has given in the past.
- TotalNumberof_Reviews: Total number of valid reviews the hotel has.
- Tags: Tags reviewer gave the hotel.
- dayssincereview: Duration between the review date and scrape date.
- AdditionalNumberof_Scoring: There are also some guests who just made a scoring on the service rather than a review. This number indicates how many valid scores without review in there.
- lat: Latitude of the hotel
- lng: longtitude of the hotel


```python
%matplotlib inline

import numpy as np
import pandas as pd
import seaborn
from pandas import Series, DataFrame
import matplotlib.pyplot as plt
```


```python
reviews = pd.read_csv("Documents\Hotel_Reviews.csv",sep=",")
reviews
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Hotel_Address</th>
      <th>Additional_Number_of_Scoring</th>
      <th>Review_Date</th>
      <th>Average_Score</th>
      <th>Hotel_Name</th>
      <th>Reviewer_Nationality</th>
      <th>Negative_Review</th>
      <th>Review_Total_Negative_Word_Counts</th>
      <th>Total_Number_of_Reviews</th>
      <th>Positive_Review</th>
      <th>Review_Total_Positive_Word_Counts</th>
      <th>Total_Number_of_Reviews_Reviewer_Has_Given</th>
      <th>Reviewer_Score</th>
      <th>Tags</th>
      <th>days_since_review</th>
      <th>lat</th>
      <th>lng</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>s Gravesandestraat 55 Oost 1092 AA Amsterdam ...</td>
      <td>194</td>
      <td>8/3/2017</td>
      <td>7.7</td>
      <td>Hotel Arena</td>
      <td>Russia</td>
      <td>I am so angry that i made this post available...</td>
      <td>397</td>
      <td>1403</td>
      <td>Only the park outside of the hotel was beauti...</td>
      <td>11</td>
      <td>7</td>
      <td>2.9</td>
      <td>[' Leisure trip ', ' Couple ', ' Duplex Double...</td>
      <td>0 days</td>
      <td>52.360576</td>
      <td>4.915968</td>
    </tr>
    <tr>
      <th>1</th>
      <td>s Gravesandestraat 55 Oost 1092 AA Amsterdam ...</td>
      <td>194</td>
      <td>8/3/2017</td>
      <td>7.7</td>
      <td>Hotel Arena</td>
      <td>Ireland</td>
      <td>No Negative</td>
      <td>0</td>
      <td>1403</td>
      <td>No real complaints the hotel was great great ...</td>
      <td>105</td>
      <td>7</td>
      <td>7.5</td>
      <td>[' Leisure trip ', ' Couple ', ' Duplex Double...</td>
      <td>0 days</td>
      <td>52.360576</td>
      <td>4.915968</td>
    </tr>
    <tr>
      <th>2</th>
      <td>s Gravesandestraat 55 Oost 1092 AA Amsterdam ...</td>
      <td>194</td>
      <td>7/31/2017</td>
      <td>7.7</td>
      <td>Hotel Arena</td>
      <td>Australia</td>
      <td>Rooms are nice but for elderly a bit difficul...</td>
      <td>42</td>
      <td>1403</td>
      <td>Location was good and staff were ok It is cut...</td>
      <td>21</td>
      <td>9</td>
      <td>7.1</td>
      <td>[' Leisure trip ', ' Family with young childre...</td>
      <td>3 days</td>
      <td>52.360576</td>
      <td>4.915968</td>
    </tr>
    <tr>
      <th>3</th>
      <td>s Gravesandestraat 55 Oost 1092 AA Amsterdam ...</td>
      <td>194</td>
      <td>7/31/2017</td>
      <td>7.7</td>
      <td>Hotel Arena</td>
      <td>United Kingdom</td>
      <td>My room was dirty and I was afraid to walk ba...</td>
      <td>210</td>
      <td>1403</td>
      <td>Great location in nice surroundings the bar a...</td>
      <td>26</td>
      <td>1</td>
      <td>3.8</td>
      <td>[' Leisure trip ', ' Solo traveler ', ' Duplex...</td>
      <td>3 days</td>
      <td>52.360576</td>
      <td>4.915968</td>
    </tr>
    <tr>
      <th>4</th>
      <td>s Gravesandestraat 55 Oost 1092 AA Amsterdam ...</td>
      <td>194</td>
      <td>7/24/2017</td>
      <td>7.7</td>
      <td>Hotel Arena</td>
      <td>New Zealand</td>
      <td>You When I booked with your company on line y...</td>
      <td>140</td>
      <td>1403</td>
      <td>Amazing location and building Romantic setting</td>
      <td>8</td>
      <td>3</td>
      <td>6.7</td>
      <td>[' Leisure trip ', ' Couple ', ' Suite ', ' St...</td>
      <td>10 days</td>
      <td>52.360576</td>
      <td>4.915968</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>515733</th>
      <td>Wurzbachgasse 21 15 Rudolfsheim F nfhaus 1150 ...</td>
      <td>168</td>
      <td>8/30/2015</td>
      <td>8.1</td>
      <td>Atlantis Hotel Vienna</td>
      <td>Kuwait</td>
      <td>no trolly or staff to help you take the lugga...</td>
      <td>14</td>
      <td>2823</td>
      <td>location</td>
      <td>2</td>
      <td>8</td>
      <td>7.0</td>
      <td>[' Leisure trip ', ' Family with older childre...</td>
      <td>704 day</td>
      <td>48.203745</td>
      <td>16.335677</td>
    </tr>
    <tr>
      <th>515734</th>
      <td>Wurzbachgasse 21 15 Rudolfsheim F nfhaus 1150 ...</td>
      <td>168</td>
      <td>8/22/2015</td>
      <td>8.1</td>
      <td>Atlantis Hotel Vienna</td>
      <td>Estonia</td>
      <td>The hotel looks like 3 but surely not 4</td>
      <td>11</td>
      <td>2823</td>
      <td>Breakfast was ok and we got earlier check in</td>
      <td>11</td>
      <td>12</td>
      <td>5.8</td>
      <td>[' Leisure trip ', ' Family with young childre...</td>
      <td>712 day</td>
      <td>48.203745</td>
      <td>16.335677</td>
    </tr>
    <tr>
      <th>515735</th>
      <td>Wurzbachgasse 21 15 Rudolfsheim F nfhaus 1150 ...</td>
      <td>168</td>
      <td>8/19/2015</td>
      <td>8.1</td>
      <td>Atlantis Hotel Vienna</td>
      <td>Egypt</td>
      <td>The ac was useless It was a hot week in vienn...</td>
      <td>19</td>
      <td>2823</td>
      <td>No Positive</td>
      <td>0</td>
      <td>3</td>
      <td>2.5</td>
      <td>[' Leisure trip ', ' Family with older childre...</td>
      <td>715 day</td>
      <td>48.203745</td>
      <td>16.335677</td>
    </tr>
    <tr>
      <th>515736</th>
      <td>Wurzbachgasse 21 15 Rudolfsheim F nfhaus 1150 ...</td>
      <td>168</td>
      <td>8/17/2015</td>
      <td>8.1</td>
      <td>Atlantis Hotel Vienna</td>
      <td>Mexico</td>
      <td>No Negative</td>
      <td>0</td>
      <td>2823</td>
      <td>The rooms are enormous and really comfortable...</td>
      <td>25</td>
      <td>3</td>
      <td>8.8</td>
      <td>[' Leisure trip ', ' Group ', ' Standard Tripl...</td>
      <td>717 day</td>
      <td>48.203745</td>
      <td>16.335677</td>
    </tr>
    <tr>
      <th>515737</th>
      <td>Wurzbachgasse 21 15 Rudolfsheim F nfhaus 1150 ...</td>
      <td>168</td>
      <td>8/9/2015</td>
      <td>8.1</td>
      <td>Atlantis Hotel Vienna</td>
      <td>Hungary</td>
      <td>I was in 3rd floor It didn t work Free Wife</td>
      <td>13</td>
      <td>2823</td>
      <td>staff was very kind</td>
      <td>6</td>
      <td>1</td>
      <td>8.3</td>
      <td>[' Leisure trip ', ' Family with young childre...</td>
      <td>725 day</td>
      <td>48.203745</td>
      <td>16.335677</td>
    </tr>
  </tbody>
</table>
<p>515738 rows × 17 columns</p>
</div>




```python
nb_row, nb_col = reviews.shape
print(f"nbr rows : {nb_row}\nnbr col : {nb_col}")
```

    nbr rows : 515738
    nbr col : 17
    


```python
reviews.columns
```




    Index(['Hotel_Address', 'Additional_Number_of_Scoring', 'Review_Date',
           'Average_Score', 'Hotel_Name', 'Reviewer_Nationality',
           'Negative_Review', 'Review_Total_Negative_Word_Counts',
           'Total_Number_of_Reviews', 'Positive_Review',
           'Review_Total_Positive_Word_Counts',
           'Total_Number_of_Reviews_Reviewer_Has_Given', 'Reviewer_Score', 'Tags',
           'days_since_review', 'lat', 'lng'],
          dtype='object')



#### Nbr of different nationalities 


```python
reviews.Reviewer_Nationality.nunique()
```




    227



#### Nbr of differnt hotels in the dataset


```python
reviews.Hotel_Address.nunique()
```




    1493



### Top of the most represented nationalities in the clients who gave a review


```python
reviews.groupby("Reviewer_Nationality").Reviewer_Nationality.count().sort_values()[-20:]
```




    Reviewer_Nationality
     South Africa                   3821
     Russia                         3900
     Romania                        4552
     Spain                          4737
     Kuwait                         4920
     Turkey                         5444
     Belgium                        6031
     Italy                          6114
     Israel                         6610
     France                         7296
     Canada                         7894
     Germany                        7941
     Switzerland                    8678
     Netherlands                    8772
     Saudi Arabia                   8951
     United Arab Emirates          10235
     Ireland                       14827
     Australia                     21686
     United States of America      35437
     United Kingdom               245246
    Name: Reviewer_Nationality, dtype: int64



### Top 20 hotels that have the LEAST reviews


```python
reviews.groupby('Hotel_Name').Hotel_Name.count().sort_values()[:20]
```




    Hotel_Name
    Hotel Gallitzinberg                           8
    Mercure Paris Porte d Orleans                10
    Hotel Wagner                                 10
    Boundary Rooms Suites                        12
    Ibis Styles Milano Palmanova                 12
    Hotel Eitlj rg                               12
    Le Lavoisier                                 12
    Hotel Daniel Paris                           12
    XO Hotel                                     13
    Renaissance Paris Republique Hotel Spa       13
    MARQUIS Faubourg St Honor Relais Ch teaux    13
    Ibis Styles Paris Gare Saint Lazare          14
    Pershing Hall                                14
    Drawing Hotel                                14
    Room Mate Gerard                             14
    Renaissance Paris Vendome Hotel              14
    Hotel Chavanel                               14
    Melia Paris Champs Elys es                   15
    Hotel Eiffel Blomet                          15
    Splendide Royal Paris                        16
    Name: Hotel_Name, dtype: int64



### Top of the 20 top rated hotels


```python
NewDf = reviews.filter(items=['Hotel_Name', 'Average_Score'])
ScoresList = NewDf.drop_duplicates()
ScoresList.sort_values(by='Average_Score')[-10:]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Hotel_Name</th>
      <th>Average_Score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>299896</th>
      <td>Palais Coburg Residenz</td>
      <td>9.5</td>
    </tr>
    <tr>
      <th>176748</th>
      <td>The Soho Hotel</td>
      <td>9.5</td>
    </tr>
    <tr>
      <th>291408</th>
      <td>Taj 51 Buckingham Gate Suites and Residences</td>
      <td>9.5</td>
    </tr>
    <tr>
      <th>14708</th>
      <td>Haymarket Hotel</td>
      <td>9.6</td>
    </tr>
    <tr>
      <th>176997</th>
      <td>H tel de La Tamise Esprit de France</td>
      <td>9.6</td>
    </tr>
    <tr>
      <th>185602</th>
      <td>41</td>
      <td>9.6</td>
    </tr>
    <tr>
      <th>402244</th>
      <td>H10 Casa Mimosa 4 Sup</td>
      <td>9.6</td>
    </tr>
    <tr>
      <th>316447</th>
      <td>Hotel Casa Camper</td>
      <td>9.6</td>
    </tr>
    <tr>
      <th>398945</th>
      <td>Hotel The Serras</td>
      <td>9.6</td>
    </tr>
    <tr>
      <th>54717</th>
      <td>Ritz Paris</td>
      <td>9.8</td>
    </tr>
  </tbody>
</table>
</div>



### Hotel ranking by number of  ONLY positive reviews


```python
OnlyPositivReview = reviews[(reviews.Review_Total_Negative_Word_Counts == 0)].groupby('Hotel_Name').Hotel_Name.count().sort_values(ascending=False)
OnlyPositivReview
```




    Hotel_Name
    Park Plaza Westminster Bridge London                 962
    Strand Palace Hotel                                  827
    DoubleTree by Hilton Hotel London Tower of London    817
    Intercontinental London The O2                       712
    Copthorne Tara Hotel London Kensington               697
                                                        ... 
    Mercure Paris Op ra Faubourg Montmartre                2
    Admiral Hotel                                          2
    Kube Hotel Ice Bar                                     1
    Ibis Styles Milano Palmanova                           1
    Hotel Liberty                                          1
    Name: Hotel_Name, Length: 1492, dtype: int64



### Evolution of the number of POSITIVE reviews for a given hotel (here 'Strand Palace Hotel' )


```python


reviews['Review_Date'] = pd.to_datetime(reviews['Review_Date']).dt.to_period('M')
TopHotelReviews = reviews[(reviews.Hotel_Name == 'Strand Palace Hotel')]

df = TopHotelReviews.filter(items=['Reviewer_Score','Review_Date','Review_Total_Negative_Word_Counts']).sort_values(by='Review_Date')
OnlyPositive = df[(df.Review_Total_Negative_Word_Counts == 0)]

plt.rcParams['figure.figsize'] = [15, 6] # Taille du graphique en pouces
plt.rcParams['figure.dpi'] = 200 # résolution en points par pouce
OnlyPositive.groupby('Review_Date').Reviewer_Score.count().plot()
```




    <AxesSubplot:xlabel='Review_Date'>




![png](output_19_1.png)


### Global representation of the average_score of the hotels


```python
data_plot = reviews[["Hotel_Name","Average_Score"]].drop_duplicates()
data_plot.groupby('Average_Score').Hotel_Name.count().plot(kind='bar')

# We can see that most hotel ratings are between 8.0 and 8.8
```




    <AxesSubplot:xlabel='Average_Score'>




![png](output_21_1.png)


### Representation of the WORSE average of the scores given according to the nationality of the reviewers


```python
dfFinal=pd.pivot_table(data=reviews,index='Reviewer_Nationality')
dfFinal.filter(items=["Reviewer_Nationality","Reviewer_Score"]).sort_values(by='Reviewer_Score')[:30].plot(kind='bar')
```




    <AxesSubplot:xlabel='Reviewer_Nationality'>




![png](output_23_1.png)

