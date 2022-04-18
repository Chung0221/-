# -Python web crawler with BeautifulSoup &amp; Requests 
import csv
import requests
from bs4 import BeautifulSoup

headers = {'user-agent': 'Mozilla/5.0'}

url = "https://icook.tw/search/%E5%AE%B6%E5%B8%B8%E8%8F%9C/"  #家常菜的網址

recipe = requests.get(url,headers=headers)  #取得網址
           
print(recipe.status_code)    #查詢狀態碼   200成功   404 失敗(找不到)

soup = BeautifulSoup(recipe.text,'lxml')   #解析網頁的內容並擷取我們所需的資料

info_items = soup.find_all('div','browse-recipe-content')

with open("食譜1.csv","w",encoding="UTF-8",newline="")as csv_file:

    csv_writer=csv.writer(csv_file)
    
    csv_writer.writerow(["菜名","食材"])    #建立 writer 物件，就可以使用 writerow 的方法寫入檔案，先寫入欄位標題

    for item in info_items:   #透過 for 迴圈一筆筆將菜單的資料寫入檔案中
        name = item.find("h2",class_="browse-recipe-name").text.strip()    #菜色名稱 ; strip()為刪除多餘空白
        ingredient = item.find("p",class_="browse-recipe-content-ingredient").text.strip()   #成分
        csv_writer.writerow([name,ingredient])
        
