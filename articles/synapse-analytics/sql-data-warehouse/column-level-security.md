---
title: Azure SYNAPSE için sütun düzeyi güvenlik nedir?
description: Sütun düzeyinde güvenlik, müşterilerin, Kullanıcı yürütme bağlamına veya grup üyeliğine göre veritabanı tablo sütunlarına erişimi denetlemesine, uygulamanızdaki güvenliğin tasarımını ve kodlanmasını basitleştirmeye ve sütun erişimi üzerinde kısıtlamalar uygulamanıza olanak tanır.
services: synapse-analytics
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/19/2020
ms.author: jrasnick
ms.reviewer: igorstan, carlrab
ms.custom: seo-lt-2019
tags: azure-synapse
ms.openlocfilehash: b0a783ad5db86ca783ff1cebceec8d77ab528047
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81687932"
---
# <a name="column-level-security"></a>Sütun düzeyinde güvenlik

Sütun düzeyinde güvenlik, müşterilerin, Kullanıcı yürütme bağlamına veya grup üyeliğine göre tablo sütunlarına erişimi denetlemesine olanak tanır.

> [!VIDEO https://www.youtube.com/embed/OU_ESg0g8r8]
Bu videonun gönderildiği [satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) Azure SYNAPSE için kullanılabilir hale geldi.

Sütun düzeyinde güvenlik, uygulamanızda güvenliğin tasarımını ve kodlamasını basitleştirir ve hassas verileri korumak için sütun erişimini kısıtlamanızı sağlar. Örneğin, belirli kullanıcıların departmanıyla ilgili bir tablonun yalnızca belirli sütunlarına erişebilmesini sağlama. Erişim kısıtlama mantığı, başka bir uygulama katmanındaki verilerden uzakta değil, veritabanı katmanında bulunur. Veri erişimi herhangi bir katmandan her denendiğinde, veritabanı erişim kısıtlamalarını uygular. Bu kısıtlama, genel güvenlik sisteminizin yüzey alanını azaltarak güvenlik düzeyini daha güvenilir ve sağlam hale getirir. Ayrıca, sütun düzeyinde güvenlik, kullanıcıların erişim kısıtlamalarını kullanıcılara önceden eklemek için sütunları filtrelemeye yönelik görüntüleme gereksinimini ortadan kaldırır.

[İzin](/sql/t-sql/statements/grant-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) T-SQL ifadesiyle sütun düzeyinde güvenlik uygulayabilirsiniz. Bu mekanizmaya, hem SQL hem de Azure Active Directory (AAD) kimlik doğrulaması desteklenir.

![CLS](./media/column-level-security/cls.png)

## <a name="syntax"></a>Sözdizimi

```syntaxsql
GRANT <permission> [ ,...n ] ON
    [ OBJECT :: ][ schema_name ]. object_name [ ( column [ ,...n ] ) ]
    TO <database_principal> [ ,...n ]
    [ WITH GRANT OPTION ]
    [ AS <database_principal> ]
<permission> ::=
    SELECT
  | UPDATE
<database_principal> ::=
      Database_user
    | Database_role
    | Database_user_mapped_to_Windows_User
    | Database_user_mapped_to_Windows_Group
```

## <a name="example"></a>Örnek

Aşağıdaki örnek, `TestUser` `SSN` `Membership` tablosunun sütununa erişimi nasıl kısıtlayabileceğini gösterir:

Sosyal `Membership` güvenlik numaralarını depolamak IÇIN kullanılan SSK sütunuyla tablo oluşturun:

```sql
CREATE TABLE Membership
  (MemberID int IDENTITY,
   FirstName varchar(100) NULL,
   SSN char(9) NOT NULL,
   LastName varchar(100) NOT NULL,
   Phone varchar(12) NULL,
   Email varchar(100) NULL);
```

Hassas `TestUser` verilere sahıp olan SSK sütunu dışındaki tüm sütunlara erişime izin ver:

```sql
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO TestUser;
```

Olarak `TestUser` yürütülen sorgular, SSN sütununu içeriyorsa başarısız olur:

```sql
SELECT * FROM Membership;

-- Msg 230, Level 14, State 1, Line 12
-- The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'.
```

## <a name="use-cases"></a>Kullanım örnekleri

Sütun düzeyi güvenliğin bugün nasıl kullanıldığı hakkında bazı örnekler:

- Finans hizmetleri firması, yalnızca hesap yöneticilerinin müşteri sosyal güvenlik numaralarına (SSN), telefon numaralarına ve diğer kişisel olarak tanımlanabilen bilgilere (PII) erişmesine izin verir.
- Bir sağlık hizmetleri sağlayıcısı, yalnızca doktorlarla ve bu verileri, faturalandırma departmanı üyelerinin bu verileri görüntülemesini engellerken gizli tıp kayıtlarına erişime sahip olmasına olanak sağlar.
