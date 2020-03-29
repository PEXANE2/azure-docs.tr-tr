---
title: Pandalarla Azure blob depolamasındaki verileri keşfedin - Ekip Veri Bilimi Süreci
description: Pandalar Python paketini kullanarak Azure blob konteynerinde depolanan verileri nasıl keşfedebilirsiniz?
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 29011760a94a05020150ceddeba4303b87c2f610
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76722195"
---
# <a name="explore-data-in-azure-blob-storage-with-pandas"></a>Pandalarla Azure blob depolamasındaki verileri keşfedin

Bu makalede, [pandalar](https://pandas.pydata.org/) Python paketi kullanılarak Azure blob kapsayıcısında depolanan verilerin nasıl keşfedileni kapsar.

Bu [görev, Ekip Veri Bilimi Sürecinde](overview.md)bir adımdır.

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, sahip olduğunuzu varsayar:

* Bir Azure depolama hesabı oluşturdu. Yönergelere ihtiyacınız [varsa,](../../storage/common/storage-account-create.md) bkz.
* Verilerinizi bir Azure blob depolama hesabında depolayın. Yönergelere ihtiyacınız varsa, [bkz.](../../storage/common/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>Verileri pandalara yükleme DataFrame
Bir veri kümesini keşfetmek ve işlemek için, önce blob kaynağından yerel bir dosyaya indirilmesi gerekir ve bu dosya daha sonra pandadataframe'e yüklenebilir. Bu yordam için izlenir adımlar şunlardır:

1. Blob hizmetini kullanarak aşağıdaki Python kod örneğiyle azure blob'undan verileri indirin. Aşağıdaki koddaki değişkeni belirli değerlerinizle değiştirin:

```python
from azure.storage.blob import BlockBlobService
import tables

STORAGEACCOUNTNAME= <storage_account_name>
STORAGEACCOUNTKEY= <storage_account_key>
LOCALFILENAME= <local_file_name>
CONTAINERNAME= <container_name>
BLOBNAME= <blob_name>

#download from blob
t1=time.time()
blob_service=BlockBlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
t2=time.time()
print(("It takes %s seconds to download "+blobname) % (t2 - t1))
```

1. İndirilen dosyadan verileri pandadataframe olarak okuyun.

```python
# LOCALFILE is the file path
dataframe_blobdata = pd.read_csv(LOCALFILE)
```

Artık verileri keşfetmeye ve bu veri setinde özellikler oluşturmaya hazırsınız.

## <a name="examples-of-data-exploration-using-pandas"></a><a name="blob-dataexploration"></a>Pandalar kullanarak veri arama örnekleri
Pandaları kullanarak verileri keşfetmenin birkaç yolu aşağıda verilmiştir:

1. Satır **ve sütun sayısını** denetleme

```python
print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
```

1. Aşağıdaki veri kümesindeki ilk veya son birkaç **satırı** **inceleyin:**

```python
dataframe_blobdata.head(10)

dataframe_blobdata.tail(10)
```

1. Her sütunun aşağıdaki örnek kodu kullanarak içe aktarılyış edildiği **veri türünü** denetleyin

```python
for col in dataframe_blobdata.columns:
    print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
```

1. Veri kümesindeki **sütunların temel istatistiklerini** aşağıdaki gibi denetleyin

```python
dataframe_blobdata.describe()
```

1. Her sütun değeri için giriş sayısına aşağıdaki gibi bakın

```python
dataframe_blobdata['<column_name>'].value_counts()
```

1. Aşağıdaki örnek kodu kullanarak her sütundaki gerçek giriş sayısına karşılık **eksik değerleri sayma**

```python
miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
print miss_num
```

1. Verilerde belirli bir sütun için **eksik değerleriniz** varsa, bunları aşağıdaki gibi bırakabilirsiniz:

```python
dataframe_blobdata_noNA = dataframe_blobdata.dropna()
dataframe_blobdata_noNA.shape
```

Eksik değerleri değiştirmenin başka bir yolu mod işlevidir:

```python
dataframe_blobdata_mode = dataframe_blobdata.fillna(
    {'<column_name>': dataframe_blobdata['<column_name>'].mode()[0]})
```

1. Bir değişkenin dağılımını çizmek için değişken sayıda depo kutusu kullanarak bir **histogram** çizimi oluşturma

```python
dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')

np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
```

1. Bir dağılım çizimi kullanarak veya yerleşik korelasyon işlevini kullanarak değişkenler arasındaki **korelasyonlara** bakın

```python
# relationship between column_a and column_b using scatter plot
plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])

# correlation between column_a and column_b
dataframe_blobdata[['<column_a>', '<column_b>']].corr()
```
