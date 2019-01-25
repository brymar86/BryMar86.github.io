---
layout: post
title: Twitter Post
date: 2017-11-27 04:00:00
tags: Machine Learning, API, Twitter/Twython
author: Bryan Marty
---


{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Requirement already satisfied: tweepy in /anaconda2/lib/python2.7/site-packages (3.6.0)\n",
      "Requirement already satisfied: requests>=2.11.1 in /anaconda2/lib/python2.7/site-packages (from tweepy) (2.18.4)\n",
      "Requirement already satisfied: PySocks>=1.5.7 in /anaconda2/lib/python2.7/site-packages (from tweepy) (1.6.8)\n",
      "Requirement already satisfied: six>=1.10.0 in /anaconda2/lib/python2.7/site-packages (from tweepy) (1.11.0)\n",
      "Requirement already satisfied: requests-oauthlib>=0.7.0 in /anaconda2/lib/python2.7/site-packages (from tweepy) (1.0.0)\n",
      "Requirement already satisfied: chardet<3.1.0,>=3.0.2 in /anaconda2/lib/python2.7/site-packages (from requests>=2.11.1->tweepy) (3.0.4)\n",
      "Requirement already satisfied: idna<2.7,>=2.5 in /anaconda2/lib/python2.7/site-packages (from requests>=2.11.1->tweepy) (2.6)\n",
      "Requirement already satisfied: urllib3<1.23,>=1.21.1 in /anaconda2/lib/python2.7/site-packages (from requests>=2.11.1->tweepy) (1.22)\n",
      "Requirement already satisfied: certifi>=2017.4.17 in /anaconda2/lib/python2.7/site-packages (from requests>=2.11.1->tweepy) (2018.4.16)\n",
      "Requirement already satisfied: oauthlib>=0.6.2 in /anaconda2/lib/python2.7/site-packages (from requests-oauthlib>=0.7.0->tweepy) (2.1.0)\n",
      "\u001b[31mdistributed 1.21.8 requires msgpack, which is not installed.\u001b[0m\n",
      "\u001b[31mgrin 1.2.1 requires argparse>=1.1, which is not installed.\u001b[0m\n",
      "\u001b[33mYou are using pip version 10.0.1, however version 18.1 is available.\n",
      "You should consider upgrading via the 'pip install --upgrade pip' command.\u001b[0m\n",
      "Requirement already satisfied: twython in /anaconda2/lib/python2.7/site-packages (3.7.0)\n",
      "Requirement already satisfied: requests-oauthlib>=0.4.0 in /anaconda2/lib/python2.7/site-packages (from twython) (1.0.0)\n",
      "Requirement already satisfied: requests>=2.1.0 in /anaconda2/lib/python2.7/site-packages (from twython) (2.18.4)\n",
      "Requirement already satisfied: oauthlib>=0.6.2 in /anaconda2/lib/python2.7/site-packages (from requests-oauthlib>=0.4.0->twython) (2.1.0)\n",
      "Requirement already satisfied: chardet<3.1.0,>=3.0.2 in /anaconda2/lib/python2.7/site-packages (from requests>=2.1.0->twython) (3.0.4)\n",
      "Requirement already satisfied: idna<2.7,>=2.5 in /anaconda2/lib/python2.7/site-packages (from requests>=2.1.0->twython) (2.6)\n",
      "Requirement already satisfied: urllib3<1.23,>=1.21.1 in /anaconda2/lib/python2.7/site-packages (from requests>=2.1.0->twython) (1.22)\n",
      "Requirement already satisfied: certifi>=2017.4.17 in /anaconda2/lib/python2.7/site-packages (from requests>=2.1.0->twython) (2018.4.16)\n",
      "\u001b[31mdistributed 1.21.8 requires msgpack, which is not installed.\u001b[0m\n",
      "\u001b[31mgrin 1.2.1 requires argparse>=1.1, which is not installed.\u001b[0m\n",
      "\u001b[33mYou are using pip version 10.0.1, however version 18.1 is available.\n",
      "You should consider upgrading via the 'pip install --upgrade pip' command.\u001b[0m\n"
     ]
    }
   ],
   "source": [
    "!pip install tweepy\n",
    "!pip install twython"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [],
   "source": [
    "from twython import Twython"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Twitter has 3 different types of API's\n",
    "- Search Option:  Basically all historic Tweets.  Means \"not happening right now\" . Limits to 100 tweets historical per search and 2,500 per day.\n",
    "- Streaming Access:  Real Time Tweets= filter given 500,000 tweets on avg per min you will get 40,000/min.  \n",
    "- Filter/ Fire hose would be 100,000/min (still within streaming but different)\n",
    "- Paid version.  You get everything you'd want but we will not use that. "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Collecting Twython\n",
      "  Using cached https://files.pythonhosted.org/packages/8c/2b/c0883f05b03a8e87787d56395d6c89515fe7e0bf80abd3778b6bb3a6873f/twython-3.7.0.tar.gz\n",
      "Collecting requests>=2.1.0 (from Twython)\n",
      "\u001b[?25l  Downloading https://files.pythonhosted.org/packages/7d/e3/20f3d364d6c8e5d2353c72a67778eb189176f08e873c9900e10c0287b84b/requests-2.21.0-py2.py3-none-any.whl (57kB)\n",
      "\u001b[K    100% |████████████████████████████████| 61kB 1.3MB/s \n",
      "\u001b[?25hCollecting requests_oauthlib>=0.4.0 (from Twython)\n",
      "  Downloading https://files.pythonhosted.org/packages/c2/e2/9fd03d55ffb70fe51f587f20bcf407a6927eb121de86928b34d162f0b1ac/requests_oauthlib-1.2.0-py2.py3-none-any.whl\n",
      "Collecting idna<2.9,>=2.5 (from requests>=2.1.0->Twython)\n",
      "\u001b[?25l  Downloading https://files.pythonhosted.org/packages/14/2c/cd551d81dbe15200be1cf41cd03869a46fe7226e7450af7a6545bfc474c9/idna-2.8-py2.py3-none-any.whl (58kB)\n",
      "\u001b[K    100% |████████████████████████████████| 61kB 1.5MB/s \n",
      "\u001b[?25hCollecting urllib3<1.25,>=1.21.1 (from requests>=2.1.0->Twython)\n",
      "\u001b[?25l  Downloading https://files.pythonhosted.org/packages/62/00/ee1d7de624db8ba7090d1226aebefab96a2c71cd5cfa7629d6ad3f61b79e/urllib3-1.24.1-py2.py3-none-any.whl (118kB)\n",
      "\u001b[K    100% |████████████████████████████████| 122kB 8.9MB/s \n",
      "\u001b[?25hCollecting chardet<3.1.0,>=3.0.2 (from requests>=2.1.0->Twython)\n",
      "\u001b[?25l  Downloading https://files.pythonhosted.org/packages/bc/a9/01ffebfb562e4274b6487b4bb1ddec7ca55ec7510b22e4c51f14098443b8/chardet-3.0.4-py2.py3-none-any.whl (133kB)\n",
      "\u001b[K    100% |████████████████████████████████| 143kB 5.6MB/s \n",
      "\u001b[?25hRequirement already satisfied: certifi>=2017.4.17 in /anaconda2/envs/py35/lib/python3.6/site-packages (from requests>=2.1.0->Twython) (2018.11.29)\n",
      "Collecting oauthlib>=3.0.0 (from requests_oauthlib>=0.4.0->Twython)\n",
      "\u001b[?25l  Downloading https://files.pythonhosted.org/packages/16/95/699466b05b72b94a41f662dc9edf87fda4289e3602ecd42d27fcaddf7b56/oauthlib-3.0.1-py2.py3-none-any.whl (142kB)\n",
      "\u001b[K    100% |████████████████████████████████| 143kB 8.7MB/s \n",
      "\u001b[?25hBuilding wheels for collected packages: Twython\n",
      "  Running setup.py bdist_wheel for Twython ... \u001b[?25ldone\n",
      "\u001b[?25h  Stored in directory: /Users/bryanmarty/Library/Caches/pip/wheels/c2/b0/a3/5c4b4b87b8c9e4d99f1494a0b471f0134a74e5fb33d426d009\n",
      "Successfully built Twython\n",
      "Installing collected packages: idna, urllib3, chardet, requests, oauthlib, requests-oauthlib, Twython\n",
      "Successfully installed Twython-3.7.0 chardet-3.0.4 idna-2.8 oauthlib-3.0.1 requests-2.21.0 requests-oauthlib-1.2.0 urllib3-1.24.1\n"
     ]
    }
   ],
   "source": [
    "!pip install Twython"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "  source equals the actual tweet.  When pulling from Twitter's API it will pull a JSON file.  \n",
    "  Twython then takes \"results\" and makes it a dicitonary\n",
    "  \n",
    "  Results has Meta Data and Statusers\n",
    "  all = results[\"statusers\"]\n",
    "  tweet_df = pd.DataFrame(all)\n",
    "  \n",
    "  Now with the dataframe we find the \"text\" column and work with it.  \n",
    "  Last column is the users.  user column in itself is a dictionary.  If you would like to explore that you are able to do so. \n",
    "  using list comprehendion.  we essentially make a DF of the users from the original statuser DF. \n",
    "  \n",
    "  "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [],
   "source": [
    "import twitter_api as access"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [],
   "source": [
    "from twython import Twython\n",
    "import twitter_api as access\n",
    "\n",
    "twitter = Twython(access.CONSUMER_KEY,access.CONSUMER_SECRET)\n",
    "\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Help on function search in module twython.endpoints:\n",
      "\n",
      "search(self, **params)\n",
      "    Returns a collection of relevant Tweets matching a specified query.\n",
      "    \n",
      "    Docs:\n",
      "    https://developer.twitter.com/en/docs/tweets/search/api-reference/get-search-tweets\n",
      "\n"
     ]
    }
   ],
   "source": [
    "help(Twython.search)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "dict"
      ]
     },
     "execution_count": 9,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "results1 = twitter.search(q = \"Crypto Currency\", geocode = (37.0902, 95.7129),count = 100, language = \"Eng\", since_id = 1000)\n",
    "results2 = twitter.search(q = \"Crypto Currency\", geocode = (37.0902, 95.7129), count = 100, language = \"Eng\", since_id = 800)\n",
    "results3 = twitter.search(q = \"Crypto Currency\", geocode = (37.0902, 95.7129), count = 100, language = \"Eng\", since_id = 600)\n",
    "results4 = twitter.search(q = \"Crypto Currency\", geocode = (37.0902, 95.7129), count = 100, language = \"Eng\", since_id = 400)\n",
    "results5 = twitter.search(q = \"Crypto Currency\", geocode = (37.0902, 95.7129), count = 100, language = \"Eng\", since_id = 200)\n",
    "\n",
    "\n",
    "\n",
    "type(results1)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Lets put these results into variables\n",
    "- Statuses and Metadata are returned\n",
    "- We only want Statuses in the Variables"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "list"
      ]
     },
     "execution_count": 10,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "all1 = results1[\"statuses\"]\n",
    "all2 = results2[\"statuses\"]\n",
    "all3 = results3[\"statuses\"]\n",
    "all4 = results4[\"statuses\"]\n",
    "all5 = results5[\"statuses\"]\n",
    "type(all1)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "[{'created_at': 'Fri Jan 25 05:24:28 +0000 2019',\n",
       "  'id': 1088668907051708416,\n",
       "  'id_str': '1088668907051708416',\n",
       "  'text': 'RT @ICEDataServices: Our enhanced crypto data feed now offers greater transparency: streaming real-time data for 400+ fiat and crypto curre…',\n",
       "  'truncated': False,\n",
       "  'entities': {'hashtags': [],\n",
       "   'symbols': [],\n",
       "   'user_mentions': [{'screen_name': 'ICEDataServices',\n",
       "     'name': 'ICE Data Services',\n",
       "     'id': 146460368,\n",
       "     'id_str': '146460368',\n",
       "     'indices': [3, 19]}],\n",
       "   'urls': []},\n",
       "  'metadata': {'iso_language_code': 'en', 'result_type': 'recent'},\n",
       "  'source': '<a href=\"http://twitter.com/download/iphone\" rel=\"nofollow\">Twitter for iPhone</a>',\n",
       "  'in_reply_to_status_id': None,\n",
       "  'in_reply_to_status_id_str': None,\n",
       "  'in_reply_to_user_id': None,\n",
       "  'in_reply_to_user_id_str': None,\n",
       "  'in_reply_to_screen_name': None,\n",
       "  'user': {'id': 880572548047663104,\n",
       "   'id_str': '880572548047663104',\n",
       "   'name': 'RK',\n",
       "   'screen_name': 'imarezzaq',\n",
       "   'location': 'Howli, India',\n",
       "   'description': 'Only I can change my life. No one can do it for me.🤔',\n",
       "   'url': None,\n",
       "   'entities': {'description': {'urls': []}},\n",
       "   'protected': False,\n",
       "   'followers_count': 143,\n",
       "   'friends_count': 193,\n",
       "   'listed_count': 0,\n",
       "   'created_at': 'Thu Jun 29 23:43:50 +0000 2017',\n",
       "   'favourites_count': 4302,\n",
       "   'utc_offset': None,\n",
       "   'time_zone': None,\n",
       "   'geo_enabled': True,\n",
       "   'verified': False,\n",
       "   'statuses_count': 1273,\n",
       "   'lang': 'en',\n",
       "   'contributors_enabled': False,\n",
       "   'is_translator': False,\n",
       "   'is_translation_enabled': False,\n",
       "   'profile_background_color': 'F5F8FA',\n",
       "   'profile_background_image_url': None,\n",
       "   'profile_background_image_url_https': None,\n",
       "   'profile_background_tile': False,\n",
       "   'profile_image_url': 'http://pbs.twimg.com/profile_images/1014458176748453888/XrzEhvQf_normal.jpg',\n",
       "   'profile_image_url_https': 'https://pbs.twimg.com/profile_images/1014458176748453888/XrzEhvQf_normal.jpg',\n",
       "   'profile_link_color': '1DA1F2',\n",
       "   'profile_sidebar_border_color': 'C0DEED',\n",
       "   'profile_sidebar_fill_color': 'DDEEF6',\n",
       "   'profile_text_color': '333333',\n",
       "   'profile_use_background_image': True,\n",
       "   'has_extended_profile': True,\n",
       "   'default_profile': True,\n",
       "   'default_profile_image': False,\n",
       "   'following': None,\n",
       "   'follow_request_sent': None,\n",
       "   'notifications': None,\n",
       "   'translator_type': 'none'},\n",
       "  'geo': None,\n",
       "  'coordinates': None,\n",
       "  'place': None,\n",
       "  'contributors': None,\n",
       "  'retweeted_status': {'created_at': 'Thu Jan 24 16:38:32 +0000 2019',\n",
       "   'id': 1088476155127259136,\n",
       "   'id_str': '1088476155127259136',\n",
       "   'text': 'Our enhanced crypto data feed now offers greater transparency: streaming real-time data for 400+ fiat and crypto cu… https://t.co/qK32OYiW9D',\n",
       "   'truncated': True,\n",
       "   'entities': {'hashtags': [],\n",
       "    'symbols': [],\n",
       "    'user_mentions': [],\n",
       "    'urls': [{'url': 'https://t.co/qK32OYiW9D',\n",
       "      'expanded_url': 'https://twitter.com/i/web/status/1088476155127259136',\n",
       "      'display_url': 'twitter.com/i/web/status/1…',\n",
       "      'indices': [117, 140]}]},\n",
       "   'metadata': {'iso_language_code': 'en', 'result_type': 'recent'},\n",
       "   'source': '<a href=\"http://twitter.com\" rel=\"nofollow\">Twitter Web Client</a>',\n",
       "   'in_reply_to_status_id': None,\n",
       "   'in_reply_to_status_id_str': None,\n",
       "   'in_reply_to_user_id': None,\n",
       "   'in_reply_to_user_id_str': None,\n",
       "   'in_reply_to_screen_name': None,\n",
       "   'user': {'id': 146460368,\n",
       "    'id_str': '146460368',\n",
       "    'name': 'ICE Data Services',\n",
       "    'screen_name': 'ICEDataServices',\n",
       "    'location': '',\n",
       "    'description': 'We provide vital market information and connectivity for participants around the world.',\n",
       "    'url': 'https://t.co/BT6Do7Xe9h',\n",
       "    'entities': {'url': {'urls': [{'url': 'https://t.co/BT6Do7Xe9h',\n",
       "        'expanded_url': 'https://www.theice.com/market-data',\n",
       "        'display_url': 'theice.com/market-data',\n",
       "        'indices': [0, 23]}]},\n",
       "     'description': {'urls': []}},\n",
       "    'protected': False,\n",
       "    'followers_count': 6001,\n",
       "    'friends_count': 173,\n",
       "    'listed_count': 140,\n",
       "    'created_at': 'Fri May 21 13:54:49 +0000 2010',\n",
       "    'favourites_count': 134,\n",
       "    'utc_offset': None,\n",
       "    'time_zone': None,\n",
       "    'geo_enabled': False,\n",
       "    'verified': True,\n",
       "    'statuses_count': 1099,\n",
       "    'lang': 'en',\n",
       "    'contributors_enabled': False,\n",
       "    'is_translator': False,\n",
       "    'is_translation_enabled': False,\n",
       "    'profile_background_color': 'C0DEED',\n",
       "    'profile_background_image_url': 'http://abs.twimg.com/images/themes/theme1/bg.png',\n",
       "    'profile_background_image_url_https': 'https://abs.twimg.com/images/themes/theme1/bg.png',\n",
       "    'profile_background_tile': True,\n",
       "    'profile_image_url': 'http://pbs.twimg.com/profile_images/974710179055878145/Jv6R2NFc_normal.jpg',\n",
       "    'profile_image_url_https': 'https://pbs.twimg.com/profile_images/974710179055878145/Jv6R2NFc_normal.jpg',\n",
       "    'profile_banner_url': 'https://pbs.twimg.com/profile_banners/146460368/1524594537',\n",
       "    'profile_link_color': '72C7E7',\n",
       "    'profile_sidebar_border_color': 'C0DEED',\n",
       "    'profile_sidebar_fill_color': 'DDEEF6',\n",
       "    'profile_text_color': '333333',\n",
       "    'profile_use_background_image': True,\n",
       "    'has_extended_profile': False,\n",
       "    'default_profile': False,\n",
       "    'default_profile_image': False,\n",
       "    'following': None,\n",
       "    'follow_request_sent': None,\n",
       "    'notifications': None,\n",
       "    'translator_type': 'none'},\n",
       "   'geo': None,\n",
       "   'coordinates': None,\n",
       "   'place': None,\n",
       "   'contributors': None,\n",
       "   'is_quote_status': False,\n",
       "   'retweet_count': 139,\n",
       "   'favorite_count': 369,\n",
       "   'favorited': False,\n",
       "   'retweeted': False,\n",
       "   'possibly_sensitive': False,\n",
       "   'lang': 'en'},\n",
       "  'is_quote_status': False,\n",
       "  'retweet_count': 139,\n",
       "  'favorite_count': 0,\n",
       "  'favorited': False,\n",
       "  'retweeted': False,\n",
       "  'lang': 'en'}]"
      ]
     },
     "execution_count": 11,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#RUN Heat map on Feature correlations between retweets & location/text/polarity?\n",
    "results1[\"statuses\"][0:1]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Building A Tweets DataFrame"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(100, 30)\n",
      "(100, 30)\n",
      "(100, 30)\n",
      "(100, 30)\n",
      "(100, 30)\n"
     ]
    }
   ],
   "source": [
    "import pandas as pd\n",
    "import numpy as np\n",
    "tweet1 = pd.DataFrame(all1)\n",
    "tweet2 = pd.DataFrame(all2)\n",
    "tweet3 = pd.DataFrame(all3)\n",
    "tweet4 = pd.DataFrame(all4)\n",
    "tweet5 = pd.DataFrame(all5)\n",
    "\n",
    "print(tweet1.shape)\n",
    "print(tweet2.shape)\n",
    "print(tweet3.shape)\n",
    "print(tweet4.shape)\n",
    "print(tweet5.shape)\n",
    "# taking results[\"statuses\"] into a DF\n",
    "# tweet_df = pd.DataFrame(all)\n",
    "# tweet_df[0:3]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Stuff all DF's into a variable & concat the variable"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(500, 30)"
      ]
     },
     "execution_count": 13,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_t = [tweet1, tweet2, tweet3, tweet4, tweet5 ]\n",
    "tweet_df = pd.concat(df_t, ignore_index = True)\n",
    "tweet_df.shape"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "500"
      ]
     },
     "execution_count": 14,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "100*5"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### YOU HAVE TO REMEMBER:\n",
    "- When Concating Dataframes with index rows where the row numbers hold no meaning:\n",
    "- ignore_index = True"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "RangeIndex: 500 entries, 0 to 499\n",
      "Data columns (total 30 columns):\n",
      "contributors                 0 non-null object\n",
      "coordinates                  0 non-null object\n",
      "created_at                   500 non-null object\n",
      "entities                     500 non-null object\n",
      "extended_entities            10 non-null object\n",
      "favorite_count               500 non-null int64\n",
      "favorited                    500 non-null bool\n",
      "geo                          0 non-null object\n",
      "id                           500 non-null int64\n",
      "id_str                       500 non-null object\n",
      "in_reply_to_screen_name      50 non-null object\n",
      "in_reply_to_status_id        50 non-null float64\n",
      "in_reply_to_status_id_str    50 non-null object\n",
      "in_reply_to_user_id          50 non-null float64\n",
      "in_reply_to_user_id_str      50 non-null object\n",
      "is_quote_status              500 non-null bool\n",
      "lang                         500 non-null object\n",
      "metadata                     500 non-null object\n",
      "place                        5 non-null object\n",
      "possibly_sensitive           125 non-null object\n",
      "quoted_status                5 non-null object\n",
      "quoted_status_id             25 non-null float64\n",
      "quoted_status_id_str         25 non-null object\n",
      "retweet_count                500 non-null int64\n",
      "retweeted                    500 non-null bool\n",
      "retweeted_status             360 non-null object\n",
      "source                       500 non-null object\n",
      "text                         500 non-null object\n",
      "truncated                    500 non-null bool\n",
      "user                         500 non-null object\n",
      "dtypes: bool(4), float64(3), int64(3), object(20)\n",
      "memory usage: 103.6+ KB\n"
     ]
    }
   ],
   "source": [
    "# Lets see time, text and 'user'\n",
    "tweet_df.info()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Lets Observe \n",
    "- Will view \"text\", \"user\", \"retweet_count\", \"retweeted\""
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>text</th>\n",
       "      <th>user</th>\n",
       "      <th>retweet_count</th>\n",
       "      <th>retweeted</th>\n",
       "      <th>geo</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>RT @ICEDataServices: Our enhanced crypto data ...</td>\n",
       "      <td>{'id': 880572548047663104, 'id_str': '88057254...</td>\n",
       "      <td>139</td>\n",
       "      <td>False</td>\n",
       "      <td>None</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>Paving way for Crypto currency ? https://t.co/...</td>\n",
       "      <td>{'id': 127342462, 'id_str': '127342462', 'name...</td>\n",
       "      <td>0</td>\n",
       "      <td>False</td>\n",
       "      <td>None</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>RT @ICEDataServices: Our enhanced crypto data ...</td>\n",
       "      <td>{'id': 2432947903, 'id_str': '2432947903', 'na...</td>\n",
       "      <td>139</td>\n",
       "      <td>False</td>\n",
       "      <td>None</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>What makes Cryptocurrency different from Fiat ...</td>\n",
       "      <td>{'id': 886568441947111424, 'id_str': '88656844...</td>\n",
       "      <td>0</td>\n",
       "      <td>False</td>\n",
       "      <td>None</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>In 2013, #dogecoin , a cryptocurrency based on...</td>\n",
       "      <td>{'id': 1025266965818953728, 'id_str': '1025266...</td>\n",
       "      <td>0</td>\n",
       "      <td>False</td>\n",
       "      <td>None</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>@xkatherine23 You Need Crypto Currency!!\\n\\nLe...</td>\n",
       "      <td>{'id': 485286887, 'id_str': '485286887', 'name...</td>\n",
       "      <td>0</td>\n",
       "      <td>False</td>\n",
       "      <td>None</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>#Nano #Coin #Compared To Next Coin - Crypto \\n...</td>\n",
       "      <td>{'id': 871976018998956034, 'id_str': '87197601...</td>\n",
       "      <td>0</td>\n",
       "      <td>False</td>\n",
       "      <td>None</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>RT @aantonop: Analyst's guide to crypto-curren...</td>\n",
       "      <td>{'id': 964202652404875264, 'id_str': '96420265...</td>\n",
       "      <td>374</td>\n",
       "      <td>False</td>\n",
       "      <td>None</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>RT @ApolloCurrency: Great article from Forbes!...</td>\n",
       "      <td>{'id': 26723847, 'id_str': '26723847', 'name':...</td>\n",
       "      <td>54</td>\n",
       "      <td>False</td>\n",
       "      <td>None</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9</th>\n",
       "      <td>RT @ICEDataServices: Our enhanced crypto data ...</td>\n",
       "      <td>{'id': 929548801022373888, 'id_str': '92954880...</td>\n",
       "      <td>139</td>\n",
       "      <td>False</td>\n",
       "      <td>None</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                                                text  \\\n",
       "0  RT @ICEDataServices: Our enhanced crypto data ...   \n",
       "1  Paving way for Crypto currency ? https://t.co/...   \n",
       "2  RT @ICEDataServices: Our enhanced crypto data ...   \n",
       "3  What makes Cryptocurrency different from Fiat ...   \n",
       "4  In 2013, #dogecoin , a cryptocurrency based on...   \n",
       "5  @xkatherine23 You Need Crypto Currency!!\\n\\nLe...   \n",
       "6  #Nano #Coin #Compared To Next Coin - Crypto \\n...   \n",
       "7  RT @aantonop: Analyst's guide to crypto-curren...   \n",
       "8  RT @ApolloCurrency: Great article from Forbes!...   \n",
       "9  RT @ICEDataServices: Our enhanced crypto data ...   \n",
       "\n",
       "                                                user  retweet_count  \\\n",
       "0  {'id': 880572548047663104, 'id_str': '88057254...            139   \n",
       "1  {'id': 127342462, 'id_str': '127342462', 'name...              0   \n",
       "2  {'id': 2432947903, 'id_str': '2432947903', 'na...            139   \n",
       "3  {'id': 886568441947111424, 'id_str': '88656844...              0   \n",
       "4  {'id': 1025266965818953728, 'id_str': '1025266...              0   \n",
       "5  {'id': 485286887, 'id_str': '485286887', 'name...              0   \n",
       "6  {'id': 871976018998956034, 'id_str': '87197601...              0   \n",
       "7  {'id': 964202652404875264, 'id_str': '96420265...            374   \n",
       "8  {'id': 26723847, 'id_str': '26723847', 'name':...             54   \n",
       "9  {'id': 929548801022373888, 'id_str': '92954880...            139   \n",
       "\n",
       "   retweeted   geo  \n",
       "0      False  None  \n",
       "1      False  None  \n",
       "2      False  None  \n",
       "3      False  None  \n",
       "4      False  None  \n",
       "5      False  None  \n",
       "6      False  None  \n",
       "7      False  None  \n",
       "8      False  None  \n",
       "9      False  None  "
      ]
     },
     "execution_count": 16,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# To improve:  Retweet_count filter such that retweet_count >= 400 for significance of information\n",
    "tweet_df.loc[0:9, [\"text\", \"user\", \"retweet_count\", \"retweeted\", \"geo\"]]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Idea\n",
    "- Once polarity found lets create a column of the polarity\n",
    "- Then lets see a HeatMap Correlation between these features"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "list"
      ]
     },
     "execution_count": 17,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "type([d[\"user\"] for d in results1[\"statuses\"]][0:1])"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "{'id': 880572548047663104,\n",
       " 'id_str': '880572548047663104',\n",
       " 'name': 'RK',\n",
       " 'screen_name': 'imarezzaq',\n",
       " 'location': 'Howli, India',\n",
       " 'description': 'Only I can change my life. No one can do it for me.🤔',\n",
       " 'url': None,\n",
       " 'entities': {'description': {'urls': []}},\n",
       " 'protected': False,\n",
       " 'followers_count': 143,\n",
       " 'friends_count': 193,\n",
       " 'listed_count': 0,\n",
       " 'created_at': 'Thu Jun 29 23:43:50 +0000 2017',\n",
       " 'favourites_count': 4302,\n",
       " 'utc_offset': None,\n",
       " 'time_zone': None,\n",
       " 'geo_enabled': True,\n",
       " 'verified': False,\n",
       " 'statuses_count': 1273,\n",
       " 'lang': 'en',\n",
       " 'contributors_enabled': False,\n",
       " 'is_translator': False,\n",
       " 'is_translation_enabled': False,\n",
       " 'profile_background_color': 'F5F8FA',\n",
       " 'profile_background_image_url': None,\n",
       " 'profile_background_image_url_https': None,\n",
       " 'profile_background_tile': False,\n",
       " 'profile_image_url': 'http://pbs.twimg.com/profile_images/1014458176748453888/XrzEhvQf_normal.jpg',\n",
       " 'profile_image_url_https': 'https://pbs.twimg.com/profile_images/1014458176748453888/XrzEhvQf_normal.jpg',\n",
       " 'profile_link_color': '1DA1F2',\n",
       " 'profile_sidebar_border_color': 'C0DEED',\n",
       " 'profile_sidebar_fill_color': 'DDEEF6',\n",
       " 'profile_text_color': '333333',\n",
       " 'profile_use_background_image': True,\n",
       " 'has_extended_profile': True,\n",
       " 'default_profile': True,\n",
       " 'default_profile_image': False,\n",
       " 'following': None,\n",
       " 'follow_request_sent': None,\n",
       " 'notifications': None,\n",
       " 'translator_type': 'none'}"
      ]
     },
     "execution_count": 18,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Thie is a single element in the Series \"User\"\n",
    "[d[\"user\"] for d in results1[\"statuses\"]][0]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Combining the \"user\" dictionary to DF Steps\n",
    "- create 5 data frames containing the users data\n",
    "- stick all 5 DF into a list and assign to variable\n",
    "- Concat the 5 DF into one big DF remembering to insert ignore_index = True\n",
    "- Merge Big \"user\" DF to the right of the tweet_df"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {},
   "outputs": [],
   "source": [
    "# The series \"user\" elements can be turned into a DF because the elements\n",
    "# are dictionaries and are also Huge.\n",
    "user1 = pd.DataFrame([d[\"user\"] for d in results1[\"statuses\"]])\n",
    "user2 = pd.DataFrame([d[\"user\"] for d in results2[\"statuses\"]])\n",
    "user3 = pd.DataFrame([d[\"user\"] for d in results3[\"statuses\"]])\n",
    "user4 = pd.DataFrame([d[\"user\"] for d in results4[\"statuses\"]])\n",
    "user5 = pd.DataFrame([d[\"user\"] for d in results5[\"statuses\"]])\n",
    "\n",
    "df_u = [user1, user2, user3, user4, user5]\n",
    "\n",
    "user_df = pd.concat(df_u, ignore_index = True)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Before we merge lets take a look at the \"user\" specific data"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>contributors_enabled</th>\n",
       "      <th>created_at</th>\n",
       "      <th>default_profile</th>\n",
       "      <th>default_profile_image</th>\n",
       "      <th>description</th>\n",
       "      <th>entities</th>\n",
       "      <th>favourites_count</th>\n",
       "      <th>follow_request_sent</th>\n",
       "      <th>followers_count</th>\n",
       "      <th>following</th>\n",
       "      <th>...</th>\n",
       "      <th>profile_text_color</th>\n",
       "      <th>profile_use_background_image</th>\n",
       "      <th>protected</th>\n",
       "      <th>screen_name</th>\n",
       "      <th>statuses_count</th>\n",
       "      <th>time_zone</th>\n",
       "      <th>translator_type</th>\n",
       "      <th>url</th>\n",
       "      <th>utc_offset</th>\n",
       "      <th>verified</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>False</td>\n",
       "      <td>Thu Jun 29 23:43:50 +0000 2017</td>\n",
       "      <td>True</td>\n",
       "      <td>False</td>\n",
       "      <td>Only I can change my life. No one can do it fo...</td>\n",
       "      <td>{'description': {'urls': []}}</td>\n",
       "      <td>4302</td>\n",
       "      <td>None</td>\n",
       "      <td>143</td>\n",
       "      <td>None</td>\n",
       "      <td>...</td>\n",
       "      <td>333333</td>\n",
       "      <td>True</td>\n",
       "      <td>False</td>\n",
       "      <td>imarezzaq</td>\n",
       "      <td>1273</td>\n",
       "      <td>None</td>\n",
       "      <td>none</td>\n",
       "      <td>None</td>\n",
       "      <td>None</td>\n",
       "      <td>False</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>1 rows × 42 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "   contributors_enabled                      created_at  default_profile  \\\n",
       "0                 False  Thu Jun 29 23:43:50 +0000 2017             True   \n",
       "\n",
       "   default_profile_image                                        description  \\\n",
       "0                  False  Only I can change my life. No one can do it fo...   \n",
       "\n",
       "                        entities  favourites_count follow_request_sent  \\\n",
       "0  {'description': {'urls': []}}              4302                None   \n",
       "\n",
       "   followers_count following   ...     profile_text_color  \\\n",
       "0              143      None   ...                 333333   \n",
       "\n",
       "   profile_use_background_image  protected  screen_name statuses_count  \\\n",
       "0                          True      False    imarezzaq           1273   \n",
       "\n",
       "   time_zone  translator_type   url  utc_offset verified  \n",
       "0       None             none  None        None    False  \n",
       "\n",
       "[1 rows x 42 columns]"
      ]
     },
     "execution_count": 20,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "user_df[0:1]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>created_at</th>\n",
       "      <th>time_zone</th>\n",
       "      <th>screen_name</th>\n",
       "      <th>statuses_count</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>Thu Jun 29 23:43:50 +0000 2017</td>\n",
       "      <td>None</td>\n",
       "      <td>imarezzaq</td>\n",
       "      <td>1273</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>Sun Mar 28 22:15:56 +0000 2010</td>\n",
       "      <td>None</td>\n",
       "      <td>SubodhSrivastva</td>\n",
       "      <td>48814</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>Tue Apr 08 02:58:05 +0000 2014</td>\n",
       "      <td>None</td>\n",
       "      <td>HMNorazizi</td>\n",
       "      <td>174</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>Sun Jul 16 12:49:22 +0000 2017</td>\n",
       "      <td>None</td>\n",
       "      <td>altcoinalerts</td>\n",
       "      <td>1545</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>Fri Aug 03 06:27:48 +0000 2018</td>\n",
       "      <td>None</td>\n",
       "      <td>LtdPrincipal</td>\n",
       "      <td>186</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>Tue Feb 07 01:39:34 +0000 2012</td>\n",
       "      <td>None</td>\n",
       "      <td>FranswaHarold</td>\n",
       "      <td>104</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>Tue Jun 06 06:24:18 +0000 2017</td>\n",
       "      <td>None</td>\n",
       "      <td>anjaliverma2usa</td>\n",
       "      <td>2349</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>Thu Feb 15 18:20:01 +0000 2018</td>\n",
       "      <td>None</td>\n",
       "      <td>cryptoslawyer</td>\n",
       "      <td>4429</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>Thu Mar 26 10:48:42 +0000 2009</td>\n",
       "      <td>None</td>\n",
       "      <td>barra43</td>\n",
       "      <td>1455</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9</th>\n",
       "      <td>Sun Nov 12 03:17:59 +0000 2017</td>\n",
       "      <td>None</td>\n",
       "      <td>ThomasDemichele</td>\n",
       "      <td>1540</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>10</th>\n",
       "      <td>Wed Oct 17 04:47:43 +0000 2018</td>\n",
       "      <td>None</td>\n",
       "      <td>CeriolaJomar</td>\n",
       "      <td>93</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                        created_at time_zone      screen_name  statuses_count\n",
       "0   Thu Jun 29 23:43:50 +0000 2017      None        imarezzaq            1273\n",
       "1   Sun Mar 28 22:15:56 +0000 2010      None  SubodhSrivastva           48814\n",
       "2   Tue Apr 08 02:58:05 +0000 2014      None       HMNorazizi             174\n",
       "3   Sun Jul 16 12:49:22 +0000 2017      None    altcoinalerts            1545\n",
       "4   Fri Aug 03 06:27:48 +0000 2018      None     LtdPrincipal             186\n",
       "5   Tue Feb 07 01:39:34 +0000 2012      None    FranswaHarold             104\n",
       "6   Tue Jun 06 06:24:18 +0000 2017      None  anjaliverma2usa            2349\n",
       "7   Thu Feb 15 18:20:01 +0000 2018      None    cryptoslawyer            4429\n",
       "8   Thu Mar 26 10:48:42 +0000 2009      None          barra43            1455\n",
       "9   Sun Nov 12 03:17:59 +0000 2017      None  ThomasDemichele            1540\n",
       "10  Wed Oct 17 04:47:43 +0000 2018      None     CeriolaJomar              93"
      ]
     },
     "execution_count": 21,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#what is statuses_count\n",
    "user_df.loc[0:10, [\"created_at\", \"time_zone\", \"screen_name\", \"statuses_count\"]]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "RangeIndex: 500 entries, 0 to 499\n",
      "Data columns (total 42 columns):\n",
      "contributors_enabled                  500 non-null bool\n",
      "created_at                            500 non-null object\n",
      "default_profile                       500 non-null bool\n",
      "default_profile_image                 500 non-null bool\n",
      "description                           500 non-null object\n",
      "entities                              500 non-null object\n",
      "favourites_count                      500 non-null int64\n",
      "follow_request_sent                   0 non-null object\n",
      "followers_count                       500 non-null int64\n",
      "following                             0 non-null object\n",
      "friends_count                         500 non-null int64\n",
      "geo_enabled                           500 non-null bool\n",
      "has_extended_profile                  500 non-null bool\n",
      "id                                    500 non-null int64\n",
      "id_str                                500 non-null object\n",
      "is_translation_enabled                500 non-null bool\n",
      "is_translator                         500 non-null bool\n",
      "lang                                  500 non-null object\n",
      "listed_count                          500 non-null int64\n",
      "location                              500 non-null object\n",
      "name                                  500 non-null object\n",
      "notifications                         0 non-null object\n",
      "profile_background_color              500 non-null object\n",
      "profile_background_image_url          280 non-null object\n",
      "profile_background_image_url_https    280 non-null object\n",
      "profile_background_tile               500 non-null bool\n",
      "profile_banner_url                    310 non-null object\n",
      "profile_image_url                     500 non-null object\n",
      "profile_image_url_https               500 non-null object\n",
      "profile_link_color                    500 non-null object\n",
      "profile_sidebar_border_color          500 non-null object\n",
      "profile_sidebar_fill_color            500 non-null object\n",
      "profile_text_color                    500 non-null object\n",
      "profile_use_background_image          500 non-null bool\n",
      "protected                             500 non-null bool\n",
      "screen_name                           500 non-null object\n",
      "statuses_count                        500 non-null int64\n",
      "time_zone                             0 non-null object\n",
      "translator_type                       500 non-null object\n",
      "url                                   150 non-null object\n",
      "utc_offset                            0 non-null object\n",
      "verified                              500 non-null bool\n",
      "dtypes: bool(11), int64(6), object(25)\n",
      "memory usage: 126.5+ KB\n"
     ]
    }
   ],
   "source": [
    "user_df.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(500, 30)\n",
      "(500, 42)\n"
     ]
    }
   ],
   "source": [
    "print(tweet_df.shape)\n",
    "print(user_df.shape)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(500, 72)"
      ]
     },
     "execution_count": 24,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# This puts it on the right hand side. \n",
    "twython_df = tweet_df.merge(user_df, left_index = True, right_index = True)\n",
    "twython_df.shape"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Merging on the index widens the DataFrame"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0"
      ]
     },
     "execution_count": 25,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df[\"location\"].isnull().sum()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "                                              170\n",
       "California, USA                                20\n",
       "United States                                  20\n",
       "Michigan, USA                                  15\n",
       "Dallas, Texas                                  10\n",
       "0x1734eD2675cdcEe41848a2Cf3d6022FF0abeE64A      5\n",
       "Spokane, WA                                     5\n",
       "Singapore                                       5\n",
       "Virginia, USA                                   5\n",
       "Indonesia                                       5\n",
       "London, Ontario                                 5\n",
       "Mexico                                          5\n",
       "puthia, Rajshahi, Bangladesh                    5\n",
       "대한민국                                            5\n",
       "Hong Kong                                       5\n",
       "Alpha Centauri                                  5\n",
       "Hell                                            5\n",
       "Nashville, TN                                   5\n",
       "The Las Vegas Strip, Paradise                   5\n",
       "Sacramento, CA                                  5\n",
       "New York, NY                                    5\n",
       "Howli, India                                    5\n",
       "Pennsylvania, USA                               5\n",
       "San Jose, CA                                    5\n",
       "Blockchain                                      5\n",
       "Bengaluru, India                                5\n",
       "Grapevine, TX                                   5\n",
       "waya st.                                        5\n",
       "Toronto                                         5\n",
       "Kirti Nagar, New Delhi                          5\n",
       "MENTION FOR RETWEET                             5\n",
       "North Carolina, USA                             5\n",
       "London, England                                 5\n",
       "Cape Town, South Africa                         5\n",
       "NSA Capital of the World                        5\n",
       "Chicago                                         5\n",
       "Nairobi, Kenya                                  5\n",
       "Fair Lawn , NJ                                  5\n",
       "Moon 🚀                                          5\n",
       "Chicago, IL                                     5\n",
       "Brisbane, Australia                             5\n",
       "world                                           5\n",
       "United Kingdom                                  5\n",
       "Layton, UT                                      5\n",
       "Mexico City                                     5\n",
       "Edison, NJ                                      5\n",
       "Омск, Россия                                    5\n",
       "Bumine Pengeran                                 5\n",
       "Amsterdam, The Netherlands                      5\n",
       "Global                                          5\n",
       "Europe                                          5\n",
       "dhubri, Assam, India                            5\n",
       "Ybor City, FL                                   5\n",
       "Worldwide                                       5\n",
       "Malaysia                                        5\n",
       "Miami, FL                                       5\n",
       "Cần Thơ, Vietnam                                5\n",
       "Kampala, Uganda                                 5\n",
       "Name: location, dtype: int64"
      ]
     },
     "execution_count": 26,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df[\"location\"].value_counts()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 27,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "58"
      ]
     },
     "execution_count": 27,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df[\"location\"].nunique()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Lets Graph"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 28,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.axes._subplots.AxesSubplot at 0x113259630>"
      ]
     },
     "execution_count": 28,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 720x720 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "import matplotlib.pyplot as plt\n",
    "%matplotlib inline\n",
    "\n",
    "twython_df.location.value_counts().plot(kind = \"bar\", figsize = (10,10))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 29,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "5"
      ]
     },
     "execution_count": 29,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df[twython_df.location == \"The Las Vegas Strip, Paradise\"].entities_x.count()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 27,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0"
      ]
     },
     "execution_count": 27,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#Lets get all the US Crypto Tweets into the graph\n",
    "twython_df[twython_df.location == \"Las Vegas, NV\"].entities_x.count()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 28,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0"
      ]
     },
     "execution_count": 28,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df[twython_df.location == \"FL\"].entities_x.count()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Lets find USA Total Twitter Responses \n",
    "- Very annoying as this changes everytime i run the code"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 29,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0"
      ]
     },
     "execution_count": 29,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#Add up all the unique cities where \"Crypto Currency\" was tweeted about\n",
    "twython_df[(twython_df.location == \"San Jose, CA\" )| (twython_df.location == \"Columbus, OH\") | (twython_df[twython_df.location == \"The Las Vegas Strip, Paradise\"].entities_x.count()) | (twython_df.location == \"Austin, TX\") |\n",
    "          (twython_df.location == \"Richmond, VA\") | (twython_df.location == \"Chicago, IL\") | (twython_df.location == \"Maryland, USA\") | (twython_df.location ==\"Santa Rosa, CA\") |\n",
    "          (twython_df.location == \"Oxnard, CA\") | (twython_df.location == \"New York NY\") ].entities_x.count()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Total \"I live in the hub\" responses"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 30,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0"
      ]
     },
     "execution_count": 30,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df[twython_df.location == \"I live in the HUB\"].entities_x.count()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Idea 2 is figuring out how to put sum of US tweets as an element in \"Location Colunm\" so I can graph it\n",
    "- From the above we do know the US had the most Tweets about Crypto Currency "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>text</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>RT @alphax_official: UNIQUE SELLING POINTS (US...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>RT @HandleMode: Don't miss your chance at the ...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>RT @alphax_official: UNIQUE SELLING POINTS (US...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>RT @Blindripper85: Arthur will certainly be a ...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>How they do it? Hazy, they mention crypto curr...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>RT @CriptonBon: @KKcoinEX I think this project...</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                                                text\n",
       "0  RT @alphax_official: UNIQUE SELLING POINTS (US...\n",
       "1  RT @HandleMode: Don't miss your chance at the ...\n",
       "2  RT @alphax_official: UNIQUE SELLING POINTS (US...\n",
       "3  RT @Blindripper85: Arthur will certainly be a ...\n",
       "4  How they do it? Hazy, they mention crypto curr...\n",
       "5  RT @CriptonBon: @KKcoinEX I think this project..."
      ]
     },
     "execution_count": 31,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df.loc[0:5, [\"text\"]]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 32,
   "metadata": {},
   "outputs": [
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "/anaconda2/lib/python2.7/site-packages/sklearn/cross_validation.py:41: DeprecationWarning: This module was deprecated in version 0.18 in favor of the model_selection module into which all the refactored classes and functions are moved. Also note that the interface of the new CV iterators are different from that of this module. This module will be removed in 0.20.\n",
      "  \"This module will be removed in 0.20.\", DeprecationWarning)\n"
     ]
    }
   ],
   "source": [
    "#i \n",
    "\n",
    "import pandas as pd\n",
    "import numpy as np\n",
    "import scipy as sp \n",
    "from sklearn.cross_validation import train_test_split\n",
    "from sklearn.feature_extraction.text import CountVectorizer, TfidfVectorizer\n",
    "\n",
    "from sklearn.naive_bayes import MultinomialNB\n",
    "from sklearn.linear_model import LogisticRegression\n",
    "from sklearn import metrics\n",
    "from textblob import TextBlob, Word\n",
    "from nltk.stem.snowball import SnowballStemmer\n",
    "%matplotlib inline\n",
    "\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 33,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>contributors</th>\n",
       "      <th>coordinates</th>\n",
       "      <th>created_at_x</th>\n",
       "      <th>entities_x</th>\n",
       "      <th>favorite_count</th>\n",
       "      <th>favorited</th>\n",
       "      <th>geo</th>\n",
       "      <th>id_x</th>\n",
       "      <th>id_str_x</th>\n",
       "      <th>in_reply_to_screen_name</th>\n",
       "      <th>...</th>\n",
       "      <th>profile_text_color</th>\n",
       "      <th>profile_use_background_image</th>\n",
       "      <th>protected</th>\n",
       "      <th>screen_name</th>\n",
       "      <th>statuses_count</th>\n",
       "      <th>time_zone</th>\n",
       "      <th>translator_type</th>\n",
       "      <th>url</th>\n",
       "      <th>utc_offset</th>\n",
       "      <th>verified</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>None</td>\n",
       "      <td>None</td>\n",
       "      <td>Sat Oct 20 16:58:12 +0000 2018</td>\n",
       "      <td>{u'symbols': [], u'user_mentions': [{u'id': 10...</td>\n",
       "      <td>0</td>\n",
       "      <td>False</td>\n",
       "      <td>None</td>\n",
       "      <td>1053691865868906496</td>\n",
       "      <td>1053691865868906496</td>\n",
       "      <td>None</td>\n",
       "      <td>...</td>\n",
       "      <td>333333</td>\n",
       "      <td>True</td>\n",
       "      <td>False</td>\n",
       "      <td>abdullahbd985</td>\n",
       "      <td>57</td>\n",
       "      <td>None</td>\n",
       "      <td>none</td>\n",
       "      <td>None</td>\n",
       "      <td>None</td>\n",
       "      <td>False</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>None</td>\n",
       "      <td>None</td>\n",
       "      <td>Sat Oct 20 16:57:42 +0000 2018</td>\n",
       "      <td>{u'symbols': [], u'user_mentions': [{u'id': 95...</td>\n",
       "      <td>0</td>\n",
       "      <td>False</td>\n",
       "      <td>None</td>\n",
       "      <td>1053691739859386368</td>\n",
       "      <td>1053691739859386368</td>\n",
       "      <td>None</td>\n",
       "      <td>...</td>\n",
       "      <td>333333</td>\n",
       "      <td>True</td>\n",
       "      <td>False</td>\n",
       "      <td>MinhTranHoang1</td>\n",
       "      <td>113</td>\n",
       "      <td>None</td>\n",
       "      <td>none</td>\n",
       "      <td>None</td>\n",
       "      <td>None</td>\n",
       "      <td>False</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>None</td>\n",
       "      <td>None</td>\n",
       "      <td>Sat Oct 20 16:57:11 +0000 2018</td>\n",
       "      <td>{u'symbols': [], u'user_mentions': [{u'id': 10...</td>\n",
       "      <td>0</td>\n",
       "      <td>False</td>\n",
       "      <td>None</td>\n",
       "      <td>1053691611287306241</td>\n",
       "      <td>1053691611287306241</td>\n",
       "      <td>None</td>\n",
       "      <td>...</td>\n",
       "      <td>333333</td>\n",
       "      <td>True</td>\n",
       "      <td>False</td>\n",
       "      <td>KiranHKoli1</td>\n",
       "      <td>298</td>\n",
       "      <td>None</td>\n",
       "      <td>none</td>\n",
       "      <td>None</td>\n",
       "      <td>None</td>\n",
       "      <td>False</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>None</td>\n",
       "      <td>None</td>\n",
       "      <td>Sat Oct 20 16:55:47 +0000 2018</td>\n",
       "      <td>{u'symbols': [], u'user_mentions': [{u'id': 29...</td>\n",
       "      <td>0</td>\n",
       "      <td>False</td>\n",
       "      <td>None</td>\n",
       "      <td>1053691257493417984</td>\n",
       "      <td>1053691257493417984</td>\n",
       "      <td>None</td>\n",
       "      <td>...</td>\n",
       "      <td>333333</td>\n",
       "      <td>True</td>\n",
       "      <td>False</td>\n",
       "      <td>GranCube</td>\n",
       "      <td>2218</td>\n",
       "      <td>None</td>\n",
       "      <td>none</td>\n",
       "      <td>https://t.co/xMXRzMHQOx</td>\n",
       "      <td>None</td>\n",
       "      <td>False</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>None</td>\n",
       "      <td>None</td>\n",
       "      <td>Sat Oct 20 16:54:49 +0000 2018</td>\n",
       "      <td>{u'symbols': [], u'user_mentions': [], u'hasht...</td>\n",
       "      <td>0</td>\n",
       "      <td>False</td>\n",
       "      <td>None</td>\n",
       "      <td>1053691017378127873</td>\n",
       "      <td>1053691017378127873</td>\n",
       "      <td>LnlyHero</td>\n",
       "      <td>...</td>\n",
       "      <td>333333</td>\n",
       "      <td>True</td>\n",
       "      <td>False</td>\n",
       "      <td>LnlyHero</td>\n",
       "      <td>2724</td>\n",
       "      <td>None</td>\n",
       "      <td>none</td>\n",
       "      <td>None</td>\n",
       "      <td>None</td>\n",
       "      <td>False</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>5 rows × 71 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "  contributors coordinates                    created_at_x  \\\n",
       "0         None        None  Sat Oct 20 16:58:12 +0000 2018   \n",
       "1         None        None  Sat Oct 20 16:57:42 +0000 2018   \n",
       "2         None        None  Sat Oct 20 16:57:11 +0000 2018   \n",
       "3         None        None  Sat Oct 20 16:55:47 +0000 2018   \n",
       "4         None        None  Sat Oct 20 16:54:49 +0000 2018   \n",
       "\n",
       "                                          entities_x  favorite_count  \\\n",
       "0  {u'symbols': [], u'user_mentions': [{u'id': 10...               0   \n",
       "1  {u'symbols': [], u'user_mentions': [{u'id': 95...               0   \n",
       "2  {u'symbols': [], u'user_mentions': [{u'id': 10...               0   \n",
       "3  {u'symbols': [], u'user_mentions': [{u'id': 29...               0   \n",
       "4  {u'symbols': [], u'user_mentions': [], u'hasht...               0   \n",
       "\n",
       "   favorited   geo                 id_x             id_str_x  \\\n",
       "0      False  None  1053691865868906496  1053691865868906496   \n",
       "1      False  None  1053691739859386368  1053691739859386368   \n",
       "2      False  None  1053691611287306241  1053691611287306241   \n",
       "3      False  None  1053691257493417984  1053691257493417984   \n",
       "4      False  None  1053691017378127873  1053691017378127873   \n",
       "\n",
       "  in_reply_to_screen_name   ...     profile_text_color  \\\n",
       "0                    None   ...                 333333   \n",
       "1                    None   ...                 333333   \n",
       "2                    None   ...                 333333   \n",
       "3                    None   ...                 333333   \n",
       "4                LnlyHero   ...                 333333   \n",
       "\n",
       "  profile_use_background_image  protected     screen_name  statuses_count  \\\n",
       "0                         True      False   abdullahbd985              57   \n",
       "1                         True      False  MinhTranHoang1             113   \n",
       "2                         True      False     KiranHKoli1             298   \n",
       "3                         True      False        GranCube            2218   \n",
       "4                         True      False        LnlyHero            2724   \n",
       "\n",
       "  time_zone translator_type                      url utc_offset verified  \n",
       "0      None            none                     None       None    False  \n",
       "1      None            none                     None       None    False  \n",
       "2      None            none                     None       None    False  \n",
       "3      None            none  https://t.co/xMXRzMHQOx       None    False  \n",
       "4      None            none                     None       None    False  \n",
       "\n",
       "[5 rows x 71 columns]"
      ]
     },
     "execution_count": 33,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df.head()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Lets Use NLTK & Text Blob=> Tokenize, Lemmatize, Sentiment"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 34,
   "metadata": {},
   "outputs": [],
   "source": [
    "import pandas as pd\n",
    "import nltk \n",
    "from textblob.utils import strip_punc\n",
    "from textblob.base import BaseTokenizer\n",
    "from textblob.decorators import requires_nltk_corpus\n",
    "\n",
    "w_tokenizer = nltk.tokenize.WhitespaceTokenizer()\n",
    "lemmatizer = nltk.stem.WordNetLemmatizer()\n",
    "#lets tokenize and lemmatize at once\n",
    "#function is a list comprehension lemmatizing each iteration for an iteration that is tokenized\n",
    "def lemmatize_text(text):\n",
    "    return [lemmatizer.lemmatize(w) for w in w_tokenizer.tokenize(text)]\n",
    "\n",
    "#Creat a Column with lemmatized and tokenized tweets from the text column\n",
    "twython_df['text_lemmatized'] = twython_df.text.apply(lemmatize_text)\n",
    "\n",
    "# #Deleting original tokenized column not needed given lemmatized and tokenized at the same time above\n",
    "# twython_df.drop([\"tokenized_text\"], axis = 1, inplace = True)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 35,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "[u'How',\n",
       " u'they',\n",
       " u'do',\n",
       " u'it?',\n",
       " u'Hazy,',\n",
       " u'they',\n",
       " u'mention',\n",
       " u'crypto',\n",
       " u'currency',\n",
       " u'and',\n",
       " u'investment,',\n",
       " u'regardless,',\n",
       " u'they',\n",
       " u'tell',\n",
       " u'those',\n",
       " u'kids,',\n",
       " u'come',\n",
       " u'off',\n",
       " u'the',\n",
       " u's\\u2026',\n",
       " u'https://t.co/HTkSaRHdn6']"
      ]
     },
     "execution_count": 35,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df['text_lemmatized'][4]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Tokenize"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 36,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>contributors</th>\n",
       "      <th>coordinates</th>\n",
       "      <th>created_at_x</th>\n",
       "      <th>entities_x</th>\n",
       "      <th>favorite_count</th>\n",
       "      <th>favorited</th>\n",
       "      <th>geo</th>\n",
       "      <th>id_x</th>\n",
       "      <th>id_str_x</th>\n",
       "      <th>in_reply_to_screen_name</th>\n",
       "      <th>...</th>\n",
       "      <th>profile_use_background_image</th>\n",
       "      <th>protected</th>\n",
       "      <th>screen_name</th>\n",
       "      <th>statuses_count</th>\n",
       "      <th>time_zone</th>\n",
       "      <th>translator_type</th>\n",
       "      <th>url</th>\n",
       "      <th>utc_offset</th>\n",
       "      <th>verified</th>\n",
       "      <th>text_lemmatized</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>None</td>\n",
       "      <td>None</td>\n",
       "      <td>Sat Oct 20 16:58:12 +0000 2018</td>\n",
       "      <td>{u'symbols': [], u'user_mentions': [{u'id': 10...</td>\n",
       "      <td>0</td>\n",
       "      <td>False</td>\n",
       "      <td>None</td>\n",
       "      <td>1053691865868906496</td>\n",
       "      <td>1053691865868906496</td>\n",
       "      <td>None</td>\n",
       "      <td>...</td>\n",
       "      <td>True</td>\n",
       "      <td>False</td>\n",
       "      <td>abdullahbd985</td>\n",
       "      <td>57</td>\n",
       "      <td>None</td>\n",
       "      <td>none</td>\n",
       "      <td>None</td>\n",
       "      <td>None</td>\n",
       "      <td>False</td>\n",
       "      <td>[RT, @alphax_official:, UNIQUE, SELLING, POINT...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>None</td>\n",
       "      <td>None</td>\n",
       "      <td>Sat Oct 20 16:57:42 +0000 2018</td>\n",
       "      <td>{u'symbols': [], u'user_mentions': [{u'id': 95...</td>\n",
       "      <td>0</td>\n",
       "      <td>False</td>\n",
       "      <td>None</td>\n",
       "      <td>1053691739859386368</td>\n",
       "      <td>1053691739859386368</td>\n",
       "      <td>None</td>\n",
       "      <td>...</td>\n",
       "      <td>True</td>\n",
       "      <td>False</td>\n",
       "      <td>MinhTranHoang1</td>\n",
       "      <td>113</td>\n",
       "      <td>None</td>\n",
       "      <td>none</td>\n",
       "      <td>None</td>\n",
       "      <td>None</td>\n",
       "      <td>False</td>\n",
       "      <td>[RT, @HandleMode:, Don't, miss, your, chance, ...</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>2 rows × 72 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "  contributors coordinates                    created_at_x  \\\n",
       "0         None        None  Sat Oct 20 16:58:12 +0000 2018   \n",
       "1         None        None  Sat Oct 20 16:57:42 +0000 2018   \n",
       "\n",
       "                                          entities_x  favorite_count  \\\n",
       "0  {u'symbols': [], u'user_mentions': [{u'id': 10...               0   \n",
       "1  {u'symbols': [], u'user_mentions': [{u'id': 95...               0   \n",
       "\n",
       "   favorited   geo                 id_x             id_str_x  \\\n",
       "0      False  None  1053691865868906496  1053691865868906496   \n",
       "1      False  None  1053691739859386368  1053691739859386368   \n",
       "\n",
       "  in_reply_to_screen_name                        ...                          \\\n",
       "0                    None                        ...                           \n",
       "1                    None                        ...                           \n",
       "\n",
       "   profile_use_background_image protected     screen_name statuses_count  \\\n",
       "0                          True     False   abdullahbd985             57   \n",
       "1                          True     False  MinhTranHoang1            113   \n",
       "\n",
       "   time_zone translator_type   url utc_offset verified  \\\n",
       "0       None            none  None       None    False   \n",
       "1       None            none  None       None    False   \n",
       "\n",
       "                                     text_lemmatized  \n",
       "0  [RT, @alphax_official:, UNIQUE, SELLING, POINT...  \n",
       "1  [RT, @HandleMode:, Don't, miss, your, chance, ...  \n",
       "\n",
       "[2 rows x 72 columns]"
      ]
     },
     "execution_count": 36,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df[0:2]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 37,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.series.Series'>\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "0    [RT, @alphax_official:, UNIQUE, SELLING, POINT...\n",
       "1    [RT, @HandleMode:, Don't, miss, your, chance, ...\n",
       "Name: stop_lemma, dtype: object"
      ]
     },
     "execution_count": 37,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "import nltk\n",
    "#nltk.download(\"stopwords\")\n",
    "from nltk.corpus import stopwords\n",
    "\n",
    "\n",
    "# remove stopwords\n",
    "\n",
    "## This isn't the proper way to add a column even though it still be called\n",
    "# twython_df.stop_lemma = [word for word in twython_df.text_lemmatized if word not in stopwords.words('english')]\n",
    "# recommended way:\n",
    "twython_df['stop_lemma'] = [word for word in twython_df.text_lemmatized if word not in stopwords.words('english')]\n",
    "\n",
    "\n",
    "\n",
    "#put twython_df.stop_lemma into twython_df\n",
    "print(type(twython_df.stop_lemma))\n",
    "twython_df.stop_lemma[0:2]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 38,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.series.Series'>\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "0    [RT, @alphax_official:, UNIQUE, SELLING, POINT...\n",
       "1    [RT, @HandleMode:, Don't, miss, your, chance, ...\n",
       "Name: stop_lemma, dtype: object"
      ]
     },
     "execution_count": 38,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "print(type(twython_df.stop_lemma))\n",
    "twython_df.stop_lemma[0:2]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Lets remove stop words from \"text_lemmatized\" Column"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 39,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(500, 73)"
      ]
     },
     "execution_count": 39,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df.shape"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 40,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "pandas.core.series.Series"
      ]
     },
     "execution_count": 40,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "type(twython_df.stop_lemma)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 41,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Index([                      u'contributors',\n",
       "                              u'coordinates',\n",
       "                             u'created_at_x',\n",
       "                               u'entities_x',\n",
       "                           u'favorite_count',\n",
       "                                u'favorited',\n",
       "                                      u'geo',\n",
       "                                     u'id_x',\n",
       "                                 u'id_str_x',\n",
       "                  u'in_reply_to_screen_name',\n",
       "                    u'in_reply_to_status_id',\n",
       "                u'in_reply_to_status_id_str',\n",
       "                      u'in_reply_to_user_id',\n",
       "                  u'in_reply_to_user_id_str',\n",
       "                          u'is_quote_status',\n",
       "                                   u'lang_x',\n",
       "                                 u'metadata',\n",
       "                                    u'place',\n",
       "                       u'possibly_sensitive',\n",
       "                            u'quoted_status',\n",
       "                         u'quoted_status_id',\n",
       "                     u'quoted_status_id_str',\n",
       "                            u'retweet_count',\n",
       "                                u'retweeted',\n",
       "                         u'retweeted_status',\n",
       "                                   u'source',\n",
       "                                     u'text',\n",
       "                                u'truncated',\n",
       "                                     u'user',\n",
       "                     u'contributors_enabled',\n",
       "                             u'created_at_y',\n",
       "                          u'default_profile',\n",
       "                    u'default_profile_image',\n",
       "                              u'description',\n",
       "                               u'entities_y',\n",
       "                         u'favourites_count',\n",
       "                      u'follow_request_sent',\n",
       "                          u'followers_count',\n",
       "                                u'following',\n",
       "                            u'friends_count',\n",
       "                              u'geo_enabled',\n",
       "                     u'has_extended_profile',\n",
       "                                     u'id_y',\n",
       "                                 u'id_str_y',\n",
       "                   u'is_translation_enabled',\n",
       "                            u'is_translator',\n",
       "                                   u'lang_y',\n",
       "                             u'listed_count',\n",
       "                                 u'location',\n",
       "                                     u'name',\n",
       "                            u'notifications',\n",
       "                 u'profile_background_color',\n",
       "             u'profile_background_image_url',\n",
       "       u'profile_background_image_url_https',\n",
       "                  u'profile_background_tile',\n",
       "                       u'profile_banner_url',\n",
       "                        u'profile_image_url',\n",
       "                  u'profile_image_url_https',\n",
       "                       u'profile_link_color',\n",
       "             u'profile_sidebar_border_color',\n",
       "               u'profile_sidebar_fill_color',\n",
       "                       u'profile_text_color',\n",
       "             u'profile_use_background_image',\n",
       "                                u'protected',\n",
       "                              u'screen_name',\n",
       "                           u'statuses_count',\n",
       "                                u'time_zone',\n",
       "                          u'translator_type',\n",
       "                                      u'url',\n",
       "                               u'utc_offset',\n",
       "                                 u'verified',\n",
       "                          u'text_lemmatized',\n",
       "                               u'stop_lemma'],\n",
       "      dtype='object')"
      ]
     },
     "execution_count": 41,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df.columns"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 42,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0    [RT, @alphax_official:, UNIQUE, SELLING, POINT...\n",
       "1    [RT, @HandleMode:, Don't, miss, your, chance, ...\n",
       "2    [RT, @alphax_official:, UNIQUE, SELLING, POINT...\n",
       "3    [RT, @Blindripper85:, Arthur, will, certainly,...\n",
       "4    [How, they, do, it?, Hazy,, they, mention, cry...\n",
       "Name: stop_lemma, dtype: object"
      ]
     },
     "execution_count": 42,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df[\"stop_lemma\"].head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 43,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(\"twython_df['stop_lemma'] : \", <class 'pandas.core.series.Series'>, ' twython_df.stop_lemma: ', <class 'pandas.core.series.Series'>)\n"
     ]
    }
   ],
   "source": [
    "# Recall\n",
    "# twython_df.stop_lemma maps to a regular Python List\n",
    "# twython_df[\"stop_lemma\"] is a regular pandas series with astype method\n",
    "\n",
    "print(\"twython_df['stop_lemma'] : \", type(twython_df[\"stop_lemma\"]), \n",
    "      \" twython_df.stop_lemma: \", type(twython_df.stop_lemma))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 44,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0    [RT, @alphax_official:, UNIQUE, SELLING, POINT...\n",
       "Name: stop_lemma, dtype: object"
      ]
     },
     "execution_count": 44,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df.stop_lemma[0:1]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 45,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0    [RT, @alphax_official:, UNIQUE, SELLING, POINT...\n",
       "1    [RT, @HandleMode:, Don't, miss, your, chance, ...\n",
       "2    [RT, @alphax_official:, UNIQUE, SELLING, POINT...\n",
       "3    [RT, @Blindripper85:, Arthur, will, certainly,...\n",
       "Name: stop_lemma, dtype: object"
      ]
     },
     "execution_count": 45,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df[\"stop_lemma\"][0:4]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 46,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "[u'RT',\n",
       " u'@alphax_official:',\n",
       " u'UNIQUE',\n",
       " u'SELLING',\n",
       " u'POINTS',\n",
       " u'(USP)',\n",
       " u'The',\n",
       " u'coins,',\n",
       " u'distributed',\n",
       " u'through',\n",
       " u'the',\n",
       " u'main',\n",
       " u'chain',\n",
       " u'in',\n",
       " u'the',\n",
       " u'ICO,',\n",
       " u'are',\n",
       " u'Alpha-X',\n",
       " u'coins.',\n",
       " u'This',\n",
       " u'is',\n",
       " u'a',\n",
       " u'POS\\u2026']"
      ]
     },
     "execution_count": 46,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df[\"stop_lemma\"][0]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 47,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "['RT',\n",
       " '@alphax_official:',\n",
       " 'UNIQUE',\n",
       " 'SELLING',\n",
       " 'POINTS',\n",
       " '(USP)',\n",
       " 'The',\n",
       " 'coins,',\n",
       " 'distributed',\n",
       " 'through',\n",
       " 'the',\n",
       " 'main',\n",
       " 'chain',\n",
       " 'in',\n",
       " 'the',\n",
       " 'ICO,',\n",
       " 'are',\n",
       " 'Alpha-X',\n",
       " 'coins.',\n",
       " 'This',\n",
       " 'is',\n",
       " 'a']"
      ]
     },
     "execution_count": 47,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# This is a list. \n",
    "test = twython_df[\"stop_lemma\"][0]\n",
    "# Create empty List\n",
    "temp = []\n",
    "# Recall: Encode  takes elements in a\n",
    "for i in test:\n",
    "#  Recall encode(ascii, ignore) is saying: if an element in the Twython_df[\"stop_lemma\"]\n",
    "# This forces the row to be in ascii and the == i tells the computer to put the entire\n",
    "# former element, to remain as such given its in ascii.  Then it ignores it's not \n",
    "    if (i.encode(\"ascii\", \"ignore\") == i):\n",
    "        temp.append(i.encode(\"ascii\", \"ignore\"))\n",
    "temp"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 48,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Only necessary for Python 2\n",
    "\n",
    "def encode_ascii(m_list):\n",
    "    \n",
    "    temp = []\n",
    "    for i in m_list:\n",
    "        if (i.encode(\"ascii\", \"ignore\") == i):\n",
    "            temp.append(i.encode(\"ascii\", \"ignore\"))\n",
    "#   Could also have done \"Return Temp\"          \n",
    "    return temp\n",
    "    \n",
    "            \n",
    "    "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 49,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "['RT',\n",
       " '@CriptonBon:',\n",
       " '@KKcoinEX',\n",
       " 'I',\n",
       " 'think',\n",
       " 'this',\n",
       " 'project',\n",
       " 'is',\n",
       " 'very',\n",
       " 'relevant',\n",
       " 'at',\n",
       " 'this',\n",
       " 'time.',\n",
       " 'The',\n",
       " 'market',\n",
       " 'of',\n",
       " 'crypto',\n",
       " 'currency',\n",
       " 'is',\n",
       " 'growing',\n",
       " 'and',\n",
       " 'it',\n",
       " 'is']"
      ]
     },
     "execution_count": 49,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df[\"stop_lemma\"] = twython_df[\"stop_lemma\"].apply(encode_ascii)\n",
    "twython_df[\"stop_lemma\"][5]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 52,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "['RT',\n",
       " '@alphax_official:',\n",
       " 'UNIQUE',\n",
       " 'SELLING',\n",
       " 'POINTS',\n",
       " '(USP)',\n",
       " 'The',\n",
       " 'coins,',\n",
       " 'distributed',\n",
       " 'through',\n",
       " 'the',\n",
       " 'main',\n",
       " 'chain',\n",
       " 'in',\n",
       " 'the',\n",
       " 'ICO,',\n",
       " 'are',\n",
       " 'Alpha-X',\n",
       " 'coins.',\n",
       " 'This',\n",
       " 'is',\n",
       " 'a']"
      ]
     },
     "execution_count": 52,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df[\"stop_lemma\"][0]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 50,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0    [RT, @alphax_official:, UNIQUE, SELLING, POINT...\n",
       "1    [RT, @HandleMode:, Don't, miss, your, chance, ...\n",
       "2    [RT, @alphax_official:, UNIQUE, SELLING, POINT...\n",
       "3    [RT, @Blindripper85:, Arthur, will, certainly,...\n",
       "4    [How, they, do, it?, Hazy,, they, mention, cry...\n",
       "Name: stop_lemma, dtype: object"
      ]
     },
     "execution_count": 50,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df[\"stop_lemma\"].head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 53,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Must not run more than once\n",
    "# What we did was just take out the brackets and commas by joining at space\n",
    "# notice no more brackets and commas because each string in the \n",
    "# per list was joined with a space in between.\n",
    "\n",
    "\n",
    "twython_df['stop_lemma'] = twython_df['stop_lemma'].apply(lambda element: ' '.join(element))\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 54,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0    RT @alphax_official: UNIQUE SELLING POINTS (US...\n",
       "1    RT @HandleMode: Don't miss your chance at the ...\n",
       "2    RT @alphax_official: UNIQUE SELLING POINTS (US...\n",
       "3    RT @Blindripper85: Arthur will certainly be a ...\n",
       "4    How they do it? Hazy, they mention crypto curr...\n",
       "Name: stop_lemma, dtype: object"
      ]
     },
     "execution_count": 54,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# notice no more brackets and commas because each string in the \n",
    "# per list was joined with a space in between.\n",
    "\n",
    "twython_df[\"stop_lemma\"][0:5]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 55,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "'RT @alphax_official: UNIQUE SELLING POINTS (USP) The coins, distributed through the main chain in the ICO, are Alpha-X coins. This is a'"
      ]
     },
     "execution_count": 55,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df.stop_lemma[0]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 56,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "WordList(['RT', 'alphax_official', 'UNIQUE', 'SELLING', 'POINTS', 'USP', 'The', 'coins', 'distributed', 'through', 'the', 'main', 'chain', 'in', 'the', 'ICO', 'are', 'Alpha-X', 'coins', 'This', 'is', 'a'])"
      ]
     },
     "execution_count": 56,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "tweet = TextBlob(twython_df[\"stop_lemma\"][0])\n",
    "tweet.words"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 57,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "WordList(['RT', 'alphax_official', 'UNIQUE', 'SELLING', 'POINTS', 'USP', 'The', 'coins', 'distributed', 'through', 'the', 'main', 'chain', 'in', 'the', 'ICO', 'are', 'Alpha-X', 'coins', 'This', 'is', 'a'])"
      ]
     },
     "execution_count": 57,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "from textblob import TextBlob, Word\n",
    "\n",
    "\n",
    "tweet = TextBlob(twython_df.stop_lemma[0])\n",
    "tweet.words"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Lets Apply a function to apply this to the DataFrame\n",
    "- Compliments Jose "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 66,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>stop_lemma</th>\n",
       "      <th>Sentiment</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>RT @HandleMode: Don't miss your chance at the ...</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>RT @alphax_official: UNIQUE SELLING POINTS (US...</td>\n",
       "      <td>0.270833</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>RT @Blindripper85: Arthur will certainly be a ...</td>\n",
       "      <td>0.607143</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>How they do it? Hazy, they mention crypto curr...</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>RT @CriptonBon: @KKcoinEX I think this project...</td>\n",
       "      <td>0.520000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>RT @CarpeNoctom: If the #MegaMillions jackpot ...</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>Did you know that SPS token solely present uti...</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>RT @alphax_official: UNIQUE SELLING POINTS (US...</td>\n",
       "      <td>0.270833</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9</th>\n",
       "      <td>RT @ElementsEstates: Did you know that the Ele...</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>10</th>\n",
       "      <td>RT @alphax_official: UNIQUE SELLING POINTS (US...</td>\n",
       "      <td>0.270833</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>11</th>\n",
       "      <td>GOLD and CRYPTO currency Get PAID! Visit: http...</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>12</th>\n",
       "      <td>New post (Revised Russian Draft Bill on Digita...</td>\n",
       "      <td>0.049716</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>13</th>\n",
       "      <td>RT @coinbundlecom: People tend to have a fear ...</td>\n",
       "      <td>-0.300000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>14</th>\n",
       "      <td>RT @HandleMode: Don't miss your chance at the ...</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>15</th>\n",
       "      <td>RT @CriptonBon: @KKcoinEX I think this project...</td>\n",
       "      <td>0.520000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>16</th>\n",
       "      <td>Arthur will certainly be a Professor at the be...</td>\n",
       "      <td>0.607143</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>17</th>\n",
       "      <td>RT @CriptonBon: @KKcoinEX I think this project...</td>\n",
       "      <td>0.520000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>18</th>\n",
       "      <td>RT @RotaCaccraEast: Our joint meeting with @Ro...</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>19</th>\n",
       "      <td>RT @coinspectator: Revised Russian Draft Bill ...</td>\n",
       "      <td>0.020833</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                                           stop_lemma  Sentiment\n",
       "1   RT @HandleMode: Don't miss your chance at the ...   0.000000\n",
       "2   RT @alphax_official: UNIQUE SELLING POINTS (US...   0.270833\n",
       "3   RT @Blindripper85: Arthur will certainly be a ...   0.607143\n",
       "4   How they do it? Hazy, they mention crypto curr...   0.000000\n",
       "5   RT @CriptonBon: @KKcoinEX I think this project...   0.520000\n",
       "6   RT @CarpeNoctom: If the #MegaMillions jackpot ...   0.000000\n",
       "7   Did you know that SPS token solely present uti...   0.000000\n",
       "8   RT @alphax_official: UNIQUE SELLING POINTS (US...   0.270833\n",
       "9   RT @ElementsEstates: Did you know that the Ele...   0.000000\n",
       "10  RT @alphax_official: UNIQUE SELLING POINTS (US...   0.270833\n",
       "11  GOLD and CRYPTO currency Get PAID! Visit: http...   0.000000\n",
       "12  New post (Revised Russian Draft Bill on Digita...   0.049716\n",
       "13  RT @coinbundlecom: People tend to have a fear ...  -0.300000\n",
       "14  RT @HandleMode: Don't miss your chance at the ...   0.000000\n",
       "15  RT @CriptonBon: @KKcoinEX I think this project...   0.520000\n",
       "16  Arthur will certainly be a Professor at the be...   0.607143\n",
       "17  RT @CriptonBon: @KKcoinEX I think this project...   0.520000\n",
       "18  RT @RotaCaccraEast: Our joint meeting with @Ro...   0.000000\n",
       "19  RT @coinspectator: Revised Russian Draft Bill ...   0.020833"
      ]
     },
     "execution_count": 66,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def detect_sentiment(text):\n",
    "    return TextBlob(str(text)).sentiment.polarity\n",
    "# Create a new DataFrame column for sentiment (Warning:Slow!)\n",
    "\n",
    "twython_df[\"Sentiment\"] = twython_df.stop_lemma.apply(detect_sentiment)\n",
    "twython_df[[\"stop_lemma\", \"Sentiment\"]][1:20]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Lets See the Average of the Tweets"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 63,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Sentiment    0.196379\n",
       "dtype: float64"
      ]
     },
     "execution_count": 63,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "twython_df[[\"stop_lemma\", \"Sentiment\"]][1:100].mean()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Nothing Follows. Below is the scratch.  Did not get to experiement with Tweepy"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "x = TextBlob(df.text)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "x = \"Zia is very helpful in teaching NLP.\"\n",
    "y = TextBlob(x)\n",
    "\n",
    "y.words\n",
    "y.sentences"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "y.sentiment"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "y.sentiment.polarity"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "x = \"Zia is very helpful in teaching NLP.  Jose is very very helpful too.  DevMasters is great\"\n",
    "#tokenize\n",
    "# take out stop words . \n",
    "# Lemma\n",
    "# td if\n",
    "# polarity\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "# x-train and x_test\n",
    "\n",
    "# vect = CountVectorizer()\n",
    "\n",
    "#print vect\n",
    "\n",
    "#print X_train\n",
    "#you just tokenized that whole DF so it's goingt to be workable now\n",
    "# With CountVectorizer each sentence is a row \n",
    "# x_trainCV = vect.fit_transform(x_train)\n",
    "# x_trainCV.shape"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Now we will go over Tweepy Streaming Approach. (7 days) "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "import pandas as pd\n",
    "import tweepy\n",
    "from tweepy.streaming import StreamListener\n",
    "from tweepy import OAuthHandler\n",
    "from tweepy import Stream\n",
    "import time"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "class savetweepyTweets(StreamListner):\n",
    "    \n",
    "    def on_data(self, data):\n",
    "        try:\n",
    "            print(type(data))\n",
    "            print(\"New Tweet\")\n",
    "            savefile = open(\"twitter.txt\", \"a\")\n",
    "            savefile = open('twitter.txt','a')\n",
    "            savefile.write(data)\n",
    "            savefile.write('\\n')\n",
    "            savefile.close()\n",
    "            \n",
    "            return True\n",
    "        except BaseException as e:\n",
    "            print (\"Failed \", str(e))\n",
    "            self.disconnect()\n",
    "            \n",
    "    def on_error(self, status):\n",
    "        print (status)\n",
    "        stream.disconnect() \n",
    "        \n",
    "tl = savetweepyTweets()\n",
    "au = OAuthHandler(\"3A49r1HERmMwKiUHgfzxiPz7i\",\"KWRwyv23ekwDqspO8Io0HHHCUAv8AqQ4On8eowqOjkRmwox8ut\")\n",
    "au.set_access_token(\"2316217729-ez4xksWsu2vfCkmTDZnlN3Nf5GCMAVjXvVBBrYq\",\"mN6ozZmsYFFcJjokcZQggfHNxOhUUwqiWXgrS4MwGa1q9\")\n",
    "stream = Stream(au,tl)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "stream.filter(track=[\"Crypto Currency\",\"Cardano\"], languages=['en'])"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "stream.disconnect()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "import json\n",
    "tweets_data = []\n",
    "json_data = open('twitter.txt','r')\n",
    "#print type(json_data)\n",
    "    for line in json_data:\n",
    "     #print type(line)\n",
    "         try:\n",
    "             twe = json.loads(line)\n",
    "             #print type(twe)\n",
    "             tweets_data.append(twe)\n",
    "        except:\n",
    "             continue"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "len(tweets_data)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "tweet_df = pd.DataFrame(tweets_data)\n",
    "tweet_df"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "pd.set_option('display.max_colwidth', 0)\n",
    "tweet_df[[\"created_at\",\"text\"]]"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.7"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
