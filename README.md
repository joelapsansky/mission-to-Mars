# Mission to Mars
  
[Mission to Mars Challenge](/Mission_to_Mars_Challenge.ipynb)  
  
## Overview of Project  
*Scrape Mars images and titles from a website  
*Write a web app that will display an update button meant to trigger our scrape code  
*Add bootstrap components to index.html file  
  
### App and Scraping files
[App.py](/app/app.py)    
[Scraping.py](/app/scraping.py)    
  
  
This code does the scraping:  
```
# Create a list to hold the images and titles
hemisphere_image_urls = []

# Write code to retrieve the image urls and titles for each hemisphere
main_html = browser.html
item_soup = soup(main_html, 'html.parser')
items = item_soup.find_all('div', class_='item')
base_main_url = 'https://astrogeology.usgs.gov//'

for i in items:
    
    try:
        title = i.find('img', class_="thumb")['alt']
        base_image_url = i.find('a')['href']
        browser.visit(base_main_url + base_image_url)
        image_html = browser.html
        image_soup = soup(image_html, 'html.parser')
        image_url = base_main_url + image_soup.find('img', class_='wide-image')['src']
      
        if (title and image_url):
            item_dict = {
                'title': title,
                'img_url': image_url
            }
            
        hemisphere_image_urls.append(item_dict)
    
    except Exception as e:
        print(e)
```    
    
It loops through all of the ‘a’ tags, extracts the title and partial URL, adds the partial URL to the base URL, and then adds all items to a list of dictionaries. The loop continues until all of the titles and images are retrieved.  
  
This is then added as a function in the scraping.py file.  
  
The app.py file later uses Flask to make a mongo connection, render a template, and use the scraping file to populate our website. Finally, in the index.html file, we add what we would like to see from the database. The HTML doesn't hold variables, but we substitute the information into the code, which the index file pulls from the server.  
  
After adding a few bootstrap components to the index.html file, I was left with a nice, mobile-friendly, website that looks something like this:  
    
  
![Website Screenshot](/website_screenshot.png "Website Screenshot")
  
