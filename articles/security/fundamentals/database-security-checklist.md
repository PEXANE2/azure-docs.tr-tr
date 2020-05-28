---
title: Azure veritabanı güvenlik denetim listesi | Microsoft Docs
description: Bu makalede, Azure veritabanı güvenliği için bir dizi denetim listesi sunulmaktadır.
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
ms.openlocfilehash: fc79d7204ba360696b8d9411cd56efd09d1678dc
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84021849"
---
# <a name="azure-database-security-checklist"></a>Azure veritabanı güvenlik denetim listesi

Güvenliği artırmaya yardımcı olmak için Azure veritabanı, erişimi sınırlandırmak ve denetlemek için kullanabileceğiniz bir dizi yerleşik güvenlik denetimi içerir.

Bu modüller şunlardır:

-    IP adresine göre bağlantıyı sınırlayan [güvenlik duvarı kuralları](../../azure-sql/database/firewall-configure.md) oluşturmanıza olanak tanıyan bir güvenlik duvarı,
-    Sunucu düzeyinde güvenlik duvarı Azure portal erişilebilir
-    SSMS 'den erişilebilen veritabanı düzeyinde güvenlik duvarı kuralları
-    Güvenli bağlantı dizeleri kullanarak veritabanınıza güvenli bağlantı
-    Erişim yönetimini kullanma
-    Veri şifrelemesi
-    SQL Veritabanı denetimi
-    SQL veritabanı tehdit algılama

## <a name="introduction"></a>Giriş
Bulut bilgi işlem, birçok uygulama kullanıcısı, veritabanı yöneticileri ve programcılara alışkın olmayan yeni güvenlik paradigmalarına gerektirir. Sonuç olarak, bazı kuruluşlar, algılanan güvenlik riskleri nedeniyle veri yönetimi için bir bulut altyapısı uygulamaya yöneliktir. Bununla birlikte, Microsoft Azure ve Microsoft Azure SQL Veritabanı yerleşik güvenlik özellikleri hakkında daha iyi bir anlayışının yanı sıra bu kaygıdan çok alleviated olabilir.

## <a name="checklist"></a>Denetim Listesi
Bu denetim listesini gözden geçirmeden önce [Azure veritabanı güvenliği En Iyi Yöntemler](database-best-practices.md) makalesini okumanızı öneririz. En iyi yöntemleri anladıktan sonra bu denetim listesinden en iyi şekilde yararlanabileceksiniz. Daha sonra bu denetim listesini kullanarak Azure veritabanı güvenliği 'nde önemli sorunları giderdiğinizden emin olabilirsiniz.


|Denetim listesi kategorisi| Açıklama|
| ------------ | -------- |
|**Verileri koruma**||
| <br> Hareket/aktarım sırasında şifreleme| <ul><li>Veriler ağlara taşınırken veri şifreleme için [Aktarım Katmanı Güvenliği](https://docs.microsoft.com/windows-server/security/tls/transport-layer-security-protocol).</li><li>Veritabanı, TLS üzerinden [tds (tablosal veri akışı)](https://msdn.microsoft.com/library/dd357628.aspx) Protokolü (Aktarım Katmanı Güvenliği) tabanlı istemcilerden güvenli iletişim gerektirir.</li></ul> |
|<br>Bekleme sırasında şifreleme| <ul><li>[Saydam veri şifrelemesi](https://go.microsoft.com/fwlink/?LinkId=526242), etkin olmayan veriler fiziksel olarak herhangi bir dijital biçimde depolanır.</li></ul>|
|**Denetim erişimi**||  
|<br> Veritabanı Erişimi | <ul><li>[Kimlik](../../azure-sql/database/logins-create-manage.md) doğrulama (Azure Active Directory kimlik doğrulaması) ad kimlik doğrulaması, Azure Active Directory tarafından yönetilen kimlikleri kullanır.</li><li>[Yetkilendirme](../../azure-sql/database/logins-create-manage.md) kullanıcılara gereken en düşük ayrıcalıkları verin.</li></ul> |
|<br>Uygulama erişimi| <ul><li>[Satır düzeyi güvenlik](https://msdn.microsoft.com/library/dn765131) (güvenlik ilkesini kullanarak, bir kullanıcının kimliğine, rolüne veya yürütme bağlamına göre satır düzeyinde erişimi kısıtlayarak aynı zamanda).</li><li>[Dinamik veri maskeleme](../../azure-sql/database/dynamic-data-masking-overview.md) (Izin & ilkesi kullanılarak, ayrıcalıklı olmayan kullanıcılara maskeleyerek hassas veri pozlamasını kısıtlar)</li></ul>|
|**Proaktif Izleme**||  
| <br>& algılama izleniyor| <ul><li>[Denetim](../../sql-database/sql-database-auditing.md) , veritabanı olaylarını izler ve bunları [Azure Depolama hesabınızdaki](../../storage/common/storage-create-storage-account.md)bir denetim günlüğüne/etkinlik günlüğüne yazar.</li><li>Azure [Izleyici etkinlik günlüklerini](../../azure-monitor/platform/platform-logs-overview.md)kullanarak Azure veritabanı sistem durumunu izleyin.</li><li>[Tehdit algılama](../../azure-sql/database/threat-detection-configure.md) , veritabanına ilişkin olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar. </li></ul> |
|<br>Azure Güvenlik Merkezi| <ul><li>[Veri izleme](../../security-center/security-center-enable-auditing-on-sql-databases.md) SQL ve diğer Azure hizmetleri için merkezi bir güvenlik izleme çözümü olarak Azure Güvenlik Merkezi 'ni kullanın.</li></ul>|        

## <a name="conclusion"></a>Sonuç
Azure veritabanı, çok sayıda kuruluş ve mevzuat uyumluluk gereksinimini karşılayan bir dizi güvenlik özelliği olan sağlam bir veritabanı platformudur. Verilerinize fiziksel erişimi denetleyerek verileri kolayca koruyabilir ve Saydam Veri Şifrelemesi, hücre düzeyinde şifreleme veya satır düzeyi güvenliği olan dosya, sütun-veya satır düzeyinde veri güvenliği için çeşitli seçenekler kullanabilirsiniz. Always Encrypted Ayrıca, uygulama güncelleştirmeleri sürecini basitleştirecek şifreli verilere karşı işlemleri de mümkün bir şekilde sunar. Buna karşılık, SQL veritabanı etkinliğinin denetim günlüklerine erişim, size gereken bilgileri sağlar ve verilere nasıl ve ne zaman erişildiğini bilmenize olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar
Yalnızca birkaç basit adımda kötü amaçlı kullanıcılara ya da yetkisiz erişime karşı veritabanınızın korumasını artırabilirsiniz. Bu öğreticide şunları öğrenirsiniz:

- Sunucunuz ve veritabanınız için [güvenlik duvarı kuralları](../../azure-sql/database/firewall-configure.md) ayarlayın.
- [Şifreleme](https://docs.microsoft.com/sql/relational-databases/security/encryption/sql-server-encryption)ile verilerinizi koruyun.
- [SQL veritabanı denetimini](../../sql-database/sql-database-auditing.md)etkinleştirin.

