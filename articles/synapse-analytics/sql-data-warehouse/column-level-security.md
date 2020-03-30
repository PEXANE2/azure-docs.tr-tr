---
title: Azure Synapse için sütun düzeyinde güvenlik nedir?
description: Sütun Düzeyinde Güvenlik, müşterilerin kullanıcının yürütme bağlamına veya grup üyeliğine dayalı veritabanı tablo sütunlarına erişimi denetlemesine, uygulamanızdaki güvenliğin tasarımını ve kodlanmasına basitleştirmesine ve sütun üzerindeki kısıtlamaları uygulamanıza olanak tanır Erişim.
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
ms.openlocfilehash: 24ead458232b096a5c69ffe8b45c6298a9da9f75
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80349087"
---
# <a name="column-level-security"></a>Sütun düzeyinde Güvenlik

Sütun Düzeyinde Güvenlik, müşterilerin kullanıcının yürütme bağlamını veya grup üyeliğini temel alan tablo sütunlarına erişimi denetlemesine olanak tanır.


> [!VIDEO https://www.youtube.com/embed/OU_ESg0g8r8]
Bu video yayınlandığından beri [Satır düzeyinde Güvenlik](/sql/relational-databases/security/row-level-security?toc=%2Fazure%2Fsql-data-warehouse%2Ftoc&view=sql-server-2017) Azure Sinaps için kullanılabilir hale geldi. 

Sütun düzeyinde güvenlik, uygulamanızdaki güvenliğin tasarımını ve kodlamasını basitleştirerek hassas verileri korumak için sütun erişimini kısıtlamanıza olanak sağlar. Örneğin, belirli kullanıcıların bölümleriyle ilgili bir tablonun yalnızca belirli sütunlarına erişebilmesini sağlamak. Erişim kısıtlaması mantığı, başka bir uygulama katmanındaki verilerden uzakta değil, veritabanı katmanında bulunur. Veritabanı, herhangi bir katmandan veri erişimi denendiği her seferde erişim kısıtlamalarını uygular. Bu kısıtlama, genel güvenlik sisteminizin yüzey alanını azaltarak güvenliğinizi daha güvenilir ve sağlam hale getirir. Buna ek olarak, sütun düzeyinde güvenlik, kullanıcılara erişim kısıtlamaları uygulamak için sütunları filtrelemek için görünüm ler sunma gereksinimini de ortadan kaldırır.

[GRANT](https://docs.microsoft.com/sql/t-sql/statements/grant-transact-sql) T-SQL deyimi yle sütun düzeyinde güvenlik uygulayabilirsiniz. Bu mekanizma yla hem SQL hem de Azure Etkin Dizin (AAD) kimlik doğrulaması desteklenir.

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
