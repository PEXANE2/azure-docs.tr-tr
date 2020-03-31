---
title: Gelişmiş Tehdit Koruması - Azure portalı - MariaDB için Azure Veritabanı
description: Tehdit Koruması, veritabanına yönelik olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: de056f61337311642589ba937c6d9842d8354d36
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79530147"
---
# <a name="advanced-threat-protection-for-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı için Gelişmiş Tehdit Koruması

MariaDB için Azure Veritabanı için Gelişmiş Tehdit Koruması, veritabanlarına erişmek veya veritabanlarından yararlanmak için olağandışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar.

Gelişmiş Tehdit Koruması, gelişmiş güvenlik yetenekleri için birleşik bir paket olan Gelişmiş Veri Güvenliği teklifinin bir parçasıdır. Gelişmiş Tehdit Koruması'na [Azure portalı](https://portal.azure.com)üzerinden erişilebilir ve yönetilebilir.

> [!IMPORTANT]
> Gelişmiş Tehdit Koruması genel önizlemede. Bu özellik, Genel Amaç ve Bellek Optimize Edilmiş sunucular için MariaDB için Azure Veritabanı'nın dağıtıldığı Azure'un tüm bölgelerinde kullanılabilir.

> [!NOTE]
> Gelişmiş Tehdit Koruması özelliği aşağıdaki Azure devlet ve egemen bulut bölgelerinde **kullanılamaz:** US Gov Texas, US Gov Arizona, US Gov Iowa, ABD, Gov Virginia, US DoD East, US DoD Central, Germany Central, Almanya Kuzey, Çin Doğu, Çin Doğu 2. Genel ürün kullanılabilirliği için [lütfen bölgeye göre mevcut ürünleri](https://azure.microsoft.com/global-infrastructure/services/) ziyaret edin.

## <a name="set-up-threat-detection"></a>Tehdit algılamayı ayarlama
1. Azure portalını [https://portal.azure.com](https://portal.azure.com)' dan başlatın.
2. Korumak istediğiniz MariaDB sunucusu için Azure Veritabanı'nın yapılandırma sayfasına gidin. Güvenlik ayarlarında **Gelişmiş Tehdit Koruması (Önizleme) seçeneğini**belirleyin.
3. Gelişmiş **Tehdit Koruması (Önizleme)** yapılandırma sayfasında:

   - Sunucuda Gelişmiş Tehdit Koruması'nı etkinleştirin.
   - **Gelişmiş Tehdit Koruması Ayarları'nda,** metin **kutusuna uyarı** gönder'de, anormal veritabanı etkinliklerinin algılanması üzerine güvenlik uyarıları almak için e-postaların listesini sağlayın.
  
   ![Tehdit algılamayı ayarlama](./media/howto-database-threat-protection-portal/set-up-threat-protection.png)

## <a name="explore-anomalous-database-activities"></a>Anormal veritabanı etkinliklerini keşfedin

Anormal veritabanı etkinliklerinin algılanması üzerine bir e-posta bildirimi alırsınız. E-posta, anormal etkinliklerin niteliği, veritabanı adı, sunucu adı, uygulama adı ve olay saati gibi şüpheli güvenlik olayı hakkında bilgi sağlar. Buna ek olarak, e-posta, veritabanına yönelik olası tehdidi araştırmak ve azaltmak için olası nedenler ve önerilen eylemler hakkında bilgi sağlar.
 
1. Azure portalını başlatmak ve SQL veritabanında algılanan etkin tehditlere genel bir bakış sağlayan Azure Güvenlik Merkezi uyarıları sayfasını göstermek için e-postadaki **son uyarıları Görüntüle** bağlantısını tıklatın.
    
    ![Anormal faaliyet raporu](./media/howto-database-threat-protection-portal/anomalous-activity-report.png)

    Etkin tehditleri görüntüleyin:

    ![Etkin tehditler](./media/howto-database-threat-protection-portal/active-threats.png)

2. Bu tehdidi araştırmak ve gelecekteki tehditleri düzeltmek için ek ayrıntılar ve eylemler almak için belirli bir uyarıyı tıklatın.
    
    ![Belirli uyarı](./media/howto-database-threat-protection-portal/specific-alert.png)

## <a name="explore-threat-detection-alerts"></a>Tehdit algılama uyarılarını keşfedin

SQL Veritabanı Tehdit Algılama, uyarılarını [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)ile tümleştirir. Veritabanıiçindeki canlı BIR SQL tehdit algılama kutucukları ve Azure portalındaki SQL ATP bıçakları etkin tehditlerin durumunu izler.

Azure Güvenlik Merkezi uyarıları sayfasını başlatmak ve veritabanında algılanan etkin SQL tehditlerine genel bir bakış almak için **Tehdit algılama uyarısını** tıklatın.

   ![Tehdit algılama uyarısı](./media/howto-database-threat-protection-portal/threat-detection-alert-asc.png)
   

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) hakkında daha fazla bilgi edinin
* Fiyatlandırma hakkında daha fazla bilgi için [MariaDB Fiyatlandırması için Azure Veritabanı sayfasına](https://azure.microsoft.com/pricing/details/mariadb/) bakın  
