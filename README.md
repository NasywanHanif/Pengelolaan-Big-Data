# Pengelolaan-Big-Data

====TAHAPAN PEMBUATAN CONTAINER, CRAWLING DATA DAN PENYIMPANAN HASIL KE CONTAINER====

POINT 1: MEMBUAT CONTAINER
1. Pull Image MongoDB
docker pull mongo:latest

2. Run Image MongoDB di docker
docker run -d --name bigdata_mongodb -p 27017:27017 mongo 

3. Mengecek apakah Container sudah berjalan 
docker ps

4. Menghubungkan MongoDB ke Container Docker
docker exec –it #kodecontainer


POINT 2 : CRAWLING DATA

!pip install serpapi
!pip install pandas
!pip install google-search-results

import pandas as pd
from serpapi import GoogleSearch

# Define parameters for the Google Maps API query
params = {
    "engine": "google_maps",
    "q": "McD",
    "ll": "@-6.905977,107.613144,15.1z",
    "type": "search",
    "api_key": "(diisi dengan api key)"
}

# Perform the Google Maps API query
search = GoogleSearch(params)
results = search.get_dict()
local_results = results["local_results"]

# Convert the results to a Pandas DataFrame
df = pd.DataFrame(local_results)

# Rename columns if needed
df = df.rename(columns={
    "name": "Name",
    "address": "Address",
    "phone": "Phone",
    "rating": "Rating"
})
print (df)

# Save the DataFrame to a CSV file
csv_filename = "pizza_places.csv"
df.to_csv(csv_filename, index=False)


POINT 3: MEMASUKAN HASIL CRAWLING DATA KE CONTAINER PADA DOCKER
1. Karena kita akan mengimport file json maka pastikan kondisi file sudah sesuai:
2. Cek docker container yang aktif
docker ps
3. Menghubungkan MongoDB ke Container Docker
docker exec -it (ID Container yang dibuat) bash
mongosh
show databases
use bigdata_Mcd
show collections
exit
4. Setelah terhubung, maka lakukan import file json yang telah di aambil dari proses crawling pada google collab:
cd tmp && ls -1
mongoimport --db bigdata_Mcd --collection tugas1_bigdata --file Mcd_BigData.json --jsonArray
5. Lakukan pengecekan 
exit dahulu 
lakukan pada database bigdata_Mcd lakukan pengecekan 
db.tugas1_bigdata.find()

====Selesai====

