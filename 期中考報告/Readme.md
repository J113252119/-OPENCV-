<h1>碩專一甲  J113252119 智慧監控系統 期中作業</h1>

## 參考網站 https://covid-19.nchc.org.tw/2023_city_confirmed.php

>![](https://github.com/J113252119/-OPENCV-/blob/main/%E6%9C%9F%E4%B8%AD%E8%80%83%E5%A0%B1%E5%91%8A/COVID-19%E5%85%A8%E7%90%83%E7%96%AB%E6%83%85%E5%9C%B0%E5%9C%96.JPG?raw=true)

## 程式執行所需平台 :
>1. Anaconda Navigator
>2. Jupyter Notebook

## 
>![](https://github.com/J113252119/-OPENCV-/blob/main/%E6%9C%9F%E4%B8%AD%E8%80%83%E5%A0%B1%E5%91%8A/Anaconda%20Navigator.JPG?raw=true)

>![](https://github.com/J113252119/-OPENCV-/blob/main/%E6%9C%9F%E4%B8%AD%E8%80%83%E5%A0%B1%E5%91%8A/Jupyter%20Notebook.JPG?raw=true)

## 程式碼所需安裝套件 : 
>1. pip install pandas
>2. pip install lxml
>3. pip install matplotlib
>4. pip install plotly

## 程式說明
import pandas as pd
import requests
import matplotlib.pyplot as plt

#定義目標網址
url = "https://covid-19.nchc.org.tw/2023_dt_csv.php?dt_name=8&ext=全國_全區"

#嘗試爬取並處理資料
try:
    #使用 requests 下載資料
    response = requests.get(url)
    response.raise_for_status()  #檢查是否下載成功

    #將下載的內容存成 CSV 並使用 pandas 讀取
    with open('data.csv', 'wb') as file:
        file.write(response.content)
    data = pd.read_csv('data.csv')

    #檢視資料的前幾行
    print(data.head())
    
    #確保有 "日期" 和 "案例數" 兩列 (列名稱根據資料調整)
    if '日期' in data.columns and '案例數' in data.columns:
        data['日期'] = pd.to_datetime(data['日期'])  #日期格式轉換
        data.set_index('日期', inplace=True)        #設置日期為索引

        #繪圖
        plt.figure(figsize=(10, 6))
        plt.plot(data.index, data['案例數'], label='案例數', marker='o')
        plt.title('疫情案例趨勢圖')
        plt.xlabel('日期')
        plt.ylabel('案例數')
        plt.legend()
        plt.grid()
        plt.show()
    else:
        print("資料中未找到需要的列，請檢查格式！")
except Exception as e:
    print(f"發生錯誤: {e}")

    ##
    
