

```python
!pip install tweepy
!pip install twython
```

    Requirement already satisfied: tweepy in /anaconda2/lib/python2.7/site-packages (3.6.0)
    Requirement already satisfied: requests>=2.11.1 in /anaconda2/lib/python2.7/site-packages (from tweepy) (2.18.4)
    Requirement already satisfied: PySocks>=1.5.7 in /anaconda2/lib/python2.7/site-packages (from tweepy) (1.6.8)
    Requirement already satisfied: six>=1.10.0 in /anaconda2/lib/python2.7/site-packages (from tweepy) (1.11.0)
    Requirement already satisfied: requests-oauthlib>=0.7.0 in /anaconda2/lib/python2.7/site-packages (from tweepy) (1.0.0)
    Requirement already satisfied: chardet<3.1.0,>=3.0.2 in /anaconda2/lib/python2.7/site-packages (from requests>=2.11.1->tweepy) (3.0.4)
    Requirement already satisfied: idna<2.7,>=2.5 in /anaconda2/lib/python2.7/site-packages (from requests>=2.11.1->tweepy) (2.6)
    Requirement already satisfied: urllib3<1.23,>=1.21.1 in /anaconda2/lib/python2.7/site-packages (from requests>=2.11.1->tweepy) (1.22)
    Requirement already satisfied: certifi>=2017.4.17 in /anaconda2/lib/python2.7/site-packages (from requests>=2.11.1->tweepy) (2018.4.16)
    Requirement already satisfied: oauthlib>=0.6.2 in /anaconda2/lib/python2.7/site-packages (from requests-oauthlib>=0.7.0->tweepy) (2.1.0)
    [31mdistributed 1.21.8 requires msgpack, which is not installed.[0m
    [31mgrin 1.2.1 requires argparse>=1.1, which is not installed.[0m
    [33mYou are using pip version 10.0.1, however version 18.1 is available.
    You should consider upgrading via the 'pip install --upgrade pip' command.[0m
    Requirement already satisfied: twython in /anaconda2/lib/python2.7/site-packages (3.7.0)
    Requirement already satisfied: requests-oauthlib>=0.4.0 in /anaconda2/lib/python2.7/site-packages (from twython) (1.0.0)
    Requirement already satisfied: requests>=2.1.0 in /anaconda2/lib/python2.7/site-packages (from twython) (2.18.4)
    Requirement already satisfied: oauthlib>=0.6.2 in /anaconda2/lib/python2.7/site-packages (from requests-oauthlib>=0.4.0->twython) (2.1.0)
    Requirement already satisfied: chardet<3.1.0,>=3.0.2 in /anaconda2/lib/python2.7/site-packages (from requests>=2.1.0->twython) (3.0.4)
    Requirement already satisfied: idna<2.7,>=2.5 in /anaconda2/lib/python2.7/site-packages (from requests>=2.1.0->twython) (2.6)
    Requirement already satisfied: urllib3<1.23,>=1.21.1 in /anaconda2/lib/python2.7/site-packages (from requests>=2.1.0->twython) (1.22)
    Requirement already satisfied: certifi>=2017.4.17 in /anaconda2/lib/python2.7/site-packages (from requests>=2.1.0->twython) (2018.4.16)
    [31mdistributed 1.21.8 requires msgpack, which is not installed.[0m
    [31mgrin 1.2.1 requires argparse>=1.1, which is not installed.[0m
    [33mYou are using pip version 10.0.1, however version 18.1 is available.
    You should consider upgrading via the 'pip install --upgrade pip' command.[0m



```python
from twython import Twython
```

# Twitter has 3 different types of API's
- Search Option:  Basically all historic Tweets.  Means "not happening right now" . Limits to 100 tweets historical per search and 2,500 per day.
- Streaming Access:  Real Time Tweets= filter given 500,000 tweets on avg per min you will get 40,000/min.  
- Filter/ Fire hose would be 100,000/min (still within streaming but different)
- Paid version.  You get everything you'd want but we will not use that. 


```python
!pip install Twython
```

    Collecting Twython
      Using cached https://files.pythonhosted.org/packages/8c/2b/c0883f05b03a8e87787d56395d6c89515fe7e0bf80abd3778b6bb3a6873f/twython-3.7.0.tar.gz
    Collecting requests>=2.1.0 (from Twython)
    [?25l  Downloading https://files.pythonhosted.org/packages/7d/e3/20f3d364d6c8e5d2353c72a67778eb189176f08e873c9900e10c0287b84b/requests-2.21.0-py2.py3-none-any.whl (57kB)
    [K    100% |████████████████████████████████| 61kB 1.3MB/s 
    [?25hCollecting requests_oauthlib>=0.4.0 (from Twython)
      Downloading https://files.pythonhosted.org/packages/c2/e2/9fd03d55ffb70fe51f587f20bcf407a6927eb121de86928b34d162f0b1ac/requests_oauthlib-1.2.0-py2.py3-none-any.whl
    Collecting idna<2.9,>=2.5 (from requests>=2.1.0->Twython)
    [?25l  Downloading https://files.pythonhosted.org/packages/14/2c/cd551d81dbe15200be1cf41cd03869a46fe7226e7450af7a6545bfc474c9/idna-2.8-py2.py3-none-any.whl (58kB)
    [K    100% |████████████████████████████████| 61kB 1.5MB/s 
    [?25hCollecting urllib3<1.25,>=1.21.1 (from requests>=2.1.0->Twython)
    [?25l  Downloading https://files.pythonhosted.org/packages/62/00/ee1d7de624db8ba7090d1226aebefab96a2c71cd5cfa7629d6ad3f61b79e/urllib3-1.24.1-py2.py3-none-any.whl (118kB)
    [K    100% |████████████████████████████████| 122kB 8.9MB/s 
    [?25hCollecting chardet<3.1.0,>=3.0.2 (from requests>=2.1.0->Twython)
    [?25l  Downloading https://files.pythonhosted.org/packages/bc/a9/01ffebfb562e4274b6487b4bb1ddec7ca55ec7510b22e4c51f14098443b8/chardet-3.0.4-py2.py3-none-any.whl (133kB)
    [K    100% |████████████████████████████████| 143kB 5.6MB/s 
    [?25hRequirement already satisfied: certifi>=2017.4.17 in /anaconda2/envs/py35/lib/python3.6/site-packages (from requests>=2.1.0->Twython) (2018.11.29)
    Collecting oauthlib>=3.0.0 (from requests_oauthlib>=0.4.0->Twython)
    [?25l  Downloading https://files.pythonhosted.org/packages/16/95/699466b05b72b94a41f662dc9edf87fda4289e3602ecd42d27fcaddf7b56/oauthlib-3.0.1-py2.py3-none-any.whl (142kB)
    [K    100% |████████████████████████████████| 143kB 8.7MB/s 
    [?25hBuilding wheels for collected packages: Twython
      Running setup.py bdist_wheel for Twython ... [?25ldone
    [?25h  Stored in directory: /Users/bryanmarty/Library/Caches/pip/wheels/c2/b0/a3/5c4b4b87b8c9e4d99f1494a0b471f0134a74e5fb33d426d009
    Successfully built Twython
    Installing collected packages: idna, urllib3, chardet, requests, oauthlib, requests-oauthlib, Twython
    Successfully installed Twython-3.7.0 chardet-3.0.4 idna-2.8 oauthlib-3.0.1 requests-2.21.0 requests-oauthlib-1.2.0 urllib3-1.24.1


  source equals the actual tweet.  When pulling from Twitter's API it will pull a JSON file.  
  Twython then takes "results" and makes it a dicitonary
  
  Results has Meta Data and Statusers
  all = results["statusers"]
  tweet_df = pd.DataFrame(all)
  
  Now with the dataframe we find the "text" column and work with it.  
  Last column is the users.  user column in itself is a dictionary.  If you would like to explore that you are able to do so. 
  using list comprehendion.  we essentially make a DF of the users from the original statuser DF. 
  
  


```python
import twitter_api as access
```


```python
from twython import Twython
import twitter_api as access

twitter = Twython(access.CONSUMER_KEY,access.CONSUMER_SECRET)


```


```python
help(Twython.search)
```

    Help on function search in module twython.endpoints:
    
    search(self, **params)
        Returns a collection of relevant Tweets matching a specified query.
        
        Docs:
        https://developer.twitter.com/en/docs/tweets/search/api-reference/get-search-tweets
    



```python
results1 = twitter.search(q = "Crypto Currency", geocode = (37.0902, 95.7129),count = 100, language = "Eng", since_id = 1000)
results2 = twitter.search(q = "Crypto Currency", geocode = (37.0902, 95.7129), count = 100, language = "Eng", since_id = 800)
results3 = twitter.search(q = "Crypto Currency", geocode = (37.0902, 95.7129), count = 100, language = "Eng", since_id = 600)
results4 = twitter.search(q = "Crypto Currency", geocode = (37.0902, 95.7129), count = 100, language = "Eng", since_id = 400)
results5 = twitter.search(q = "Crypto Currency", geocode = (37.0902, 95.7129), count = 100, language = "Eng", since_id = 200)



type(results1)
```




    dict



### Lets put these results into variables
- Statuses and Metadata are returned
- We only want Statuses in the Variables


```python
all1 = results1["statuses"]
all2 = results2["statuses"]
all3 = results3["statuses"]
all4 = results4["statuses"]
all5 = results5["statuses"]
type(all1)
```




    list




```python
#RUN Heat map on Feature correlations between retweets & location/text/polarity?
results1["statuses"][0:1]
```




    [{'created_at': 'Fri Jan 25 05:24:28 +0000 2019',
      'id': 1088668907051708416,
      'id_str': '1088668907051708416',
      'text': 'RT @ICEDataServices: Our enhanced crypto data feed now offers greater transparency: streaming real-time data for 400+ fiat and crypto curre…',
      'truncated': False,
      'entities': {'hashtags': [],
       'symbols': [],
       'user_mentions': [{'screen_name': 'ICEDataServices',
         'name': 'ICE Data Services',
         'id': 146460368,
         'id_str': '146460368',
         'indices': [3, 19]}],
       'urls': []},
      'metadata': {'iso_language_code': 'en', 'result_type': 'recent'},
      'source': '<a href="http://twitter.com/download/iphone" rel="nofollow">Twitter for iPhone</a>',
      'in_reply_to_status_id': None,
      'in_reply_to_status_id_str': None,
      'in_reply_to_user_id': None,
      'in_reply_to_user_id_str': None,
      'in_reply_to_screen_name': None,
      'user': {'id': 880572548047663104,
       'id_str': '880572548047663104',
       'name': 'RK',
       'screen_name': 'imarezzaq',
       'location': 'Howli, India',
       'description': 'Only I can change my life. No one can do it for me.🤔',
       'url': None,
       'entities': {'description': {'urls': []}},
       'protected': False,
       'followers_count': 143,
       'friends_count': 193,
       'listed_count': 0,
       'created_at': 'Thu Jun 29 23:43:50 +0000 2017',
       'favourites_count': 4302,
       'utc_offset': None,
       'time_zone': None,
       'geo_enabled': True,
       'verified': False,
       'statuses_count': 1273,
       'lang': 'en',
       'contributors_enabled': False,
       'is_translator': False,
       'is_translation_enabled': False,
       'profile_background_color': 'F5F8FA',
       'profile_background_image_url': None,
       'profile_background_image_url_https': None,
       'profile_background_tile': False,
       'profile_image_url': 'http://pbs.twimg.com/profile_images/1014458176748453888/XrzEhvQf_normal.jpg',
       'profile_image_url_https': 'https://pbs.twimg.com/profile_images/1014458176748453888/XrzEhvQf_normal.jpg',
       'profile_link_color': '1DA1F2',
       'profile_sidebar_border_color': 'C0DEED',
       'profile_sidebar_fill_color': 'DDEEF6',
       'profile_text_color': '333333',
       'profile_use_background_image': True,
       'has_extended_profile': True,
       'default_profile': True,
       'default_profile_image': False,
       'following': None,
       'follow_request_sent': None,
       'notifications': None,
       'translator_type': 'none'},
      'geo': None,
      'coordinates': None,
      'place': None,
      'contributors': None,
      'retweeted_status': {'created_at': 'Thu Jan 24 16:38:32 +0000 2019',
       'id': 1088476155127259136,
       'id_str': '1088476155127259136',
       'text': 'Our enhanced crypto data feed now offers greater transparency: streaming real-time data for 400+ fiat and crypto cu… https://t.co/qK32OYiW9D',
       'truncated': True,
       'entities': {'hashtags': [],
        'symbols': [],
        'user_mentions': [],
        'urls': [{'url': 'https://t.co/qK32OYiW9D',
          'expanded_url': 'https://twitter.com/i/web/status/1088476155127259136',
          'display_url': 'twitter.com/i/web/status/1…',
          'indices': [117, 140]}]},
       'metadata': {'iso_language_code': 'en', 'result_type': 'recent'},
       'source': '<a href="http://twitter.com" rel="nofollow">Twitter Web Client</a>',
       'in_reply_to_status_id': None,
       'in_reply_to_status_id_str': None,
       'in_reply_to_user_id': None,
       'in_reply_to_user_id_str': None,
       'in_reply_to_screen_name': None,
       'user': {'id': 146460368,
        'id_str': '146460368',
        'name': 'ICE Data Services',
        'screen_name': 'ICEDataServices',
        'location': '',
        'description': 'We provide vital market information and connectivity for participants around the world.',
        'url': 'https://t.co/BT6Do7Xe9h',
        'entities': {'url': {'urls': [{'url': 'https://t.co/BT6Do7Xe9h',
            'expanded_url': 'https://www.theice.com/market-data',
            'display_url': 'theice.com/market-data',
            'indices': [0, 23]}]},
         'description': {'urls': []}},
        'protected': False,
        'followers_count': 6001,
        'friends_count': 173,
        'listed_count': 140,
        'created_at': 'Fri May 21 13:54:49 +0000 2010',
        'favourites_count': 134,
        'utc_offset': None,
        'time_zone': None,
        'geo_enabled': False,
        'verified': True,
        'statuses_count': 1099,
        'lang': 'en',
        'contributors_enabled': False,
        'is_translator': False,
        'is_translation_enabled': False,
        'profile_background_color': 'C0DEED',
        'profile_background_image_url': 'http://abs.twimg.com/images/themes/theme1/bg.png',
        'profile_background_image_url_https': 'https://abs.twimg.com/images/themes/theme1/bg.png',
        'profile_background_tile': True,
        'profile_image_url': 'http://pbs.twimg.com/profile_images/974710179055878145/Jv6R2NFc_normal.jpg',
        'profile_image_url_https': 'https://pbs.twimg.com/profile_images/974710179055878145/Jv6R2NFc_normal.jpg',
        'profile_banner_url': 'https://pbs.twimg.com/profile_banners/146460368/1524594537',
        'profile_link_color': '72C7E7',
        'profile_sidebar_border_color': 'C0DEED',
        'profile_sidebar_fill_color': 'DDEEF6',
        'profile_text_color': '333333',
        'profile_use_background_image': True,
        'has_extended_profile': False,
        'default_profile': False,
        'default_profile_image': False,
        'following': None,
        'follow_request_sent': None,
        'notifications': None,
        'translator_type': 'none'},
       'geo': None,
       'coordinates': None,
       'place': None,
       'contributors': None,
       'is_quote_status': False,
       'retweet_count': 139,
       'favorite_count': 369,
       'favorited': False,
       'retweeted': False,
       'possibly_sensitive': False,
       'lang': 'en'},
      'is_quote_status': False,
      'retweet_count': 139,
      'favorite_count': 0,
      'favorited': False,
      'retweeted': False,
      'lang': 'en'}]



### Building A Tweets DataFrame


```python
import pandas as pd
import numpy as np
tweet1 = pd.DataFrame(all1)
tweet2 = pd.DataFrame(all2)
tweet3 = pd.DataFrame(all3)
tweet4 = pd.DataFrame(all4)
tweet5 = pd.DataFrame(all5)

print(tweet1.shape)
print(tweet2.shape)
print(tweet3.shape)
print(tweet4.shape)
print(tweet5.shape)
# taking results["statuses"] into a DF
# tweet_df = pd.DataFrame(all)
# tweet_df[0:3]
```

    (100, 30)
    (100, 30)
    (100, 30)
    (100, 30)
    (100, 30)


### Stuff all DF's into a variable & concat the variable


```python
df_t = [tweet1, tweet2, tweet3, tweet4, tweet5 ]
tweet_df = pd.concat(df_t, ignore_index = True)
tweet_df.shape
```




    (500, 30)




```python
100*5
```




    500



### YOU HAVE TO REMEMBER:
- When Concating Dataframes with index rows where the row numbers hold no meaning:
- ignore_index = True


```python
# Lets see time, text and 'user'
tweet_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 500 entries, 0 to 499
    Data columns (total 30 columns):
    contributors                 0 non-null object
    coordinates                  0 non-null object
    created_at                   500 non-null object
    entities                     500 non-null object
    extended_entities            10 non-null object
    favorite_count               500 non-null int64
    favorited                    500 non-null bool
    geo                          0 non-null object
    id                           500 non-null int64
    id_str                       500 non-null object
    in_reply_to_screen_name      50 non-null object
    in_reply_to_status_id        50 non-null float64
    in_reply_to_status_id_str    50 non-null object
    in_reply_to_user_id          50 non-null float64
    in_reply_to_user_id_str      50 non-null object
    is_quote_status              500 non-null bool
    lang                         500 non-null object
    metadata                     500 non-null object
    place                        5 non-null object
    possibly_sensitive           125 non-null object
    quoted_status                5 non-null object
    quoted_status_id             25 non-null float64
    quoted_status_id_str         25 non-null object
    retweet_count                500 non-null int64
    retweeted                    500 non-null bool
    retweeted_status             360 non-null object
    source                       500 non-null object
    text                         500 non-null object
    truncated                    500 non-null bool
    user                         500 non-null object
    dtypes: bool(4), float64(3), int64(3), object(20)
    memory usage: 103.6+ KB


### Lets Observe 
- Will view "text", "user", "retweet_count", "retweeted"


```python
# To improve:  Retweet_count filter such that retweet_count >= 400 for significance of information
tweet_df.loc[0:9, ["text", "user", "retweet_count", "retweeted", "geo"]]
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
      <th>text</th>
      <th>user</th>
      <th>retweet_count</th>
      <th>retweeted</th>
      <th>geo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>RT @ICEDataServices: Our enhanced crypto data ...</td>
      <td>{'id': 880572548047663104, 'id_str': '88057254...</td>
      <td>139</td>
      <td>False</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Paving way for Crypto currency ? https://t.co/...</td>
      <td>{'id': 127342462, 'id_str': '127342462', 'name...</td>
      <td>0</td>
      <td>False</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2</th>
      <td>RT @ICEDataServices: Our enhanced crypto data ...</td>
      <td>{'id': 2432947903, 'id_str': '2432947903', 'na...</td>
      <td>139</td>
      <td>False</td>
      <td>None</td>
    </tr>
    <tr>
      <th>3</th>
      <td>What makes Cryptocurrency different from Fiat ...</td>
      <td>{'id': 886568441947111424, 'id_str': '88656844...</td>
      <td>0</td>
      <td>False</td>
      <td>None</td>
    </tr>
    <tr>
      <th>4</th>
      <td>In 2013, #dogecoin , a cryptocurrency based on...</td>
      <td>{'id': 1025266965818953728, 'id_str': '1025266...</td>
      <td>0</td>
      <td>False</td>
      <td>None</td>
    </tr>
    <tr>
      <th>5</th>
      <td>@xkatherine23 You Need Crypto Currency!!\n\nLe...</td>
      <td>{'id': 485286887, 'id_str': '485286887', 'name...</td>
      <td>0</td>
      <td>False</td>
      <td>None</td>
    </tr>
    <tr>
      <th>6</th>
      <td>#Nano #Coin #Compared To Next Coin - Crypto \n...</td>
      <td>{'id': 871976018998956034, 'id_str': '87197601...</td>
      <td>0</td>
      <td>False</td>
      <td>None</td>
    </tr>
    <tr>
      <th>7</th>
      <td>RT @aantonop: Analyst's guide to crypto-curren...</td>
      <td>{'id': 964202652404875264, 'id_str': '96420265...</td>
      <td>374</td>
      <td>False</td>
      <td>None</td>
    </tr>
    <tr>
      <th>8</th>
      <td>RT @ApolloCurrency: Great article from Forbes!...</td>
      <td>{'id': 26723847, 'id_str': '26723847', 'name':...</td>
      <td>54</td>
      <td>False</td>
      <td>None</td>
    </tr>
    <tr>
      <th>9</th>
      <td>RT @ICEDataServices: Our enhanced crypto data ...</td>
      <td>{'id': 929548801022373888, 'id_str': '92954880...</td>
      <td>139</td>
      <td>False</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
</div>



### Idea
- Once polarity found lets create a column of the polarity
- Then lets see a HeatMap Correlation between these features


```python
type([d["user"] for d in results1["statuses"]][0:1])
```




    list




```python
# Thie is a single element in the Series "User"
[d["user"] for d in results1["statuses"]][0]
```




    {'id': 880572548047663104,
     'id_str': '880572548047663104',
     'name': 'RK',
     'screen_name': 'imarezzaq',
     'location': 'Howli, India',
     'description': 'Only I can change my life. No one can do it for me.🤔',
     'url': None,
     'entities': {'description': {'urls': []}},
     'protected': False,
     'followers_count': 143,
     'friends_count': 193,
     'listed_count': 0,
     'created_at': 'Thu Jun 29 23:43:50 +0000 2017',
     'favourites_count': 4302,
     'utc_offset': None,
     'time_zone': None,
     'geo_enabled': True,
     'verified': False,
     'statuses_count': 1273,
     'lang': 'en',
     'contributors_enabled': False,
     'is_translator': False,
     'is_translation_enabled': False,
     'profile_background_color': 'F5F8FA',
     'profile_background_image_url': None,
     'profile_background_image_url_https': None,
     'profile_background_tile': False,
     'profile_image_url': 'http://pbs.twimg.com/profile_images/1014458176748453888/XrzEhvQf_normal.jpg',
     'profile_image_url_https': 'https://pbs.twimg.com/profile_images/1014458176748453888/XrzEhvQf_normal.jpg',
     'profile_link_color': '1DA1F2',
     'profile_sidebar_border_color': 'C0DEED',
     'profile_sidebar_fill_color': 'DDEEF6',
     'profile_text_color': '333333',
     'profile_use_background_image': True,
     'has_extended_profile': True,
     'default_profile': True,
     'default_profile_image': False,
     'following': None,
     'follow_request_sent': None,
     'notifications': None,
     'translator_type': 'none'}



### Combining the "user" dictionary to DF Steps
- create 5 data frames containing the users data
- stick all 5 DF into a list and assign to variable
- Concat the 5 DF into one big DF remembering to insert ignore_index = True
- Merge Big "user" DF to the right of the tweet_df


```python
# The series "user" elements can be turned into a DF because the elements
# are dictionaries and are also Huge.
user1 = pd.DataFrame([d["user"] for d in results1["statuses"]])
user2 = pd.DataFrame([d["user"] for d in results2["statuses"]])
user3 = pd.DataFrame([d["user"] for d in results3["statuses"]])
user4 = pd.DataFrame([d["user"] for d in results4["statuses"]])
user5 = pd.DataFrame([d["user"] for d in results5["statuses"]])

df_u = [user1, user2, user3, user4, user5]

user_df = pd.concat(df_u, ignore_index = True)
```

### Before we merge lets take a look at the "user" specific data


```python
user_df[0:1]
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
      <th>contributors_enabled</th>
      <th>created_at</th>
      <th>default_profile</th>
      <th>default_profile_image</th>
      <th>description</th>
      <th>entities</th>
      <th>favourites_count</th>
      <th>follow_request_sent</th>
      <th>followers_count</th>
      <th>following</th>
      <th>...</th>
      <th>profile_text_color</th>
      <th>profile_use_background_image</th>
      <th>protected</th>
      <th>screen_name</th>
      <th>statuses_count</th>
      <th>time_zone</th>
      <th>translator_type</th>
      <th>url</th>
      <th>utc_offset</th>
      <th>verified</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>False</td>
      <td>Thu Jun 29 23:43:50 +0000 2017</td>
      <td>True</td>
      <td>False</td>
      <td>Only I can change my life. No one can do it fo...</td>
      <td>{'description': {'urls': []}}</td>
      <td>4302</td>
      <td>None</td>
      <td>143</td>
      <td>None</td>
      <td>...</td>
      <td>333333</td>
      <td>True</td>
      <td>False</td>
      <td>imarezzaq</td>
      <td>1273</td>
      <td>None</td>
      <td>none</td>
      <td>None</td>
      <td>None</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 42 columns</p>
</div>




```python
#what is statuses_count
user_df.loc[0:10, ["created_at", "time_zone", "screen_name", "statuses_count"]]
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
      <th>created_at</th>
      <th>time_zone</th>
      <th>screen_name</th>
      <th>statuses_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Thu Jun 29 23:43:50 +0000 2017</td>
      <td>None</td>
      <td>imarezzaq</td>
      <td>1273</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Sun Mar 28 22:15:56 +0000 2010</td>
      <td>None</td>
      <td>SubodhSrivastva</td>
      <td>48814</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Tue Apr 08 02:58:05 +0000 2014</td>
      <td>None</td>
      <td>HMNorazizi</td>
      <td>174</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sun Jul 16 12:49:22 +0000 2017</td>
      <td>None</td>
      <td>altcoinalerts</td>
      <td>1545</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Fri Aug 03 06:27:48 +0000 2018</td>
      <td>None</td>
      <td>LtdPrincipal</td>
      <td>186</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Tue Feb 07 01:39:34 +0000 2012</td>
      <td>None</td>
      <td>FranswaHarold</td>
      <td>104</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Tue Jun 06 06:24:18 +0000 2017</td>
      <td>None</td>
      <td>anjaliverma2usa</td>
      <td>2349</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Thu Feb 15 18:20:01 +0000 2018</td>
      <td>None</td>
      <td>cryptoslawyer</td>
      <td>4429</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Thu Mar 26 10:48:42 +0000 2009</td>
      <td>None</td>
      <td>barra43</td>
      <td>1455</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Sun Nov 12 03:17:59 +0000 2017</td>
      <td>None</td>
      <td>ThomasDemichele</td>
      <td>1540</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Wed Oct 17 04:47:43 +0000 2018</td>
      <td>None</td>
      <td>CeriolaJomar</td>
      <td>93</td>
    </tr>
  </tbody>
</table>
</div>




```python
user_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 500 entries, 0 to 499
    Data columns (total 42 columns):
    contributors_enabled                  500 non-null bool
    created_at                            500 non-null object
    default_profile                       500 non-null bool
    default_profile_image                 500 non-null bool
    description                           500 non-null object
    entities                              500 non-null object
    favourites_count                      500 non-null int64
    follow_request_sent                   0 non-null object
    followers_count                       500 non-null int64
    following                             0 non-null object
    friends_count                         500 non-null int64
    geo_enabled                           500 non-null bool
    has_extended_profile                  500 non-null bool
    id                                    500 non-null int64
    id_str                                500 non-null object
    is_translation_enabled                500 non-null bool
    is_translator                         500 non-null bool
    lang                                  500 non-null object
    listed_count                          500 non-null int64
    location                              500 non-null object
    name                                  500 non-null object
    notifications                         0 non-null object
    profile_background_color              500 non-null object
    profile_background_image_url          280 non-null object
    profile_background_image_url_https    280 non-null object
    profile_background_tile               500 non-null bool
    profile_banner_url                    310 non-null object
    profile_image_url                     500 non-null object
    profile_image_url_https               500 non-null object
    profile_link_color                    500 non-null object
    profile_sidebar_border_color          500 non-null object
    profile_sidebar_fill_color            500 non-null object
    profile_text_color                    500 non-null object
    profile_use_background_image          500 non-null bool
    protected                             500 non-null bool
    screen_name                           500 non-null object
    statuses_count                        500 non-null int64
    time_zone                             0 non-null object
    translator_type                       500 non-null object
    url                                   150 non-null object
    utc_offset                            0 non-null object
    verified                              500 non-null bool
    dtypes: bool(11), int64(6), object(25)
    memory usage: 126.5+ KB



```python
print(tweet_df.shape)
print(user_df.shape)
```

    (500, 30)
    (500, 42)



```python
# This puts it on the right hand side. 
twython_df = tweet_df.merge(user_df, left_index = True, right_index = True)
twython_df.shape
```




    (500, 72)



### Merging on the index widens the DataFrame


```python
twython_df["location"].isnull().sum()
```




    0




```python
twython_df["location"].value_counts()
```




                                                  170
    California, USA                                20
    United States                                  20
    Michigan, USA                                  15
    Dallas, Texas                                  10
    0x1734eD2675cdcEe41848a2Cf3d6022FF0abeE64A      5
    Spokane, WA                                     5
    Singapore                                       5
    Virginia, USA                                   5
    Indonesia                                       5
    London, Ontario                                 5
    Mexico                                          5
    puthia, Rajshahi, Bangladesh                    5
    대한민국                                            5
    Hong Kong                                       5
    Alpha Centauri                                  5
    Hell                                            5
    Nashville, TN                                   5
    The Las Vegas Strip, Paradise                   5
    Sacramento, CA                                  5
    New York, NY                                    5
    Howli, India                                    5
    Pennsylvania, USA                               5
    San Jose, CA                                    5
    Blockchain                                      5
    Bengaluru, India                                5
    Grapevine, TX                                   5
    waya st.                                        5
    Toronto                                         5
    Kirti Nagar, New Delhi                          5
    MENTION FOR RETWEET                             5
    North Carolina, USA                             5
    London, England                                 5
    Cape Town, South Africa                         5
    NSA Capital of the World                        5
    Chicago                                         5
    Nairobi, Kenya                                  5
    Fair Lawn , NJ                                  5
    Moon 🚀                                          5
    Chicago, IL                                     5
    Brisbane, Australia                             5
    world                                           5
    United Kingdom                                  5
    Layton, UT                                      5
    Mexico City                                     5
    Edison, NJ                                      5
    Омск, Россия                                    5
    Bumine Pengeran                                 5
    Amsterdam, The Netherlands                      5
    Global                                          5
    Europe                                          5
    dhubri, Assam, India                            5
    Ybor City, FL                                   5
    Worldwide                                       5
    Malaysia                                        5
    Miami, FL                                       5
    Cần Thơ, Vietnam                                5
    Kampala, Uganda                                 5
    Name: location, dtype: int64




```python
twython_df["location"].nunique()
```




    58



### Lets Graph


```python
import matplotlib.pyplot as plt
%matplotlib inline

twython_df.location.value_counts().plot(kind = "bar", figsize = (10,10))
```




    <matplotlib.axes._subplots.AxesSubplot at 0x113259630>




![png](output_37_1.png)



```python
twython_df[twython_df.location == "The Las Vegas Strip, Paradise"].entities_x.count()
```




    5




```python
#Lets get all the US Crypto Tweets into the graph
twython_df[twython_df.location == "Las Vegas, NV"].entities_x.count()
```




    0




```python
twython_df[twython_df.location == "FL"].entities_x.count()
```




    0



### Lets find USA Total Twitter Responses 
- Very annoying as this changes everytime i run the code


```python
#Add up all the unique cities where "Crypto Currency" was tweeted about
twython_df[(twython_df.location == "San Jose, CA" )| (twython_df.location == "Columbus, OH") | (twython_df[twython_df.location == "The Las Vegas Strip, Paradise"].entities_x.count()) | (twython_df.location == "Austin, TX") |
          (twython_df.location == "Richmond, VA") | (twython_df.location == "Chicago, IL") | (twython_df.location == "Maryland, USA") | (twython_df.location =="Santa Rosa, CA") |
          (twython_df.location == "Oxnard, CA") | (twython_df.location == "New York NY") ].entities_x.count()
```




    0



### Total "I live in the hub" responses


```python
twython_df[twython_df.location == "I live in the HUB"].entities_x.count()
```




    0



### Idea 2 is figuring out how to put sum of US tweets as an element in "Location Colunm" so I can graph it
- From the above we do know the US had the most Tweets about Crypto Currency 


```python
twython_df.loc[0:5, ["text"]]
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
      <th>text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>RT @alphax_official: UNIQUE SELLING POINTS (US...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>RT @HandleMode: Don't miss your chance at the ...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>RT @alphax_official: UNIQUE SELLING POINTS (US...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>RT @Blindripper85: Arthur will certainly be a ...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>How they do it? Hazy, they mention crypto curr...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>RT @CriptonBon: @KKcoinEX I think this project...</td>
    </tr>
  </tbody>
</table>
</div>




```python
#i 

import pandas as pd
import numpy as np
import scipy as sp 
from sklearn.cross_validation import train_test_split
from sklearn.feature_extraction.text import CountVectorizer, TfidfVectorizer

from sklearn.naive_bayes import MultinomialNB
from sklearn.linear_model import LogisticRegression
from sklearn import metrics
from textblob import TextBlob, Word
from nltk.stem.snowball import SnowballStemmer
%matplotlib inline


```

    /anaconda2/lib/python2.7/site-packages/sklearn/cross_validation.py:41: DeprecationWarning: This module was deprecated in version 0.18 in favor of the model_selection module into which all the refactored classes and functions are moved. Also note that the interface of the new CV iterators are different from that of this module. This module will be removed in 0.20.
      "This module will be removed in 0.20.", DeprecationWarning)



```python
twython_df.head()
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
      <th>contributors</th>
      <th>coordinates</th>
      <th>created_at_x</th>
      <th>entities_x</th>
      <th>favorite_count</th>
      <th>favorited</th>
      <th>geo</th>
      <th>id_x</th>
      <th>id_str_x</th>
      <th>in_reply_to_screen_name</th>
      <th>...</th>
      <th>profile_text_color</th>
      <th>profile_use_background_image</th>
      <th>protected</th>
      <th>screen_name</th>
      <th>statuses_count</th>
      <th>time_zone</th>
      <th>translator_type</th>
      <th>url</th>
      <th>utc_offset</th>
      <th>verified</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>None</td>
      <td>None</td>
      <td>Sat Oct 20 16:58:12 +0000 2018</td>
      <td>{u'symbols': [], u'user_mentions': [{u'id': 10...</td>
      <td>0</td>
      <td>False</td>
      <td>None</td>
      <td>1053691865868906496</td>
      <td>1053691865868906496</td>
      <td>None</td>
      <td>...</td>
      <td>333333</td>
      <td>True</td>
      <td>False</td>
      <td>abdullahbd985</td>
      <td>57</td>
      <td>None</td>
      <td>none</td>
      <td>None</td>
      <td>None</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>None</td>
      <td>None</td>
      <td>Sat Oct 20 16:57:42 +0000 2018</td>
      <td>{u'symbols': [], u'user_mentions': [{u'id': 95...</td>
      <td>0</td>
      <td>False</td>
      <td>None</td>
      <td>1053691739859386368</td>
      <td>1053691739859386368</td>
      <td>None</td>
      <td>...</td>
      <td>333333</td>
      <td>True</td>
      <td>False</td>
      <td>MinhTranHoang1</td>
      <td>113</td>
      <td>None</td>
      <td>none</td>
      <td>None</td>
      <td>None</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>None</td>
      <td>None</td>
      <td>Sat Oct 20 16:57:11 +0000 2018</td>
      <td>{u'symbols': [], u'user_mentions': [{u'id': 10...</td>
      <td>0</td>
      <td>False</td>
      <td>None</td>
      <td>1053691611287306241</td>
      <td>1053691611287306241</td>
      <td>None</td>
      <td>...</td>
      <td>333333</td>
      <td>True</td>
      <td>False</td>
      <td>KiranHKoli1</td>
      <td>298</td>
      <td>None</td>
      <td>none</td>
      <td>None</td>
      <td>None</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>None</td>
      <td>None</td>
      <td>Sat Oct 20 16:55:47 +0000 2018</td>
      <td>{u'symbols': [], u'user_mentions': [{u'id': 29...</td>
      <td>0</td>
      <td>False</td>
      <td>None</td>
      <td>1053691257493417984</td>
      <td>1053691257493417984</td>
      <td>None</td>
      <td>...</td>
      <td>333333</td>
      <td>True</td>
      <td>False</td>
      <td>GranCube</td>
      <td>2218</td>
      <td>None</td>
      <td>none</td>
      <td>https://t.co/xMXRzMHQOx</td>
      <td>None</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>None</td>
      <td>None</td>
      <td>Sat Oct 20 16:54:49 +0000 2018</td>
      <td>{u'symbols': [], u'user_mentions': [], u'hasht...</td>
      <td>0</td>
      <td>False</td>
      <td>None</td>
      <td>1053691017378127873</td>
      <td>1053691017378127873</td>
      <td>LnlyHero</td>
      <td>...</td>
      <td>333333</td>
      <td>True</td>
      <td>False</td>
      <td>LnlyHero</td>
      <td>2724</td>
      <td>None</td>
      <td>none</td>
      <td>None</td>
      <td>None</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 71 columns</p>
</div>



### Lets Use NLTK & Text Blob=> Tokenize, Lemmatize, Sentiment


```python
import pandas as pd
import nltk 
from textblob.utils import strip_punc
from textblob.base import BaseTokenizer
from textblob.decorators import requires_nltk_corpus

w_tokenizer = nltk.tokenize.WhitespaceTokenizer()
lemmatizer = nltk.stem.WordNetLemmatizer()
#lets tokenize and lemmatize at once
#function is a list comprehension lemmatizing each iteration for an iteration that is tokenized
def lemmatize_text(text):
    return [lemmatizer.lemmatize(w) for w in w_tokenizer.tokenize(text)]

#Creat a Column with lemmatized and tokenized tweets from the text column
twython_df['text_lemmatized'] = twython_df.text.apply(lemmatize_text)

# #Deleting original tokenized column not needed given lemmatized and tokenized at the same time above
# twython_df.drop(["tokenized_text"], axis = 1, inplace = True)

```


```python
twython_df['text_lemmatized'][4]
```




    [u'How',
     u'they',
     u'do',
     u'it?',
     u'Hazy,',
     u'they',
     u'mention',
     u'crypto',
     u'currency',
     u'and',
     u'investment,',
     u'regardless,',
     u'they',
     u'tell',
     u'those',
     u'kids,',
     u'come',
     u'off',
     u'the',
     u's\u2026',
     u'https://t.co/HTkSaRHdn6']



### Tokenize


```python
twython_df[0:2]
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
      <th>contributors</th>
      <th>coordinates</th>
      <th>created_at_x</th>
      <th>entities_x</th>
      <th>favorite_count</th>
      <th>favorited</th>
      <th>geo</th>
      <th>id_x</th>
      <th>id_str_x</th>
      <th>in_reply_to_screen_name</th>
      <th>...</th>
      <th>profile_use_background_image</th>
      <th>protected</th>
      <th>screen_name</th>
      <th>statuses_count</th>
      <th>time_zone</th>
      <th>translator_type</th>
      <th>url</th>
      <th>utc_offset</th>
      <th>verified</th>
      <th>text_lemmatized</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>None</td>
      <td>None</td>
      <td>Sat Oct 20 16:58:12 +0000 2018</td>
      <td>{u'symbols': [], u'user_mentions': [{u'id': 10...</td>
      <td>0</td>
      <td>False</td>
      <td>None</td>
      <td>1053691865868906496</td>
      <td>1053691865868906496</td>
      <td>None</td>
      <td>...</td>
      <td>True</td>
      <td>False</td>
      <td>abdullahbd985</td>
      <td>57</td>
      <td>None</td>
      <td>none</td>
      <td>None</td>
      <td>None</td>
      <td>False</td>
      <td>[RT, @alphax_official:, UNIQUE, SELLING, POINT...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>None</td>
      <td>None</td>
      <td>Sat Oct 20 16:57:42 +0000 2018</td>
      <td>{u'symbols': [], u'user_mentions': [{u'id': 95...</td>
      <td>0</td>
      <td>False</td>
      <td>None</td>
      <td>1053691739859386368</td>
      <td>1053691739859386368</td>
      <td>None</td>
      <td>...</td>
      <td>True</td>
      <td>False</td>
      <td>MinhTranHoang1</td>
      <td>113</td>
      <td>None</td>
      <td>none</td>
      <td>None</td>
      <td>None</td>
      <td>False</td>
      <td>[RT, @HandleMode:, Don't, miss, your, chance, ...</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 72 columns</p>
</div>




```python
import nltk
#nltk.download("stopwords")
from nltk.corpus import stopwords


# remove stopwords

## This isn't the proper way to add a column even though it still be called
# twython_df.stop_lemma = [word for word in twython_df.text_lemmatized if word not in stopwords.words('english')]
# recommended way:
twython_df['stop_lemma'] = [word for word in twython_df.text_lemmatized if word not in stopwords.words('english')]



#put twython_df.stop_lemma into twython_df
print(type(twython_df.stop_lemma))
twython_df.stop_lemma[0:2]
```

    <class 'pandas.core.series.Series'>





    0    [RT, @alphax_official:, UNIQUE, SELLING, POINT...
    1    [RT, @HandleMode:, Don't, miss, your, chance, ...
    Name: stop_lemma, dtype: object




```python
print(type(twython_df.stop_lemma))
twython_df.stop_lemma[0:2]
```

    <class 'pandas.core.series.Series'>





    0    [RT, @alphax_official:, UNIQUE, SELLING, POINT...
    1    [RT, @HandleMode:, Don't, miss, your, chance, ...
    Name: stop_lemma, dtype: object



### Lets remove stop words from "text_lemmatized" Column


```python
twython_df.shape
```




    (500, 73)




```python
type(twython_df.stop_lemma)
```




    pandas.core.series.Series




```python
twython_df.columns
```




    Index([                      u'contributors',
                                  u'coordinates',
                                 u'created_at_x',
                                   u'entities_x',
                               u'favorite_count',
                                    u'favorited',
                                          u'geo',
                                         u'id_x',
                                     u'id_str_x',
                      u'in_reply_to_screen_name',
                        u'in_reply_to_status_id',
                    u'in_reply_to_status_id_str',
                          u'in_reply_to_user_id',
                      u'in_reply_to_user_id_str',
                              u'is_quote_status',
                                       u'lang_x',
                                     u'metadata',
                                        u'place',
                           u'possibly_sensitive',
                                u'quoted_status',
                             u'quoted_status_id',
                         u'quoted_status_id_str',
                                u'retweet_count',
                                    u'retweeted',
                             u'retweeted_status',
                                       u'source',
                                         u'text',
                                    u'truncated',
                                         u'user',
                         u'contributors_enabled',
                                 u'created_at_y',
                              u'default_profile',
                        u'default_profile_image',
                                  u'description',
                                   u'entities_y',
                             u'favourites_count',
                          u'follow_request_sent',
                              u'followers_count',
                                    u'following',
                                u'friends_count',
                                  u'geo_enabled',
                         u'has_extended_profile',
                                         u'id_y',
                                     u'id_str_y',
                       u'is_translation_enabled',
                                u'is_translator',
                                       u'lang_y',
                                 u'listed_count',
                                     u'location',
                                         u'name',
                                u'notifications',
                     u'profile_background_color',
                 u'profile_background_image_url',
           u'profile_background_image_url_https',
                      u'profile_background_tile',
                           u'profile_banner_url',
                            u'profile_image_url',
                      u'profile_image_url_https',
                           u'profile_link_color',
                 u'profile_sidebar_border_color',
                   u'profile_sidebar_fill_color',
                           u'profile_text_color',
                 u'profile_use_background_image',
                                    u'protected',
                                  u'screen_name',
                               u'statuses_count',
                                    u'time_zone',
                              u'translator_type',
                                          u'url',
                                   u'utc_offset',
                                     u'verified',
                              u'text_lemmatized',
                                   u'stop_lemma'],
          dtype='object')




```python
twython_df["stop_lemma"].head()
```




    0    [RT, @alphax_official:, UNIQUE, SELLING, POINT...
    1    [RT, @HandleMode:, Don't, miss, your, chance, ...
    2    [RT, @alphax_official:, UNIQUE, SELLING, POINT...
    3    [RT, @Blindripper85:, Arthur, will, certainly,...
    4    [How, they, do, it?, Hazy,, they, mention, cry...
    Name: stop_lemma, dtype: object




```python
# Recall
# twython_df.stop_lemma maps to a regular Python List
# twython_df["stop_lemma"] is a regular pandas series with astype method

print("twython_df['stop_lemma'] : ", type(twython_df["stop_lemma"]), 
      " twython_df.stop_lemma: ", type(twython_df.stop_lemma))
```

    ("twython_df['stop_lemma'] : ", <class 'pandas.core.series.Series'>, ' twython_df.stop_lemma: ', <class 'pandas.core.series.Series'>)



```python
twython_df.stop_lemma[0:1]
```




    0    [RT, @alphax_official:, UNIQUE, SELLING, POINT...
    Name: stop_lemma, dtype: object




```python
twython_df["stop_lemma"][0:4]
```




    0    [RT, @alphax_official:, UNIQUE, SELLING, POINT...
    1    [RT, @HandleMode:, Don't, miss, your, chance, ...
    2    [RT, @alphax_official:, UNIQUE, SELLING, POINT...
    3    [RT, @Blindripper85:, Arthur, will, certainly,...
    Name: stop_lemma, dtype: object




```python
twython_df["stop_lemma"][0]
```




    [u'RT',
     u'@alphax_official:',
     u'UNIQUE',
     u'SELLING',
     u'POINTS',
     u'(USP)',
     u'The',
     u'coins,',
     u'distributed',
     u'through',
     u'the',
     u'main',
     u'chain',
     u'in',
     u'the',
     u'ICO,',
     u'are',
     u'Alpha-X',
     u'coins.',
     u'This',
     u'is',
     u'a',
     u'POS\u2026']




```python
# This is a list. 
test = twython_df["stop_lemma"][0]
# Create empty List
temp = []
# Recall: Encode  takes elements in a
for i in test:
#  Recall encode(ascii, ignore) is saying: if an element in the Twython_df["stop_lemma"]
# This forces the row to be in ascii and the == i tells the computer to put the entire
# former element, to remain as such given its in ascii.  Then it ignores it's not 
    if (i.encode("ascii", "ignore") == i):
        temp.append(i.encode("ascii", "ignore"))
temp
```




    ['RT',
     '@alphax_official:',
     'UNIQUE',
     'SELLING',
     'POINTS',
     '(USP)',
     'The',
     'coins,',
     'distributed',
     'through',
     'the',
     'main',
     'chain',
     'in',
     'the',
     'ICO,',
     'are',
     'Alpha-X',
     'coins.',
     'This',
     'is',
     'a']




```python
# Only necessary for Python 2

def encode_ascii(m_list):
    
    temp = []
    for i in m_list:
        if (i.encode("ascii", "ignore") == i):
            temp.append(i.encode("ascii", "ignore"))
#   Could also have done "Return Temp"          
    return temp
    
            
    
```


```python
twython_df["stop_lemma"] = twython_df["stop_lemma"].apply(encode_ascii)
twython_df["stop_lemma"][5]
```




    ['RT',
     '@CriptonBon:',
     '@KKcoinEX',
     'I',
     'think',
     'this',
     'project',
     'is',
     'very',
     'relevant',
     'at',
     'this',
     'time.',
     'The',
     'market',
     'of',
     'crypto',
     'currency',
     'is',
     'growing',
     'and',
     'it',
     'is']




```python
twython_df["stop_lemma"][0]
```




    ['RT',
     '@alphax_official:',
     'UNIQUE',
     'SELLING',
     'POINTS',
     '(USP)',
     'The',
     'coins,',
     'distributed',
     'through',
     'the',
     'main',
     'chain',
     'in',
     'the',
     'ICO,',
     'are',
     'Alpha-X',
     'coins.',
     'This',
     'is',
     'a']




```python
twython_df["stop_lemma"].head()
```




    0    [RT, @alphax_official:, UNIQUE, SELLING, POINT...
    1    [RT, @HandleMode:, Don't, miss, your, chance, ...
    2    [RT, @alphax_official:, UNIQUE, SELLING, POINT...
    3    [RT, @Blindripper85:, Arthur, will, certainly,...
    4    [How, they, do, it?, Hazy,, they, mention, cry...
    Name: stop_lemma, dtype: object




```python
# Must not run more than once
# What we did was just take out the brackets and commas by joining at space
# notice no more brackets and commas because each string in the 
# per list was joined with a space in between.


twython_df['stop_lemma'] = twython_df['stop_lemma'].apply(lambda element: ' '.join(element))

```


```python
# notice no more brackets and commas because each string in the 
# per list was joined with a space in between.

twython_df["stop_lemma"][0:5]
```




    0    RT @alphax_official: UNIQUE SELLING POINTS (US...
    1    RT @HandleMode: Don't miss your chance at the ...
    2    RT @alphax_official: UNIQUE SELLING POINTS (US...
    3    RT @Blindripper85: Arthur will certainly be a ...
    4    How they do it? Hazy, they mention crypto curr...
    Name: stop_lemma, dtype: object




```python
twython_df.stop_lemma[0]
```




    'RT @alphax_official: UNIQUE SELLING POINTS (USP) The coins, distributed through the main chain in the ICO, are Alpha-X coins. This is a'




```python
tweet = TextBlob(twython_df["stop_lemma"][0])
tweet.words
```




    WordList(['RT', 'alphax_official', 'UNIQUE', 'SELLING', 'POINTS', 'USP', 'The', 'coins', 'distributed', 'through', 'the', 'main', 'chain', 'in', 'the', 'ICO', 'are', 'Alpha-X', 'coins', 'This', 'is', 'a'])




```python
from textblob import TextBlob, Word


tweet = TextBlob(twython_df.stop_lemma[0])
tweet.words
```




    WordList(['RT', 'alphax_official', 'UNIQUE', 'SELLING', 'POINTS', 'USP', 'The', 'coins', 'distributed', 'through', 'the', 'main', 'chain', 'in', 'the', 'ICO', 'are', 'Alpha-X', 'coins', 'This', 'is', 'a'])



### Lets Apply a function to apply this to the DataFrame
- Compliments Jose 


```python
def detect_sentiment(text):
    return TextBlob(str(text)).sentiment.polarity
# Create a new DataFrame column for sentiment (Warning:Slow!)

twython_df["Sentiment"] = twython_df.stop_lemma.apply(detect_sentiment)
twython_df[["stop_lemma", "Sentiment"]][1:20]
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
      <th>stop_lemma</th>
      <th>Sentiment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>RT @HandleMode: Don't miss your chance at the ...</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>RT @alphax_official: UNIQUE SELLING POINTS (US...</td>
      <td>0.270833</td>
    </tr>
    <tr>
      <th>3</th>
      <td>RT @Blindripper85: Arthur will certainly be a ...</td>
      <td>0.607143</td>
    </tr>
    <tr>
      <th>4</th>
      <td>How they do it? Hazy, they mention crypto curr...</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>RT @CriptonBon: @KKcoinEX I think this project...</td>
      <td>0.520000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>RT @CarpeNoctom: If the #MegaMillions jackpot ...</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Did you know that SPS token solely present uti...</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>RT @alphax_official: UNIQUE SELLING POINTS (US...</td>
      <td>0.270833</td>
    </tr>
    <tr>
      <th>9</th>
      <td>RT @ElementsEstates: Did you know that the Ele...</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>10</th>
      <td>RT @alphax_official: UNIQUE SELLING POINTS (US...</td>
      <td>0.270833</td>
    </tr>
    <tr>
      <th>11</th>
      <td>GOLD and CRYPTO currency Get PAID! Visit: http...</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>12</th>
      <td>New post (Revised Russian Draft Bill on Digita...</td>
      <td>0.049716</td>
    </tr>
    <tr>
      <th>13</th>
      <td>RT @coinbundlecom: People tend to have a fear ...</td>
      <td>-0.300000</td>
    </tr>
    <tr>
      <th>14</th>
      <td>RT @HandleMode: Don't miss your chance at the ...</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>15</th>
      <td>RT @CriptonBon: @KKcoinEX I think this project...</td>
      <td>0.520000</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Arthur will certainly be a Professor at the be...</td>
      <td>0.607143</td>
    </tr>
    <tr>
      <th>17</th>
      <td>RT @CriptonBon: @KKcoinEX I think this project...</td>
      <td>0.520000</td>
    </tr>
    <tr>
      <th>18</th>
      <td>RT @RotaCaccraEast: Our joint meeting with @Ro...</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>19</th>
      <td>RT @coinspectator: Revised Russian Draft Bill ...</td>
      <td>0.020833</td>
    </tr>
  </tbody>
</table>
</div>



### Lets See the Average of the Tweets


```python
twython_df[["stop_lemma", "Sentiment"]][1:100].mean()
```




    Sentiment    0.196379
    dtype: float64



### Nothing Follows. Below is the scratch.  Did not get to experiement with Tweepy


```python
x = TextBlob(df.text)
```


```python
x = "Zia is very helpful in teaching NLP."
y = TextBlob(x)

y.words
y.sentences
```


```python
y.sentiment
```


```python
y.sentiment.polarity
```


```python
x = "Zia is very helpful in teaching NLP.  Jose is very very helpful too.  DevMasters is great"
#tokenize
# take out stop words . 
# Lemma
# td if
# polarity

```


```python
# x-train and x_test

# vect = CountVectorizer()

#print vect

#print X_train
#you just tokenized that whole DF so it's goingt to be workable now
# With CountVectorizer each sentence is a row 
# x_trainCV = vect.fit_transform(x_train)
# x_trainCV.shape
```

### Now we will go over Tweepy Streaming Approach. (7 days) 


```python
import pandas as pd
import tweepy
from tweepy.streaming import StreamListener
from tweepy import OAuthHandler
from tweepy import Stream
import time
```


```python
class savetweepyTweets(StreamListner):
    
    def on_data(self, data):
        try:
            print(type(data))
            print("New Tweet")
            savefile = open("twitter.txt", "a")
            savefile = open('twitter.txt','a')
            savefile.write(data)
            savefile.write('\n')
            savefile.close()
            
            return True
        except BaseException as e:
            print ("Failed ", str(e))
            self.disconnect()
            
    def on_error(self, status):
        print (status)
        stream.disconnect() 
        
tl = savetweepyTweets()
au = OAuthHandler("3A49r1HERmMwKiUHgfzxiPz7i","KWRwyv23ekwDqspO8Io0HHHCUAv8AqQ4On8eowqOjkRmwox8ut")
au.set_access_token("2316217729-ez4xksWsu2vfCkmTDZnlN3Nf5GCMAVjXvVBBrYq","mN6ozZmsYFFcJjokcZQggfHNxOhUUwqiWXgrS4MwGa1q9")
stream = Stream(au,tl)
```


```python
stream.filter(track=["Crypto Currency","Cardano"], languages=['en'])
```


```python
stream.disconnect()
```


```python
import json
tweets_data = []
json_data = open('twitter.txt','r')
#print type(json_data)
    for line in json_data:
     #print type(line)
         try:
             twe = json.loads(line)
             #print type(twe)
             tweets_data.append(twe)
        except:
             continue
```


```python
len(tweets_data)
```


```python
tweet_df = pd.DataFrame(tweets_data)
tweet_df
```


```python
pd.set_option('display.max_colwidth', 0)
tweet_df[["created_at","text"]]
```
