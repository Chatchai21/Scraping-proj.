# Into the database :closed_lock_with_key:

*tools or equipment to make our information into the database* <br>

   _**Import the library and set the URL**_ :bulb:
   
      from email import header
      
      from webbrowser import Mozilla
      
      import bs4
      
      import requests
      
      import mysql.connector
      
   
   Then define the desired url by storing it in the url variable. This way we can use the url variable throughout our project. in case it can be used in the future If you want to change the link, just fix it here, no need to hardcode it everywhere.
   
     url = 'https://propertyhub.in.th/เช่าคอนโด/ถนนสีลม'
     
## All the code in the first section :arrow_lower_right:

      from email import header
      
      from webbrowser import Mozilla
      
      import bs4
      
      import requests
      
      import mysql.connector

      page=1
      
      name_list = []
      
      url_list = []
      
      price_list = []
      
      while page <= 1:
      
      url = 'https://propertyhub.in.th/เช่าคอนโด/ถนนสีลม'
      
## extract information from the website :incoming_envelope:

The next step is to extract information from the website [https://propertyhub.in.th]

    url = 'https://propertyhub.in.th/เช่าคอนโด/ถนนสีลม'
    
    data = requests.get(url,headers={'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.5060.53        Safari/537.36'})
    
    soup = bs4.BeautifulSoup(data.text,'html.parser')
    
    place_data = soup.find_all('div', {'class':'i5hg7z-2 eGZDxx'})
    
![image](https://scontent.fbkk10-1.fna.fbcdn.net/v/t1.15752-9/318779968_668259588086126_7518729051720835986_n.png?_nc_cat=105&ccb=1-7&_nc_sid=ae9488&_nc_ohc=9H7mVCA15sIAX-7v59j&_nc_ht=scontent.fbkk10-1.fna&oh=03_AdTWWT3XmaCwroHtdE6e01EUDnOmCuiFJ_f6sZSVLvMhGQ&oe=63C088FA)
    
It will retrieve information in the name and price section. Of accommodation in the Silom area,<br> the part that pulls information from website stored as json

    for i in place_data :
    
        name_list.append(i.find('span',{'class':'i5hg7z-3 eHiabG'}).text)
        
        price_list.append(i.find('div' ,{'class':'sc-152o12i-11 eUXUzN priceTag roomPrice'}).text) 
        
        u = i.find("a",{'class':'sc-152o12i-1 eVFiiC'}).get('href').rstrip().replace(" ","")
        
        urll = 'https://propertyhub.in.th'+ str(u)
        
        url_list.append(urll)
        
    print('completed page number:',page)   
    
    page += 1 
    
## Connect to database :book:

_This is the step in which we will connect the information retrieved from the desired website to the database that we have created_

    item_count = len(name_list)
     
    con = mysql.connector.connect(
    
    host = "localhost",
    
    user = "root",
    
    db = "databyb"
    
## Create a data table in the database :bar_chart:

_Take the information fetched into the database to create a table_

    cursorpy = con.cursor()
    
    for i in range(item_count) :
    
    query = "INSERT INTO condorent (Name,Price,Url) VALUES (%s , %s , %s)"
    
    vaule = (name_list[i],(price_list[i]),url_list[i])
    
    cursorpy.execute(query , vaule)

    con.commit()
    
    print(cursorpy)
    
## Display stage :tv:

