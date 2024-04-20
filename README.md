# dharani portal

```python
import requests
from bs4 import BeautifulSoup

def get_survey_numbers(district, mandal, village):
    url = "https://dharani.telangana.gov.in/knowLandStatus"
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/97.0.4692.99 Safari/537.36"
    }

    payload = {
        "district": district,
        "mandal": mandal,
        "village": village
    }

    response = requests.post(url, headers=headers, data=payload)

    if response.status_code == 200:
        soup = BeautifulSoup(response.text, "html.parser")
        survey_numbers = []
        for option in soup.select("#surveynumber option"):
            survey_numbers.append(option.text.strip())
        return survey_numbers
    else:
        print("Failed to fetch data. Status code:", response.status_code)
        return None

district = "district"
mandal = "mandal"
village = "village"
survey_numbers = get_survey_numbers(district, mandal, village)
if survey_numbers:
    print("Survey Numbers for District:", district, ", Mandal:", mandal, ", Village:", village)
    for num in survey_numbers:
        print(num)
else:
    print("Failed to fetch survey numbers.")
