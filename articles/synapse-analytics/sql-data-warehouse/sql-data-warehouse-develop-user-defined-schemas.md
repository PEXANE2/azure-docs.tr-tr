---
title: Kullanıcı tanımlı şemaları kullanma
description: Synapse SQL havuzunda çözümler geliştirmek için T-SQL kullanıcı tanımlı şemalar kullanma ipuçları.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 7144fa75d156ca7aed9d8215592f89c167cfb221
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633461"
---
# <a name="user-defined-schemas-in-synapse-sql-pool"></a>Synapse SQL havuzunda kullanıcı tanımlı şemalar
Bu makalede, Synapse SQL havuzunda çözümler geliştirmek için T-SQL kullanıcı tanımlı şemalar kullanmak için çeşitli ipuçları sağlamaya odaklanılmaktadır.

## <a name="schemas-for-application-boundaries"></a>Uygulama sınırları için şemalar

Geleneksel veri ambarları genellikle iş yükü, etki alanı veya güvenliğe dayalı uygulama sınırları oluşturmak için ayrı veritabanları kullanır. 

Örnek olarak, geleneksel bir SQL Server veri ambarı bir evreleme veritabanı, veri ambarı veritabanı ve bazı veri mart veritabanları içerebilir. Bu topolojide, her veritabanı mimaride bir iş yükü ve güvenlik sınırı olarak çalışır.

Bunun aksine, SQL havuzu tüm veri ambarı iş yükünü tek bir veritabanında çalıştırZ. Çapraz veritabanı birleştirmelerine izin verilmez. SQL havuzu, ambar tarafından kullanılan tüm tabloların tek bir veritabanında depolanmasını bekler.

> [!NOTE]
> SQL havuzu her türlü çapraz veritabanı sorgularını desteklemez. Sonuç olarak, bu modelden yararlanan veri ambarı uygulamalarının gözden geçirilmesi gerekir.
> 
> 

## <a name="recommendations"></a>Öneriler
Aşağıda, kullanıcı tanımlı şemalar kullanarak iş yüklerini, güvenlik, etki alanı ve işlevsel sınırları birleştirme önerileri şunlardır:

- Tüm veri ambarı iş yükünüzü çalıştırmak için bir SQL havuzu veritabanı kullanın.
- Bir SQL havuzu veritabanı nı kullanmak için varolan veri ambarı ortamınızı birleştirin.
- Daha önce veritabanları kullanılarak uygulanan sınırı sağlamak için **kullanıcı tanımlı şemalardan** yararlanın.

Kullanıcı tanımlı şemalar daha önce kullanılmamışsa, temiz bir sayfanız vardır. SQL havuz veritabanında kullanıcı tanımlı şemalarınızın temeli olarak eski veritabanı adını kullanın.

Şemalar zaten kullanılmışsa, birkaç seçeneğiniz vardır:

- Eski şema adlarını kaldırın ve yeni bir başlangıç.
- Eski şema adlarını, eski şema adını tablo adına önceden bekleterek koruyun.
- Eski şema yapısını yeniden oluşturmak için tablo üzerinde görünümleri ek bir şema da uygulayarak eski şema adlarını koruyun.

> [!NOTE]
> İlk denetim seçeneği 3 en cazip seçenek gibi görünebilir. Ancak, şeytan ayrıntılı olarak. Görünümler yalnızca SQL havuzunda okunur. Herhangi bir veri veya tablo değişikliği taban tablokarşı yapılması gerekir. Seçenek 3, sisteminize bir görünüm katmanı da sunar. Mimarinizdeki görünümleri zaten kullanıyorsanız, bu konuda ek bir düşünce vermek isteyebilirsiniz.
> 
> 

### <a name="examples"></a>Örnekler:
Veritabanı adlarına göre kullanıcı tanımlı şemalar uygulayın:

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Eski şema adlarını tablo adına önceden bekleterek saklayın. İş yükü sınırı için şemakullanın:

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Görünümleri kullanarak eski şema adlarını saklayın:

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
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
> Şema stratejisindeki herhangi bir değişiklik için veritabanı nın güvenlik modelinin gözden geçirilmesi gerekir. Çoğu durumda, şema düzeyinde izinler atayarak güvenlik modelini basitleştirebilirsiniz. Daha ayrıntılı izinler gerekiyorsa, veritabanı rollerini kullanabilirsiniz.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipucu için [geliştirme genel bakış](sql-data-warehouse-overview-develop.md)ına bakın.

