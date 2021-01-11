---
layout: post
title: "Stock Prices Alerts <br>(part 1)"
subtitle: "A python script that pushes notifications to an iPhone"
date: 2021-01-10 09:00:00 -0500
background: '/img/posts/Python-Stock-Price-Alert/lo-lo-CeVj8lPBJSc-unsplash.jpg'
---

I needed to keep track of my investments while not having to constantly check them everyday. There is plenty of subscription services that does that but I wanted to learn some python so I got the perfect excuse to try and build one. I will start with a Raspberry pi as the base to serve that service since it will always be running on my network.

In this series I will start from the beginnings to a working notification system. I also expect the functionality to evolve trough the process.

##### Here are a couple links to my inspiration:

* [How to Build a Stock Price Alert Using Python](https://medium.com/illumination/how-to-build-a-stock-price-alert-using-python-d7d61ec12f2)
* [How to Create Stock Alert System using Python and Windows Task Scheduler](https://medium.com/quantjam/how-to-create-stock-alert-system-using-python-and-windows-task-scheduler-5e909c24af27)
* [StockPriceCheck.py from Moving-Electrons on Github](https://gist.github.com/Moving-Electrons/8387168)
* [Python Pushover from laprice on Github](https://github.com/laprice/pushover)

<br/>
#### Setting up the prerequisites
You need an Alpha Vantage account at [alphavantage.co](https://www.alphavantage.co) and generate an api key or #####AV_API_KEY#####, it's free.

Also you need to install Pushover on your device (iPhone / android / Desktop) at [pushover.net](https://pushover.net). On their website you will need to note your User key or #####PUSHOVER_CLIENT_TOKEN##### and create an application to generate an api key or #####PUSHOVER_API_TOKEN#####. The Pushover app has a trial and is not expensive considering it's usefulness.

To create Python apps or scripts, you need to have Python installed. Python 2.7.16 is preinstalled on my Raspberry Pi 4 so I will install the latest in the process. I will also install virtualenv because it gives you the chance to isolate a python installation per project so less clutter, possibly less trouble figuring bugs I don't really know.

While reading trough the inspiration links I figured that I will probably need a couple python libraries:

* [Github project pandas](https://github.com/pandas-dev/pandas)
* [Github project alpha_vantage](https://github.com/RomelTorres/alpha_vantage)
* [Github project matplotlib](https://github.com/matplotlib/matplotlib)
* [Github project python-pushover](https://github.com/Thibauth/python-pushover)


```bash
## let's install python3 and virtualenv on our Raspberry Pi 4.
sudo apt-get update   #This updates all the apps on the Raspberry Pi
sudo apt-get install python3 virtualenv -y
cd ~/Dev/
virtualenv -p python3 StockPriceAlert
cd StockPriceAlert
python -V
  >Python 2.7.16   #This is the system version of python

sudo apt-get install libopenjp2-7-dev   #Missing libraries on Raspberry Pi
sudo apt-get install libtiff5-dev   #Missing libraries on Raspberry Pi
source bin/activate
python -V
  >Python 3.9.1   #In my case I my virtualenv uses the same version a the system one. It could easily be different.
pip freeze
  >
pip install pandas
pip install alpha_vantage   #Github project alpha_vantage
pip install matplotlib
pip install python-pushover   #Github project python-pushover
pip freeze
  >aiohttp==3.7.3
  >alpha-vantage==2.3.1
  >async-timeout==3.0.1
  >attrs==20.3.0
  >certifi==2020.12.5
  >chardet==3.0.4
  >cycler==0.10.0
  >idna==2.10
  >kiwisolver==1.3.1
  >matplotlib==3.3.3
  >multidict==5.1.0
  >numpy==1.19.5
  >pandas==1.2.0
  >Pillow==8.1.0
  >pyparsing==2.4.7
  >python-dateutil==2.8.1
  >python-pushover==0.4
  >pytz==2020.5
  >requests==2.25.1
  >six==1.15.0
  >typing-extensions==3.7.4.3
  >urllib3==1.26.2
  >yarl==1.6.3
touch app.py  #Creates the file app.py where your script resides.
nano app.py   #Edit the content of app.py.
python app.py   #Run the script.
deactivate  #Exit the virtualenv
```

<br/>
#### The contents of app.py:

```Python
import pandas as pd #Data manipulation and analysis package
from alpha_vantage.timeseries import TimeSeries #Enables data pull from Alpha Vantage
import matplotlib.pyplot as plt #If you want to plot your findings
import time
from pushover import Client #Enables you to push notifications to your phone

symbol = "AAPL"
target_sell_price = 230 #enter the price you want to sell at
target_buy_price = 128 #enter the price you want to buy at
interval = '5min' #enter the interval between getting the data from Alpha Vantage

#Getting the data from alpha_vantage
ts = TimeSeries(key='#####AV_API_KEY#####', output_format='pandas')
data, meta_data = ts.get_intraday(symbol,interval, outputsize='full')

#We are currently interested in the latest price
close_data = data['4. close'] #The close data column
last_price = close_data[0] #Selecting the last price from the close_data column

client = Client("#####PUSHOVER_CLIENT_TOKEN#####", api_token="#####PUSHOVER_API_TOKEN#####")#auth with pushover

if last_price > target_sell_price:
    message = str(symbol) + " STOCK ALERT!!! The stock is now above $" + format(target_sell_price, '.2f') + " it's at $" + format(last_price, '.2f')#The message you want to send
    client.send_message(message, title="Stock Price Alert")

if last_price <= target_buy_price:
    message = str(symbol) + " STOCK ALERT!!! The stock is now under $" + format(target_buy_price, '.2f') + " it's at $" + format(last_price, '.2f')#The message you want to send
    client.send_message(message, title="Stock Price Alert")

```

<br/>
To be continued...
