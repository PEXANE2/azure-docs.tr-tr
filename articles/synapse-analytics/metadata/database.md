---
title: Azure SYNAPSE Analytics paylaşılan veritabanı
description: Azure SYNAPSE Analytics, Apache Spark bir veritabanı oluşturmak için SQL isteğe bağlı (Önizleme) ve SQL havuzu altyapılarından erişilebilen bir paylaşılan meta veri modeli sağlar.
services: synapse-analytics
author: MikeRys
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: mrys
ms.reviewer: jrasnick
ms.openlocfilehash: e3651467de86d3b026ab348675249f93ebf3a86a
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81424149"
---
# <a name="azure-synapse-analytics-shared-database"></a>Azure SYNAPSE Analytics paylaşılan veritabanı

Azure SYNAPSE Analytics, farklı hesaplama çalışma alanı altyapılarının Spark havuzları (Önizleme), SQL isteğe bağlı (Önizleme) altyapısı ve SQL havuzları arasında veritabanlarını ve tabloları paylaşmasına izin verir.

[!INCLUDE [synapse-analytics-preview-terms](../../../includes/synapse-analytics-preview-terms.md)]

Spark işiyle oluşturulmuş bir veritabanı, çalışma alanındaki tüm geçerli ve gelecekteki Spark havuzlarının (Önizleme) yanı sıra aynı adla ve SQL isteğe bağlı altyapısından görünür hale gelir.

Çalışma alanında meta veri eşitlemesi etkinleştirilmiş SQL havuzları varsa veya meta veri eşitlemesi etkinleştirilmiş yeni bir SQL havuzu oluşturursanız, bu Spark oluşturulan veritabanları, SQL havuzu veritabanında özel şemalarda otomatik olarak eşlenir. 

Her şema Spark veritabanı adından sonra ek bir ön ek `$` ile adlandırılır. Spark tarafından oluşturulan veritabanındaki dış ve yönetilen tablolar, karşılık gelen özel şemada dış tablo olarak sunulur.

Adlı `default`Spark varsayılan veritabanı, adlı `default`bir veritabanı olarak SQL isteğe bağlı bağlamda ve şema `$default`olarak meta veri eşitlemesi açık olan SQL havuzu veritabanlarında de görünür olacaktır.

Veritabanları, istek üzerine SQL ve SQL havuzlarının zaman uyumsuz olarak eşitlendiğinden, görünene kadar bir gecikme olur.

## <a name="manage-a-spark-created-database"></a>Spark tarafından oluşturulan bir veritabanını yönetme

Spark tarafından oluşturulan veritabanlarını yönetmek için Spark 'ı kullanın. Örneğin, bir Spark havuzu işi aracılığıyla silin ve Spark ' dan tablo oluşturun.

SQL isteğe bağlı SQL kullanarak Spark tarafından oluşturulan bir veritabanında nesneler oluşturursanız veya veritabanını bırakmaya çalışırsanız, işlem başarılı olur. Ancak özgün Spark veritabanı değiştirilmez.

Eşitlenmiş şemayı bir SQL havuzunda bırakmayı veya bir tablo oluşturmayı denerseniz, Azure bir hata döndürür.

## <a name="handling-of-name-conflicts"></a>Ad çakışmalarını işleme

Spark veritabanının adı var olan bir SQL isteğe bağlı veritabanının adıyla çakışıyorsa, bir sonek SQL 'e Spark veritabanına eklenir. İsteğe bağlı SQL 'de son ek `_<workspace name>-ondemand-DefaultSparkConnector`.

Örneğin, Azure SYNAPSE çalışma alanında `mydb` `myws` aranan bir Spark veritabanı varsa ve bu adı taşıyan bir SQL isteğe bağlı VERITABANı zaten mevcutsa, SQL isteğe bağlı olarak Spark veritabanının adı `mydb_myws-ondemand-DefaultSparkConnector`kullanılarak başvurulması gerekir.

> [!CAUTION]
> Dikkat: Bu davranışa bir bağımlılık uygulamanız gerekmez.

## <a name="security-model"></a>Güvenlik modeli

Spark veritabanları ve tabloları, SQL altyapılarındaki eşitlenmiş temsillerinin yanı sıra, temel alınan depolama düzeyinde güvenli hale getirilir.

Bir veritabanı oluşturan güvenlik sorumlusu, veritabanının sahibi olarak değerlendirilir ve veritabanı ve nesneleri için tüm haklara sahiptir.

Bir kullanıcı veya güvenlik grubu gibi bir güvenlik sorumlusu sağlamak için, bir veritabanına erişim, `warehouse` dizindeki temel klasörler ve dosyalar IÇIN uygun POSIX klasörünü ve dosya izinlerini sağlayın. 

Örneğin, bir güvenlik sorumlusunun bir veritabanındaki bir tabloyu okuyabilmesi için, `warehouse` dizindeki veritabanı klasöründen başlayan tüm klasörlerin, bu güvenlik sorumlusuna ve `X` `R` izinlerine sahip olması gerekir. Ayrıca, dosyalar (tablonun temel alınan veri dosyaları gibi) için izinler gerekir `R` . 

Bir güvenlik sorumlusu bir veritabanında nesne oluşturma veya nesneleri bırakma özelliğini gerektiriyorsa, `W` `warehouse` klasördeki klasörler ve dosyalar üzerinde ek izinler gereklidir.

## <a name="examples"></a>Örnekler

### <a name="create--connect-to-spark-database---sql-on-demand"></a>Spark veritabanına bağlantı oluşturma & bağlanma-isteğe bağlı SQL

İlk olarak, çalışma alanınızda zaten oluşturmuş `mytestdb` olduğunuz Spark kümesi kullanılarak adlı yeni bir Spark veritabanı oluşturun. Bunun için, örneğin, aşağıdaki .NET Spark ifadesiyle bir Spark C# Not defteri kullanarak bunu yapabilirsiniz:

```csharp
spark.Sql("CREATE DATABASE mytestdb")
```

Kısa bir gecikmeden sonra, veritabanını isteğe bağlı SQL 'den görebilirsiniz. Örneğin, isteğe bağlı SQL 'de aşağıdaki ifadeyi çalıştırın.

```sql
SELECT * FROM sys.databases;
```

`mytestdb` Sonuçlara dahil edildiğini doğrulayın.

### <a name="exposing-a-spark-database-in-a-sql-pool"></a>Bir SQL havuzunda Spark veritabanını gösterme

Önceki örnekte oluşturulan veritabanı ile, çalışma alanınızda meta veri eşitlemesini sağlayan adlı `mysqlpool` bir SQL havuzu oluşturun.

`mysqlpool` SQL havuzunda aşağıdaki ifadeyi çalıştırın:

```sql
SELECT * FROM sys.schema;
```

Sonuçlarda yeni oluşturulan veritabanının şemasını doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SYNAPSE Analytics 'te paylaşılan meta veriler hakkında daha fazla bilgi edinin](overview.md)
- [Azure SYNAPSE Analytics ' paylaşılan meta veri tabloları hakkında daha fazla bilgi edinin](table.md)

<!-- - [Learn more about the Synchronization with SQL Analytics on-demand](overview.md)
- [Learn more about the Synchronization with SQL Analytics pools](overview.md)-->
