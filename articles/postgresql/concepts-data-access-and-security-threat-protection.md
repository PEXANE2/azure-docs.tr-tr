---
title: Gelişmiş Tehdit Koruması - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Veritabanına yönelik olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılamak için Gelişmiş Tehdit Koruması'nı kullanma hakkında bilgi edinin.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 3d86c76472580567c95d285924761e1714465d6f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74768750"
---
# <a name="advanced-threat-protection-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanında Gelişmiş Tehdit Koruması - Tek Sunucu

PostgreSQL için Azure Veritabanı Gelişmiş Tehdit Koruması, veritabanlarınıza erişme veya bunları kullanma konusunda olağandışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri tespit eder.

> [!NOTE]
> Gelişmiş Tehdit Koruması genel önizlemede.

Tehdit Koruması, gelişmiş güvenlik yetenekleri için birleşik bir paket olan Gelişmiş Tehdit Koruması (ATP) teklifinin bir parçasıdır. Gelişmiş Tehdit Koruması'na [Azure portalı](https://portal.azure.com) üzerinden veya [REST API](/rest/api/postgresql/serversecurityalertpolicies)kullanılarak erişilebilir ve yönetilebilir. Bu özellik Genel Amaçlı ve Bellek Optimize Edilmiş sunucular için kullanılabilir.

> [!NOTE]
> Gelişmiş Tehdit Koruması özelliği aşağıdaki Azure devlet ve egemen bulut bölgelerinde **kullanılamaz:** US Gov Texas, US Gov Arizona, US Gov Iowa, ABD, Gov Virginia, US DoD East, US DoD Central, Germany Central, Almanya Kuzey, Çin Doğu, Çin Doğu 2. Genel ürün kullanılabilirliği için [lütfen bölgeye göre mevcut ürünleri](https://azure.microsoft.com/global-infrastructure/services/) ziyaret edin.

## <a name="what-is-advanced-threat-protection"></a>Gelişmiş Tehdit Koruması Nedir?

PostgreSQL için Azure Veritabanı için Gelişmiş Tehdit Koruması, müşterilerin anormal etkinliklerde güvenlik uyarıları sağlayarak olası tehditleri algılamasını ve bunlara yanıt vermesine olanak tanıyan yeni bir güvenlik katmanı sağlar. Kullanıcılar, şüpheli veritabanı etkinlikleri ve olası güvenlik açıklarının yanı sıra anormal veritabanı erişimi ve sorgu desenleri hakkında bir uyarı alır. PostgreSQL için Azure Veritabanı için Gelişmiş Tehdit Koruması, uyarıları şüpheli etkinliğin ayrıntılarını içeren ve tehdidi nasıl araştırıp azaltacağı konusunda harekete geçmenizi öneren [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)ile bütünleştirir. PostgreSQL için Azure Veritabanı için Gelişmiş Tehdit Koruması, bir güvenlik uzmanı olmaya veya gelişmiş güvenlik izleme sistemlerini yönetmeye gerek kalmadan veritabanına yönelik olası tehditleri ele almamayı kolaylaştırır. 

![Gelişmiş Tehdit Koruma Konsepti](media/concepts-data-access-and-security-threat-protection/advanced-threat-protection-concept.png)

## <a name="advanced-threat-protection-alerts"></a>Gelişmiş Tehdit Koruması uyarıları 
PostgreSQL için Azure Veritabanı için Gelişmiş Tehdit Koruması, veritabanlarına erişmek veya bu veritabanından yararlanmak için olağandışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar ve aşağıdaki uyarıları tetikleyebilir:
- **Olağandışı konumdan erişim**: Bu uyarı, birinin alışılmadık bir coğrafi konumdan PostgreSQL sunucusu için Azure Veritabanı'nda oturum açtığı PostgreSQL sunucusu için Azure Veritabanı'na erişim deseninde bir değişiklik olduğunda tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni bir uygulama veya geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Olağandışı Azure veri merkezinden erişim**: Bu uyarı, birisinin son dönemde bu sunucuda görülen alışılmadık bir Azure veri merkezinden sunucuya oturum açtığı PostgreSQL sunucusu için Azure Veritabanı'na erişim deseninde bir değişiklik olduğunda tetiklenir. Bazı durumlarda, uyarı yasal bir eylemi algılar (Azure'daki yeni uygulamanız, Power BI, PostgreSQL Sorgu Düzenleyicisi için Azure Veritabanı). Diğer durumlarda, uyarı Azure kaynağı/hizmetinden kaynaklanan kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Yabancı anaparadan erişim**: Bu uyarı, birinin alışılmadık bir temel (PostgreSQL kullanıcısı için Azure Veritabanı) kullanarak sunucuda oturum açtığı PostgreSQL sunucusu için Azure Veritabanı'na erişim deseninde bir değişiklik olduğunda tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni uygulama ve geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Zararlı olabilecek bir uygulamadan erişim**: Bu uyarı, zararlı olabilecek bir uygulama veritabanına erişmeye çalıştığında tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, yarı yaygın saldırı araçlarının kullandığı saldırıları algılar.
- **PostgreSQL kimlik bilgileri için Kaba kuvvet Azure Veritabanı**: Bu uyarı, farklı kimlik bilgilerine sahip anormal yüksek sayıda başarısız giriş olduğunda tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, uyarı deneme yanılma saldırılarını algılar.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) hakkında daha fazla bilgi edinin
* Fiyatlandırma hakkında daha fazla bilgi için [PostgreSQL Fiyatlandırması için Azure Veritabanı sayfasına](https://azure.microsoft.com/pricing/details/postgresql/) bakın 
* Azure portalını kullanarak [PostgreSQL Gelişmiş Tehdit Koruması için Azure Veritabanını](howto-database-threat-protection-portal.md) Yapılandır  
