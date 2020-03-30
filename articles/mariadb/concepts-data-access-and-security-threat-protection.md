---
title: Gelişmiş Tehdit Koruması - MariaDB için Azure Veritabanı
description: Gelişmiş Tehdit Koruması, veritabanına yönelik olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 8485907eba75f80c8f0ed4fd0cc7368c6147b9fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79532187"
---
# <a name="azure-database-for-mariadb-advanced-threat-protection"></a>MariaDB Gelişmiş Tehdit Koruması için Azure Veritabanı

MariaDB için Azure Veritabanı için Gelişmiş Tehdit Koruması, veritabanlarına erişmek veya veritabanlarından yararlanmak için olağandışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar.

> [!IMPORTANT]
> Gelişmiş Tehdit Koruması genel önizlemede.

Gelişmiş Tehdit Koruması, gelişmiş güvenlik yetenekleri için birleşik bir paket olan Gelişmiş Veri Güvenliği teklifinin bir parçasıdır. Gelişmiş Tehdit Koruması'na [Azure portalı](https://portal.azure.com)üzerinden erişilebilir ve yönetilebilir. Bu özellik Genel Amaçlı ve Bellek Optimize Edilmiş sunucular için kullanılabilir.

> [!NOTE]
> Gelişmiş Tehdit Koruması özelliği aşağıdaki Azure devlet ve egemen bulut bölgelerinde **kullanılamaz:** US Gov Texas, US Gov Arizona, US Gov Iowa, ABD, Gov Virginia, US DoD East, US DoD Central, Germany Central, Almanya Kuzey, Çin Doğu, Çin Doğu 2. Genel ürün kullanılabilirliği için [lütfen bölgeye göre mevcut ürünleri](https://azure.microsoft.com/global-infrastructure/services/) ziyaret edin.


## <a name="what-is-advanced-threat-protection"></a>Gelişmiş Tehdit Koruması Nedir?

MariaDB için Azure Veritabanı için Gelişmiş Tehdit Koruması, müşterilerin anormal etkinliklerde güvenlik uyarıları sağlayarak olası tehditleri algılamasını ve bunlara yanıt vermesine olanak tanıyan yeni bir güvenlik katmanı sağlar. Kullanıcılar, şüpheli veritabanı etkinlikleri ve olası güvenlik açıklarının yanı sıra anormal veritabanı erişimi ve sorgu desenleri hakkında bir uyarı alır. MariaDB için Azure Veritabanı için Gelişmiş Tehdit Koruması, uyarıları şüpheli etkinliğin ayrıntılarını içeren ve tehdidi nasıl araştırıp azaltacağı konusunda harekete geçmenizi öneren [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)ile tümleştirir. MariaDB için Azure Veritabanı için Gelişmiş Tehdit Koruması, bir güvenlik uzmanı olmaya veya gelişmiş güvenlik izleme sistemlerini yönetmeye gerek kalmadan veritabanına yönelik olası tehditleri ele almamayı kolaylaştırır. 

![Gelişmiş Tehdit Koruma Konsepti](media/concepts-data-access-and-security-threat-protection/advanced-threat-protection-concept.png)

## <a name="advanced-threat-protection-alerts"></a>Gelişmiş Tehdit Koruması uyarıları 
MariaDB için Azure Veritabanı için Gelişmiş Tehdit Koruması, veritabanlarına erişmek veya veritabanından yararlanmak için olağandışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar ve aşağıdaki uyarıları tetikleyebilir:
- **Olağandışı konumdan erişim**: Bu uyarı, mariadb sunucusu için Azure Veritabanı'na erişim deseninde bir değişiklik olduğunda tetiklenir ve burada birisi alışılmadık bir coğrafi konumdan MariaDB sunucusu için Azure Veritabanı'nda oturum açmıştır. Bazı durumlarda uyarı güvenli işlemleri (yeni bir uygulama veya geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Olağandışı Azure veri merkezinden erişim**: Bu uyarı, mariadb sunucusu için Azure Veritabanı'na erişim deseninde bir değişiklik olduğunda tetiklenir ve burada birisi son dönemde bu sunucuda görülen alışılmadık bir Azure veri merkezinden sunucuya oturum açmıştır. Bazı durumlarda, uyarı yasal bir eylem algılar (Azure' daki yeni uygulamanız, Power BI). Diğer durumlarda, uyarı Azure kaynağı/hizmetinden kaynaklanan kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Yabancı anaparadan erişim**: Bu uyarı, birinin alışılmadık bir temel (MariaDB kullanıcısı için Azure Veritabanı) kullanarak sunucuda oturum açtığı MariaDB sunucusu için Azure Veritabanı'na erişim deseninde bir değişiklik olduğunda tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni uygulama ve geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Zararlı olabilecek bir uygulamadan erişim**: Bu uyarı, zararlı olabilecek bir uygulama veritabanına erişmeye çalıştığında tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, yarı yaygın saldırı araçlarının kullandığı saldırıları algılar.
- **MariaDB kimlik bilgileri için Kaba kuvvet Azure Veritabanı**: Bu uyarı, farklı kimlik bilgilerine sahip anormal yüksek sayıda başarısız giriş olduğunda tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, uyarı deneme yanılma saldırılarını algılar.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) hakkında daha fazla bilgi edinin
* Fiyatlandırma hakkında daha fazla bilgi için [MariaDB Fiyatlandırması için Azure Veritabanı sayfasına](https://azure.microsoft.com/pricing/details/mariadb/) bakın 
* Azure portalını kullanarak [MariaDB Gelişmiş Tehdit Koruması için Azure Veritabanını](howto-database-threat-protection-portal.md) yapılandırın  
