### Code used for data acquisition
##### Link to get your own API KEY :  <http://code.google.com/apis/console>
##### Link to get your own cx :  <http://programmablesearchengine.google.com/controlpanel/all>
You will need to create your search engine using above link, make sure to enable image search, **IT IF OFF BY DEFAULT**, then copy the search engine ID and use it in cx section of the res().execute()
```
from googleapiclient.discovery import build
import requests

def main():
    # Build a service object for interacting with the API. Visit
    # the Google APIs Console <http://code.google.com/apis/console>
    # to get an API key for your own application.
    service = build(
        "customsearch", "v1", developerKey="YOUR_API_KEY_HERE"
    )
    productLst = ['bathtub','shower pan','bathroom mirror','shower faucet','bathroom faucet','vanity tops','bathroom sinks','shower door','medicine cabinet','bathroom toilet']
    f = open("url_file2", 'w')
    for product in productLst:
        for i in range(0, 100, 10):
            res = (
                service.cse()
                .list(
                    q=product,
                    cx="YOUR_OWN_SEARCH_ENGINE_ID_HERE",
                    imgSize="LARGE",
                    searchType="image",
                    start=i
                )
                .execute()
            )
            for item in res['items']:
                f.write(item['link'])
                f.write('\n')
                # print(item['link'])
    f.close()
    f = open("url_file2", 'r')
    lines = f.readlines()
    url = []
    for line in lines:
        url.append(line.strip())
    for img in url:
        # this should be file_name
        file_name = img.split('/')[-1]
        print("Downloading file:%s" % file_name)
        try:
            r = requests.get(img, stream=True, timeout=5)
            with open(file_name, 'wb') as f:
                for chunk in r:
                    f.write(chunk)
        except:
            pass
    f.close()
if __name__ == "__main__":
    main()
```
