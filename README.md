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

```http
#gaboleh ada cell yg kosong
produk = list(data["nama produk"])
produkMi = list(filter(lambda x: x[:6] == "Xiaomi", produk))

produkMi
```

```http
len(produkMi)
```

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

## Mendata/merekap 10 produk dengan keuntungan kecil & produk dengan keuntungan besar 

#### Mengambil Data di File Excel dan menampilkan dalam bentuk DataFrame
```http
import pandas as pd
from pandas import ExcelFile

namaFile = "E:\dataset\smartphone.xlsx"
data = pd.read_excel(namaFile, sheet_name="Sheet1")
data
```

#### Menghilangkan & Mengubah beberapa karakter dengan fungsi replace()
```http
data["terjual"] = data["terjual"].astype(str) #ubah type data
data["terjual"] = [x.replace("rb","00") for x in data["terjual"]] #mengubah rb mrnjadi 00
data["terjual"] = [x.replace("+","") for x in data["terjual"]] #menghilangkan +
data["terjual"] = [x.replace(",","") for x in data["terjual"]] #menghilangkan ,
data
```

#### Mengubah tipe data kolom & mengalikan kolom 
```http
data["terjual"] = data["terjual"].astype(float) #ubah type data
data["harga"] = data["harga"].astype(float) #ubah type data
keuntungan = data["harga"] * data["terjual"]
keuntungan
```

#### Menambah kolom ke DataFrame 
```http
#Menambah kolom 
data["keuntungan"] = keuntungan
data
```

#### Menampilkan beberapa kolom/field saja dengan fungsi filter()
```http
#fungsi .filter()
#hanya menampilkan beberapa kolom/field
from pandas import DataFrame
data = data.filter(items=["nama produk", "harga", "terjual", "keuntungan"])
data
```

#### Mengurutkan data di kolom dari besar ke kecil atau secara descending
```http
# fungsi .sort_values()
# mengurutkan kolom keuntungan dari besar ke kecil
data.sort_values("keuntungan", axis=0, ascending=False, inplace=True)
data
```

#### Mengubah bentuk data dari DataFrame ke bentuk data list dengan fungsi tolist()
```http
# fungsi tolist()
# menjadikan data berbentuk list
dataList = data.values.tolist()
dataList
```

#### memfilter kolom/filed dengan parameter tertentu menggunakan fungsi filter() & lambda (menampilkan data dengan keuntungan <= 4.509590e+08)
```http
# fungsi filter() & lambda
# memfilter kolom/filed keuntungan kurang dari sama dengan 4.509590e+08 tampilkan
data_UntungKecil = list(filter(lambda x: x[3] <= 4.509590e+08, dataList))
data_UntungKecil

#data = pd.DataFrame(data_UntungKecil)
#data
```

#### memfilter kolom/filed dengan parameter tertentu menggunakan fungsi filter() & lambda (menampilkan data dengan keuntungan > 4.509590e+08)
```http
# fungsi filter() & lambda
# memfilter kolom/filed keuntungan lebih dari 4.509590e+08 tampilkan
data_UntungBesar = list(filter(lambda x: x[3] > 4.509590e+08, dataList))
data_UntungBesar

#data = pd.DataFrame(data_UntungBesar)
#data
```

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

#### memfilter kolom/filed dengan parameter tertentu menggunakan fungsi filter() & lambda (membaca 6 digit jika Xiaomi maka tampilkan)
```http
# fungsi filter() & lambda
# memfilter kolom/filed nama produk dengan membaca 6 digit jika Xiaomi maka tampilkan (yang ada di dataList yang memenuhi)

data_Xiaomi = list(filter(lambda x: x[0][:6]=="Xiaomi", dataList))
data_Xiaomi

#data = pd.DataFrame(data_Xiaomi)
#data
```

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



