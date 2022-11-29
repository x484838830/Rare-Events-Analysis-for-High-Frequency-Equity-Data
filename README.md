(11/29HW)Rare Events Analysis for High-Frequency Equity Data 
===============================================================
## 摘要 ##
在這次研究中，提出了一種檢測罕見事件的方法，這些事件被定義為相對於交易量的大幅價格變動，在檢測到這些罕見事件後分析股票的行為。我們提供了基於對這些事件的檢測來校準交易規則的方法，並針對特定的價差進行說明。我方法應用於一年內數蘋果股票的報價數據。

### 資料來源 ###
1. Yahoo! Finance(yifinance)抓取蘋果公司(AAPL)股價
2. 時間:1年

### 數學公式 ###
<img width="600" alt="image" src="https://user-images.githubusercontent.com/71696727/204494032-879f11f9-2b77-498b-925c-b8fb88e63b9f.png">

### 從yifinance抓取AAPL資料 ###

```python
!pip install yfinance
import yfinance as yf
msft = yf.Ticker("AAPL")
msft.info     # 獲取股價資訊
hist = msft.history(period="1y") #獲取歷史資料(這裡期間設置1年)
```

### 將收盤價、成交量、時間列出 ###
```python
hist.reset_index(inplace=True) #將Index轉成Column
hist = hist.rename(columns = {'Date':'Date'}) #轉成Column後命名成Date
#data = hist[["Close", "Volume",'Date']]
data_date = list(hist[["Date"]].iloc[:, 0]) #交易日期
data_close = list(hist[["Close"]].iloc[:, 0])#收盤價格
data_volume = list(hist[["Volume"]].iloc[:, 0])#成交量
```

### 設定V0 ###
```python
V0 = hist["Volume"].mean()  #取成交量平均值
```

### 取得價差與時間 ###
```python
dif_close=[]  #收盤價
dif_date=[]   #日期

for i in range(len(data)-1,-1,-1): #由後面算到前面，
  v_sum=0
  for j in range(i-1,-1,-1):
    v_sum=0
    if (v_sum < V0):
      v_sum = v_sum + data.iloc[i, 1]  #累加 v_sum
      Spread = data['Close'][i]-data['Close'][j] #算價差 ＝ Spread
      dif_close.append(Spread) #將Spread加入dif_close
      dif_date.append([(data_date[(data_close.index(max(data_close)))]), (data_date[(data_close.index(min(data_close)))])]) #抓出日期
    else:
      v_sum = 0                         #若v_sum大於V0 初始化v_sum
      break                             #跳出迴圈
print('max(dif_close) = ',max(dif_close))
print('dif_date = ',dif_date[0])
```




