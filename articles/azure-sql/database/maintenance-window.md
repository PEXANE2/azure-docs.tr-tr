---
title: Bakım Penceresi
description: Azure SQL veritabanı ve yönetilen örnek Bakımı penceresinin nasıl yapılandırılabileceğini anlayın.
services: sql-database
ms.service: sql-db-mi
ms.subservice: service
ms.topic: conceptual
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: sstein
ms.custom: references_regions
ms.date: 03/02/2021
ms.openlocfilehash: 9dc4d17ea95362dd915bd1dfdfd82f4cdec611b8
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101692819"
---
# <a name="maintenance-window-preview"></a>Bakım penceresi (Önizleme)
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Bakım penceresi özelliği, [Azure SQL veritabanı](sql-database-paas-overview.md) ve [SQL yönetilen örneği](../managed-instance/sql-managed-instance-paas-overview.md)için öngörülebilir bakım penceresi zamanlamalarının yapılandırılmasına izin verir. 

Bakım olayları hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı ve Azure SQL yönetilen örneği 'Nde Azure bakım olaylarını planlayın](planned-maintenance.md).

## <a name="overview"></a>Genel Bakış

Azure, temel alınan donanım, temel işletim sistemi (OS) gibi yazılımlar ve SQL altyapısı dahil olmak üzere Azure SQL veritabanı ve SQL yönetilen örnek kaynakları üzerinde düzenli olarak planlı bakım güncelleştirmeleri gerçekleştirir. Bakım güncelleştirmesi sırasında kaynaklar tamamen kullanılabilir ve erişilebilir durumdadır, ancak bakım güncelleştirmelerinin bazıları, bakım güncelleştirmelerinin uygulanması için kısa bir süre (ortalama süre içinde sekiz saniye) uygulamak üzere örnekleri çevrimdışı duruma getirmek için yük devretme gerektirir.  Planlı bakım güncelleştirmeleri ortalama olarak her 35 günde bir gerçekleşir. Bu, müşterinin her bir Azure SQL veritabanı veya SQL yönetilen örneği başına aylık bir planlanmış bakım olayı beklemesine ve yalnızca müşteri tarafından seçilen bakım penceresi yuvaları sırasında gerçekleşmesine yol açabilir.   

Bakım penceresi, planlı bakım olaylarının sonucunda ortaya çıkan, aralıklı bağlantı sorunlarına dayanıklı olmayan iş yükleri için tasarlanmıştır.

Bakım penceresi Azure portal, PowerShell, CLı veya Azure API 'SI kullanılarak yapılandırılabilir. Bu, oluşturma sırasında veya mevcut SQL veritabanları ve SQL yönetilen örnekleri için yapılandırılabilir.

### <a name="gain-more-predictability-with-maintenance-window"></a>Bakım penceresiyle daha öngörülebilirlik kazanın

Varsayılan olarak, tüm Azure SQL veritabanları ve yönetilen örnek veritabanları, en yüksek iş saatleri kesintilerini önlemek için günlük olarak yalnızca 5 PM ile 8:00:00 yerel kez güncelleştirilir. Yerel saat, kaynağı barındıran [Azure bölgesi](https://azure.microsoft.com/global-infrastructure/geographies/) tarafından belirlenir. İki ek bakım penceresi yuvası arasından seçim yaparak, bakım güncelleştirmelerini veritabanınıza uygun bir zamana ayarlayabilirsiniz:

* **Varsayılan** pencere, 5 pm-8:00:00 yerel saat Pazartesi-Pazar 
* Hafta içi pencere, 10PM-6:00:00 yerel saat Pazartesi – Perşembe
* Hafta sonu penceresi, 10:00-00 yerel saat Cuma-Pazar

Bakım penceresi seçimi yapıldıktan sonra, tüm planlı bakım güncelleştirmeleri yalnızca sizin tercih ettiğiniz pencerede gerçekleşmeyecektir.   

> [!Note]
> Planlı bakım güncelleştirmelerine ek olarak, nadir koşullarda planlanmamış bakım olayları kullanılamaz duruma neden olabilir. 

### <a name="cost-and-eligibility"></a>Maliyet ve uygunluk

Bir bakım penceresinin seçilmesi şu abonelik [teklifi türleri](https://azure.microsoft.com/support/legal/offer-details/)için ücretsizdir: Kullandıkça öde, bulut çözümü sağlayıcısı (CSP), Microsoft Enterprise veya Microsoft Müşteri Sözleşmesi.

> [!Note]
> Azure teklifi, sahip olduğunuz Azure aboneliğinin türüdür. Örneğin, [Kullandıkça Öde tarifesine](https://azure.microsoft.com/offers/ms-azr-0003p/)sahip bir abonelik, [Open ile Azure](https://azure.microsoft.com/en-us/offers/ms-azr-0111p/)ve [Visual Studio Enterprise](https://azure.microsoft.com/en-us/offers/ms-azr-0063p/) tüm Azure tekliflerdir. Her teklif veya planın farklı hüküm ve avantajları vardır. Teklifiniz veya planınız, aboneliğin Genel Bakış sayfasında gösterilir. Aboneliğinizi farklı bir teklifle değiştirme hakkında daha fazla bilgi için bkz. [Azure aboneliğinizi farklı bir teklifle değiştirme](/azure/cost-management-billing/manage/switch-azure-offer).

## <a name="advance-notifications"></a>Bildirimleri ilerlet

Bakım bildirimleri, Yaklaşan planlı bakım olaylarına ilişkin müşterileri, bakım sırasında ve bakım penceresi tamamlandığında, gelecek şekilde 24 saat önce uyaracak şekilde yapılandırılabilir. Daha fazla bilgi için bkz. [ön bildirimler](advance-notifications.md).

## <a name="availability"></a>Kullanılabilirlik

### <a name="supported-service-level-objectives"></a>Desteklenen hizmet düzeyi amaçları

Varsayılan dışında bir bakım penceresi seçmek, **aşağıdakiler hariç** tüm SLOS üzerinde kullanılabilir:
* Hiper Ölçek 
* Eski 4. nesil sanal çekirdek
* Temel, S0 ve S1 
* DC, Fsv2, d serisi

### <a name="azure-region-support"></a>Azure bölge desteği

Varsayılan dışında bir bakım penceresinin seçilmesi şu bölgelerde kullanılabilir:

- Doğu Avustralya
- Avustralya Güneydoğu
- Güney Brezilya
- Orta Kanada
- Central US
- Doğu ABD
- Doğu ABD 2
- Doğu Japonya
- Kuzey Orta ABD
- Kuzey Avrupa
- Orta Güney ABD
- Güneydoğu Asya
- Güney Birleşik Krallık
- West Europe
- Batı ABD
- Batı ABD 2

## <a name="gateway-maintenance-for-azure-sql-database"></a>Azure SQL veritabanı için ağ geçidi Bakımı

Bakım pencerelerinin en büyük avantajlarından yararlanabilmek için, istemci uygulamalarınızın yeniden yönlendirme bağlantı ilkesini kullandığınızdan emin olun. Yeniden yönlendirme, istemcilerin veritabanını barındıran düğüme doğrudan bağlantı kurmasını, azaltılmış gecikme süresine ve geliştirilmiş aktarım hızına kadar bir bağlantı kurmasını sağlar.  

* Azure SQL veritabanı 'nda, proxy bağlantı ilkesini kullanan tüm bağlantılar, seçilen bakım penceresi ve bir ağ geçidi düğümü bakım penceresinden etkilenebilir. Ancak, önerilen yeniden yönlendirme bağlantı ilkesini kullanan istemci bağlantıları, bir ağ geçidi düğümü Bakımı yük devretmesinin etkilenmemiştir. 

* Azure SQL yönetilen örneği 'nde, ağ geçidi düğümleri [sanal kümede](../../azure-sql/managed-instance/connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture) yer alan ve yönetilen örnekle aynı bakım penceresine sahip olduğundan, proxy bağlantı ilkesinin kullanılması ek bir bakım penceresine bağlantı sunmayabilir.

Azure SQL veritabanı 'nda istemci bağlantı ilkesi hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı bağlantı ilkesi](../database/connectivity-architecture.md#connection-policy). 

Azure SQL yönetilen örneği 'nde istemci bağlantı ilkesi hakkında daha fazla bilgi için bkz. [Azure SQL yönetilen örnek bağlantı türleri](../../azure-sql/managed-instance/connection-types-overview.md).


## <a name="next-steps"></a>Sonraki adımlar

* [Bildirimleri ilerlet](advance-notifications.md)
* [Bakım penceresini Yapılandır](maintenance-window-configure.md)

## <a name="learn-more"></a>Daha fazla bilgi edinin

* [Bakım penceresi hakkında SSS](maintenance-window-faq.yml)
* [Azure SQL Veritabanı](sql-database-paas-overview.md) 
* [SQL Yönetilen Örnek](../managed-instance/sql-managed-instance-paas-overview.md)
* [Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde Azure bakım olaylarını planlayın](planned-maintenance.md)




