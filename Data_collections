import requests
import json

# Define API Key and API Endpoint
api_key = "iNeP9NbDXJibWdAzNDyuGFzq62bwtF3k"
endpoint = "https://api.nytimes.com/svc/archive/v1/{}/{}.json"

# Define function to extract titles from NY Times Archive API for a given date range
def extract_titles(start_date, end_date, output_file):
    # Build API URL with the given start date end date and API Key
    url = endpoint.format(start_date[:4], start_date[5:7], api_key)

    # Define query parameters
    params = {
        "begin_date": start_date,
        "end_date": end_date,
        "api-key": api_key
    }

    # Make request to API and get the response
    response = requests.get(url, params=params)

    # Check if request was successful
    if response.status_code != 200:
        print("Error occurred: status code {}".format(response.status_code))
        return

    # Parse response data from JSON format into a Python dictionary
    data = json.loads(response.text)

    # Get the number of search results returned by the API
    num_results = data["response"]["meta"]["hits"]

    # Get the titles of articles returned by the API that were published within the given month and year
    titles = []
    for article in data["response"]["docs"]:
        pub_date = article["pub_date"][:10]
        if start_date <= pub_date <= end_date:
            title = article["headline"]["main"].strip()
            titles.append(title)

    # Write the titles to the output file
    with open(output_file, "w") as f:
        f.write("\n".join(titles))

    # Print a status message to indicate how many titles were extracted and where they were saved
    print("Extracted {} titles and wrote them to {}".format(num_results, output_file))

# Define dates for October 1918 and October 2020
start_date_1918 = "1918-10-01"
end_date_1918 = "1918-10-31"

start_date_2020 = "2020-10-01"
end_date_2020 = "2020-10-31"

# Extract titles from October 1918 and save them to a file
extract_titles(start_date_1918, end_date_1918, "titles_1918.txt")

# Extract titles from October 2020 and save them to a file
extract_titles(start_date_2020, end_date_2020, "titles_2020.txt")
