---
title: Synapse SQL içinde kullanıcı tanımlı şemalar
description: Aşağıdaki bölümlerde, Azure Synapse Analytics'in Synapse SQL özelliğiyle çözümler geliştirmek için T-SQL kullanıcı tanımlı şemaları kullanmak için çeşitli ipuçları bulacaksınız.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: ac4753da1405fe6b8cd209bb4899192e9f317aa1
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81428712"
---
# <a name="user-defined-schemas-within-synapse-sql"></a>Synapse SQL içinde kullanıcı tanımlı şemalar

Aşağıdaki bölümlerde, Synapse SQL içinde çözümler geliştirmek için T-SQL kullanıcı tanımlı şemaları kullanmak için çeşitli ipuçları bulacaksınız.

## <a name="schemas-for-application-boundaries"></a>Uygulama sınırları için şemalar

Geleneksel analitik mimarisi genellikle iş yükü, etki alanı veya güvenliğe dayalı uygulama sınırları oluşturmak için ayrı veritabanları kullanır. Örneğin, geleneksel bir SQL Server analiz altyapısı bir evreleme veritabanı, bir analiz veritabanı ve veri mart veritabanları içerebilir. Bu topolojide, her veritabanı mimaride bir iş yükü ve güvenlik sınırı olarak çalışır.

Bunun yerine, Synapse SQL tüm analitik iş yükünü tek bir veritabanında çalıştırın. Çapraz veritabanı birleştirmelerine izin verilmez. Synapse SQL, ambar tarafından kullanılan tüm tabloların tek bir veritabanında depolanmasını bekler.

> [!NOTE]
> SQL havuzları herhangi bir veritabanı sorguları çapraz desteklemez. Sonuç olarak, bu modelden yararlanan analiz uygulamalarının gözden geçirilmesi gerekir. SQL isteğe bağlı (önizleme) çapraz veritabanı sorgularını destekler.

## <a name="user-defined-schema-recommendations"></a>Kullanıcı tanımlı şema önerileri

Kullanıcı tanımlı şemalar kullanarak iş yüklerini, güvenlik, etki alanı ve işlevsel sınırları birleştirme önerileri dahildir:

- Tüm analitik iş yükünüzü çalıştırmak için tek bir veritabanı kullanın.
- Tek bir veritabanı nı kullanmak için mevcut analiz ortamınızı birleştirin.
- Daha önce veritabanları kullanılarak uygulanan sınırı sağlamak için **kullanıcı tanımlı şemalardan** yararlanın.

Kullanıcı tanımlı şemalar daha önce kullanılmamışsa, temiz bir sayfanız vardır. Synapse SQL veritabanında kullanıcı tanımlı şemalarınızın temeli olarak eski veritabanı adını kullanın.

Şemalar zaten kullanılmışsa, birkaç seçeneğiniz vardır:

- Eski şema adlarını kaldırın ve yeni bir başlangıç
- Eski şema adlarını, eski şema adını tablo adına önceden bekleyene göre tutun
- Eski şema yapısını yeniden oluşturan ek bir şemada tablo üzerinde görünümler uygulayarak eski şema adlarını koruyun.

> [!NOTE]
> İlk incelemede, seçenek 3 en cazip seçim gibi görünebilir. Görünümler yalnızca Synapse SQL'de okunur. Herhangi bir veri veya tablo değişikliği taban tablokarşı yapılması gerekir. Seçenek 3, sisteminize bir görünüm katmanı da sunar. Mimarinizdeki görünümleri zaten kullanıyorsanız, bu konuda ek bir düşünce vermek isteyebilirsiniz.
> 
> 

### <a name="examples"></a>Örnekler

Veritabanı adlarına göre kullanıcı tanımlı şemalar uygulayın.

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the analytics
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create analytics tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Eski şema adlarını tablo adına önceden bekleterek saklayın. İş yükü sınırı için şemakullanın.

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the analytics boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the analytics boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Görünümleri kullanarak eski şema adlarını koruyun.

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the analytics boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base analytics tables in the analytics boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [!NOTE]
> Şema stratejisindeki herhangi bir değişiklik, veritabanı için güvenlik modelinin gözden geçirilmesini gerektirir. Çoğu durumda, şema düzeyinde izinler atayarak güvenlik modelini basitleştirebilirsiniz.

Daha fazla tanecikli izin gerekiyorsa, veritabanı rollerini kullanabilirsiniz. Veritabanı rolleri hakkında daha fazla bilgi için [veritabanı rollerini ve kullanıcı makalesini yönet'e](../../analysis-services/analysis-services-database-users.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipucu için [Synapse SQL geliştirme genel bakış'a](develop-overview.md)bakın.
