import requests
from datetime import datetime, timedelta

# Replace with your Aladhan API key
api_key = "YOUR_API_KEY"

# Endpoint URLs for Isha (عشاء) and Fajr (الفجر) prayer times in Giza, Egypt
isha_url = f"https://api.aladhan.com/v1/timingsByCity?city=Giza&country=Egypt&method=5&school=1&apikey={api_key}"

# Send a request to get today's prayer times
response = requests.get(isha_url)
data = response.json()
isha_time = data['data']['timings']['Isha']
fajr_time = data['data']['timings']['Fajr']

# Convert Isha and Fajr times to datetime objects
isha_datetime = datetime.strptime(isha_time, '%H:%M')
fajr_datetime = datetime.strptime(fajr_time, '%H:%M')

# Calculate bedtime (after Isha)
bedtime = isha_datetime + timedelta(minutes=30)  # Adjust the hours as needed

# Calculate wake-up time (after Isha)
time_between_isha_fajr = (fajr_datetime - isha_datetime) / 2
wakeup_time = isha_datetime + time_between_isha_fajr

# Print today's date and sleep/wake times
today = datetime.now()
print(f"Bedtime: {bedtime.strftime('%I:%M %p')}")
print(f"Wake-up time: {wakeup_time.strftime('%I:%M %p')}")
