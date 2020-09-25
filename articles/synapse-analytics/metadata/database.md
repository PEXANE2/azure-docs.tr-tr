---
title: Paylaşılan veritabanı
description: Azure SYNAPSE Analytics, Apache Spark bir veritabanı oluşturmak için SQL isteğe bağlı (Önizleme) ve SQL havuzu altyapılarından erişilebilen bir paylaşılan meta veri modeli sağlar.
services: synapse-analytics
author: MikeRys
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: metadata
ms.date: 05/01/2020
ms.author: mrys
ms.reviewer: jrasnick
ms.custom: devx-track-csharp
ms.openlocfilehash: 58c1aea944d89872a79d0672a925b1696791c1a8
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91260861"
---
# <a name="azure-synapse-analytics-shared-database"></a>Azure SYNAPSE Analytics paylaşılan veritabanı

Azure SYNAPSE Analytics, farklı hesaplama çalışma alanı altyapılarının Spark havuzları (Önizleme) ile SQL isteğe bağlı (Önizleme) altyapısı arasında veritabanlarını ve tabloları paylaşmasına izin verir.

[!INCLUDE [synapse-analytics-preview-terms](../../../includes/synapse-analytics-preview-terms.md)]

Spark işiyle oluşturulmuş bir veritabanı, SQL isteğe bağlı altyapısı dahil, çalışma alanındaki tüm geçerli ve gelecekteki Spark havuzlarıyla (Önizleme) aynı ad ile görünür hale gelir.

Adlı Spark varsayılan veritabanı, `default` SQL isteğe bağlı bağlamı içinde adlı veritabanı olarak da görünür `default` .

Veritabanları istek üzerine zaman uyumsuz olarak eşitlendiğinden, görünene kadar bir gecikme olur.

## <a name="manage-a-spark-created-database"></a>Spark tarafından oluşturulan bir veritabanını yönetme

Spark tarafından oluşturulan veritabanlarını yönetmek için Spark 'ı kullanın. Örneğin, bir Spark havuzu işi aracılığıyla silin ve Spark ' dan tablo oluşturun.

SQL isteğe bağlı SQL kullanarak Spark tarafından oluşturulan bir veritabanında nesneler oluşturursanız veya veritabanını bırakmaya çalışırsanız, işlem başarılı olur. Ancak özgün Spark veritabanı değiştirilmez.

## <a name="how-name-conflicts-are-handled"></a>Ad çakışmalarının işlenme şekli

Spark veritabanının adı var olan bir SQL isteğe bağlı veritabanının adıyla çakışıyorsa, bir sonek SQL 'e Spark veritabanına eklenir. İsteğe bağlı SQL 'de son ek `_<workspace name>-ondemand-DefaultSparkConnector` .

Örneğin, Azure SYNAPSE çalışma alanında aranan bir Spark veritabanı varsa `mydb` `myws` ve bu adı taşıyan bir SQL isteğe bağlı veritabanı zaten mevcutsa, SQL isteğe bağlı olarak Spark veritabanının adı kullanılarak başvurulması gerekir `mydb_myws-ondemand-DefaultSparkConnector` .

> [!CAUTION]
> Dikkat: Bu davranışa bir bağımlılık uygulamanız gerekmez.

## <a name="security-model"></a>Güvenlik modeli

Spark veritabanları ve tabloları, SQL altyapısındaki eşitlenmiş temsillerinin yanı sıra, temel alınan depolama düzeyinde güvenli hale getirilir.

Bir veritabanı oluşturan güvenlik sorumlusu, veritabanının sahibi olarak değerlendirilir ve veritabanı ve nesneleri için tüm haklara sahiptir.

Bir kullanıcı veya güvenlik grubu gibi bir güvenlik sorumlusu sağlamak için, bir veritabanına erişim, dizindeki temel klasörler ve dosyalar için uygun POSIX klasörünü ve dosya izinlerini sağlayın `warehouse` . 

Örneğin, bir güvenlik sorumlusunun bir veritabanındaki bir tabloyu okuyabilmesi için, dizindeki veritabanı klasöründen başlayan tüm klasörlerin, `warehouse` `X` `R` Bu güvenlik sorumlusuna ve izinlerine sahip olması gerekir. Ayrıca, dosyalar (tablonun temel alınan veri dosyaları gibi) için izinler gerekir `R` . 

Bir güvenlik sorumlusu bir veritabanında nesne oluşturma veya nesneleri bırakma özelliğini gerektiriyorsa, `W` klasördeki klasörler ve dosyalar üzerinde ek izinler gereklidir `warehouse` .

## <a name="examples"></a>Örnekler

### <a name="create-and-connect-to-spark-database-with-sql-on-demand"></a>İsteğe bağlı SQL ile Spark veritabanı oluşturun ve bu veritabanına bağlanın

İlk `mytestdb` olarak, çalışma alanınızda zaten oluşturmuş olduğunuz Spark kümesi kullanılarak adlı yeni bir Spark veritabanı oluşturun. Bunun için, örneğin, aşağıdaki .NET Spark ifadesiyle bir Spark C# Not defteri kullanarak bunu yapabilirsiniz:

```csharp
spark.Sql("CREATE DATABASE mytestdb")
```

Kısa bir gecikmeden sonra, veritabanını isteğe bağlı SQL 'den görebilirsiniz. Örneğin, isteğe bağlı SQL 'de aşağıdaki ifadeyi çalıştırın.

```sql
SELECT * FROM sys.databases;
```

`mytestdb`Sonuçlara dahil edildiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SYNAPSE Analytics 'te paylaşılan meta veriler hakkında daha fazla bilgi edinin](overview.md)
- [Azure SYNAPSE Analytics ' paylaşılan meta veri tabloları hakkında daha fazla bilgi edinin](table.md)
