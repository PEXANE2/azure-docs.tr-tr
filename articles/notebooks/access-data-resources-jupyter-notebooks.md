---
title: Azure üzerinde Jupyter Notebook erişim veri kaynakları
description: Jupyter not defteri erişimi dosyalara, REST API'leri, veritabanları ve farklı Azure depolama kaynaklarını nasıl.
ms.topic: article
ms.date: 12/04/2018
ms.openlocfilehash: 23acdf99f6cb69f100e484e236580f3b2b43ba94
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277692"
---
# <a name="access-cloud-data-in-a-notebook"></a>Bir not defteri bulut verilerine erişim

İlginç bir Jupyter not defteri iş verileri gerektiriyor. Aslında, dizüstü bilgisayarlar, lifeblood verilerdir.

Bir dosyayı doğrudan indirmek için bir not defteri içinden `curl` gibi komutları kullanarak [bir projeye tamamen veri dosyalarını içeri aktarabilirsiniz](work-with-project-data-files.md). Ancak, REST API'ler, ilişkisel veritabanları gibi dosya olmayan kaynaklardan kullanılabilir daha kapsamlı verilerle çalışın ve bulut depolama gibi Azure tabloları gerektiğini, olasıdır.

Bu makalede, bu farklı seçenekler kısaca özetler. Veri erişiminin en iyi şekilde görüldüğü için, Azure Notebooks örneklerimizde çalıştırılabilir kodu bulabilirsiniz [-verilerinize erişebilirsiniz](https://github.com/Microsoft/AzureNotebooks/blob/master/Samples/Access%20your%20data%20in%20Azure%20Notebooks.ipynb).

## <a name="rest-apis"></a>REST API'leri

Genel olarak bakıldığında, yüksek miktarda veri Internet'ten kullanılabilir dosyaları üzerinden değil, ancak REST API'leri aracılığıyla erişilir. Neyse ki, bir not defteri hücre istediğiniz herhangi bir kod içerebileceğinden, JSON veri istekleri göndermek ve almak için kod kullanabilirsiniz. Ardından, JSON, bir pandas dataframe gibi kullanmak istediğiniz her biçimine dönüştürebilirsiniz.

REST API kullanarak verilere erişmek için başka bir uygulamada kullandığınız bir not defterinin kod hücreleri aynı kodu kullanın. İstekleri kitaplığını kullanarak genel yapısı şu şekildedir:

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

SQL Server veritabanlarını pyodbc veya pymssql kitaplıkları yardıma erişebilir.

[Azure SQL veritabanını sorgulamak Için Python 'U kullanarak](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python) AdventureWorks verileri içeren bir veritabanı oluşturma hakkında yönergeler sağlar ve bu verilerin nasıl sorgulanalınacağını gösterir. Bu makalede örnek not defterinde aynı kod gösterilmektedir.

## <a name="azure-storage"></a>Azure Storage

Azure depolama, ilişkisel olmayan depolama, sahip olduğunuz veriler ve nasıl erişim sağlaması gereken türüne bağlı olarak birkaç farklı türde sağlar:

- Tablo depolama: toplanan algılayıcı günlükler, tanılama günlükleri ve benzeri gibi tablo biçimindeki veriler için düşük maliyetli, yüksek hacimli depolama sağlar.
- BLOB Depolama: her türden veriyi dosya benzeri depolama sağlar.

Örnek Not Defteri hem tabloları ve bloblar için salt okunur erişime izin vermek için paylaşılan erişim imzası kullanma dahil olmak üzere, bloblar ile çalışma gösterir.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB

Azure Cosmos DB tamamen dizinli bir NoSQL deposu için JSON belgelerini sağlar). Aşağıdaki makaleler birkaç python'dan Cosmos DB ile çalışmak için farklı yollar sunar:

- [Python ile SQL API uygulaması derleme](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-python)
- [MongoDB için Azure Cosmos DB API 'SI ile bir Flask uygulaması oluşturun](https://docs.microsoft.com/azure/cosmos-db/create-mongodb-flask)
- [Python ve Gremlin API kullanarak bir grafik veritabanı oluşturma](https://docs.microsoft.com/azure/cosmos-db/create-graph-python)
- [Python ve Azure Cosmos DB ile Cassandra uygulaması derleme](https://docs.microsoft.com/azure/cosmos-db/create-cassandra-python)
- [Python ve Azure Cosmos DB Tablo API'si uygulama oluşturma](https://docs.microsoft.com/azure/cosmos-db/create-table-python)

Cosmos DB ile çalışırken, [Azure-cosmosdb-Table](https://pypi.org/project/azure-cosmosdb-table/) kitaplığını kullanabilirsiniz.

## <a name="other-azure-databases"></a>Diğer Azure veritabanları

Azure, bir dizi kullanabileceğiniz diğer veritabanı türleri sağlar. Aşağıdaki makaleleri veritabanlarınızdaki Python'dan erişmeye yönelik rehberlik sağlar:

- [PostgreSQL için Azure veritabanı: bağlanmak ve veri sorgulamak için Python kullanma](https://docs.microsoft.com/azure/postgresql/connect-python)
- [Hızlı başlangıç: Python ile Azure Redis Cache kullanma](https://docs.microsoft.com/azure/redis-cache/cache-python-get-started)
- [MySQL için Azure veritabanı: bağlanmak ve veri sorgulamak için Python kullanma](https://docs.microsoft.com/azure/mysql/connect-python)
- [Azure Data Factory](https://azure.microsoft.com/services/data-factory/)
  - [Azure Data Factory için kopyalama Sihirbazı](https://azure.microsoft.com/updates/code-free-copy-wizard-for-azure-data-factory/)

## <a name="next-steps"></a>Sonraki adımlar

- [Nasıl yapılır: proje veri dosyalarıyla çalışma](work-with-project-data-files.md)
