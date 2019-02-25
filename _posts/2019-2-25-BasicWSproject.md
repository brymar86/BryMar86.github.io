layout: post
title: Basic Webscraping Project
date: 2019-02-25 04:00:00
tags: Webscrapping BeautifulSoup API
author: Bryan Marty

```python
import requests
from bs4 import BeautifulSoup
from fake_useragent import UserAgent

user_agent = UserAgent()
# Lets ID the main landing page on Coding bat
main_url = 'http://codingbat.com/java'
# Get the page & convert it to Beautiful Soup
page = requests.get(main_url, headers = {'user-agent': user_agent.chrome})
soup = BeautifulSoup(page.content, 'lxml')
# Set base url for the variable
base_url = 'http://codingbat.com'
# Inspect HTML Tags of coding bat and to pull in all relevant tags that map to the various question sections
all_divs = soup.find_all('div', class_ = "summ")
#iterae through the div tags and pull out the section links this prints out ALL the section LinkS on coding bat. 
for div in all_divs:
    print(base_url + div.a['href'])
```

    http://codingbat.com/java/Warmup-1
    http://codingbat.com/java/Warmup-2
    http://codingbat.com/java/String-1
    http://codingbat.com/java/Array-1
    http://codingbat.com/java/Logic-1
    http://codingbat.com/java/Logic-2
    http://codingbat.com/java/String-2
    http://codingbat.com/java/String-3
    http://codingbat.com/java/Array-2
    http://codingbat.com/java/Array-3
    http://codingbat.com/java/AP-1
    http://codingbat.com/java/Recursion-1
    http://codingbat.com/java/Recursion-2
    http://codingbat.com/java/Map-1
    http://codingbat.com/java/Map-2
    http://codingbat.com/java/Functional-1
    http://codingbat.com/java/Functional-2



```python
user_agent = UserAgent()
# Lets ID the main landing page on Coding bat
main_url = 'http://codingbat.com/java'
# Get the page & convert it to Beautiful Soup
page = requests.get(main_url, headers = {'user-agent': user_agent.chrome})
soup = BeautifulSoup(page.content, 'lxml')
# Set base url for the variable
base_url = 'http://codingbat.com'
# Inspect HTML Tags of coding bat and to pull in all relevant tags that map to the various question sections
all_divs = soup.find_all('div', class_ = "summ")

# Where all the Section links on the main page are.
section_links = [base_url + div.a['href'] for div in all_divs]

for link in section_links:
# we can request data from the section links and store the information/content in a variable.
    inner_page = requests.get(link, headers = {'user-agent': user_agent.chrome})
    inner_soup = BeautifulSoup(inner_page.content, 'lxml')
#  Notice i'm going to use the find method so this will find only the first spot of this on the page. (which is find b/c that where the q's are.)
    div = inner_soup.find('div', class_ = 'tabc')
    
    
    # Lets test som code out.  WE just saw that div.table tag contains the questions and that they are located on the td.a tag.
# Make sure you are for looping within the for loop link so this picks up the code after you're inside the div tag on the section link

# This Works yea!
    for td in div.table.find_all('td'):
        print(base_url + td.a['href'])
    break
```

    http://codingbat.com/prob/p187868
    http://codingbat.com/prob/p181646
    http://codingbat.com/prob/p154485
    http://codingbat.com/prob/p116624
    http://codingbat.com/prob/p140449
    http://codingbat.com/prob/p182873
    http://codingbat.com/prob/p184004
    http://codingbat.com/prob/p159227
    http://codingbat.com/prob/p191914
    http://codingbat.com/prob/p190570
    http://codingbat.com/prob/p123384
    http://codingbat.com/prob/p136351
    http://codingbat.com/prob/p161642
    http://codingbat.com/prob/p112564
    http://codingbat.com/prob/p183592
    http://codingbat.com/prob/p191022
    http://codingbat.com/prob/p192082
    http://codingbat.com/prob/p144535
    http://codingbat.com/prob/p178986
    http://codingbat.com/prob/p165701
    http://codingbat.com/prob/p100905
    http://codingbat.com/prob/p151713
    http://codingbat.com/prob/p199720
    http://codingbat.com/prob/p101887
    http://codingbat.com/prob/p172021
    http://codingbat.com/prob/p132134
    http://codingbat.com/prob/p177372
    http://codingbat.com/prob/p173784
    http://codingbat.com/prob/p125339
    http://codingbat.com/prob/p125268
    http://codingbat.com/prob/p196441



```python
user_agent = UserAgent()
# Lets ID the main landing page on Coding bat
main_url = 'http://codingbat.com/java'
# Get the page & convert it to Beautiful Soup
page = requests.get(main_url, headers = {'user-agent': user_agent.chrome})
soup = BeautifulSoup(page.content, 'lxml')
# Set base url for the variable
base_url = 'http://codingbat.com'
# Inspect HTML Tags of coding bat and to pull in all relevant tags that map to the various question sections
all_divs = soup.find_all('div', class_ = "summ")

# Where all the Section links on the main page are.
section_links = [base_url + div.a['href'] for div in all_divs]

for link in section_links:
# we can request data from the section links and store the information/content in a variable.
    inner_page = requests.get(link, headers = {'user-agent': user_agent.chrome})
    inner_soup = BeautifulSoup(inner_page.content, 'lxml')
#  Notice i'm going to use the find method so this will find only the first spot of this on the page. (which is find b/c that where the q's are.)
    div = inner_soup.find('div', class_ = 'tabc')
#     OK so observe this. I'm on the div tag class 'tabc' I can map down to the table tag and then from there find all <td> tags where the links
#     are located. 
    question_links = [base_url + td.a['href'] for td in div.table.find_all('td')]
    
    for question_link in question_links:
        final_page = requests.get(question_link)
        final_soup = BeautifulSoup(final_page.content, 'lxml')
#         This is were the the entire Page lives.  (div where attrs = class and value = indent) 
        indent_div = final_soup.find('div', attrs = {'class' : 'indent'})
#     Here we get down to where the quesiton is asked.  
        problem_statement = indent_div.table.div.string
        print(problem_statement)
#        Check it out
        break
    break
        

```

    The parameter weekday is true if it is a weekday, and the parameter vacation is true if we are on vacation. We sleep in if it is not a weekday or we're on vacation. Return true if we sleep in.



```python
import requests
from bs4 import BeautifulSoup
from fake_useragent import UserAgent

user_agent = UserAgent()
# Lets ID the main landing page on Coding bat
main_url = 'http://codingbat.com/java'
# Get the page & convert it to Beautiful Soup
page = requests.get(main_url, headers = {'user-agent': user_agent.chrome})
soup = BeautifulSoup(page.content, 'lxml')
# Set base url for the variable
base_url = 'http://codingbat.com'
# Inspect HTML Tags of coding bat and to pull in all relevant tags that map to the various question sections
all_divs = soup.find_all('div', class_ = "summ")

# Where all the Section links on the main page are.
section_links = [base_url + div.a['href'] for div in all_divs]

for link in section_links:
# we can request data from the section links and store the information/content in a variable.
    inner_page = requests.get(link, headers = {'user-agent': user_agent.chrome})
    inner_soup = BeautifulSoup(inner_page.content, 'lxml')
#  Notice i'm going to use the find method so this will find only the first spot of this on the page. (which is find b/c that where the q's are.)
    div = inner_soup.find('div', class_ = 'tabc')
#     OK so observe this. I'm on the div tag class 'tabc' I can map down to the table tag and then from there find all <td> tags where the links
#     are located. 
    question_links = [base_url + td.a['href'] for td in div.table.find_all('td')]
    
    for question_link in question_links:
        final_page = requests.get(question_link)
        final_soup = BeautifulSoup(final_page.content, 'lxml')
#         This is were the the entire Page lives.  (div where attrs = class and value = indent) 
        indent_div = final_soup.find('div', attrs = {'class' : 'indent'})
#     Here we get down to where the quesiton is asked.  
        problem_statement = indent_div.table.div.string
#         print(problem_statement)
#        Check it out

        sibling_of_statement = indent_div.table.div.next_siblings
        
        for sibling in sibling_of_statement:
# ok so I checked sibling (bunch of stuff I don't want), then sibling.string(had none's), then the latter and it worked alright.           
            if sibling.string is not None:
                print(sibling)
        break
    break
```

    sleepIn(false, false) → true
    sleepIn(true, false) → false
    sleepIn(false, true) → true



```python
import requests 
from bs4 import BeautifulSoup
from fake_useragent import UserAgent
# Instantiate proper variables and pull the main page links.  Create url variables. 
user_agent = UserAgent()
main_url = 'http://codingbat.com/java'
page = requests.get(main_url, headers = {'user-agent': user_agent.chrome})
soup = BeautifulSoup(page.content, 'lxml')

base_url = 'http://codingbat.com'

#Verify the Main Parts of the Webpage HTML tags the links sit on. 
all_divs = soup.find_all('div', class_ = 'summ')
# loop through the links and store in a variable. 
section_links = [base_url + div.a['href'] for div in all_divs]

#Now lets open these links in the section links and get the Question links
for link in section_links:
    inner_page = requests.get(link, headers = {'user-agent': user_agent.chrome})
    inner_soup = BeautifulSoup(inner_page.content, 'lxml')
#  Lets find the tags now on this page that maps to the question links we want. YOU NEED TO use FIND here if you LC like below. 
    div = inner_soup.find('div', class_ = 'tabc')
# Observe: I'm on the div tag with attribute=class and value='tabc' I can map down to the table tag and then from there find all <td> tags 
# where the links are located.    
    question_links = [base_url + td.a['href'] for td in div.table.find_all('td')]

# lets now open the question links, print the question and examples on that page
    for question_link in question_links:
        final_page = requests.get(question_link, headers = {'user-agent': user_agent.chrome})
        final_soup = BeautifulSoup(final_page.content, 'lxml')
        indent_div = final_soup.find('div', attrs = {'class': 'indent'})
        #This is going to store the question in a variable.  I could use a different drill down tree but I know this works. 
        problem_statement = indent_div.table.div.string
#  For the other part we need for the examples is found on the problem_statement's sibling tags.
        sibling_of_statement = indent_div.table.div.next_siblings
        examples = [sibling for sibling in sibling_of_statement if sibling.string is not None]
        print(problem_statement)
        print('\n')
        for example in examples:
            print(example)
            
        break
    break 
    



```

    The parameter weekday is true if it is a weekday, and the parameter vacation is true if we are on vacation. We sleep in if it is not a weekday or we're on vacation. Return true if we sleep in.
    
    
    sleepIn(false, false) → true
    sleepIn(true, false) → false
    sleepIn(false, true) → true

