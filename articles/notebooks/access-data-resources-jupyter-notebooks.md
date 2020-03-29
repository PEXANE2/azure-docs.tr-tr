---
title: Jupyter dizüstü bilgisayarlardaki verilere erişin - Azure Dizüstü Bilgisayarlar Önizleme
description: Jupyter dizüstü bilgisayarından dosyalara, REST API'lerine, veritabanlarına ve farklı Azure Depolama kaynaklarına nasıl erişiriz öğrenin.
ms.topic: how-to
ms.date: 12/04/2018
ms.openlocfilehash: 47d2f869021851c1451a66a84b1a70ec4ff4998f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75646356"
---
# <a name="access-cloud-data-in-a-notebook"></a>Not defterinde bulut verilerine erişme

Jupyter dizüstü bilgisayarında ilginç işler yapmak veri gerektirir. Veri, gerçekten, dizüstü bilgisayarların can damarıdır.

Bir dosyayı doğrudan indirmek için not defterinin `curl` içinden gelen komutları kullanarak bile [veri dosyalarını bir projeye](work-with-project-data-files.md)kesinlikle aktarabilirsiniz. Ancak, REST API'leri, ilişkisel veritabanları ve Azure tabloları gibi bulut depolama gibi dosya dışı kaynaklardan edinilebilen çok daha kapsamlı verilerle çalışmanız gerekebilir.

Bu makalede, bu farklı seçenekler kısaca özetler. Veri erişimi en iyi iş başında görüldüğünden, Azure Not Defterleri Örneklerinde runnable kodu bulabilirsiniz [- Verilerinize erişin.](https://github.com/Microsoft/AzureNotebooks/blob/master/Samples/Access%20your%20data%20in%20Azure%20Notebooks.ipynb)

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

## <a name="rest-apis"></a>REST API'leri

Genel olarak konuşursak, Internet'ten elde edilen büyük miktardaki verilere dosyalar aracılığıyla değil, REST API'leri aracılığıyla erişilir. Neyse ki, bir not defteri hücresi istediğiniz kodu içerebildiği için, istek göndermek ve JSON verileri almak için kodu kullanabilirsiniz. Daha sonra bu JSON'u pandalar veri çerçevesi gibi kullanmak istediğiniz biçime dönüştürebilirsiniz.

REST API kullanarak verilere erişmek için, not defterinin kod hücrelerinde başka bir uygulamada kullandığınız kodu kullanın. İstek kitaplığını kullanan genel yapı aşağıdaki gibidir:

```python
import pandas
import requests

# New York City taxi data for 2014
data_url = 'https://data.cityofnewyork.us/resource/gkne-dk5s.json'

# General data request; include other API keys and credentials as needed in the data argument
response = requests.get(data_url, data={"limit": "20"})

if response.status_code == 200:
    dataframe_rest2 = pandas.DataFrame.from_records(response.json())
    print(dataframe_rest2)
```

## <a name="azure-sql-databases"></a>Azure SQL veritabanları

Sql Server veritabanlarına pyodbc veya pymssql kitaplıkları yardımıyla erişebilirsiniz.

[Bir Azure SQL veritabanını sorgulamak için Python'u kullanın,](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python) Size AdventureWorks verilerini içeren bir veritabanı oluşturma yla ilgili yönergeler verir ve bu verileri nasıl sorgularız gösterir. Aynı kod, bu makalenin örnek not defterinde gösterilir.

## <a name="azure-storage"></a>Azure Storage

Azure Depolama, sahip olduğunuz veri türüne ve bu depolama alanına nasıl erişmeniz gerektiğine bağlı olarak birkaç farklı ilişkisel olmayan depolama alanı sağlar:

- Tablo Depolama: toplanan sensör günlükleri, tanılama günlükleri ve benzeri gibi tablo verileri için düşük maliyetli, yüksek hacimli depolama sağlar.
- Blob depolama: her tür veri için dosya benzeri depolama sağlar.

Örnek not defteri, yalnızca okunur şekilde okunabilir blob'lara erişime izin vermek için paylaşılan erişim imzasının nasıl kullanılacağı da dahil olmak üzere hem tablolarla hem de lekelerle çalışmayı gösterir.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB

Azure Cosmos DB, JSON belgeleri için tam dizinlenmiş bir NoSQL deposu sağlar). Aşağıdaki makaleler Python Cosmos DB ile çalışmak için farklı yollar bir dizi sağlar:

- [Python ile bir SQL API uygulaması oluşturma](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-python)
- [MongoDB için Azure Cosmos DB'nin API'si ile bir Flask uygulaması oluşturun](https://docs.microsoft.com/azure/cosmos-db/create-mongodb-flask)
- [Python ve Gremlin API'sini kullanarak grafik veritabanı oluşturma](https://docs.microsoft.com/azure/cosmos-db/create-graph-python)
- [Python ve Azure Cosmos DB ile Cassandra uygulaması oluşturun](https://docs.microsoft.com/azure/cosmos-db/create-cassandra-python)
- [Python ve Azure Cosmos DB ile Tablo API uygulaması oluşturma](https://docs.microsoft.com/azure/cosmos-db/create-table-python)

Cosmos DB ile çalışırken [azure-cosmosdb-tablo](https://pypi.org/project/azure-cosmosdb-table/) kitaplığını kullanabilirsiniz.

## <a name="other-azure-databases"></a>Diğer Azure veritabanları

Azure, kullanabileceğiniz diğer veritabanı türleri sağlar. Aşağıdaki makaleler Python bu veritabanları erişmek için rehberlik sağlar:

- [PostgreSQL için Azure Veritabanı: Bağlanmak ve veri sorgulamak için Python'ı kullanma](https://docs.microsoft.com/azure/postgresql/connect-python)
- [Hızlı başlangıç: Python ile Azure Redis Cache kullanma](https://docs.microsoft.com/azure/redis-cache/cache-python-get-started)
- [MySQL için Azure Veritabanı: Python'u kullanarak bağlanma ve veri sorgulama](https://docs.microsoft.com/azure/mysql/connect-python)
- [Azure Data Factory](https://azure.microsoft.com/services/data-factory/)
  - [Azure Veri Fabrikası için Kopyalama Sihirbazı](https://azure.microsoft.com/updates/code-free-copy-wizard-for-azure-data-factory/)

## <a name="next-steps"></a>Sonraki adımlar

- [Nasıl kullanılır: Proje veri dosyalarıyla çalışma](work-with-project-data-files.md)
