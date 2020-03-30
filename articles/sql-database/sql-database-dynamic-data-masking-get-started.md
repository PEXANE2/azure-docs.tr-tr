---
title: Dinamik veri maskeleme
description: Dinamik veri maskeleme, SQL Veritabanı ve Azure Synapse için ayrıcalıklı olmayan kullanıcılara maskeleyerek hassas veri maruziyetini sınırlar
services: sql-database
ms.service: sql-database
ms.subservice: security
titleSuffix: Azure SQL Database and Azure Synapse
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 02/06/2020
tags: azure-synpase
ms.openlocfilehash: e5b281d59245d8fbd32b18f4ac5fe577fc7ff309
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78192923"
---
# <a name="dynamic-data-masking-for-azure-sql-database-and-azure-synapse-analytics"></a>Azure SQL Veritabanı ve Azure Synapse Analytics için dinamik veri maskeleme

SQL Veritabanı dinamik veri maskeleme, hassas verilerin görünürlüğünü ayrıcalık sahibi olmayan kullanıcılardan gizleyerek sınırlar. 

Dinamik veri maskeleme müşterilerin uygulama katmanını çok az etkileyerek hassas verilerin ne kadarının gösterileceğini belirlemelerini sağlar ve hassas verilere yetkisiz erişimin engellenmesine yardımcı olur. Bu özellik, hassas verileri belirlenen veritabanı alanlarına yapılan sorgunun sonuç kümesinde gizleyen ancak veritabanındaki verileri değiştirmeyen ilke tabanlı bir güvenlik özelliğidir.

Örneğin, bir çağrı merkezindeki bir servis temsilcisi, arayanları kredi kartı numarasının birkaç hanesiyle tanımlayabilir, ancak bu veri öğeleri hizmet temsilcisine tam olarak maruz kalmalıdır. Herhangi bir sorgunun sonuç kümesindeki herhangi bir kredi kartı numarasının son dört hanesi hariç tümünü maskeleyen bir maskeleme kuralı tanımlanabilir. Başka bir örnek olarak, bir geliştiricinin uyumluluk düzenlemelerini ihlal etmeden sorun giderme amacıyla üretim ortamlarını sorgulayabilmesi için kişisel olarak tanımlanabilir bilgileri (PII) verileri korumak için uygun bir veri maskesi tanımlanabilir.

## <a name="dynamic-data-masking-basics"></a>Dinamik veri maskeleme temelleri

SQL Veritabanı yapılandırma bıçak veya ayarlar bıçak dinamik veri maskeleme işlemi seçerek Azure portalında dinamik bir veri maskeleme ilkesi ayarlayın. Bu özellik Azure Synapse portalı kullanılarak ayarlanamaz (Lütfen Powershell veya REST API'yi kullanın)

### <a name="dynamic-data-masking-permissions"></a>Dinamik veri maskeleme izinleri

Dinamik veri maskeleme, Azure SQL Veritabanı yöneticisi, sunucu yöneticisi veya [SQL Security Manager](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager) rolleri tarafından yapılandırılabilir.

### <a name="dynamic-data-masking-policy"></a>Dinamik veri maskeleme ilkesi

* **Maskeleme dışında bulunan SQL kullanıcıları** - SQL sorgusu sonuçlarında maskesiz veri alan bir dizi SQL kullanıcısı veya AAD kimlikleri kümesi. Yönetici ayrıcalıklarına sahip kullanıcılar her zaman maskeleme nin dışında tutulur ve orijinal verileri maskesiz olarak görür.
* **Maskeleme kuralları** - Maskelenecek belirtilen alanları ve kullanılan maskeleme işlevini tanımlayan kurallar kümesi. Belirlenen alanlar bir veritabanı şeması adı, tablo adı ve sütun adı kullanılarak tanımlanabilir.
* **Maskeleme işlevleri** - Farklı senaryolar için verilerin maruz kalmasını kontrol eden bir yöntem kümesi.

| Maskeleme Fonksiyonu | Maskeleme Mantığı |
| --- | --- |
| **Varsayılan** |**Belirlenen alanların veri türlerine göre tam maskeleme**<br/><br/>• Alanın boyutu dize veri türleri (nchar, ntext, nvarchar) için 4 karakterden azise XXXX veya daha az X kullanın.<br/>• Sayısal veri türleri için sıfır değer kullanın (bigint, bit, ondalık, int, para, sayısal, smallint, smallmoney, tinyint, float, real).<br/>• Tarih/saat veri türleri (tarih, datetime2, datetime, datetimeof, smalldatetime, time) için 01-01-1900 kullanın.<br/>• SQL varyantı için geçerli türün varsayılan değeri kullanılır.<br/>• XML için \<maskeli/> belge kullanılır.<br/>• Özel veri türleri (zaman damgası tablosu, hiyerarşiyid, GUID, ikili, görüntü, varbinary uzamsal türleri) için boş bir değer kullanın. |
| **Kredi kartı** |Belirlenen alanların son dört hanesini ortaya çıkaran ve kredi kartı biçiminde önek olarak sabit bir dize **ekleyen maskeleme yöntemi.**<br/><br/>XXXX-XXXX-XXXX-1234 |
| **E-posta** |İlk harfi ortaya çıkaran ve etki alanını e-posta adresi biçiminde sabit bir dize öneki kullanarak XXX.com değiştiren **maskeleme yöntemi.**<br/><br/>aXX@XXXX.com |
| **Rastgele sayı** |Seçilen sınırlara ve gerçek veri türlerine göre **rasgele bir sayı üreten maskeleme yöntemi.** Belirlenen sınırlar eşitse, maskeleme işlevi sabit bir sayıdır.<br/><br/>![Gezinti bölmesi](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Özel metin** |İlk ve son karakterleri ortaya çıkaran ve ortasına özel bir dolgu dizesi **ekleyen maskeleme yöntemi.** Özgün dize, açıkta kalan önek ve sonekten daha kısaysa, yalnızca dolgu dizesi kullanılır. <br/>önek[doldurma]soneki<br/><br/>![Gezinti bölmesi](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |

<a name="Anchor1"></a>

### <a name="recommended-fields-to-mask"></a>Maskelemek için önerilen alanlar

DDM öneriler motoru, maskeleme için iyi adaylar olabilir potansiyel olarak hassas alanlar olarak veritabanınızdan belirli alanları bayraklar. Portaldaki Dinamik Veri Maskeleme bıçağında veritabanınız için önerilen sütunları görürsünüz. Tek yapmanız gereken bir veya daha fazla sütun için **Maske Ekle'yi** tıklatın ve sonra bu alanlar için bir maske uygulamak için **Kaydet'i** tıklatın.

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>PowerShell cmdlets kullanarak veritabanınız için dinamik veri maskeleme ayarlayın

Bkz. [Azure SQL Veritabanı Cmdlets.](https://docs.microsoft.com/powershell/module/az.sql)

## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>REST API'yi kullanarak veritabanınız için dinamik veri maskeleme ayarlama

Bkz. [Azure SQL Veritabanı Işlemleri.](https://docs.microsoft.com/rest/api/sql/)
