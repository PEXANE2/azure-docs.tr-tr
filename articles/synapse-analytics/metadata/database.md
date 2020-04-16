---
title: Azure Synapse Analytics paylaşılan veritabanı
description: Azure Synapse Analytics, Apache Spark'ta bir veritabanı oluşturmanın onu isteğe bağlı SQL (önizleme) ve SQL havuz motorlarından erişilebilir hale getireceği paylaşılan bir meta veri modeli sağlar.
services: synapse-analytics
author: MikeRys
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: mrys
ms.reviewer: jrasnick
ms.openlocfilehash: e3651467de86d3b026ab348675249f93ebf3a86a
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424149"
---
# <a name="azure-synapse-analytics-shared-database"></a>Azure Synapse Analytics paylaşılan veritabanı

Azure Synapse Analytics, farklı hesaplamalı çalışma alanı motorlarının Veritabanlarını ve tabloları Spark havuzları (önizleme), ISTEĞe bağlı (önizleme) altyapısı ve SQL havuzları arasında paylaşmasına olanak tanır.

[!INCLUDE [synapse-analytics-preview-terms](../../../includes/synapse-analytics-preview-terms.md)]

Bir Spark işiyle oluşturulan bir veritabanı, aynı adla çalışma alanındaki tüm mevcut ve gelecekteki Spark havuzlarına (önizleme) ve SQL on-demand altyapısına görünür hale gelir.

Çalışma alanında meta veri eşitlemesi etkinleştirilmiş SQL havuzları varsa veya meta veri eşitlemesi etkinleştirilmiş yeni bir SQL havuzu oluşturursanız, bu Spark oluşturulan veritabanları otomatik olarak SQL havuz veritabanında özel şemalara eşlenir. 

Her şema, ek `$` bir önek ile Spark veritabanı adı sonra adlandırılır. Kıvılcım oluşturulan veritabanında hem dış hem de yönetilen tablolar ilgili özel şema dış tablolar olarak maruz kalır.

Spark varsayılan veritabanı, `default`olarak adlandırılan , ayrıca bir veritabanı olarak SQL `default`on-demand bağlamında görünür olacak , ve meta veri senkronizasyonu şema `$default`olarak açık olan SQL havuzu veritabanlarıherhangi.

Veritabanları isteğe bağlı SQL'e eşitlendive SQL havuzları eşzamanlı olarak eşitlendi, bunlar ortaya çıkana kadar gecikme olacaktır.

## <a name="manage-a-spark-created-database"></a>Spark oluşturulan veritabanını yönetme

Spark oluşturulan veritabanlarını yönetmek için Spark'ı kullanın. Örneğin, bir Spark havuz işi aracılığıyla silin ve Içinde Spark tablolar oluşturun.

SQL isteğe bağlı olarak oluşturulan bir Spark veritabanında nesneler oluşturursanız veya veritabanını düşürmeye çalışırsanız, işlem başarılı olur. Ancak, orijinal Spark veritabanı değiştirilmez.

Senkronize şemayı BIR SQL havuzuna düşürmeyi veya içinde tablo oluşturmaya çalışırsanız, Azure bir hata döndürür.

## <a name="handling-of-name-conflicts"></a>Ad çakışmalarının işlenmesi

Bir Spark veritabanının adı varolan bir SQL isteğe bağlı veritabanının adı ile çakışıyorsa, Spark veritabanına isteğe bağlı olarak SQL'e bir sonek eklenir. İsteğe bağlı SQL'de sonek `_<workspace name>-ondemand-DefaultSparkConnector`.

Örneğin, Azure `mydb` Synapse çalışma alanında `myws` bir Spark veritabanı oluşturuluyorsa ve bu ada sahip bir SQL on-demand veritabanı zaten varsa, isteğe `mydb_myws-ondemand-DefaultSparkConnector`bağlı SQL'deki Spark veritabanının adı kullanılarak başvurulması gerekir.

> [!CAUTION]
> Dikkat: Bu davranışa bağımlılık yapmamanız gerekir.

## <a name="security-model"></a>Güvenlik modeli

Spark veritabanları ve tabloları, SQL motorlarındaki senkronize gösterimleriyle birlikte temel depolama düzeyinde güvence altına alınacaktır.

Veritabanı oluşturan güvenlik ilkesi, bu veritabanının sahibi olarak kabul edilir ve veritabanı ve nesnelerinin tüm haklarına sahiptir.

Bir kullanıcı veya güvenlik grubu gibi bir güvenlik ilkesine veritabanına erişim vermek için, `warehouse` dizindeki temel klasör ve dosyalara uygun POSIX klasörünü ve dosya izinlerini sağlayın. 

Örneğin, bir güvenlik sorumlusunun veritabanındaki tabloyu okuyabilmesi için, `warehouse` dizindeki veritabanı klasöründen başlayan `X` tüm `R` klasörlerin olması ve bu güvenlik ilkesine atanan izinlere sahip olması gerekir. Ayrıca, dosyalar (tablonun temel veri dosyaları gibi) izinler gerektirir. `R` 

Bir güvenlik ilkesi nesne oluşturma veya nesneleri veritabanına bırakma özelliği `W` gerektiriyorsa, `warehouse` klasördeki klasör ve dosyalarda ek izinler gerekir.

## <a name="examples"></a>Örnekler

### <a name="create--connect-to-spark-database---sql-on-demand"></a>Spark veritabanına & bağlantı oluşturma - SQL isteğe bağlı

İlk olarak çalışma alanınızda `mytestdb` zaten oluşturduğunuz Bir Kıvılcım kümesini kullanarak adında yeni bir Spark veritabanı oluşturun. Bunu, örneğin, Kıvılcım deyimi için aşağıdaki .NET ile bir Kıvılcım C# Not Defteri kullanarak elde edebilirsiniz:

```csharp
spark.Sql("CREATE DATABASE mytestdb")
```

Kısa bir gecikmeden sonra, veritabanını isteğe bağlı OLARAK SQL'den görebilirsiniz. Örneğin, isteğe bağlı OLARAK SQL'den aşağıdaki deyimi çalıştırın.

```sql
SELECT * FROM sys.databases;
```

Sonuçlara `mytestdb` dahil olduğunu doğrulayın.

### <a name="exposing-a-spark-database-in-a-sql-pool"></a>SQL havuzunda Bir Kıvılcım veritabanını açığa çıkarma

Önceki örnekte oluşturulan veritabanı yla, şimdi çalışma alanınızda `mysqlpool` meta veri eşitlemesini sağlayan bir SQL havuzu oluşturun.

AŞAĞıDAKI deyimi SQL `mysqlpool` havuzuna karşı çalıştırın:

```sql
SELECT * FROM sys.schema;
```

Sonuçlarda yeni oluşturulan veritabanıiçin şema doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Synapse Analytics'in paylaşılan meta verileri hakkında daha fazla bilgi edinin](overview.md)
- [Azure Synapse Analytics'in paylaşılan meta veri Tabloları hakkında daha fazla bilgi edinin](table.md)

<!-- - [Learn more about the Synchronization with SQL Analytics on-demand](overview.md)
- [Learn more about the Synchronization with SQL Analytics pools](overview.md)-->
