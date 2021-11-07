# RihalChallenge


#### First we OCR the image

import pytesseract
from PIL import Image
pytesseract.pytesseract.tesseract_cmd = "C:\Program Files\Tesseract-OCR\\tesseract.exe" 


path = r'C:\Users\*********\Rihal OCR Challange\image.png'

img = Image.open(path)
display(img)

text = pytesseract.image_to_string(Image.open(path))
print(text)

#### Next, we create two dataframes. One for Rooms and the other for Individuals:

import pandas as pd
rooms = pd.DataFrame(columns=['Room Name', 'Date Available', 'Rate'])
indv = pd.DataFrame(columns=['First Name','Last Name', 'Email Address'])



#### Find emails and add it to the individuals dataframe:
import re

email_pattern = '<(.*?)>'
email = re.findall(email_pattern, text)
print(email)
indv['Email Address'] = email
print(indv)

#### Find first and last names and add it to the dataframe:

firstN_pattern = ', (.*?) <'

f_name = re.findall(firstN_pattern, text)
print(f_name)
indv['First Name'] = f_name
print(indv)


lastN_pattern = ': (A.*?),|; (.*?),'

l_name = ["".join(x) for x in re.findall(lastN_pattern, text)]
print(l_name)
indv['Last Name'] = l_name
print(indv)


#### Find Rooms Names, Dates available and Rates and add it to Rooms dataframe:


room_pattern = 'Room: (.*?)[.,]'
room = re.findall(room_pattern, text)
print(room)
rooms['Room Name'] = room
print(rooms)

date_pattern = 'on (.*?)\. '
date = re.findall(date_pattern, text)
print(date)
rooms['Date Available'] = date
print(rooms)


rate_pattern = '\$\d+'
rate = re.findall(rate_pattern, text)
print(rate)
rooms['Rate'] = rate
print(rooms)



### Finaly this is our output:

print(rooms,'\n', indv)
