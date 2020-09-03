---
title: Azure SYNAPSE Analytics için Apache Spark bağlı hizmetlerle erişim kimlik bilgilerinin güvenliğini sağlama
description: Bu makalede, SYNAPSE Analytics için Apache Spark bağlı hizmetleri ve belirteç kitaplığı 'nı kullanarak diğer hizmetlerle güvenli bir şekilde tümleştirme hakkında kavramlar sunulmaktadır
services: synapse-analytics
author: mlee3gsd
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: spark
ms.date: 08/26/2020
ms.author: martinle
ms.reviewer: euang
ms.openlocfilehash: 21b571c859ec8ecc66c1c9a222e0648dc7f28f4f
ms.sourcegitcommit: 9c262672c388440810464bb7f8bcc9a5c48fa326
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89422353"
---
# <a name="securing-your-credentials-through-linked-services-with-the-tokenlibrary"></a>TokenLibrary ile bağlı hizmetler aracılığıyla kimlik bilgilerinizin güvenliğini sağlama
Dış kaynaklardaki verilere erişmek ortak bir modeldir. Dış veri kaynağı anonim erişime izin veriyorsa, bağlantınızı kimlik bilgileri, gizli anahtar veya bağlantı dizesiyle güvenli hale getirmeniz gerekir.  Azure SYNAPSE Analytics, bağlantı ayrıntılarını bağlı bir hizmette veya Azure Key Vault depolayarak tümleştirme işlemini basitleştirmek için bağlı hizmetler sağlar. Bağlı bir hizmet oluşturduktan sonra Apache Spark, bağlantı bilgilerini kodunuzda uygulamak için bağlantılı hizmete başvurabilir. Daha fazla bilgi için bkz. [bağlı hizmetler](../../data-factory/concepts-linked-services.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).
> [!NOTE]
> Çalışma alanınızdaki Azure Data Lake Storage dosyalara erişmek, kimlik doğrulaması için AAD PASSTHROUGH kullanır, bu nedenle TokenLibrary kullanmanız gerekmez. 


## <a name="prerequisite"></a>Önkoşul
* Bağlı hizmet-dış veri kaynağına bağlı bir hizmet oluşturmanız ve belirteç kitaplığından bağlantılı hizmete başvurmanız gerekir. [Bağlı hizmetler](../../data-factory/concepts-linked-services.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)hakkında daha fazla bilgi edinin.


## <a name="connecting-to-adls-gen2-outside-of-synapse-workspace"></a>SYNAPSE çalışma alanı dışından ADLS 2. bağlanma

SYNAPSE, Azure Data Lake Storage 2. için tümleşik bir bağlı hizmetler deneyimi sağlar.

```scala
// Scala code
val sc = spark.sparkContext
sc.hadoopConfiguration.set("spark.storage.synapse.linkedServiceName", "<LINKED SERVICE NAME>")
sc.hadoopConfiguration.set("fs.azure.account.auth.type", "SAS")
sc.hadoopConfiguration.set("fs.azure.sas.token.provider.type", "com.microsoft.azure.synapse.tokenlibrary.LinkedServiceBasedSASProvider")

val df = spark.read.csv("abfss://<CONTAINER>@<ACCOUNT>.dfs.core.windows.net/<DIRECTORY PATH>")

df.show()
```

```python
# Python code
sc._jsc.hadoopConfiguration().set("spark.storage.synapse.linkedServiceName", "<lINKED SERVICE NAME>")
sc._jsc.hadoopConfiguration().set("fs.azure.account.auth.type", "SAS")
sc._jsc.hadoopConfiguration().set("fs.azure.sas.token.provider.type", "com.microsoft.azure.synapse.tokenlibrary.LinkedServiceBasedSASProvider")

df = spark.read.csv("abfss://<CONTAINER>@<ACCOUNT>.dfs.core.windows.net/<DIRECTORY PATH>")

df.show()
```
## <a name="using-the-token-library"></a>Belirteç kitaplığını kullanma

Diğer bağlı hizmetlere bağlanmak için, TokenLibrary 'e doğrudan çağrı yapabilirsiniz.

### <a name="getconnectionstring"></a>GetConnectionString
 Bağlantı dizesini almak için <b>GetConnectionString</b> işlevini kullanın ve <b>bağlı hizmet adını</b>geçirin.

```scala
// Scala
// retrieve connectionstring from TokenLibrary

import com.microsoft.azure.synapse.tokenlibrary.TokenLibrary

val connectionString: String = TokenLibrary.getConnectionString("<LINKED SERVICE NAME>")
println(connectionString)
```

```python
# Python
# retrieve connectionstring from TokenLibrary

from pyspark.sql import SparkSession

sc = SparkSession.builder.getOrCreate()
token_library = sc._jvm.com.microsoft.azure.synapse.tokenlibrary.TokenLibrary
connection_string = token_library.getConnectionString("<LINKED SERVICE NAME>")
print(connection_string)
```
```csharp
// C#
// retrieve connectionstring from TokenLibrary

using Microsoft.Spark.Extensions.Azure.Synapse.Analytics.Utils;

string connectionString = TokenLibrary.GetConnectionString(<LINKED SERVICE NAME>);
Console.WriteLine(connectionString);
```

### <a name="getconnectionstringasmap"></a>GetConnectionStringAsMap
Belirli değerleri, bağlantı dizesindeki bir <i>anahtar = değer</i> çiftinden ayrıştırmak için 

<i>DefaultEndpointsProtocol = https; AccountName = \<AccountName> ; AccountKey =\<AccountKey></i>

<b>getConnectionStringAsMap</b> işlevini kullanın ve değeri döndürmek için anahtarı geçirin.
```scala
// Linked services can be used for storing and retreiving credentials (e.g, account key)
// Example connection string (for storage): "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
import com.microsoft.azure.synapse.tokenlibrary.TokenLibrary

val accountKey: String = TokenLibrary.getConnectionStringAsMap("<LINKED SERVICE NAME">).get("<KEY NAME>")
println(accountKey)
```

```python
# Linked services can be used for storing and retreiving credentials (e.g, account key)
# Example connection string (for storage): "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
from pyspark.sql import SparkSession

sc = SparkSession.builder.getOrCreate()
token_library = sc._jvm.com.microsoft.azure.synapse.tokenlibrary.TokenLibrary
accountKey = token_library.getConnectionStringAsMap("<LINKED SERVICE NAME>").get("<KEY NAME>")
print(accountKey)
```

## <a name="next-steps"></a>Sonraki adımlar

- [SQL havuzuna yaz](./synapse-spark-sql-pool-import-export.md)

