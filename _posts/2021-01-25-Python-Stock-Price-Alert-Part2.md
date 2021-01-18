---
layout: post
title: "Stock Prices Alerts <br>(part 2)"
subtitle: "A python script that pushes notifications to an iPhone"
date: 2021-01-18 09:00:00 -0500
background: '/img/posts/Python-Stock-Price-Alert/aditya-vyas-7ygsBEajOG0-unsplash.jpg'
---

#### It's now time to refine the working base script
I need the code to take into account multiple stocks with custom target_buy_price and target_sell_price for each. It could be on a spreadsheet or directly in the script code.
The script should run on [boot](https://raspberrypi.stackexchange.com/questions/36927/activate-virtualenv-start-script-on-boot-rpi) in case the server [runs out of power](https://domoticproject.com/protect-raspberry-pi-ups-power-failure/).
