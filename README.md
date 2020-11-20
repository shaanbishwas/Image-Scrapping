# Image-Scrapping : How to download google images using Python and Selenium?
---
### Prerequisites
* Python 3.x
* Firefox Browser
---
## Step 1: Download Firefox Web Driver According to your OS:

[windows 64 bit](https://github.com/mozilla/geckodriver/releases/download/v0.26.0/geckodriver-v0.26.0-win64.zip)

---
## Step 2: Place the web driver on a particular location
After downloading web driver, extraxt in and place exe in a particular location. In my case as I am using windows, I created a folder in C drive as "Drivers" and pasted the "geckodriver.exe" in it

---
## Step 3: Install BeautifulSoup and Selenium
Open cmd and write below commands to install BeautifulSoup and Selenium
```python
pip install bs4

pip install selenium
```
---
## Step 4: Code
```pyhton
from selenium import webdriver
from bs4 import BeautifulSoup
import requests
import urllib.request
import time
import sys
import os


#taking user input
print("What do you want to download?")
download = input()
site = 'https://www.google.com/search?tbm=isch&q='+download


#providing driver path
driver = webdriver.Firefox(executable_path = 'C:\Drivers\geckodriver.exe')

#passing site url
driver.get(site)


#if you just want to download 10-15 images then skip the while loop and just write
#driver.execute_script("window.scrollBy(0,document.body.scrollHeight)")


#below while loop scrolls the webpage 7 times(if available)

i = 0

while i<7:  
	#for scrolling page
    driver.execute_script("window.scrollBy(0,document.body.scrollHeight)")
    
    try:
		#for clicking show more results button
        driver.find_element_by_xpath("/html/body/div[2]/c-wiz/div[3]/div[1]/div/div/div/div/div[5]/input").click()
    except Exception as e:
        pass
    time.sleep(5)
    i+=1

#parsing
soup = BeautifulSoup(driver.page_source, 'html.parser')


#closing web browser
driver.close()


#scraping image urls with the help of image tag and class used for images
img_tags = soup.find_all("img", class_="rg_i")


count = 0
for i in img_tags:
    #print(i['src'])
    try:
		#passing image urls one by one and downloading
        urllib.request.urlretrieve(i['src'], str(count)+".jpg")
        count+=1
        print("Number of images downloaded = "+str(count),end='\r')
    except Exception as e:
        pass
```
