---
title: Azure Cosmos DB 'daki yerleşik Not defteri komutlarını ve özelliklerini kullanma (Önizleme)
description: Azure Cosmos DB yerleşik not defterlerini kullanarak genel işlemleri yapmak için yerleşik komutları ve özellikleri nasıl kullanacağınızı öğrenin.
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/07/2019
ms.author: dech
ms.openlocfilehash: 61d46bbf0ccdeb5cd2e95e36e19f1aa81cfeeb48
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76513408"
---
# <a name="use-built-in-notebook-commands-and-features-in-azure-cosmos-db-preview"></a>Azure Cosmos DB 'daki yerleşik Not defteri komutlarını ve özelliklerini kullanma (Önizleme)

Azure Cosmos DB 'daki yerleşik jupi Not defterleri, Azure portal verilerinizi analiz etmenize ve görselleştirmenize olanak tanır. Bu makale, ortak işlemleri gerçekleştirmek için yerleşik not defteri komutlarını kullanmayı açıklar.

## <a name="install-a-new-package"></a>Yeni bir paket yükler
Azure Cosmos hesaplarınız için Not defteri desteğini etkinleştirdikten sonra, yeni bir not defteri açıp bir paket yükleyebilirsiniz.

Yeni bir kod hücresinde, istenen Python paketiyle ``PackageToBeInstalled`` değiştirerek aşağıdaki kodu ekleyin ve çalıştırın.
```python
import sys
!{sys.executable} -m pip install PackageToBeInstalled –user
```
Bu paket, Azure Cosmos hesap çalışma alanındaki herhangi bir not defterinden kullanılabilir. 

> [!TIP]
> Not defteriniz için özel bir paket gerekiyorsa, [çalışma alanını sıfırladıktan](#reset-notebooks-workspace)sonra paketler kaldırıladıkça, paketi yüklemek için Not defterinize bir hücre eklemenizi öneririz.  

## <a name="run-a-sql-query"></a>SQL sorgusu çalıştırma

Hesabınızdaki herhangi bir kapsayıcıya karşı bir [SQL sorgusu](sql-query-getting-started.md) çalıştırmak için ``%%sql`` Magic komutunu kullanabilirsiniz. Sözdizimini kullanın:

```bash
%%sql --database {database_id} --container {container_id}
{Query text}
```

- ``{database_id}`` ve ``{container_id}`` değerini Cosmos hesabınızdaki veritabanı ve kapsayıcının adıyla değiştirin. ``--database`` ve ``--container`` bağımsız değişkenleri sağlanmazsa, sorgu [varsayılan veritabanı ve kapsayıcıda](#set-default-database-for-queries)yürütülür.
- Azure Cosmos DB için geçerli olan herhangi bir SQL sorgusunu çalıştırabilirsiniz. Sorgu metni yeni bir satırda olmalıdır.

Örneğin: 
```bash
%%sql --database RetailDemo --container WebsiteData
SELECT c.Action, c.Price as ItemRevenue, c.Country, c.Item FROM c
```
Not defterindeki SQL Magic komutu için Yardım belgelerini görmek üzere hücrede ```%%sql?``` çalıştırın.

## <a name="run-a-sql-query-and-output-to-a-pandas-dataframe"></a>Bir Pandas DataFrame 'e SQL sorgusu ve çıkışı çalıştırma

Bir ``%%sql`` sorgusunun sonuçlarını [Pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/getting_started/dsintro.html#dataframe)'e aktarabilirsiniz. Sözdizimini kullanın: 

```bash
%%sql --database {database_id} --container {container_id} --output {outputDataFrameVar}
{Query text}
```
- ``{database_id}`` ve ``{container_id}`` değerini Cosmos hesabınızdaki veritabanı ve kapsayıcının adıyla değiştirin. ``--database`` ve ``--container`` bağımsız değişkenleri sağlanmazsa, sorgu [varsayılan veritabanı ve kapsayıcıda](#set-default-database-for-queries)yürütülür.
- ``{outputDataFrameVar}``, sonuçları içerecek olan DataFrame değişkeninin adıyla değiştirin.
- Azure Cosmos DB için geçerli olan herhangi bir SQL sorgusunu çalıştırabilirsiniz. Sorgu metni yeni bir satırda olmalıdır. 

Örneğin:

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
Bir JSON dosyasındaki verileri belirtilen Azure Cosmos kapsayıcısına yüklemek için ``%%upload`` Magic komutunu kullanabilirsiniz. Öğeleri karşıya yüklemek için aşağıdaki komutu kullanın:

```bash
%%upload --databaseName {database_id} --containerName {container_id} --url {url_location_of_file}
```

- ``{database_id}`` ve ``{container_id}`` değerini Azure Cosmos hesabınızdaki veritabanı ve kapsayıcının adıyla değiştirin. ``--database`` ve ``--container`` bağımsız değişkenleri sağlanmazsa, sorgu [varsayılan veritabanı ve kapsayıcıda](#set-default-database-for-queries)yürütülür.
- ``{url_location_of_file}``, JSON dosyanızın konumuyla değiştirin. Dosya geçerli bir JSON nesneleri dizisi olmalıdır ve bu, genel Internet üzerinden erişilebilir olmalıdır.

Örneğin:

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
Çıkış istatistikleriyle, öğeleri karşıya yüklemek için kullanılan geçerli RU/s 'yi hesaplayabilirsiniz. Örneğin, 38 saniyenin üzerinde 25.000 ru kullanılıyorsa, geçerli RU/s, 25.000 ru/38 saniye = 658 RU/s olur.

## <a name="set-default-database-for-queries"></a>Sorgular için varsayılan veritabanını ayarla
Not defteri için kullanılacak varsayılan veritabanı ```%%sql``` komutları ayarlayabilirsiniz. ```{database_id}```, veritabanınızın adıyla değiştirin.

```bash
%database {database_id}
```
Not defterindeki belgeleri görmek için ```%database?``` bir hücrede çalıştırın.

## <a name="set-default-container-for-queries"></a>Sorgular için varsayılan kapsayıcıyı ayarla
Not defteri için kullanılacak varsayılan kapsayıcı ```%%sql``` komutları ayarlayabilirsiniz. ```{container_id}```, kapsayıcının adıyla değiştirin.

```bash
%container {container_id}
```
Not defterindeki belgeleri görmek için ```%container?``` bir hücrede çalıştırın.

## <a name="use-built-in-nteract-data-explorer"></a>Yerleşik nteryasası Veri Gezgini 'ni kullanma
Yerleşik [nteryasası Veri Gezgini](https://blog.nteract.io/designing-the-nteract-data-explorer-f4476d53f897) 'ni kullanarak bir veri çerçevesini filtreleyebilir ve görselleştirebilirsiniz. Bu özelliği etkinleştirmek için, ``pd.options.display.html.table_schema`` seçeneğini ``True`` ve istenen değere ``pd.options.display.max_rows`` ayarlayın (``pd.options.display.max_rows``, tüm sonuçları gösterecek şekilde ``None`` ayarlayabilirsiniz).

```python
import pandas as pd
pd.options.display.html.table_schema = True
pd.options.display.max_rows = None

df_cosmos.groupby("Item").size()
```
![nteryasası Veri Gezgini](media/use-notebook-features-and-commands/nteract-built-in-chart.png)

## <a name="use-the-built-in-python-sdk"></a>Yerleşik Python SDK 'sını kullanma
[SQL API için Azure Cosmos DB Python SDK](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cosmos/azure-cosmos) 'nın sürüm 4 ' ü yüklenir ve Azure Cosmos hesabı için Not defteri ortamına dahil edilmiştir.

Herhangi bir SDK işlemini çalıştırmak için yerleşik ``cosmos_client`` örneğini kullanın. 

Örneğin:

```python
## Import modules as needed
from azure.cosmos.partition_key import PartitionKey

## Create a new database if it doesn't exist
database = cosmos_client.create_database_if_not_exists('RetailDemo')

## Create a new container if it doesn't exist
container = database.create_container_if_not_exists(id='WebsiteData', partition_key=PartitionKey(path='/CartID'))
```
Bkz. [Python SDK örnekleri](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cosmos/azure-cosmos/samples). 

> [!IMPORTANT]
> Yerleşik Python SDK 'Sı yalnızca SQL (çekirdek) API hesapları için desteklenir. Diğer API 'Ler için, API 'ye karşılık gelen [Ilgili Python sürücüsünü yüklemeniz](#install-a-new-package) gerekir. 

## <a name="create-a-custom-instance-of-cosmos_client"></a>Özel bir ``cosmos_client`` örneği oluşturma
Daha fazla esneklik için, aşağıdakileri yapmak üzere ``cosmos_client`` özel bir örneği oluşturabilirsiniz:

- [Bağlantı ilkesini](https://docs.microsoft.com/python/api/azure-cosmos/azure.cosmos.documents.connectionpolicy?view=azure-python-preview) özelleştirme
- İşlemleri, farklı bir Azure Cosmos hesabına göre çalıştırma

Geçerli hesabın bağlantı dizesine ve birincil anahtarına [ortam değişkenleri](#access-the-account-endpoint-and-primary-key-env-variables)aracılığıyla erişebilirsiniz. 

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
## <a name="access-the-account-endpoint-and-primary-key-env-variables"></a>Hesap uç noktasına ve birincil anahtar env değişkenlerine erişin
```python
import os

endpoint = os.environ["COSMOS_ENDPOINT"]
primary_key = os.environ["COSMOS_KEY"]
```
> [!IMPORTANT]
> ``COSMOS_ENDPOINT`` ve ``COSMOS_KEY`` ortam değişkenleri yalnızca SQL API 'SI için geçerlidir. Diğer API 'Ler için, Azure Cosmos hesabınızdaki **bağlantı dizeleri** veya **anahtarlar** dikey penceresinde uç noktasını ve anahtarı bulun.  

## <a name="reset-notebooks-workspace"></a>Not defteri çalışma alanını Sıfırla
Not defteri çalışma alanını varsayılan ayarlara sıfırlamak için komut çubuğundan **çalışma alanını Sıfırla** ' yı seçin. Bu, tüm özel yüklü paketleri kaldıracak ve jupi sunucusunu yeniden başlatacak. Not defterleriniz, dosyalarınız ve Azure Cosmos kaynaklarınızın etkilenmemesi gerekir.  

![Not defteri çalışma alanını Sıfırla](media/use-notebook-features-and-commands/reset-workspace.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Jupyıter Not defterlerinin Azure Cosmos DB](cosmosdb-jupyter-notebooks.md) avantajları hakkında bilgi edinin
- [SQL API Için Python SDK Azure Cosmos DB](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cosmos/azure-cosmos) hakkında bilgi edinin
