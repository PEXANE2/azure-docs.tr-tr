---
title: Azure Application Gateway yenilikleri
description: En son sürüm notları, bilinen sorunlar, hata düzeltmeleri, kullanım dışı işlevler ve yaklaşan değişiklikler gibi Azure Application Gateway yenilikleri hakkında bilgi edinin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: overview
ms.date: 4/30/2019
ms.author: victorh
ms.openlocfilehash: c6d4d290493bbd234ab048e613b88f8857513cc8
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78299564"
---
# <a name="whats-new-in-azure-application-gateway"></a>Azure Application Gateway 'daki yenilikler nelerdir?

Azure Application Gateway, sürekli olarak güncelleştirilir. En son gelişmelerden haberdar olmak için bu makalede hakkında bilgi verilmektedir:

- En son sürümleri
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlev

## <a name="new-features"></a>Yeni Özellikler

|Özellik  |Açıklama  |Eklenme tarihi  |
|---------|---------|---------|
|Benzeşim tanımlama bilgisi değişiklikleri |Tanımlama bilgisi tabanlı benzeşim etkinleştirildiğinde, mevcut Applicationgatewaybenzeşim tanımlama bilgisine ek olarak *Applicationgatewayaffinitycors* adlı başka bir özdeş tanımlama bilgisini Application Gateway çıkartır. *Applicationgatewayaffinitycors* öğesine eklenmiş iki özniteliğe sahiptir (*SameSite = None; Güvenli*), geçici oturumlar için de yapışkan oturumların korunmasını sağlamak üzere. Daha fazla bilgi için bkz. [Application Gateway tanımlama bilgisi tabanlı benzeşim](configuration-overview.md#cookie-based-affinity) . |Şubat 2020 |
|Araştırma geliştirmeleri |Application Gateway v2 SKU 'sunda özel araştırma geliştirmeleriyle, [araştırma yapılandırması](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-probe-portal#create-probe-for-application-gateway-v2-sku)basitleştik ve [isteğe bağlı arka uç durumu testlerini](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-probe-portal#test-backend-health-with-the-probe) kolaylaştırdık ve arka uç sistem durumu sorunlarını gidermenize yardımcı olmak için [daha fazla tanılama bilgisi](https://docs.microsoft.com/azure/application-gateway/application-gateway-backend-health-troubleshooting#error-messages) ekledik.  |Ekim 2019 |
|Daha fazla ölçüm |Application Gateway v2 SKU 'nuzu izlemenize yardımcı olması için aşağıdaki yeni ölçümleri ekledik: [zamanlamala ilgili ölçümler](https://docs.microsoft.com/azure/application-gateway/application-gateway-metrics#timing-metrics), arka uç yanıt durumu, alınan bayt, gönderilen bayt, istemci TLS protokolü ve geçerli işlem birimleri. Bkz. [Application Gateway v2 SKU 'su tarafından desteklenen ölçümler](https://docs.microsoft.com/azure/application-gateway/application-gateway-metrics#metrics-supported-by-application-gateway-v2-sku). |Ağustos 2019 |
|WAF özel kuralları |Application Gateway WAF_v2 artık özel kurallar oluşturmayı destekliyor. Bkz. [Application Gateway özel kurallar](custom-waf-rules-overview.md). |Haziran 2019 |
|Otomatik ölçeklendirme, bölge artıklığı, statik VIP desteği GA |Otomatik ölçeklendirmeyi, bölge yedekliliği, performansı, statik VIP 'leri, Key Vault, üst bilgi yeniden yazmayı destekleyen v2 SKU 'SU için genel kullanılabilirlik. Bkz. [Application Gateway otomatik ölçeklendirme belgeleri](application-gateway-autoscaling-zone-redundant.md). |Nisan 2019 |
|Key Vault tümleştirme |Application Gateway artık HTTPS etkin dinleyicilerine eklenen sunucu sertifikaları için Key Vault (genel önizlemede) tümleştirmeyi desteklemektedir. [Key Vault sertifikalarla SSL sonlandırmasına](key-vault-certs.md)bakın. |Nisan 2019 |
|Üst bilgi CRUD/yeniden yazar     |Artık HTTP üstbilgilerini yeniden yazabilirsiniz. Daha fazla bilgi için bkz. [öğretici: uygulama ağ geçidi oluşturma ve HTTP üstbilgilerini yeniden yazma](tutorial-http-header-rewrite-powershell.md) .|Aralık 2018|
|WAF yapılandırma ve dışlama listesi     |WAF 'nizi yapılandırmanıza ve hatalı pozitif sonuçları azaltmanıza yardımcı olacak daha fazla seçenek ekledik. Daha fazla bilgi için bkz. [Web uygulaması güvenlik duvarı istek boyutu sınırları ve dışlama listeleri](application-gateway-waf-configuration.md).|Aralık 2018|
|Otomatik ölçeklendirme, bölge artıklığı, statik VIP desteği      |V2 SKU 'SU sayesinde, otomatik ölçeklendirme, gelişmiş performans ve daha fazlası gibi birçok geliştirme vardır. Daha fazla bilgi için bkz. [Azure Application Gateway nedir?](overview.md) .|Eylül 2018|
|Bağlantı boşaltma     |Bağlantı boşaltma, arka uç havuzlarınızdaki üyeleri sorunsuz bir şekilde kaldırmanıza olanak sağlar. Daha fazla bilgi için bkz. [Bağlantı boşaltma](features.md#connection-draining).|Eylül 2018|
|Özel hata sayfaları     |Özel hata sayfaları ile, Web sitelerinizin geri kalanının biçimi içinde bir hata sayfası oluşturabilirsiniz. Bunu etkinleştirmek için, bkz. [Application Gateway özel hata sayfaları oluşturma](custom-error.md).|Eylül 2018|
|Ölçüm geliştirmeleri     |Gelişmiş ölçümlerle Application Gateway durumunu daha iyi bir şekilde görüntüleyebilirsiniz. Application Gateway ölçümleri etkinleştirmek için, [Application Gateway Için arka uç sistem durumu, tanılama günlükleri ve ölçümler](application-gateway-diagnostics.md)bölümüne bakın.|Haziran 2018|

## <a name="next-steps"></a>Sonraki adımlar

Azure Application Gateway hakkında daha fazla bilgi için bkz. [azure Application Gateway nedir?](overview.md)
