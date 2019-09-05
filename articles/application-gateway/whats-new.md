---
title: Azure Application Gateway yenilikleri
description: En son sürüm notları, bilinen sorunlar, hata düzeltmeleri, kullanım dışı işlevler ve yaklaşan değişiklikler gibi Azure Application Gateway yenilikleri hakkında bilgi edinin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: overview
ms.date: 4/30/2019
ms.author: victorh
ms.openlocfilehash: 6755825aa123b3d7d1eed23291790ff3dc477ac5
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70382475"
---
# <a name="whats-new-in-azure-application-gateway"></a>Azure Application Gateway 'daki yenilikler nelerdir?

Azure Application Gateway, sürekli olarak güncelleştirilir. İle en son gelişmeleri güncel kalmak için bu makalede, ile hakkında bilgi sağlar:

- En son sürümleri
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlev

## <a name="new-features"></a>Yeni Özellikler

|Özellik  |Açıklama  |Eklenme tarihi  |
|---------|---------|---------|
|Daha fazla ölçüm |Uygulama ağ geçidi v2 SKU 'nuzu izlemenize yardımcı olması için aşağıdaki yeni ölçümleri ekledik: [Zamanlama ile ilgili ölçümler](https://docs.microsoft.com/azure/application-gateway/application-gateway-metrics#timing-metrics), arka uç yanıtı durumu, alınan bayt, gönderilen bayt, istemci TLS protokolü ve geçerli işlem birimleri. Bkz. [Application Gateway v2 SKU 'su tarafından desteklenen ölçümler](https://docs.microsoft.com/azure/application-gateway/application-gateway-metrics#metrics-supported-by-application-gateway-v2-sku). |Ağustos 2019 |
|WAF özel kuralları |Uygulama ağ geçidi WAF_v2 artık özel kuralların oluşturulmasını desteklemektedir. Bkz. [Application Gateway özel kurallar](custom-waf-rules-overview.md). |Haziran 2019 |
|Otomatik ölçeklendirme, bölge artıklığı, statik VIP desteği GA |Otomatik ölçeklendirmeyi, bölge yedekliliği, performansı, statik VIP 'leri, Key Vault, üst bilgi yeniden yazmayı destekleyen v2 SKU 'SU için genel kullanılabilirlik. Bkz. [Application Gateway otomatik ölçeklendirme belgeleri](application-gateway-autoscaling-zone-redundant.md). |2019 Nisan |
|Key Vault tümleştirme |Application Gateway artık HTTPS etkin dinleyicilerine eklenen sunucu sertifikaları için Key Vault (genel önizlemede) tümleştirmeyi desteklemektedir. [Key Vault sertifikalarla SSL sonlandırmasına](key-vault-certs.md)bakın. |2019 Nisan |
|Üst bilgi CRUD/yeniden yazar     |Artık HTTP üstbilgilerini yeniden yazabilirsiniz. Öğreticiye bakın [: Daha fazla bilgi için bir uygulama ağ geçidi](tutorial-http-header-rewrite-powershell.md) oluşturun ve HTTP üstbilgilerini yeniden yazın.|Aralık 2018|
|WAF yapılandırma ve dışlama listesi     |WAF 'nizi yapılandırmanıza ve hatalı pozitif sonuçları azaltmanıza yardımcı olacak daha fazla seçenek ekledik. Daha fazla bilgi için bkz. [Web uygulaması güvenlik duvarı istek boyutu sınırları ve dışlama listeleri](application-gateway-waf-configuration.md) .|Aralık 2018|
|Otomatik ölçeklendirme, bölge artıklığı, statik VIP desteği      |V2 SKU 'SU sayesinde, otomatik ölçeklendirme, gelişmiş performans ve daha fazlası gibi birçok geliştirme vardır. Daha fazla bilgi için bkz. [Azure Application Gateway nedir?](overview.md) .|Eylül 2018|
|Bağlantı boşaltma     |Bağlantı boşaltma, arka uç havuzlarınızdaki üyeleri sorunsuz bir şekilde kaldırmanıza olanak sağlar. Daha fazla bilgi için bkz. [Bağlantı boşaltma](overview.md#connection-draining).|Eylül 2018|
|Özel hata sayfaları     |Özel hata sayfaları ile, Web sitelerinizin geri kalanının biçimi içinde bir hata sayfası oluşturabilirsiniz. Bunu etkinleştirmek için, bkz. [Application Gateway özel hata sayfaları oluşturma](custom-error.md).|Eylül 2018|
|Ölçüm geliştirmeleri     |Gelişmiş ölçümlerle Application Gateway durumunu daha iyi bir şekilde görüntüleyebilirsiniz. Application Gateway ölçümleri etkinleştirmek için, [Application Gateway Için arka uç sistem durumu, tanılama günlükleri ve ölçümler](application-gateway-diagnostics.md)bölümüne bakın.|Haziran 2018|

## <a name="next-steps"></a>Sonraki adımlar

Azure Application Gateway hakkında daha fazla bilgi için bkz. [azure Application Gateway nedir?](overview.md)
