---
title: Azure Web uygulaması güvenlik duvarındaki yenilikler
description: En son sürüm notları, bilinen sorunlar, hata düzeltmeleri, kullanım dışı işlevler ve yaklaşan değişiklikler gibi Azure Web uygulaması güvenlik duvarı yenilikleri hakkında bilgi edinin.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.topic: overview
ms.date: 10/24/2019
ms.author: victorh
ms.openlocfilehash: 5033d19de2321e0dfd3b6d89d2da3306b1723bd0
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86146597"
---
# <a name="whats-new-in-azure-web-application-firewall"></a>Azure Web uygulaması güvenlik duvarındaki yenilikler nelerdir?

Azure Web uygulaması güvenlik duvarı sürekli olarak güncelleştirilir. En son gelişmelerden haberdar olmak için, bu makalede hakkında bilgi verilmektedir:

- En son yayınlar
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlevsellik

## <a name="new-features"></a>Yeni özellikler

|Özellik  |Açıklama  |Eklenme tarihi  |
|---------|---------|---------|
|Site başına ilke GA| Site başına ilke artık genel kullanıma sunulmuştur. | Temmuz 2020 |
|Bot azaltma RuleSet (Önizleme)|Seçtiğiniz yönettiğiniz RuleSet 'in yanı sıra bir bot Azaltma kuralı kümesini etkinleştirebilirsiniz. | Kasım 2019 |
|GeoDB tümleştirmesi (Önizleme)|Artık trafiği ülkeye/bölgeye göre sınırlayan özel kurallar oluşturabilirsiniz. | Kasım 2019 |
|Dakikada WAF-site/URI ilkesi (Önizleme)|WAF-v2 artık dinleyicilerine bir ilke uygulanmasını ve yol tabanlı kuralları desteklemektedir. Bkz. [WAF Ilkesi oluşturma](create-waf-policy-ag.md). | Kasım 2019 |
|WAF özel kuralları |Application Gateway WAF_v2 artık özel kurallar oluşturmayı destekliyor. Bkz. [Application Gateway özel kurallar](custom-waf-rules-overview.md). |Haziran 2019 |
|WAF yapılandırma ve dışlama listesi     |WAF 'nizi yapılandırmanıza ve hatalı pozitif sonuçları azaltmanıza yardımcı olacak daha fazla seçenek ekledik. Daha fazla bilgi için bkz. [Web uygulaması güvenlik duvarı istek boyutu sınırları ve dışlama listeleri](application-gateway-waf-configuration.md) .|Aralık 2018|

## <a name="next-steps"></a>Sonraki adımlar

Application Gateway Web uygulaması güvenlik duvarı hakkında daha fazla bilgi için bkz. Azure ['Da Azure Web uygulaması güvenlik duvarı Application Gateway](ag-overview.md).
