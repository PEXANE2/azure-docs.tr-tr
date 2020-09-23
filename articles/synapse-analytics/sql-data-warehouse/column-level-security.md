---
title: Azure SYNAPSE için sütun düzeyi güvenlik nedir?
description: Sütun düzeyinde güvenlik, müşterilerin, Kullanıcı yürütme bağlamına veya grup üyeliğine göre veritabanı tablo sütunlarına erişimi denetlemesine, uygulamanızdaki güvenliğin tasarımını ve kodlanmasını basitleştirmeye ve sütun erişimi üzerinde kısıtlamalar uygulamanıza olanak tanır.
services: synapse-analytics
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/19/2020
ms.author: jrasnick
ms.reviewer: igorstan, carlrab
ms.custom: seo-lt-2019
tags: azure-synapse
ms.openlocfilehash: 1364da4849a07100ac28644a51409837012f93cc
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90986223"
---
# <a name="column-level-security"></a>Sütun düzeyinde güvenlik

Sütun düzeyinde güvenlik, müşterilerin, Kullanıcı yürütme bağlamına veya grup üyeliğine göre tablo sütunlarına erişimi denetlemesine olanak tanır.

> [!VIDEO https://www.youtube.com/embed/OU_ESg0g8r8]
Bu videonun gönderildiği [satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) Azure SYNAPSE için kullanılabilir hale geldi.

Sütun düzeyinde güvenlik, uygulamanızda güvenliğin tasarımını ve kodlamasını basitleştirir ve hassas verileri korumak için sütun erişimini kısıtlamanızı sağlar. Örneğin, belirli kullanıcıların departmanıyla ilgili bir tablonun yalnızca belirli sütunlarına erişebilmesini sağlama. Erişim kısıtlama mantığı, başka bir uygulama katmanındaki verilerden uzakta değil, veritabanı katmanında bulunur. Veri erişimi herhangi bir katmandan her denendiğinde, veritabanı erişim kısıtlamalarını uygular. Bu kısıtlama, genel güvenlik sisteminizin yüzey alanını azaltarak güvenlik düzeyini daha güvenilir ve sağlam hale getirir. Ayrıca, sütun düzeyinde güvenlik, kullanıcıların erişim kısıtlamalarını kullanıcılara önceden eklemek için sütunları filtrelemeye yönelik görüntüleme gereksinimini ortadan kaldırır.

[İzin](/sql/t-sql/statements/grant-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) T-SQL ifadesiyle sütun düzeyinde güvenlik uygulayabilirsiniz. Bu mekanizmaya, hem SQL hem de Azure Active Directory (Azure AD) kimlik doğrulaması desteklenir.

![Diyagramda, bir kapalı bir asma kilidi ile bir şematik tablo ve diğer sütunlar beyaz hücreler olduğunda, turuncu renkli bir sütun gösterilmektedir.](./media/column-level-security/cls.png)

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

Aşağıdaki örnek, `TestUser` tablosunun sütununa erişimi nasıl kısıtlayabileceğini gösterir `SSN` `Membership` :

`Membership`Sosyal güvenlik numaralarını depolamak için kullanılan SSK sütunuyla tablo oluşturun:

```sql
CREATE TABLE Membership
  (MemberID int IDENTITY,
   FirstName varchar(100) NULL,
   SSN char(9) NOT NULL,
   LastName varchar(100) NOT NULL,
   Phone varchar(12) NULL,
   Email varchar(100) NULL);
```

`TestUser`Hassas verilere sahip olan SSK sütunu dışındaki tüm sütunlara erişime izin ver:

```sql
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO TestUser;
```

Olarak yürütülen sorgular `TestUser` , SSN sütununu içeriyorsa başarısız olur:

```sql
SELECT * FROM Membership;

-- Msg 230, Level 14, State 1, Line 12
-- The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'.
```

## <a name="use-cases"></a>Kullanım Örnekleri

Sütun düzeyi güvenliğin bugün nasıl kullanıldığı hakkında bazı örnekler:

- Finans hizmetleri firması, yalnızca hesap yöneticilerinin müşteri sosyal güvenlik numaralarına (SSN), telefon numaralarına ve diğer kişisel olarak tanımlanabilen bilgilere (PII) erişmesine izin verir.
- Bir sağlık hizmetleri sağlayıcısı, yalnızca doktorlarla ve bu verileri, faturalandırma departmanı üyelerinin bu verileri görüntülemesini engellerken gizli tıp kayıtlarına erişime sahip olmasına olanak sağlar.
