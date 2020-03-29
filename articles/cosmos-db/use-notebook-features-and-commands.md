---
title: Azure Cosmos DB'de yerleşik not defteri komutlarını ve özelliklerini kullanma (önizleme)
description: Azure Cosmos DB'nin yerleşik dizüstü bilgisayarlarını kullanarak ortak işlemleri yapmak için yerleşik komutları ve özellikleri nasıl kullanacağınızı öğrenin.
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/07/2019
ms.author: dech
ms.openlocfilehash: 61d46bbf0ccdeb5cd2e95e36e19f1aa81cfeeb48
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76513408"
---
# <a name="use-built-in-notebook-commands-and-features-in-azure-cosmos-db-preview"></a>Azure Cosmos DB'de yerleşik not defteri komutlarını ve özelliklerini kullanma (önizleme)

Azure Cosmos DB'deki yerleşik Jupyter dizüstü bilgisayarlar, Verilerinizi Azure portalından analiz etmenizi ve görselleştirmenizi sağlar. Bu makale, ortak işlemleri gerçekleştirmek için yerleşik not defteri komutlarını kullanmayı açıklar.

## <a name="install-a-new-package"></a>Yeni bir paket yükleme
Azure Cosmos hesaplarınız için not defteri desteğini etkinleştirdikten sonra yeni bir not defteri açabilir ve bir paket yükleyebilirsiniz.

Yeni bir kod hücresinde, istediğiniz Python paketiyle ``PackageToBeInstalled`` değiştirerek aşağıdaki kodu ekleyin ve çalıştırın.
```python
import sys
!{sys.executable} -m pip install PackageToBeInstalled –user
```
Bu paket, Azure Cosmos hesap çalışma alanındaki herhangi bir dizüstü bilgisayardan kullanılabilir. 

> [!TIP]
> Not defteriniz özel bir paket gerektiriyorsa, [çalışma alanını sıfırlarsanız](#reset-notebooks-workspace)paketler kaldırıldığından, paketi yüklemek için not defterinize bir hücre eklemenizi öneririz.  

## <a name="run-a-sql-query"></a>SQL sorgusu çalıştırma

Bilgisayarınızdaki ``%%sql`` herhangi bir kapsayıcıya karşı bir [SQL sorgusu](sql-query-getting-started.md) çalıştırmak için sihirli komutu kullanabilirsiniz. Sözdizimini kullanın:

```bash
%%sql --database {database_id} --container {container_id}
{Query text}
```

- Cosmos ``{container_id}`` hesabınızdaki veritabanı ve kapsayıcının adını değiştirin. ``{database_id}`` Ve bağımsız değişkenler sağlanmazsa, sorgu [varsayılan veritabanı nda ve kapsayıcıda](#set-default-database-for-queries)yürütülür. ``--container`` ``--database``
- Azure Cosmos DB'de geçerli olan tüm SQL sorgusunu çalıştırabilirsiniz. Sorgu metni yeni bir satırda olmalıdır.

Örnek: 
```bash
%%sql --database RetailDemo --container WebsiteData
SELECT c.Action, c.Price as ItemRevenue, c.Country, c.Item FROM c
```
Not ```%%sql?``` defterindeki sql magic komutu için yardım belgelerini görmek için bir hücrede çalıştırın.

## <a name="run-a-sql-query-and-output-to-a-pandas-dataframe"></a>Bir SQL sorgusu ve çıktıyı Pandas DataFrame'e çalıştırma

Bir ``%%sql`` sorgunun sonuçlarını [Pandas DataFrame'e](https://pandas.pydata.org/pandas-docs/stable/getting_started/dsintro.html#dataframe)aktarabilirsiniz. Sözdizimini kullanın: 

```bash
%%sql --database {database_id} --container {container_id} --output {outputDataFrameVar}
{Query text}
```
- Cosmos ``{container_id}`` hesabınızdaki veritabanı ve kapsayıcının adını değiştirin. ``{database_id}`` Ve bağımsız değişkenler sağlanmazsa, sorgu [varsayılan veritabanı nda ve kapsayıcıda](#set-default-database-for-queries)yürütülür. ``--container`` ``--database``
- Sonuçları ``{outputDataFrameVar}`` içeren DataFrame değişkeninin adı ile değiştirin.
- Azure Cosmos DB'de geçerli olan tüm SQL sorgusunu çalıştırabilirsiniz. Sorgu metni yeni bir satırda olmalıdır. 

Örnek:

```bash
%%sql --database RetailDemo --container WebsiteData --output df_cosmos
SELECT c.Action, c.Price as ItemRevenue, c.Country, c.Item FROM c
```
```bash
df_cosmos.head(10)

    Action  ItemRevenue Country Item
0   Viewed  9.00    Tunisia Black Tee
1   Viewed  19.99   Antigua and Barbuda Flannel Shirt
2   Added   3.75    Guinea-Bissau   Socks
3   Viewed  3.75    Guinea-Bissau   Socks
4   Viewed  55.00   Czech Republic  Rainjacket
5   Viewed  350.00  Iceland Cosmos T-shirt
6   Added   19.99   Syrian Arab Republic    Button-Up Shirt
7   Viewed  19.99   Syrian Arab Republic    Button-Up Shirt
8   Viewed  33.00   Tuvalu  Red Top
9   Viewed  14.00   Cape Verde  Flip Flop Shoes
```
## <a name="upload-json-items-to-a-container"></a>JSON öğelerini bir kapsayıcıya yükleme
``%%upload`` JSON dosyasındaki verileri belirli bir Azure Cosmos kapsayıcısına yüklemek için sihirli komutu kullanabilirsiniz. Öğeleri yüklemek için aşağıdaki komutu kullanın:

```bash
%%upload --databaseName {database_id} --containerName {container_id} --url {url_location_of_file}
```

- Azure ``{database_id}`` ``{container_id}`` Cosmos hesabınızdaki veritabanı nın ve kapsayıcının adını değiştirin. Ve bağımsız değişkenler sağlanmazsa, sorgu [varsayılan veritabanı nda ve kapsayıcıda](#set-default-database-for-queries)yürütülür. ``--container`` ``--database``
- JSON dosyanızın konumunu değiştirin. ``{url_location_of_file}`` Dosya geçerli JSON nesnelerinbir dizi olmalıdır ve ortak Internet üzerinden erişilebilir olmalıdır.

Örnek:

```bash
%%upload --database databaseName --container containerName --url 
https://contoso.com/path/to/data.json
```
```bash
Documents successfully uploaded to ContainerName
Total number of documents imported : 2654
Total time taken : 00:00:38.1228087 hours
Total RUs consumed : 25022.58
```
Çıktı istatistikleri yle, öğeleri yüklemek için kullanılan etkili RU/s'leri hesaplayabilirsiniz. Örneğin, 38 saniye boyunca 25.000 RUs tüketildiyse, etkili RU/s 25.000 RUs / 38 saniye = 658 RU/s'dir.

## <a name="set-default-database-for-queries"></a>Sorgular için varsayılan veritabanını ayarlama
Not defteri için ```%%sql``` kullanılacak varsayılan veritabanı komutlarını ayarlayabilirsiniz. Veritabanınızın adı ile değiştirin. ```{database_id}```

```bash
%database {database_id}
```
Not ```%database?``` defterindeki belgeleri görmek için hücrede çalıştırın.

## <a name="set-default-container-for-queries"></a>Sorgular için varsayılan kapsayıcıyı ayarlama
Not defteri için ```%%sql``` kullanılacak varsayılan kapsayıcı komutlarını ayarlayabilirsiniz. Kabınızın adı ile değiştirin. ```{container_id}```

```bash
%container {container_id}
```
Not ```%container?``` defterindeki belgeleri görmek için hücrede çalıştırın.

## <a name="use-built-in-nteract-data-explorer"></a>Yerleşik nteract veri gezginini kullanma
Bir DataFrame'i filtrelemek ve görselleştirmek için yerleşik [nteract veri gezginini](https://blog.nteract.io/designing-the-nteract-data-explorer-f4476d53f897) kullanabilirsiniz. Bu özelliği etkinleştirmek için, ``True`` ``pd.options.display.max_rows`` seçeneği ``pd.options.display.html.table_schema`` istenilen değere ve ``pd.options.display.max_rows`` ``None`` istediğiniz değere ayarlayın (tüm sonuçları gösterecek şekilde ayarlayabilirsiniz).

```python
import pandas as pd
pd.options.display.html.table_schema = True
pd.options.display.max_rows = None

df_cosmos.groupby("Item").size()
```
![nteract veri gezgini](media/use-notebook-features-and-commands/nteract-built-in-chart.png)

## <a name="use-the-built-in-python-sdk"></a>Yerleşik Python SDK'yı kullanma
[SQL API için Azure Cosmos DB Python SDK](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cosmos/azure-cosmos) sürümü 4 yüklenir ve Azure Cosmos hesabı için not defteri ortamına dahildir.

Herhangi bir SDK işlemini çalıştırmak için yerleşik ``cosmos_client`` örneği kullanın. 

Örnek:

```python
## Import modules as needed
from azure.cosmos.partition_key import PartitionKey

## Create a new database if it doesn't exist
database = cosmos_client.create_database_if_not_exists('RetailDemo')

## Create a new container if it doesn't exist
container = database.create_container_if_not_exists(id='WebsiteData', partition_key=PartitionKey(path='/CartID'))
```
[Bkz. Python SDK örnekleri](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cosmos/azure-cosmos/samples). 

> [!IMPORTANT]
> Yerleşik Python SDK yalnızca SQL (Core) API hesapları için desteklenir. Diğer API'ler için, API'ye karşılık gelen [ilgili Python sürücüsünü yüklemeniz](#install-a-new-package) gerekir. 

## <a name="create-a-custom-instance-of-cosmos_client"></a>Özel bir örnek oluşturma``cosmos_client``
Daha fazla esneklik için, şu ``cosmos_client`` lar için özel bir örnek oluşturabilirsiniz:

- [Bağlantı ilkesini](https://docs.microsoft.com/python/api/azure-cosmos/azure.cosmos.documents.connectionpolicy?view=azure-python-preview) özelleştirme
- İşlemleri, içinde bulunandan farklı bir Azure Cosmos hesabına karşı çalıştırma

Çevre [değişkenleri](#access-the-account-endpoint-and-primary-key-env-variables)üzerinden cari hesabın bağlantı dizesi ve birincil anahtarına erişebilirsiniz. 

```python
import os
import azure.cosmos.cosmos_client as cosmos
import azure.cosmos.documents as documents

# These should be set to a region you've added for Azure Cosmos DB
region_1 = "Central US" 
region_2 = "East US 2"

custom_connection_policy = documents.ConnectionPolicy()
custom_connection_policy.PreferredLocations = [region_1, region_2] # Set the order of regions the SDK will route requests to. The regions should be regions you've added for Cosmos, otherwise this will error.

# Create a new instance of CosmosClient, getting the endpoint and key from the environment variables
custom_client = cosmos.CosmosClient(url=os.environ["COSMOS_ENDPOINT"], credential=os.environ["COSMOS_KEY"], connection_policy=custom_connection_policy)
```
## <a name="access-the-account-endpoint-and-primary-key-env-variables"></a>Hesap bitiş noktasına ve birincil anahtar env değişkenlerine erişin
```python
import os

endpoint = os.environ["COSMOS_ENDPOINT"]
primary_key = os.environ["COSMOS_KEY"]
```
> [!IMPORTANT]
> ``COSMOS_ENDPOINT`` Ve ``COSMOS_KEY`` ortam değişkenleri yalnızca SQL API için geçerlidir. Diğer API'ler için, Azure Cosmos hesabınızdaki **Bağlantı Dizeleri** veya **Anahtarlar** bıçağındaki bitiş noktasını ve anahtarı bulun.  

## <a name="reset-notebooks-workspace"></a>Not defterlerini sıfırlama çalışma alanını
Not defterlerinin çalışma alanını varsayılan ayarlara sıfırlamak için komut çubuğundaki **Çalışma Alanını Sıfırla'yı** seçin. Bu, özel yüklü paketleri kaldırır ve Jupyter sunucusunu yeniden başlatacaktır. Not defterleriniz, dosyalarınız ve Azure Cosmos kaynaklarınız etkilenmez.  

![Not defterlerini sıfırlama çalışma alanını](media/use-notebook-features-and-commands/reset-workspace.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB Jupyter dizüstü bilgisayarların](cosmosdb-jupyter-notebooks.md) avantajları hakkında bilgi edinin
- [SQL API için Azure Cosmos DB Python SDK](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cosmos/azure-cosmos) hakkında bilgi edinin
