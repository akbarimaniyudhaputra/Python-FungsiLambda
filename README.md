# Python - Fungsi Lambda

## Mendata/merekap jumlah setiap jenis/kategori/series dari produk Xiaomi
 - Mengambil Data di File Excel dan menampilkan dalam bentuk DataFrame
 - Fungsi list() berguna untuk menjadikan data berbentuk list
 - Fungsi filter() dengan fungsi lambda serta parameternya (lambda x: x[:6] == "Xiaomi", produk) artinya tampilkan data yang 6 digit pertamanya ada kata Xiaomi yang ada di variabel produk
 - Fungsi len() untuk mengetahui jumlah data dari suatu variabel  
 - Mendata/merekap jumlah setiap jenis/kategori/series dari produk Xiaomi, menggunakan dictionary dengan jenis/kategori/series sebagai kata kuncinya (key) 
 - Kategori dapat diketahui dari digit/indeks ke 7 sampai dengan digit/indeks ke 12 

```http
import pandas as pd
from pandas import ExcelFile

namaFile = "E:\dataset\smartphone.xlsx"
data = pd.read_excel(namaFile, sheet_name="Sheet1")

data
#data[:10] #menampilkan 10 baris teratas
```
![1 1](https://user-images.githubusercontent.com/86678205/155518267-c93631e2-c687-4e7b-ab41-702419744776.PNG)

```http
#gaboleh ada cell yg kosong
produk = list(data["nama produk"])
produkMi = list(filter(lambda x: x[:6] == "Xiaomi", produk))

produkMi
```
![1 2](https://user-images.githubusercontent.com/86678205/155518290-3dc7ff83-af89-497b-b24f-29e796dfd790.PNG)

```http
len(produkMi)
```
![1 3](https://user-images.githubusercontent.com/86678205/155518303-2475ae89-6501-47b7-b0aa-10723f119d75.PNG)

```http
# membuat sebuah dictionary awal
rekap = {}

for i in range(len(produkMi)):
    if(produkMi[i][7:12] in rekap):
        rekap[produkMi[i][7:12]] += 1
    else:
        rekap[produkMi[i][7:12]] = 1
print(rekap)
```
![1 4](https://user-images.githubusercontent.com/86678205/155518316-aa313d20-c33a-481d-a2b3-e5f49fdd021a.PNG)

## Mendata/merekap 10 produk dengan keuntungan kecil & produk dengan keuntungan besar 

#### Mengambil Data di File Excel dan menampilkan dalam bentuk DataFrame
```http
import pandas as pd
from pandas import ExcelFile

namaFile = "E:\dataset\smartphone.xlsx"
data = pd.read_excel(namaFile, sheet_name="Sheet1")
data
```
![2 1](https://user-images.githubusercontent.com/86678205/155518374-b69a7f4c-4de4-48f5-873b-10be21c87cc6.PNG)

#### Menghilangkan & Mengubah beberapa karakter dengan fungsi replace()
```http
data["terjual"] = data["terjual"].astype(str) #ubah type data
data["terjual"] = [x.replace("rb","00") for x in data["terjual"]] #mengubah rb mrnjadi 00
data["terjual"] = [x.replace("+","") for x in data["terjual"]] #menghilangkan +
data["terjual"] = [x.replace(",","") for x in data["terjual"]] #menghilangkan ,
data
```
![2 2](https://user-images.githubusercontent.com/86678205/155518393-f01c590e-5619-4771-8958-b80ccba74593.PNG)

#### Mengubah tipe data kolom & mengalikan kolom 
```http
data["terjual"] = data["terjual"].astype(float) #ubah type data
data["harga"] = data["harga"].astype(float) #ubah type data
keuntungan = data["harga"] * data["terjual"]
keuntungan
```
![2 3](https://user-images.githubusercontent.com/86678205/155518523-7679744c-cdac-4d70-94ef-e67d04237c2d.PNG)

#### Menambah kolom ke DataFrame 
```http
#Menambah kolom 
data["keuntungan"] = keuntungan
data
```
![2 4](https://user-images.githubusercontent.com/86678205/155518531-99a84100-e103-4c22-a275-a6501b4f8a59.PNG)

#### Menampilkan beberapa kolom/field saja dengan fungsi filter()
```http
#fungsi .filter()
#hanya menampilkan beberapa kolom/field
from pandas import DataFrame
data = data.filter(items=["nama produk", "harga", "terjual", "keuntungan"])
data
```
![2 5](https://user-images.githubusercontent.com/86678205/155518543-fb540385-43c2-490b-9665-8e651b219e82.PNG)

#### Mengurutkan data di kolom dari besar ke kecil atau secara descending
```http
# fungsi .sort_values()
# mengurutkan kolom keuntungan dari besar ke kecil
data.sort_values("keuntungan", axis=0, ascending=False, inplace=True)
data
```
![2 6](https://user-images.githubusercontent.com/86678205/155518555-6cb07434-709e-4e1d-9fb9-e3eb0c82aa92.PNG)

#### Mengubah bentuk data dari DataFrame ke bentuk data list dengan fungsi tolist()
```http
# fungsi tolist()
# menjadikan data berbentuk list
dataList = data.values.tolist()
dataList
```
![2 7](https://user-images.githubusercontent.com/86678205/155518565-0958ee05-737d-4298-a35f-d37838cc0913.PNG)

#### memfilter kolom/filed dengan parameter tertentu menggunakan fungsi filter() & lambda (menampilkan data dengan keuntungan <= 4.509590e+08)
```http
# fungsi filter() & lambda
# memfilter kolom/filed keuntungan kurang dari sama dengan 4.509590e+08 tampilkan
data_UntungKecil = list(filter(lambda x: x[3] <= 4.509590e+08, dataList))
data_UntungKecil

#data = pd.DataFrame(data_UntungKecil)
#data
```
![2 8](https://user-images.githubusercontent.com/86678205/155518584-7d59a36b-cc1f-4bf8-a250-a391a24dd311.PNG)

#### memfilter kolom/filed dengan parameter tertentu menggunakan fungsi filter() & lambda (menampilkan data dengan keuntungan > 4.509590e+08)
```http
# fungsi filter() & lambda
# memfilter kolom/filed keuntungan lebih dari 4.509590e+08 tampilkan
data_UntungBesar = list(filter(lambda x: x[3] > 4.509590e+08, dataList))
data_UntungBesar

#data = pd.DataFrame(data_UntungBesar)
#data
```
![2 9](https://user-images.githubusercontent.com/86678205/155518597-93740efc-cee8-432c-8313-246adad84c1e.PNG)

#### mendata/merekap 10 produk dengan keuntungan terkecil
```http
# membuat sebuah dictionary awal
total_UntungKecil = {}

for i in range(len(data_UntungKecil)):
    if data_UntungKecil[i][0] in total_UntungKecil: 
        total_UntungKecil[data_UntungKecil[i][0]] += data_UntungKecil[i][3] #menampilkan data_UntungKecil indeks-0 (kolom/field nama_produk) & indeks-3 (kolom/field keuntungan)
    else:
        total_UntungKecil[data_UntungKecil[i][0]] = data_UntungKecil[i][3] #menampilkan data_UntungKecil indeks-0 (kolom/field nama_produk) & indeks-3 (kolom/field keuntungan)
total_UntungKecil
```
![2 10](https://user-images.githubusercontent.com/86678205/155518618-5bde6602-7268-4125-95d1-540884063947.PNG)

#### mensortir data berdasarkan keuntungan terkecil
```http
#sorting data berdasarkan total_UntungKecil key=itemgetter(1)
#untuk melihat produk dengan keuntungan terkecil

from operator import itemgetter
sorted_total_UntungKecil = sorted(total_UntungKecil.items(), key=itemgetter(1), reverse=False)
sorted_total_UntungKecil

#data = pd.DataFrame(sorted_total_UntungKecil)
#data
```
![2 11](https://user-images.githubusercontent.com/86678205/155518632-df967ba9-2624-4088-8e62-59a2c73cbb3f.PNG)

#### mendata/merekap produk dengan keuntungan besar
```http
# membuat sebuah dictionary awal
total_UntungBesar = {}

for i in range(len(data_UntungBesar)):
    if data_UntungBesar[i][0] in total_UntungBesar: 
        total_UntungBesar[data_UntungBesar[i][0]] += data_UntungBesar[i][3] #menampilkan data_UntungBesar indeks-0 (kolom/field nama_produk) & indeks-3 (kolom/field keuntungan)
    else:
        total_UntungBesar[data_UntungBesar[i][0]] = data_UntungBesar[i][3] #menampilkan data_UntungBesar indeks-0 (kolom/field nama_produk) & indeks-3 (kolom/field keuntungan)
total_UntungBesar
```
![2 12](https://user-images.githubusercontent.com/86678205/155518664-e1dba0ae-a65e-46f7-9583-de132ec1d787.PNG)

#### mensortir data berdasarkan keuntungan terbesar
```http
#sorting data berdasarkan total_UntungBesar key=itemgetter(1)
#untuk melihat produk dengan keuntungan terbesar

from operator import itemgetter
sorted_total_UntungBesar = sorted(total_UntungBesar.items(), key=itemgetter(1), reverse=True)
sorted_total_UntungBesar

#data = pd.DataFrame(sorted_total_UntungBesar)
#data
```
![2 13](https://user-images.githubusercontent.com/86678205/155518703-dee3ce3e-0455-41b0-8d06-d1f8bc520a51.PNG)

#### memfilter kolom/filed dengan parameter tertentu menggunakan fungsi filter() & lambda (membaca 6 digit jika Xiaomi maka tampilkan)
```http
# fungsi filter() & lambda
# memfilter kolom/filed nama produk dengan membaca 6 digit jika Xiaomi maka tampilkan (yang ada di dataList yang memenuhi)

data_Xiaomi = list(filter(lambda x: x[0][:6]=="Xiaomi", dataList))
data_Xiaomi

#data = pd.DataFrame(data_Xiaomi)
#data
```
![2 14](https://user-images.githubusercontent.com/86678205/155518722-123eaa04-be42-40ce-bbaf-3bac738b8fa8.PNG)

#### memfilter kolom/filed dengan parameter tertentu menggunakan fungsi filter() & lambda (jika terjual lebih dari 1000 maka tampilkan)
```http
# fungsi filter() & lambda
# memfilter kolom/filed terjual jika terjual lebih dari 1000 maka tampilkan (yang ada di data_Xiaomi yang memenuhi)

Xiaomi_UntungBesar = list(filter(lambda x: x[2] > 1000, data_Xiaomi))
Xiaomi_UntungBesar
#Xiaomi_UntungBesar[:5]

#data = pd.DataFrame(Xiaomi_UntungBesar)
#data
```
![2 15](https://user-images.githubusercontent.com/86678205/155518747-bf326d5f-0f09-4d2f-b2d4-2a4884def741.PNG)

#### mendata/merekap produk Xiaomi dengan keuntungan besar
```http
# membuat sebuah dictionary awal
total_Xiaomi_UntungBesar = {}

for i in range(len(Xiaomi_UntungBesar)): 
    if Xiaomi_UntungBesar[i][0] in total_Xiaomi_UntungBesar:
        total_Xiaomi_UntungBesar[Xiaomi_UntungBesar[i][0]] += Xiaomi_UntungBesar[i][3] #menampilkan Xiaomi_UntungBesar indeks-0 (kolom/field nama_produk) & indeks-3 (kolom/field keuntungan)
    else: 
        total_Xiaomi_UntungBesar[Xiaomi_UntungBesar[i][0]] = Xiaomi_UntungBesar[i][3] #menampilkan Xiaomi_UntungBesar indeks-0 (kolom/field nama_produk) & indeks-3 (kolom/field keuntungan)
```
![2 18](https://user-images.githubusercontent.com/86678205/155518773-16b1f123-85d1-4cee-9e82-3bb105db5adb.PNG)

#### mensortir data produk Xiaomi berdasarkan keuntungan terbesar
```http
#sorting data berdasarkan total_Xiaomi_UntungBesar key=itemgetter(1)
#untuk melihat produk Xiaomi dengan keuntungan terbesar

from operator import itemgetter
sorted_total_Xiaomi_UntungBesar = sorted(total_Xiaomi_UntungBesar.items(), key=itemgetter(1), reverse=True)
sorted_total_Xiaomi_UntungBesar[:5]

#data = pd.DataFrame(sorted_total_Xiaomi_UntungBesar)
#data
```
![2 19](https://user-images.githubusercontent.com/86678205/155518795-5bad6d05-0214-40ef-beca-c9f69de16ea0.PNG)

#### memfilter kolom/filed dengan parameter tertentu menggunakan fungsi filter() & lambda (jika terjual <= 1000 maka tampilkan)
```http
# fungsi filter() & lambda
# memfilter kolom/filed terjual jika terjual kurang dari sama dengan 1000 maka tampilkan (yang ada di data_Xiaomi yang memenuhi)

Xiaomi_UntungKecil = list(filter(lambda x: x[2] <= 1000, data_Xiaomi))
Xiaomi_UntungKecil
#Xiaomi_UntungKecil[:5]

#data = pd.DataFrame(Xiaomi_UntungKecil)
#data
```
![2 20](https://user-images.githubusercontent.com/86678205/155518853-ba6a7cae-eb65-4e2a-a0fc-b592a73050ee.PNG)

#### mendata/merekap produk Xiaomi dengan keuntungan terkecil
```http
# membuat sebuah dictionary awal
total_Xiaomi_UntungKecil = {}

for i in range(len(Xiaomi_UntungKecil)):
    if Xiaomi_UntungKecil[i][0] in total_Xiaomi_UntungKecil: 
        total_Xiaomi_UntungKecil[Xiaomi_UntungKecil[i][0]] += Xiaomi_UntungKecil[i][3] #menampilkan Xiaomi_UntungKecil indeks-0 (kolom/field nama_produk) & indeks-3 (kolom/field keuntungan)
    else:
        total_Xiaomi_UntungKecil[Xiaomi_UntungKecil[i][0]] = Xiaomi_UntungKecil[i][3] #menampilkan Xiaomi_UntungKecil indeks-0 (kolom/field nama_produk) & indeks-3 (kolom/field keuntungan)
```
![2 21](https://user-images.githubusercontent.com/86678205/155518864-29849acd-184d-41e6-ac62-f158fd03babe.PNG)

#### mensortir data produk Xiaomi berdasarkan keuntungan terkecil
```http
#sorting data berdasarkan total_Xiaomi_UntungKecil key=itemgetter(1)
#untuk melihat produk Xiaomi dengan keuntungan terkecil

from operator import itemgetter
sorted_total_Xiaomi_UntungKecil = sorted(total_Xiaomi_UntungKecil.items(), key=itemgetter(1), reverse=False)
sorted_total_Xiaomi_UntungKecil[:5]

#data = pd.DataFrame(sorted_total_Xiaomi_UntungKecil)
#data
```
![2 22](https://user-images.githubusercontent.com/86678205/155518884-0dda808f-007c-4287-8f65-ba81a8c88b61.PNG)
