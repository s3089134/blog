---
layout: post
title: Collecting data
---
import requests
from bs4 import BeautifulSoup
import pandas as pd

def scrape(url,df_dict):  #to scrape 'Title','Company','Location','Salary' and 'JobSummary' from each HTML page
    # establishing the connection to the webpage
    response = requests.get(url)

    # You can use status codes to understand how the target server responds to your request.
    #Ex. 200 = OK, 400 = Bad Request, 403 = Forbidden, 404 = Not Found
    print('Status Code: ',response.status_code)

    if response.status_code==200:
        # Pull HTML string out of requests and convert to a python string
        html = response.text
        soup = BeautifulSoup(html, 'lxml')  #'parsing HTML data'
        record=soup.findAll('article',{'data-automation':'normalJob'})
        for i in range(len(record)): #iteration through records
            s0=record[i].find('span',{'class':'lwHBT6d'}) #extract salary
            if s0:

                df_dict['Salary'].append(s0.text)
            else:
                df_dict['Salary'].append('NA')

