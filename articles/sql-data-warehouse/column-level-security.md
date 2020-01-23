---
title: SQL veri ambarı için sütun düzeyi güvenlik nedir?
description: Sütun düzeyinde güvenlik, müşterilerin, Kullanıcı yürütme bağlamına veya grup üyeliğine göre veritabanı tablo sütunlarına erişimi denetlemesine, uygulamanızdaki güvenliğin tasarımını ve kodlanmasını basitleştirmeye ve sütun üzerinde kısıtlamalar uygulamanıza olanak tanır erişmesini.
services: sql-data-warehouse
author: julieMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: security
ms.date: 04/02/2019
ms.author: jrasnick
ms.reviewer: igorstan, carlrab
ms.custom: seo-lt-2019
ms.openlocfilehash: 344701989a753e17d8a026f6bb771a6030bdb71f
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76513057"
---
# <a name="column-level-security"></a>Sütun düzeyinde güvenlik

Sütun düzeyinde güvenlik, müşterilerin, Kullanıcı yürütme bağlamına veya grup üyeliğine göre tablo sütunlarına erişimi denetlemesine olanak tanır.


> [!VIDEO https://www.youtube.com/embed/OU_ESg0g8r8]
Bu videonun gönderildiği [satır düzeyi GÜVENLIK](/sql/relational-databases/security/row-level-security?toc=%2Fazure%2Fsql-data-warehouse%2Ftoc&view=sql-server-2017) SQL veri ambarı için kullanılabilir hale geldi. 

Sütun düzeyinde güvenlik, uygulamanızda güvenliğin tasarımını ve kodlamasını basitleştirir ve hassas verileri korumak için sütun erişimini kısıtlamanızı sağlar. Örneğin, belirli kullanıcıların departmanıyla ilgili bir tablonun yalnızca belirli sütunlarına erişebilmesini sağlama. Erişim kısıtlama mantığı, başka bir uygulama katmanındaki verilerden uzakta değil, veritabanı katmanında bulunur. Veri erişimi herhangi bir katmandan her denendiğinde, veritabanı erişim kısıtlamalarını uygular. Bu kısıtlama, genel güvenlik sisteminizin yüzey alanını azaltarak güvenlik düzeyini daha güvenilir ve sağlam hale getirir. Ayrıca, sütun düzeyinde güvenlik, kullanıcıların erişim kısıtlamalarını kullanıcılara önceden eklemek için sütunları filtrelemeye yönelik görüntüleme gereksinimini ortadan kaldırır.

[İzin](https://docs.microsoft.com/sql/t-sql/statements/grant-transact-sql) T-SQL ifadesiyle sütun düzeyinde güvenlik uygulayabilirsiniz. Bu mekanizmaya, hem SQL hem de Azure Active Directory (AAD) kimlik doğrulaması desteklenir.

![CLS](./media/column-level-security/cls.png)

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
Aşağıdaki örnek, `TestUser` `Membership` tablosunun `SSN` sütununa erişimi nasıl kısıtlayabileceğini gösterir:

Sosyal güvenlik numaralarını depolamak için kullanılan SSN sütunuyla birlikte `Membership` tablosu oluşturun:

```sql
CREATE TABLE Membership
  (MemberID int IDENTITY,
   FirstName varchar(100) NULL,
   SSN char(9) NOT NULL,
   LastName varchar(100) NOT NULL,
   Phone varchar(12) NULL,
   Email varchar(100) NULL);
```

Hassas verilere sahip olan SSK sütunu dışındaki tüm sütunlara erişim `TestUser` izin ver:

```sql
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO TestUser;
```

`TestUser` olarak yürütülen sorgular, SSN sütununu içeriyorsa başarısız olur:

```sql
SELECT * FROM Membership;

Msg 230, Level 14, State 1, Line 12
The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'.
```

## <a name="use-cases"></a>Kullanım Örnekleri

Sütun düzeyi güvenliğin bugün nasıl kullanıldığı hakkında bazı örnekler:

- Finans hizmetleri firması, yalnızca hesap yöneticilerinin müşteri sosyal güvenlik numaralarına (SSN), telefon numaralarına ve diğer kişisel olarak tanımlanabilen bilgilere (PII) erişmesine izin verir.
- Bir sağlık hizmetleri sağlayıcısı, yalnızca doktorlarla ve bu verileri, faturalandırma departmanı üyelerinin bu verileri görüntülemesini engellerken gizli tıp kayıtlarına erişime sahip olmasına olanak sağlar.
