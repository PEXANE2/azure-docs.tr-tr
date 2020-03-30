---
title: Azure Web Uygulaması Güvenlik Duvarı'ndaki yenilikler
description: En son sürüm notları, bilinen sorunlar, hata düzeltmeleri, amortismana uygun işlevler ve yaklaşan değişiklikler gibi Azure Web Uygulaması Güvenlik Duvarı'nda nelerin yeni olduğunu öğrenin.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.topic: overview
ms.date: 10/24/2019
ms.author: victorh
ms.openlocfilehash: 0e0e67bfb893e1bf141182e45ce4a49f5f6880ca
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73502250"
---
# <a name="whats-new-in-azure-web-application-firewall"></a>Azure Web Uygulaması Güvenlik Duvarı'ndaki yenilikler nelerdir?

Azure Web Uygulaması Güvenlik Duvarı sürekli olarak güncelleştirilir. En son gelişmelerden haberdar olmak için bu makalede, aşağıdakiler hakkında bilgi verilmektedir:

- En son sürümler
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlevsellik

## <a name="new-features"></a>Yeni özellikler

|Özellik  |Açıklama  |Eklenen tarih  |
|---------|---------|---------|
|Bot Azaltma Kural Kümesi (önizleme)|Seçtiğiniz CRS kural kümesinin yanı sıra bir Bot Azaltma kural kümesini etkinleştirebilirsiniz. | Kasım 2019 |
|GeoDB Tümleştirme (önizleme)|Artık trafiği menşe ülkeye göre kısıtlayan özel kurallar oluşturabilirsiniz. | Kasım 2019 |
|WAF site başına/URI başına politikası (önizleme)|WAF-v2 artık dinleyicilere bir ilkenin yanı sıra yol tabanlı kurallar uygulanmasını da destekliyor. Bkz. [WAF İlkesi Oluştur.](create-waf-policy-ag.md) | Kasım 2019 |
|WAF özel kuralları |Uygulama Ağ Geçidi WAF_v2 artık özel kurallar oluşturmayı destekler. Bkz. [Uygulama Ağ Geçidi özel kuralları.](custom-waf-rules-overview.md) |Haziran 2019 |
|WAF yapılandırma ve dışlama listesi     |WAF'ınızı yapılandırmanıza ve yanlış pozitif leri azaltmanıza yardımcı olacak daha fazla seçenek ekledik. Daha fazla bilgi için [Web uygulaması güvenlik duvarı isteği boyut sınırları ve dışlama listelerine](application-gateway-waf-configuration.md) bakın.|Aralık 2018|

## <a name="next-steps"></a>Sonraki adımlar

Uygulama Ağ Geçidi'ndeki Web Uygulaması Güvenlik Duvarı hakkında daha fazla bilgi için [Azure Uygulama Ağ Geçidi'ndeki Azure Web Uygulaması Güvenlik Duvarı'na](ag-overview.md)bakın.
