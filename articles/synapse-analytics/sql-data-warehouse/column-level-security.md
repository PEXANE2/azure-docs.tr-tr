---
title: Azure Synapse için sütun düzeyinde güvenlik nedir?
description: Sütun Düzeyinde Güvenlik, müşterilerin kullanıcının yürütme bağlamına veya grup üyeliğine dayalı veritabanı tablo sütunlarına erişimi denetlemesine, uygulamanızdaki güvenliğin tasarımını ve kodlanmasıbasitleştirilmesini ve sütun erişimine kısıtlamalar uygulamanıza olanak tanır.
services: synapse-analytics
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/05/2020
ms.author: jrasnick
ms.reviewer: igorstan, carlrab
ms.custom: seo-lt-2019
tags: azure-synapse
ms.openlocfilehash: 61a3e2eadaf79cdb30a931b31cff709298d0a22c
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631291"
---
# <a name="column-level-security"></a>Sütun düzeyinde Güvenlik

Sütun Düzeyinde Güvenlik, müşterilerin kullanıcının yürütme bağlamını veya grup üyeliğini temel alan tablo sütunlarına erişimi denetlemesine olanak tanır.

> [!VIDEO https://www.youtube.com/embed/OU_ESg0g8r8]
Bu video yayınlandığından beri [Satır düzeyinde Güvenlik](/sql/relational-databases/security/row-level-security?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) Azure Sinaps için kullanılabilir hale geldi.

Sütun düzeyinde güvenlik, uygulamanızdaki güvenliğin tasarımını ve kodlamasını basitleştirerek hassas verileri korumak için sütun erişimini kısıtlamanıza olanak sağlar. Örneğin, belirli kullanıcıların bölümleriyle ilgili bir tablonun yalnızca belirli sütunlarına erişebilmesini sağlamak. Erişim kısıtlaması mantığı, başka bir uygulama katmanındaki verilerden uzakta değil, veritabanı katmanında bulunur. Veritabanı, herhangi bir katmandan veri erişimi denendiği her seferde erişim kısıtlamalarını uygular. Bu kısıtlama, genel güvenlik sisteminizin yüzey alanını azaltarak güvenliğinizi daha güvenilir ve sağlam hale getirir. Buna ek olarak, sütun düzeyinde güvenlik, kullanıcılara erişim kısıtlamaları uygulamak için sütunları filtrelemek için görünüm ler sunma gereksinimini de ortadan kaldırır.

[GRANT](/sql/t-sql/statements/grant-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) T-SQL deyimi yle sütun düzeyinde güvenlik uygulayabilirsiniz. Bu mekanizma yla hem SQL hem de Azure Etkin Dizin (AAD) kimlik doğrulaması desteklenir.

![Cls](./media/column-level-security/cls.png)

## <a name="syntax"></a>Sözdizimi

```sql
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

Aşağıdaki örnek, tablonun `TestUser` `SSN` `Membership` sütununa erişimin nasıl kısıtlanır olduğunu gösterir:

Sosyal `Membership` güvenlik numaralarını depolamak için kullanılan SSN sütunlu tablo oluşturma:

```sql
CREATE TABLE Membership
  (MemberID int IDENTITY,
   FirstName varchar(100) NULL,
   SSN char(9) NOT NULL,
   LastName varchar(100) NOT NULL,
   Phone varchar(12) NULL,
   Email varchar(100) NULL);
```

Hassas `TestUser` verilere sahip SSN sütunu dışındaki tüm sütunlara erişmene izin verin:

```sql
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO TestUser;
```

SSN `TestUser` sütununa dahil edildiklerinde başarısız olacak şekilde yürütülen sorgular:

```sql
SELECT * FROM Membership;

Msg 230, Level 14, State 1, Line 12
The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'.
```

## <a name="use-cases"></a>Kullanım Örnekleri

Sütun düzeyinde güvenliğin günümüzde nasıl kullanıldığına dair bazı örnekler:

- Bir finansal hizmet firması, yalnızca hesap yöneticilerinin müşteri sosyal güvenlik numaralarına (SSN), telefon numaralarına ve diğer kişisel olarak tanımlanabilir bilgilere (PII) erişebilmelerine olanak tanır.
- Bir sağlık kuruluşu, faturalandırma departmanı üyelerinin bu verileri görüntülemesini engellerken yalnızca doktor ve hemşirelerin hassas tıbbi kayıtlara erişmesine izin verir.
