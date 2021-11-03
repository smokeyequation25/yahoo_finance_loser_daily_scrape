
print("importing libraries.....")
import requests
from bs4 import BeautifulSoup
import csv
import os
import pandas as pd
import datetime
from datetime import datetime, date
import time
import numpy as np
import certifi
import json
import ssl
try:
    # For Python 3.0 and later
    from urllib.request import urlopen
except ImportError:
    # Fall back to Python 2's urllib2
    from urllib2 import urlopen


print("finished importing libraries.....loading in script")




######################## Step 1 #######################################

print("working on step 1 now.....")

today = date.today()

real_date = today.strftime("%m-%d-%Y")

FILENAME = "y_finance_day_loser_stocks - " + real_date + ".csv"

os.chdir("/chronos_exports/Yahoo_Finance_Scrapes//Day_Losers")
names=[]
prices=[]
changes=[]
percentChanges=[]
marketCaps=[]
totalVolumes=[]
circulatingSupplys=[]
symbols=[]
 

CryptoCurrenciesUrl = "https://finance.yahoo.com/screener/predefined/day_losers"
r= requests.get(CryptoCurrenciesUrl)
data=r.text
soup=BeautifulSoup(data, "html.parser")
 
for listing in soup.find_all('tr',{'class':'simpTblRow'}):
   for symbol in listing.find_all('td', attrs={'aria-label':'Symbol'}):
      symbols.append(symbol.text)
   for name in listing.find_all('td',attrs={'aria-label':'Name'}):
      #print("names are: " + name)
      names.append(name.text)
   for price in listing.find_all('td', attrs={'aria-label':'Price (Intraday)'}):
      prices.append(price.text)
   for change in listing.find_all('td', attrs={'aria-label':'Change'}):
      changes.append(change.text)
   for percentChange in listing.find_all('td', attrs={'aria-label':'% Change'}):
      percentChanges.append(percentChange.text)
   for marketCap in listing.find_all('td', attrs={'aria-label':'Market Cap'}):
      marketCaps.append(marketCap.text)
   for totalVolume in listing.find_all('td', attrs={'aria-label':'Avg Vol 3 month'}):
      totalVolumes.append(totalVolume.text)
   for circulatingSupply in listing.find_all('td', attrs={'aria-label':'Volume'}):
      circulatingSupplys.append(circulatingSupply.text)
#print(prices)
#with open('output.csv', 'w') as csvfile:
    #writer = csv.writer(csvfile)
    #writer.writerow(["Names","Change", "% Change", "Market Cap", "Average Volume", "Volume"])
most_active = pd.DataFrame({"Symbols": symbols,"Names": names,"Prices": prices, "Change_Dollars": changes, "Change_Percent": percentChanges, "Current_Volume": circulatingSupplys, "Market_Cap": marketCaps})
print(most_active)

most_active .to_csv(FILENAME, index=False)


print("Step 1 complete check this directory for output: /chronos_exports/Yahoo_Finance_Scrapes//Day_Losers")



########################### Step 2 #######################################

print("working on step 2 now.....")

today = date.today()

real_date = today.strftime("%m-%d-%Y")

FILENAME = "y_finance_price_target_stocks - " + real_date + ".csv"

FILENAMETOREAD = "y_finance_day_loser_stocks - " + real_date + ".csv"

os.chdir("/chronos_exports/Yahoo_Finance_Scrapes//Day_Losers")

list_of_stocks_to_test = pd.read_csv(FILENAMETOREAD)
#print(list_of_stocks_to_test)

# Create an empty list
#Row_list =[]
  
# Iterate over each row
#for index, rows in list_of_stocks_to_test.iterrows():
    # Create list for the current row
    #my_list =[rows.Symbols]
      
    # append the list to the final list
    #Row_list.append(my_list)
Row_list= list_of_stocks_to_test['Symbols'].tolist()
  
# Print the list
#print(Row_list)

names=[]
prices=[]
changes=[]
percentChanges=[]
marketCaps=[]
totalVolumes=[]
circulatingSupplys=[]
symbols=[]
openprices=[]
previouscloses=[]
fifty_two_wk_ranges=[]
volumes=[]
avg_vols=[]
price_targets=[]
final_tickers=[]
 
for i in Row_list:
    #print(i)
    time.sleep(5)
    a_url = "https://finance.yahoo.com/quote"
    CryptoCurrenciesUrl = '{}/{}'.format(a_url, i)
    r= requests.get(CryptoCurrenciesUrl)
    print("fetching url:" + CryptoCurrenciesUrl)
    data=r.text
    print("extracting text from url:" + CryptoCurrenciesUrl)
    soup=BeautifulSoup(data, "html.parser")
    print("parsing soup from url:" + CryptoCurrenciesUrl)
    final_tickers.append(i)
    print("finding table in soup from url:" + CryptoCurrenciesUrl)
    for listing in soup.find_all('tr',{'class':'Bxz(bb)'}):
   #print(listing)
       for openprice in listing.find_all('td', attrs={'data-test':'OPEN-value'}):
          openprices.append(openprice.text)
          print("getting last open price from url:" + CryptoCurrenciesUrl)
       for previousclose in listing.find_all('td', attrs={'data-test':'PREV_CLOSE-value'}):
          print("getting previous close from url:" + CryptoCurrenciesUrl)
          previouscloses.append(previousclose.text)
       for fifty_two_wk_range in listing.find_all('td', attrs={'data-test':'FIFTY_TWO_WK_RANGE-value'}):
          print("finding 52 week range from url:" + CryptoCurrenciesUrl)
          fifty_two_wk_ranges.append(fifty_two_wk_range.text)
       for volume in listing.find_all('td', attrs={'data-test':'TD_VOLUME-value'}):
          print("finding volume from url:" + CryptoCurrenciesUrl)
          volumes.append(volume.text)
       for avg_vol in listing.find_all('td', attrs={'data-test':'AVERAGE_VOLUME_3MONTH-value'}):
          print("finding avg volume from url:" + CryptoCurrenciesUrl)
          avg_vols.append(avg_vol.text)
       for price_target in listing.find_all('td', attrs={'data-test':'ONE_YEAR_TARGET_PRICE-value'}):
          print("finding price targets from url:" + CryptoCurrenciesUrl)
          price_targets.append(price_target.text)



#print(prices)
#with open('output.csv', 'w') as csvfile:
    #writer = csv.writer(csvfile)
    #writer.writerow(["Names","Change", "% Change", "Market Cap", "Average Volume", "Volume"])
stock_page = pd.DataFrame({"Symbols": final_tickers,"Open Price": openprices,"Prev Close": previouscloses, "Current Volume": volumes, "Avg Volume": avg_vols, "Price Target": price_targets})#.dropna(axis=0, subset=['Price Target'])
#stock_page_final = stock_page.dropna(how='any', inplace=True)
print(stock_page)
os.chdir("/chronos_exports/Yahoo_Finance_Scrapes//Price_Targets")
stock_page.to_csv(FILENAME, index=False)

print("Step 2 complete check this directory for output: /chronos_exports/Yahoo_Finance_Scrapes//Price_Targets")


######################### Step 3 ########################################

print("working on step 3 now.....")

today = date.today()




real_date = today.strftime("%m-%d-%Y")

FINALFILENAME = "y_finance_ingest_file - " + real_date + ".csv"
pricetargetfile = "/chronos_exports/Yahoo_Finance_Scrapes/Price_Targets/y_finance_price_target_stocks - " + real_date + ".csv"
losersfile = "/chronos_exports/Yahoo_Finance_Scrapes/Day_Losers/y_finance_day_loser_stocks - " + real_date + ".csv"

price_target_file = pd.read_csv(pricetargetfile)

losers_file = pd.read_csv(losersfile)


print(price_target_file)
print(losers_file)

all_metrics_joined = pd.merge(price_target_file,losers_file[['Symbols','Names', 'Prices', 'Change_Dollars','Change_Percent']], on = "Symbols", how = 'left')

print(all_metrics_joined)

os.chdir("/chronos_exports/Yahoo_Finance_Scrapes//Final_Investment_Reccomendations")

all_metrics_joined['Profit Estimate Per Share'] = (all_metrics_joined['Price Target'] - all_metrics_joined['Prices']).astype(float).round(2)

#all_metrics_joined['Percent Increase or Decrease in Volume'] = all_metrics_joined['Current Volume']

investment_dollars = 1000



all_metrics_joined['Number of Shares I Can Afford'] = (investment_dollars / all_metrics_joined['Prices']).astype(float).round(0)

all_metrics_joined['Profit Estimate with 1000 invested'] = (all_metrics_joined['Profit Estimate Per Share'] * all_metrics_joined['Number of Shares I Can Afford']).astype(float).round(2)

all_metrics_joined['Percent_Return_Estimate'] = ((all_metrics_joined['Profit Estimate with 1000 invested'] / investment_dollars) *100).astype(float).round(2)
all_metrics_joined = all_metrics_joined.dropna()

final_file = all_metrics_joined.loc[all_metrics_joined['Percent_Return_Estimate'] > 65]

final_file.to_csv(FINALFILENAME, index=False)

print("Step 3 complete check this directory for output: /chronos_exports/Yahoo_Finance_Scrapes//Final_Investment_Reccomendations")


######################## Step 4 ################################


print("working on step 4 now.....")
os.chdir("/chronos_exports/Yahoo_Finance_Scrapes//Final_Investment_Reccomendations")

today = date.today()

real_date = today.strftime("%m-%d-%Y")
FINALFILENAME = "y_finance_ingest_file - " + real_date + ".csv"
final_list = pd.read_csv(FINALFILENAME)

symbols_list= final_list['Symbols'].tolist()

print(symbols_list)
buy_rating_data = []
symbols = []
rating_reccomendations = []

def get_jsonparsed_data(url):
    """
    Receive the content of ``url``, parse it as JSON and return the object.

    Parameters
    ----------
    url : str

    Returns
    -------
    dict
    """
    ssl_context = ssl.SSLContext(ssl.PROTOCOL_TLSv1)
    response = urlopen(url, context=ssl_context)
    data = response.read().decode("utf-8")
    return json.loads(data)
for i in symbols_list:
    #print(i)
    api_key = "apikey=a746868d59f6318e79d87aac9e887d1c"
    a_url = "https://financialmodelingprep.com/api/v3/rating"
    CryptoCurrenciesUrl = '{}/{}'.format(a_url, i)
    final_url = '{}?{}'.format(CryptoCurrenciesUrl, api_key) 
    print(final_url)
    #response = get_jsonparsed_data(final_url)
    
    response = requests.get(final_url).json()
    
    buy_rating_data.append(response)
    #response_decoded = json.loads(response)
    symbol = response[0]['symbol']
    rating_reccomendation = response[0]['ratingRecommendation']
    #buy_rating_data.append(response.text)
    symbols.append(symbol)
    rating_reccomendations.append(rating_reccomendation)
    #print(symbol)
    #print(rating_reccomendation)
    #print(response)
print(symbols)
print(rating_reccomendations)

reccomendations = pd.DataFrame({"Symbols": symbols,"Rating": rating_reccomendations})#.dropna(axis=0, subset=['Price Target'])

FILENAME = "fmp_ratings - " + real_date + ".csv"

os.chdir("/chronos_exports/Yahoo_Finance_Scrapes//Combining")

reccomendations.to_csv(FILENAME, index=False)

print("Step 4 complete check this directory for output: /chronos_exports/Yahoo_Finance_Scrapes//Combining")

####################### Step 5 ##############################################
print("working on step 5 now.....")

today = date.today()




real_date = today.strftime("%m-%d-%Y")

FINALFILENAME = "huginn_stocks - " + real_date + ".csv"
buyrating = "/chronos_exports/Yahoo_Finance_Scrapes//Combining/fmp_ratings - " + real_date + ".csv"
othermetrics = "/chronos_exports/Yahoo_Finance_Scrapes//Final_Investment_Reccomendations/y_finance_ingest_file - " + real_date + ".csv"

othermetricsfile = pd.read_csv(othermetrics)

buyratingfile = pd.read_csv(buyrating)


print(othermetricsfile)
print(buyratingfile)

all_metrics_joined = pd.merge(othermetricsfile,buyratingfile[['Symbols','Rating']], on = "Symbols", how = 'left')

print(all_metrics_joined)

os.chdir("/chronos_exports/Yahoo_Finance_Scrapes/Huginn_Stocks")


all_metrics_joined.rename(columns=({'Open Price': 'Open_Price', 'Prev Close': 'Prev_Close', 'Current Volume': 'Curr_Vol', 'Avg Volume': 'Avg_Vol', 'Price Target': 'Price_Target', 'Profit Estimate Per Share': 'Profit_Estimate_Per_Share', 'Number of Shares I Can Afford': 'Number_of_Shares_I_Can_Afford', 'Profit Estimate with 1000 invested': 'Profit_Estimate_with_1000_invested'}),inplace=True,)
all_metrics_joined.to_csv(FINALFILENAME, index=False)

print("Step 5 complete check this directory for output: /chronos_exports/Yahoo_Finance_Scrapes/Huginn_Stocks")
