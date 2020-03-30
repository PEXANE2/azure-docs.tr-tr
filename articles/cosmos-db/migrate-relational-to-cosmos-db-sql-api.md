---
title: Bire birkaç ilişkisel veriyi Azure Cosmos DB SQL API'ye geçirme
description: SQL API'ye bir-az ilişki için karmaşık veri geçişini nasıl işleyeceğinizi öğrenin
author: TheovanKraay
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 12/12/2019
ms.author: thvankra
ms.openlocfilehash: 467e9627a2623779bd808ca5aebdf76d8a5eda42
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75896642"
---
# <a name="migrate-one-to-few-relational-data-into-azure-cosmos-db-sql-api-account"></a>Birila birkaç ilişkisel veriyi Azure Cosmos DB SQL API hesabına geçirme

İlişkisel bir veritabanından Azure Cosmos DB SQL API'ye geçiş yapabilmek için, optimizasyon için veri modelinde değişiklik yapmak gerekebilir.

Yaygın dönüşümlerden biri, ilgili alt öğeleri bir JSON belgesine katıştırarak verilerin normalden arındırılabıdır. Burada Azure Veri Fabrikası veya Azure Veri Tuğlaları kullanarak bunun birkaç seçeneğini inceliyoruz. Cosmos DB için veri modelleme hakkında genel kılavuz için lütfen [Azure Cosmos DB'deki Veri modellemeyi](modeling-data.md)gözden geçirin.  

## <a name="example-scenario"></a>Örnek Senaryo

SQL veritabanımızda aşağıdaki iki tablo olduğunu varsayalım, Orders and OrderDetails.


![Sipariş Detayları](./media/migrate-relational-to-cosmos-sql-api/orders.png)

Bu bire bir ilişkiyi geçiş sırasında tek bir JSON belgesinde birleştirmek istiyoruz. Bunu yapmak için aşağıdaki gibi "FOR JSON" kullanarak bir T-SQL sorgusu oluşturabiliriz:

```sql
SELECT
  o.OrderID,
  o.OrderDate,
  o.FirstName,
  o.LastName,
  o.Address,
  o.City,
  o.State,
  o.PostalCode,
  o.Country,
  o.Phone,
  o.Total,
  (select OrderDetailId, ProductId, UnitPrice, Quantity from OrderDetails od where od.OrderId = o.OrderId for json auto) as OrderDetails
FROM Orders o;
```

Bu sorgunun sonuçları aşağıdaki gibi görünecek: 

![Sipariş Detayları](./media/migrate-relational-to-cosmos-sql-api/for-json-query-result.png#lightbox)


İdeal olarak, SQL verilerini kaynak olarak sorgulamak ve çıktıyı doğrudan Uygun JSON nesneleri olarak Azure Cosmos DB lavabosına yazmak için tek bir Azure Veri Fabrikası (ADF) kopyalama etkinliği kullanmak istiyorsunuz. Şu anda, gerekli JSON dönüşüm gerçekleştirmek mümkün değildir bir kopya etkinliği. Yukarıdaki sorgunun sonuçlarını bir Azure Cosmos DB SQL API kapsayıcısına kopyalamaya çalışırsak, OrderDetails alanını beklenen JSON dizisi yerine belgemizin bir dize özelliği olarak görürüz.

Bu geçerli sınırlamayı aşağıdaki yollardan biriyle çözebiliriz:

* **Azure Veri Fabrikası'nı iki kopyalama aktivitesiyle kullanın:** 
  1. SQL'den JSON biçimli verileri bir ara blob depolama konumundaki bir metin dosyasına alın ve 
  2. JSON metin dosyasındaki verileri Azure Cosmos DB'deki bir kapsayıcıya yükleyin.

* **SQL'den okumak ve Azure Cosmos DB'ye yazmak için Azure Databricks'i kullanın** - burada iki seçenek sunacağız.


Bu yaklaşımlara daha ayrıntılı olarak bakalım:

## <a name="azure-data-factory"></a>Azure Data Factory

Hedef Cosmos DB belgesine OrderDetails'ı JSON dizisi olarak katıştıramasak da, iki ayrı Kopyalama Etkinlikleri kullanarak sorunu çözebiliriz.

### <a name="copy-activity-1-sqljsontoblobtext"></a>Kopyalama Etkinliği #1: SqlJsonToBlobText

Kaynak veriler için, SQL Server OPENJSON ve FOR JSON PATH özelliklerini kullanarak satır başına bir JSON nesnesi (Siparişi temsil eden) tek bir sütun olarak sonuç kümesini almak için bir SQL sorgusu kullanırız:

```sql
SELECT [value] FROM OPENJSON(
  (SELECT
    id = o.OrderID,
    o.OrderDate,
    o.FirstName,
    o.LastName,
    o.Address,
    o.City,
    o.State,
    o.PostalCode,
    o.Country,
    o.Phone,
    o.Total,
    (select OrderDetailId, ProductId, UnitPrice, Quantity from OrderDetails od where od.OrderId = o.OrderId for json auto) as OrderDetails
   FROM Orders o FOR JSON PATH)
)
```

![ADF kopyası](./media/migrate-relational-to-cosmos-sql-api/adf1.png)


SqlJsonToBlobText kopyalama etkinliğinin batması için "Sınırlı Metin"i seçiyoruz ve dinamik olarak oluşturulmuş benzersiz bir dosya adı (örneğin,@concat' (pipeline(). RunId,'.json').
Since our text file is not really "delimited" and we do not want it to be parsed into separate columns using commas and want to preserve the double-quotes ("), we set "Column delimiter" to a Tab ("\t") - or another character not occurring in the data - and "Quote character" "Alıntı karakteri yok" için.

![ADF kopyası](./media/migrate-relational-to-cosmos-sql-api/adf2.png)

### <a name="copy-activity-2-blobjsontocosmos"></a>Kopyalama Etkinliği #2: BlobJsonToCosmos

Ardından, ilk etkinlik tarafından oluşturulan metin dosyası için Azure Blob Depolama'da görünen ikinci Kopyalama Etkinliği'ni ekleyerek ADF ardışık sistemimizi değiştiririz. Cosmos DB lavaboya metin dosyasında bulunan JSON satırı başına bir belge olarak eklemek için "JSON" kaynağı olarak işler.

![ADF kopyası](./media/migrate-relational-to-cosmos-sql-api/adf3.png)

İsteğe bağlı olarak, her çalıştırmadan önce /Orders/ klasöründe kalan önceki dosyaların tümünü silebilmek için ardışık düzene bir "Sil" etkinliği de ekleriz. Bizim ADF boru hattı şimdi şuna benzer:

![ADF kopyası](./media/migrate-relational-to-cosmos-sql-api/adf4.png)

Yukarıdaki ardışık hattı tetikledikten sonra, ara Azure Blob Depolama konumumuzda satır başına bir JSON nesnesi içeren bir dosya oluşturulur:

![ADF kopyası](./media/migrate-relational-to-cosmos-sql-api/adf5.png)

Cosmos DB koleksiyonumuza düzgün bir şekilde gömülü Sipariş Ayrıntıları eklenmiş Sipariş belgelerini de görürler:

![ADF kopyası](./media/migrate-relational-to-cosmos-sql-api/adf6.png)


## <a name="azure-databricks"></a>Azure Databricks

Azure Veri [Tuğlaları'ndaki](https://azure.microsoft.com/services/databricks/) Spark'ı, Azure Blob Depolama'da ara metin/JSON dosyaları oluşturmadan SQL Veritabanı kaynağımızdan Azure Cosmos DB hedefine kopyalamak için de kullanabiliriz. 

> [!NOTE]
> Netlik ve basitlik için, aşağıdaki kod parçacıkları açıkça satır satırda sahte veritabanı parolalarını içerir, ancak azure databricks sırlarını her zaman kullanmanız gerekir.
>

İlk olarak, gerekli [SQL bağlayıcısını](https://docs.databricks.com/data/data-sources/sql-databases-azure.html) ve [Azure Cosmos DB bağlayıcı](https://docs.databricks.com/data/data-sources/azure/cosmosdb-connector.html) kitaplıklarını Azure Databricks kümemize oluşturur ve iliştirileriz. Kitaplıkların yüklendiğinden emin olmak için kümeyi yeniden başlatın.

![Databricks](./media/migrate-relational-to-cosmos-sql-api/databricks1.png)

Daha sonra, Scala ve Python için iki örnek satacağız. 

### <a name="scala"></a>Scala
Burada, "FOR JSON" çıktısı ile SQL sorgusunun sonuçlarını dataframe'e alıyoruz:

```scala
// Connect to Azure SQL https://docs.databricks.com/data/data-sources/sql-databases-azure.html
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._
val configSql = Config(Map(
  "url"          -> "xxxx.database.windows.net",
  "databaseName" -> "xxxx",
  "queryCustom"  -> "SELECT o.OrderID, o.OrderDate, o.FirstName, o.LastName, o.Address, o.City, o.State, o.PostalCode, o.Country, o.Phone, o.Total, (SELECT OrderDetailId, ProductId, UnitPrice, Quantity FROM OrderDetails od WHERE od.OrderId = o.OrderId FOR JSON AUTO) as OrderDetails FROM Orders o",
  "user"         -> "xxxx", 
  "password"     -> "xxxx" // NOTE: For clarity and simplicity, this example includes secrets explicitely as a string, but you should always use Databricks secrets
))
// Create DataFrame from Azure SQL query
val orders = sqlContext.read.sqlDB(configSql)
display(orders)
```

![Databricks](./media/migrate-relational-to-cosmos-sql-api/databricks2.png)

Sonra, Cosmos DB veritabanımıza ve koleksiyonumuza bağlanıyoruz:

```scala
// Connect to Cosmos DB https://docs.databricks.com/data/data-sources/azure/cosmosdb-connector.html
import org.joda.time._
import org.joda.time.format._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark.CosmosDBSpark
import com.microsoft.azure.cosmosdb.spark.config.Config
import org.apache.spark.sql.functions._
import org.joda.time._
import org.joda.time.format._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark.CosmosDBSpark
import com.microsoft.azure.cosmosdb.spark.config.Config
import org.apache.spark.sql.functions._
val configMap = Map(
  "Endpoint" -> "https://xxxx.documents.azure.com:443/",
  // NOTE: For clarity and simplicity, this example includes secrets explicitely as a string, but you should always use Databricks secrets
  "Masterkey" -> "xxxx",
  "Database" -> "StoreDatabase",
  "Collection" -> "Orders")
val configCosmos = Config(configMap)
```

Son olarak, şemamızı tanımlıyoruz ve CosmosDB koleksiyonuna kaydetmeden önce DataFrame'i uygulamak için from_json kullanıyoruz.

```scala
// Convert DataFrame to proper nested schema
import org.apache.spark.sql.types._
val orderDetailsSchema = ArrayType(StructType(Array(
    StructField("OrderDetailId",IntegerType,true),
    StructField("ProductId",IntegerType,true),
    StructField("UnitPrice",DoubleType,true),
    StructField("Quantity",IntegerType,true)
  )))
val ordersWithSchema = orders.select(
  col("OrderId").cast("string").as("id"),
  col("OrderDate").cast("string"),
  col("FirstName").cast("string"),
  col("LastName").cast("string"),
  col("Address").cast("string"),
  col("City").cast("string"),
  col("State").cast("string"),
  col("PostalCode").cast("string"),
  col("Country").cast("string"),
  col("Phone").cast("string"),
  col("Total").cast("double"),
  from_json(col("OrderDetails"), orderDetailsSchema).as("OrderDetails")
)
display(ordersWithSchema)
// Save nested data to Cosmos DB
CosmosDBSpark.save(ordersWithSchema, configCosmos)
```

![Databricks](./media/migrate-relational-to-cosmos-sql-api/databricks3.png)


### <a name="python"></a>Python

Alternatif bir yaklaşım olarak, Spark'ta JSON dönüşümlerini yürütmeniz gerekebilir (kaynak veritabanı "FOR JSON" veya benzer bir işlemi desteklemiyorsa) veya çok büyük bir veri kümesi için paralel işlemler kullanmak isteyebilirsiniz. Burada bir PySpark örneği salıyoruz. İlk hücredeki kaynak ve hedef veritabanı bağlantılarını yapılandırarak başlayın:

```python
import uuid
import pyspark.sql.functions as F
from pyspark.sql.functions import col
from pyspark.sql.types import StringType,DateType,LongType,IntegerType,TimestampType
 
#JDBC connect details for SQL Server database
jdbcHostname = "jdbcHostname"
jdbcDatabase = "OrdersDB"
jdbcUsername = "jdbcUsername"
jdbcPassword = "jdbcPassword"
jdbcPort = "1433"
 
connectionProperties = {
  "user" : jdbcUsername,
  "password" : jdbcPassword,
  "driver" : "com.microsoft.sqlserver.jdbc.SQLServerDriver"
}
jdbcUrl = "jdbc:sqlserver://{0}:{1};database={2};user={3};password={4}".format(jdbcHostname, jdbcPort, jdbcDatabase, jdbcUsername, jdbcPassword)
 
#Connect details for Target Azure Cosmos DB SQL API account
writeConfig = {
    "Endpoint": "Endpoint",
    "Masterkey": "Masterkey",
    "Database": "OrdersDB",
    "Collection": "Orders",
    "Upsert": "true"
}
```

Daha sonra, sonuçları Spark Dataframes'e koyarak hem sipariş hem de sipariş ayrıntı kayıtları için kaynak Veritabanını (bu durumda SQL Server) sorgularız. Ayrıca, tüm sipariş dislerini içeren bir liste ve paralel işlemler için bir Iş parçacığı havuzu oluştururuz:

```python
import json
import ast
import pyspark.sql.functions as F
import uuid
import numpy as np
import pandas as pd
from functools import reduce
from pyspark.sql import SQLContext
from pyspark.sql.types import *
from pyspark.sql import *
from pyspark.sql.functions import exp
from pyspark.sql.functions import col
from pyspark.sql.functions import lit
from pyspark.sql.functions import array
from pyspark.sql.types import *
from multiprocessing.pool import ThreadPool
 
#get all orders
orders = sqlContext.read.jdbc(url=jdbcUrl, table="orders", properties=connectionProperties)
 
#get all order details
orderdetails = sqlContext.read.jdbc(url=jdbcUrl, table="orderdetails", properties=connectionProperties)
 
#get all OrderId values to pass to map function 
orderids = orders.select('OrderId').collect()
 
#create thread pool big enough to process merge of details to orders in parallel
pool = ThreadPool(10)
```

Ardından, hedef SQL API koleksiyonuna Siparişler yazmak için bir işlev oluşturun. Bu işlev, verilen sipariş kimliği için tüm sipariş ayrıntılarını filtreler, bunları bir JSON dizisine dönüştürür ve diziyi bu sipariş için hedef SQL API Koleksiyonu'na yazabileceğimiz bir JSON belgesine ekler:

```python
def writeOrder(orderid):
  #filter the order on current value passed from map function
  order = orders.filter(orders['OrderId'] == orderid[0])
  
  #set id to be a uuid
  order = order.withColumn("id", lit(str(uuid.uuid1())))
  
  #add details field to order dataframe
  order = order.withColumn("details", lit(''))
  
  #filter order details dataframe to get details we want to merge into the order document
  orderdetailsgroup = orderdetails.filter(orderdetails['OrderId'] == orderid[0])
  
  #convert dataframe to pandas
  orderpandas = order.toPandas()
  
  #convert the order dataframe to json and remove enclosing brackets
  orderjson = orderpandas.to_json(orient='records', force_ascii=False)
  orderjson = orderjson[1:-1] 
  
  #convert orderjson to a dictionaory so we can set the details element with order details later
  orderjsondata = json.loads(orderjson)
  
  
  #convert orderdetailsgroup dataframe to json, but only if details were returned from the earlier filter
  if (orderdetailsgroup.count() !=0):
    #convert orderdetailsgroup to pandas dataframe to work better with json
    orderdetailsgroup = orderdetailsgroup.toPandas()
    
    #convert orderdetailsgroup to json string
    jsonstring = orderdetailsgroup.to_json(orient='records', force_ascii=False)
    
    #convert jsonstring to dictionary to ensure correct encoding and no corrupt records
    jsonstring = json.loads(jsonstring)
    
    #set details json element in orderjsondata to jsonstring which contains orderdetailsgroup - this merges order details into the order 
    orderjsondata['details'] = jsonstring
 
  #convert dictionary to json
  orderjsondata = json.dumps(orderjsondata)
 
  #read the json into spark dataframe
  df = spark.read.json(sc.parallelize([orderjsondata]))
  
  #write the dataframe (this will be a single order record with merged many-to-one order details) to cosmos db using spark the connector
  #https://docs.microsoft.com/azure/cosmos-db/spark-connector
  df.write.format("com.microsoft.azure.cosmosdb.spark").mode("append").options(**writeConfig).save()
```

Son olarak, daha önce oluşturduğumuz sıralı işlevler listesini geçerek paralel olarak yürütmek için iş parçacığı havuzunda bir harita işlevi kullanarak yukarıdaki leri çağıracağız:

```python
#map order details to orders in parallel using the above function
pool.map(writeOrder, orderids)
```
Her iki yaklaşımda da, sonunda, Cosmos DB koleksiyonundaki her Sipariş belgesinde gömülü OrderDetails'ı düzgün bir şekilde kaydetmemiz gerekir:

![Databricks](./media/migrate-relational-to-cosmos-sql-api/databricks4.png)

## <a name="next-steps"></a>Sonraki adımlar
* Azure [Cosmos DB'de veri modelleme](https://docs.microsoft.com/azure/cosmos-db/modeling-data) hakkında bilgi edinin
* [Azure Cosmos DB'de verileri nasıl modelleyip bölümlendireceklerini](https://docs.microsoft.com/azure/cosmos-db/how-to-model-partition-example) öğrenin
