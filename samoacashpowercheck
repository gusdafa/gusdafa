# Samoa EPC Smart Meter cash power check script, using python, driven by Gus Crichton and coded and debugged by ChatGPT
import requests
import base64
import re
import smtplib
from email.mime.text import MIMEText
from bs4 import BeautifulSoup

# Set up a session to persist the login
session = requests.Session()

# Define the login URL and credentials
url = "https://www.energyinsight.ws/EnergyInsight/j_spring_security_check"
username = "yourEPCuser"
password = "yourpassword"

# Encode the credentials in Base64
creds = f"{username}:{password}"
encoded_creds = base64.b64encode(creds.encode("utf-8")).decode("utf-8")

# Define the headers and data for the login request
headers = {
    "Authorization": f"Basic {encoded_creds}"
}
data = {
    "j_username": username,
    "j_password": password
}

# Send the login request
response = requests.post(url, headers=headers, data=data)
soup = BeautifulSoup(response.content, 'html.parser')


# find the span tag with class 'balance' and id 'accountBalance'
balance_span = soup.find('span', {'class': 'balance', 'id': 'accountBalance'})

# extract the balance text
balance_text = balance_span.text.strip()

# convert the balance text into an integer by removing non-digit characters and decimal point
balance_int = int(''.join(filter(str.isdigit, balance_text)))

print(balance_int)

# set up SMTP server
smtp_server = 'smtp.youremailserver.com'
smtp_port = 25

# create message object
msg = MIMEText('')
msg['Subject'] = 'WARNING!! Low Cash Power Balance'
msg['From'] = 'youremail@mail.com'
msg['To'] = 'youremail@mail.com'


# send email if cash power is less than 50 units
if balance_int < 5000:
    #do soup and post form here to initiate cashpower topup
    #configure activeexperts to grab TAC for auth to complete the transaction, email receipt and amount 
    msg.set_payload(f'WARNING: Your account balance is only {balance_int}! That is less than 50 Tala. Top up da cashpowaaa!!!!!!!')
    with smtplib.SMTP(smtp_server, smtp_port) as server:
        server.sendmail(msg['From'], msg['To'], msg.as_string())
    print('Email sent!')
else:
    print('Account balance is sufficient.')
