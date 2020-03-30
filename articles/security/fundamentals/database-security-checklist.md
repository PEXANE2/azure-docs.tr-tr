---
title: Azure veritabanı güvenlik denetim listesi| Microsoft Dokümanlar
description: Bu makalede, Azure veritabanı güvenliği için bir denetim listesi kümesi sağlar.
services: security
documentationcenter: na
author: unifycloud
manager: barbkess
editor: tomsh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: d9283a36d5f7ccb82b2cc211485487d5a3dcce7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79201034"
---
# <a name="azure-database-security-checklist"></a>Azure veritabanı güvenlik denetim listesi

Güvenliği artırmaya yardımcı olmak için Azure Veritabanı, erişimi sınırlamak ve denetlemek için kullanabileceğiniz bir dizi yerleşik güvenlik denetimi içerir.

Bunlar:

-    IP adresine göre bağlantıyı sınırlayan [güvenlik duvarı kuralları](../../sql-database/sql-database-firewall-configure.md) oluşturmanıza olanak tanıyan bir güvenlik duvarı,
-    Azure portalından erişilebilir sunucu düzeyinde güvenlik duvarı
-    SSMS'den erişilebilen veritabanı düzeyinde güvenlik duvarı kuralları
-    Güvenli bağlantı dizeleri kullanarak veritabanınıza güvenli bağlantı
-    Erişim yönetimini kullanma
-    Veri şifrelemesi
-    SQL Veritabanı denetimi
-    SQL Veritabanı tehdit algılama

## <a name="introduction"></a>Giriş
Bulut bilgi işlem, birçok uygulama kullanıcısı, veritabanı yöneticisi ve programcının tanımadığı yeni güvenlik paradigmaları gerektirir. Sonuç olarak, bazı kuruluşlar algılanan güvenlik riskleri nedeniyle veri yönetimi için bir bulut altyapısı uygulamakta tereddüt eder. Ancak, bu endişenin çoğu, Microsoft Azure ve Microsoft Azure SQL Veritabanı'nda yerleşik güvenlik özelliklerinin daha iyi anlaşılması yoluyla azaltılabilir.

## <a name="checklist"></a>Denetim Listesi
Bu denetim listesini gözden geçirmeden önce [Azure Veritabanı Güvenliği En İyi Uygulamalar](database-best-practices.md) makalesini okumanızı öneririz. En iyi uygulamaları anladıktan sonra bu denetim listesinden en iyi şekilde birini elde edebileceksiniz. Daha sonra, Azure veritabanı güvenliğindeki önemli sorunları ele aldığınızdan emin olmak için bu denetim listesini kullanabilirsiniz.


|Kontrol Listesi Kategorisi| Açıklama|
| ------------ | -------- |
|**Verileri Koru**||
| <br> Hareket/Geçişte Şifreleme| <ul><li>Veri ağlara taşınırken veri şifreleme için [Katman Güvenliği'ni taşdırın.](https://docs.microsoft.com/windows-server/security/tls/transport-layer-security-protocol)</li><li>Veritabanı TLS (Aktarım Katmanı Güvenliği) üzerinden [TDS (Tabular Data Stream)](https://msdn.microsoft.com/library/dd357628.aspx) protokolüne dayalı istemcilerden güvenli iletişim gerektirir.</li></ul> |
|<br>Bekleme sırasında şifreleme| <ul><li>[Saydam Veri Şifrelemesi,](https://go.microsoft.com/fwlink/?LinkId=526242)etkin olmayan veriler herhangi bir dijital biçimde fiziksel olarak depolandığında.</li></ul>|
|**Kontrol Erişimi**||  
|<br> Veritabanı Erişimi | <ul><li>[Kimlik doğrulama](../../sql-database/sql-database-manage-logins.md) (Azure Etkin Dizin Kimlik Doğrulaması) AD kimlik doğrulaması, Azure Active Directory tarafından yönetilen kimlikleri kullanır.</li><li>[Yetkilendirme,](../../sql-database/sql-database-manage-logins.md) kullanıcılara gereken en az ayrıcalıkları verir.</li></ul> |
|<br>Uygulama Erişimi| <ul><li>[Satır düzeyi Güvenliği](https://msdn.microsoft.com/library/dn765131) (Güvenlik İlkesi'ni kullanma, aynı zamanda kullanıcının kimliğine, rolüne veya yürütme bağlamına göre satır düzeyinde erişimi kısıtlama).</li><li>[Dinamik Veri Maskeleme](../../sql-database/sql-database-dynamic-data-masking-get-started.md) (İzin & İlkesi kullanma, hassas veri maruziyetini ayrıcalıklı olmayan kullanıcılarla maskeleyerek sınırlar)</li></ul>|
|**Proaktif İzleme**||  
| <br>İzleme & Algılama| <ul><li>[Denetim](../../sql-database/sql-database-auditing.md) veritabanı olaylarını izler ve bunları Azure Depolama [hesabınızdaki](../../storage/common/storage-create-storage-account.md)Denetim günlüğüne/ Etkinlik günlüğüne yazar.</li><li>[Azure MonitörÜ Etkinlik Günlüklerini](../../azure-monitor/platform/platform-logs-overview.md)kullanarak Azure Veritabanı durumunu izleyin.</li><li>[Tehdit Algılama](../../sql-database/sql-database-threat-detection.md) veritabanına potansiyel güvenlik tehditleri gösteren anormal veritabanı etkinlikleri algılar. </li></ul> |
|<br>Azure Güvenlik Merkezi| <ul><li>[Veri İzleme](../../security-center/security-center-enable-auditing-on-sql-databases.md) Azure Güvenlik Merkezi'ni SQL ve diğer Azure hizmetleri için merkezi bir güvenlik izleme çözümü olarak kullanın.</li></ul>|        

## <a name="conclusion"></a>Sonuç
Azure Veritabanı, birçok kuruluş ve mevzuata uygunluk gereksinimini karşılayan çok çeşitli güvenlik özelliklerine sahip sağlam bir veritabanı platformudur. Verilerinize fiziksel erişimi denetleyerek ve Saydam Veri Şifreleme, Hücre Düzeyi Şifreleme veya Satır Düzeyi Güvenliği ile dosya, sütun veya satır düzeyinde veri güvenliği için çeşitli seçenekler kullanarak verileri kolayca koruyabilirsiniz. Her Zaman Şifrelenmiş, şifreli verilere karşı işlemlere de olanak sağlayarak uygulama güncelleştirmelerinin işlemini basitleştirir. Buna karşılık, SQL Veritabanı etkinliğinin denetim günlüklerine erişim, verilere nasıl ve ne zaman erişilebildiğini bilmenizi sağlayan, ihtiyacınız olan bilgileri sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Yalnızca birkaç basit adımda kötü amaçlı kullanıcılara ya da yetkisiz erişime karşı veritabanınızın korumasını artırabilirsiniz. Bu öğreticide şunları öğrenirsiniz:

- Sunucunuz ve veritabanınız için [güvenlik duvarı kuralları](../../sql-database/sql-database-firewall-configure.md) ayarlayın.
- Verilerinizi [şifreleme](https://docs.microsoft.com/sql/relational-databases/security/encryption/sql-server-encryption)ile koruyun.
- [SQL Veritabanı denetimini etkinleştirin.](../../sql-database/sql-database-auditing.md)

