# Scraping-IMDB
Scraping IMDB fan favourite movies

from selenium import webdriver
import pandas as pd

website_url = 'https://www.imdb.com/what-to-watch/fan-favorites/?ref_=hm_fanfav_sm'

driver = webdriver.Chrome()

driver.get(website_url)

driver.maximize_window()
Getting fan favorite movie titles & ratings
container = driver.find_element(by='xpath',value='//section[contains(@class, "ipc-page-section ipc-page-section--none ipc-page-section--bp-none")]')

audible_list = container.find_elements(by='xpath',value='./div[2]/div/div/div[contains(@class,"ipc-sub-grid ipc-sub-grid--page-span-3 ipc-sub-grid--wrap sc-550c1afc-0 kQtiZR ipc-page-grid__item ipc-page-grid__item--span-3")]/div')

movie_title = []
movie_rattings = []

for product in audible_list:
    
    movie_title.append(product.find_element(by='xpath',value='.//span[contains(@data-testid, "title")]').text)
    movie_rattings.append(product.find_element(by='xpath',value='.//div[contains(@class, "ipc-rating-star-group")]').text)
    
df = pd.DataFrame({"Movie title":movie_title,"Movie rating": movie_rattings})

df.to_excel("Fan_favorite_movie_details.xlsx",index= False)
