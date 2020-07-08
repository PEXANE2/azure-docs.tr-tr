---
title: Azure Cosmos DB Python not defterlerinde yerleşik Not defteri komutlarını ve özelliklerini kullanma (Önizleme)
description: Azure Cosmos DB yerleşik Python not defterlerini kullanarak genel işlemleri yapmak için yerleşik komutları ve özellikleri nasıl kullanacağınızı öğrenin.
author: deborahc
ms.service: cosmos-db
ms.topic: how-to
ms.date: 05/19/2020
ms.author: dech
ms.openlocfilehash: c35c34d5ffe3f3349430f17e71ad8192f4d8256e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85263373"
---
# <a name="use-built-in-notebook-commands-and-features-in-azure-cosmos-db-python-notebooks-preview"></a>Azure Cosmos DB Python not defterlerinde yerleşik Not defteri komutlarını ve özelliklerini kullanma (Önizleme)

Azure Cosmos DB 'daki yerleşik jupi Not defterleri, Azure portal verilerinizi analiz etmenize ve görselleştirmenize olanak tanır. Bu makalede, Python not defterlerinde ortak işlemler yapmak için yerleşik Not defteri komutlarının ve özelliklerinin nasıl kullanılacağı açıklanır.

## <a name="install-a-new-package"></a>Yeni bir paket yükler
Azure Cosmos hesaplarınız için Not defteri desteğini etkinleştirdikten sonra, yeni bir not defteri açıp bir paket yükleyebilirsiniz.

Yeni bir kod hücresinde, istenen Python paketiyle değiştirerek aşağıdaki kodu ekleyin ve çalıştırın ``PackageToBeInstalled`` .
```python
import sys
!{sys.executable} -m pip install PackageToBeInstalled --user
```
Bu paket, Azure Cosmos hesap çalışma alanındaki herhangi bir not defterinden kullanılabilir. 

> [!TIP]
> Not defteriniz için özel bir paket gerekiyorsa, [çalışma alanını sıfırladıktan](#reset-notebooks-workspace)sonra paketler kaldırıladıkça, paketi yüklemek için Not defterinize bir hücre eklemenizi öneririz.  

## <a name="run-a-sql-query"></a>SQL sorgusu çalıştırma

``%%sql``Bir [SQL sorgusunu](sql-query-getting-started.md) hesabınızdaki herhangi bir kapsayıcıya karşı çalıştırmak için Magic komutunu kullanabilirsiniz. Sözdizimini kullanın:

```python
%%sql --database {database_id} --container {container_id}
{Query text}
```

- ``{database_id}``Ve ' i, ``{container_id}`` Cosmos hesabınızdaki veritabanı ve kapsayıcının adıyla değiştirin. ``--database``Ve ``--container`` bağımsız değişkenleri sağlanmazsa, sorgu [varsayılan veritabanı ve kapsayıcıda](#set-default-database-for-queries)yürütülür.
- Azure Cosmos DB için geçerli olan herhangi bir SQL sorgusunu çalıştırabilirsiniz. Sorgu metni yeni bir satırda olmalıdır.

Örneğin: 
```python
%%sql --database RetailDemo --container WebsiteData
SELECT c.Action, c.Price as ItemRevenue, c.Country, c.Item FROM c
```
```%%sql?```Not defterindeki SQL Magic komutunun Yardım belgelerini görmek için bir hücrede çalıştırın.

## <a name="run-a-sql-query-and-output-to-a-pandas-dataframe"></a>Bir Pandas DataFrame 'e SQL sorgusu ve çıkışı çalıştırma

Bir ``%%sql`` sorgunun sonuçlarını bir [Pandas dataframe](https://pandas.pydata.org/pandas-docs/stable/getting_started/dsintro.html#dataframe)'e aktarabilirsiniz. Sözdizimini kullanın: 

```python
%%sql --database {database_id} --container {container_id} --output {outputDataFrameVar}
{Query text}
```
- ``{database_id}``Ve ' i, ``{container_id}`` Cosmos hesabınızdaki veritabanı ve kapsayıcının adıyla değiştirin. ``--database``Ve ``--container`` bağımsız değişkenleri sağlanmazsa, sorgu [varsayılan veritabanı ve kapsayıcıda](#set-default-database-for-queries)yürütülür.
- ``{outputDataFrameVar}``Sonuçları Içerecek DataFrame değişkeninin adıyla değiştirin.
- Azure Cosmos DB için geçerli olan herhangi bir SQL sorgusunu çalıştırabilirsiniz. Sorgu metni yeni bir satırda olmalıdır. 

Örneğin:

```python
%%sql --database RetailDemo --container WebsiteData --output df_cosmos
SELECT c.Action, c.Price as ItemRevenue, c.Country, c.Item FROM c
```
```python
df_cosmos.head(10)

    Action    ItemRevenue    Country/Region    Item
0    Viewed    9.00    Tunisia    Black Tee
1    Viewed    19.99    Antigua and Barbuda    Flannel Shirt
2    Added    3.75    Guinea-Bissau    Socks
3    Viewed    3.75    Guinea-Bissau    Socks
4    Viewed    55.00    Czech Republic    Rainjacket
5    Viewed    350.00    Iceland    Cosmos T-shirt
6    Added    19.99    Syrian Arab Republic    Button-Up Shirt
7    Viewed    19.99    Syrian Arab Republic    Button-Up Shirt
8    Viewed    33.00    Tuvalu    Red Top
9    Viewed    14.00    Cabo Verde    Flip Flop Shoes
```
## <a name="set-default-database-for-queries"></a>Sorgular için varsayılan veritabanını ayarla
```%%sql```Not defteri için kullanılacak varsayılan veritabanı komutlarını ayarlayabilirsiniz. ```{database_id}```Veritabanınızın adıyla değiştirin.

```python
%database {database_id}
```
```%database?```Not defterindeki belgeleri görmek için bir hücrede çalıştırın.

## <a name="set-default-container-for-queries"></a>Sorgular için varsayılan kapsayıcıyı ayarla
```%%sql```Not defteri için kullanılacak varsayılan kapsayıcı komutlarını ayarlayabilirsiniz. ```{container_id}```Kapsayıcının adıyla değiştirin.

```python
%container {container_id}
```
```%container?```Not defterindeki belgeleri görmek için bir hücrede çalıştırın.

## <a name="upload-json-items-to-a-container"></a>JSON öğelerini bir kapsayıcıya yükleme
``%%upload``BIR JSON dosyasındaki verileri belirtilen Azure Cosmos kapsayıcısına yüklemek için Magic komutunu kullanabilirsiniz. Öğeleri karşıya yüklemek için aşağıdaki komutu kullanın:

```python
%%upload --databaseName {database_id} --containerName {container_id} --url {url_location_of_file}
```

- ``{database_id}``Ve ' i ``{container_id}`` Azure Cosmos hesabınızdaki veritabanı ve kapsayıcının adıyla değiştirin. ``--database``Ve ``--container`` bağımsız değişkenleri sağlanmazsa, sorgu [varsayılan veritabanı ve kapsayıcıda](#set-default-database-for-queries)yürütülür.
- ``{url_location_of_file}``JSON dosyanızın konumuyla değiştirin. Dosya geçerli bir JSON nesneleri dizisi olmalıdır ve bu, genel Internet üzerinden erişilebilir olmalıdır.

Örneğin:

```python
%%upload --database databaseName --container containerName --url 
https://contoso.com/path/to/data.json
```
```
Documents successfully uploaded to ContainerName
Total number of documents imported : 2654
Total time taken : 00:00:38.1228087 hours
Total RUs consumed : 25022.58
```
Çıkış istatistikleriyle, öğeleri karşıya yüklemek için kullanılan geçerli RU/s 'yi hesaplayabilirsiniz. Örneğin, 38 saniyenin üzerinde 25.000 ru kullanılıyorsa, geçerli RU/s, 25.000 ru/38 saniye = 658 RU/s olur.

## <a name="run-another-notebook-in-current-notebook"></a>Geçerli not defterinde başka bir not defteri Çalıştır 
``%%run``Geçerli not defterinizdeki çalışma alanınızda başka bir not defteri çalıştırmak için Magic komutunu kullanabilirsiniz. Sözdizimini kullanın:

```python
%%run ./path/to/{notebookName}.ipynb
```
``{notebookName}``Çalıştırmak istediğiniz Not defterinin adıyla değiştirin. Not defteri, geçerli ' My not defterim ' çalışma alanınızda olmalıdır. 

## <a name="use-built-in-nteract-data-explorer"></a>Yerleşik nteryasası Veri Gezgini 'ni kullanma
Yerleşik [nteryasası Veri Gezgini](https://blog.nteract.io/designing-the-nteract-data-explorer-f4476d53f897) 'ni kullanarak bir veri çerçevesini filtreleyebilir ve görselleştirebilirsiniz. Bu özelliği etkinleştirmek için seçeneğini ``pd.options.display.html.table_schema`` ``True`` ve ``pd.options.display.max_rows`` istenen değere ayarlayın ( ``pd.options.display.max_rows`` ``None`` tüm sonuçları göstermek için öğesini olarak ayarlayabilirsiniz).

```python
import pandas as pd
pd.options.display.html.table_schema = True
pd.options.display.max_rows = None

df_cosmos.groupby("Item").size()
```
:::image type="content" source="media/use-notebook-features-and-commands/nteract-built-in-chart.png" alt-text="nteryasası Veri Gezgini":::

## <a name="use-the-built-in-python-sdk"></a>Yerleşik Python SDK 'sını kullanma
[SQL API için Azure Cosmos DB Python SDK](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cosmos/azure-cosmos) 'nın sürüm 4 ' ü yüklenir ve Azure Cosmos hesabı için Not defteri ortamına dahil edilmiştir.

``cosmos_client``Herhangi BIR SDK işlemini çalıştırmak için yerleşik örneği kullanın. 

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

## <a name="create-a-custom-instance-of-cosmos_client"></a>Özel bir örneği oluşturma``cosmos_client``
Daha fazla esneklik için, aşağıdakileri yapmak için özel bir örneği oluşturabilirsiniz ``cosmos_client`` :

- [Bağlantı ilkesini](https://docs.microsoft.com/python/api/azure-cosmos/azure.cosmos.documents.connectionpolicy?view=azure-python-preview) özelleştirme
- İşlemleri, farklı bir Azure Cosmos hesabına göre çalıştırma

Geçerli hesabın bağlantı dizesine ve birincil anahtarına [ortam değişkenleri](#access-the-account-endpoint-and-primary-key-env-variables)aracılığıyla erişebilirsiniz. 

```python
import azure.cosmos.cosmos_client as cosmos
import azure.cosmos.documents as documents

# These should be set to a region you've added for Azure Cosmos DB
region_1 = "Central US" 
region_2 = "East US 2"

custom_connection_policy = documents.ConnectionPolicy()
custom_connection_policy.PreferredLocations = [region_1, region_2] # Set the order of regions the SDK will route requests to. The regions should be regions you've added for Cosmos, otherwise this will error.

# Create a new instance of CosmosClient, getting the endpoint and key from the environment variables
custom_client = cosmos.CosmosClient(url=COSMOS.ENDPOINT, credential=COSMOS.KEY, connection_policy=custom_connection_policy)

```
## <a name="access-the-account-endpoint-and-primary-key-env-variables"></a>Hesap uç noktasına ve birincil anahtar env değişkenlerine erişin
Not defterinizin bulunduğu hesabın yerleşik uç noktasına ve anahtarına erişebilirsiniz.

```python
endpoint = COSMOS.ENDPOINT
primary_key = COSMOS.KEY
```
> [!IMPORTANT]
> ``COSMOS.ENDPOINT``Ve ``COSMOS.KEY`` DEĞIŞKENLERI yalnızca SQL API için geçerlidir. Diğer API 'Ler için, Azure Cosmos hesabınızdaki **bağlantı dizeleri** veya **anahtarlar** dikey penceresinde uç noktasını ve anahtarı bulun.  

## <a name="reset-notebooks-workspace"></a>Not defteri çalışma alanını Sıfırla
Not defteri çalışma alanını varsayılan ayarlara sıfırlamak için komut çubuğundan **çalışma alanını Sıfırla** ' yı seçin. Bu, tüm özel yüklü paketleri kaldıracak ve jupi sunucusunu yeniden başlatacak. Not defterleriniz, dosyalarınız ve Azure Cosmos kaynaklarınızın etkilenmemesi gerekir.  

:::image type="content" source="media/use-notebook-features-and-commands/reset-workspace.png" alt-text="Not defteri çalışma alanını Sıfırla":::

## <a name="next-steps"></a>Sonraki adımlar

- [Jupyıter Not defterlerinin Azure Cosmos DB](cosmosdb-jupyter-notebooks.md) avantajları hakkında bilgi edinin
- [SQL API Için Python SDK Azure Cosmos DB](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cosmos/azure-cosmos) hakkında bilgi edinin
