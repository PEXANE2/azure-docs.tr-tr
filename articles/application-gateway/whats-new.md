---
title: Azure Uygulama Ağ Geçidi'ndeki yenilikler
description: En son sürüm notları, bilinen sorunlar, hata düzeltmeleri, amortismana uygun işlevler ve yaklaşan değişiklikler gibi Azure Uygulama Ağ Geçidi'nde yenilikleri öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: overview
ms.date: 03/24/2020
ms.author: victorh
ms.openlocfilehash: 70014b5ab37a07e01eaa2db3d729b7d8af520842
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81311770"
---
# <a name="whats-new-in-azure-application-gateway"></a>Azure Uygulama Ağ Geçidi'ndeki yenilikler nelerdir?

Azure Application Gateway sürekli olarak güncelleştirilir. En son gelişmelerden haberdar olmak için bu makalede, aşağıdakiler hakkında bilgi verilmektedir:

- En son sürümler
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlevsellik

## <a name="new-features"></a>Yeni özellikler

|Özellik  |Açıklama  |Eklenen tarih  |
|---------|---------|---------|
| V2'de Kullanıcı Tanımlı Rotalar (UDR) (Önizleme) |Kullanıcı tanımlı yollar artık Application Gateway v2 SK'lerde bazı senaryolarda desteklenir. Daha fazla bilgi için Bkz. [Uygulama Ağ Geçidi yapılandırmagenel bakışı.](configuration-overview.md#user-defined-routes-supported-on-the-application-gateway-subnet) |Mart 2020 |
|Yakınlık çerez değişiklikleri |Çerez tabanlı yakınlık etkinleştirildiğinde, Application Gateway, varolan ApplicationGatewayAffinity çerezine ek olarak *ApplicationGatewayAffinityCORS* adlı başka bir özdeş çerez enjekte eder. *ApplicationGatewayAffinityCORS'un* iki özniteliği daha eklendi (*SameSite=None; Güvenli)* böylece yapışkan oturumları çapraz kökenli istekler için bile korunur. Daha fazla bilgi için [Uygulama Ağ Geçidi Çerezi tabanlı yakınlık](configuration-overview.md#cookie-based-affinity) temeline bakın. |Şubat 2020 |
|Sonda geliştirmeleri |Application Gateway v2 SKU'daki özel sonda geliştirmeleri ile, [sonda yapılandırmasını](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-probe-portal#create-probe-for-application-gateway-v2-sku)basitleştirdik, [isteğe bağlı arka uç sağlık testlerini](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-probe-portal#test-backend-health-with-the-probe) kolaylaştırdık ve arka uç sağlık sorunlarını gidermenize yardımcı olacak [daha fazla tanısal bilgi](https://docs.microsoft.com/azure/application-gateway/application-gateway-backend-health-troubleshooting#error-messages) ekledik.  |Ekim 2019 |
|Daha fazla ölçüm |Uygulama Ağ Geçidi v2 SKU: [Zamanlama ile ilgili ölçümler,](https://docs.microsoft.com/azure/application-gateway/application-gateway-metrics#timing-metrics)Arka uç yanıt durumu, Alınan Baytlar, Baytlar gönderildi, İstemci TLS protokolü ve Geçerli bilgi işlem birimlerinizi izlemenize yardımcı olmak için aşağıdaki yeni ölçümleri ekledik. [Uygulama Ağ Geçidi V2 SKU tarafından desteklenen Ölçümlere](https://docs.microsoft.com/azure/application-gateway/application-gateway-metrics#metrics-supported-by-application-gateway-v2-sku)bakın. |Ağustos 2019 |
|WAF özel kuralları |Uygulama Ağ Geçidi WAF_v2 artık özel kurallar oluşturmayı destekler. Bkz. [Uygulama Ağ Geçidi özel kuralları.](custom-waf-rules-overview.md) |Haziran 2019 |
|Otomatik ölçekleme, bölge artıklığı, statik VIP desteği GA |Otomatik ölçekleme, bölge artıklığını destekleyen v2 SKU için genel kullanılabilirlik, performansı artırır, statik VIP'ler, Key Vault, Header yeniden yazın. Bkz. [Uygulama Ağ Geçidi otomatikleştirme belgeleri.](application-gateway-autoscaling-zone-redundant.md) |Nisan 2019 |
|Anahtar Vault entegrasyonu |Uygulama Ağ Geçidi artık HTTPS özellikli dinleyicilere bağlı sunucu sertifikaları için Key Vault (genel önizleme) ile tümleştirmeyi destekler. [Key Vault sertifikaları ile TLS sonlandırma bakın.](key-vault-certs.md) |Nisan 2019 |
|Üstbilgi CRUD/Rewrites     |Şimdi HTTP üstbilgi yeniden yazabilirsiniz. Bkz. Öğretici: Bir uygulama ağ geçidi oluşturun ve daha fazla bilgi için [HTTP üstbilgilerini yeniden yazın.](tutorial-http-header-rewrite-powershell.md)|Aralık 2018|
|WAF yapılandırma ve dışlama listesi     |WAF'ınızı yapılandırmanıza ve yanlış pozitif leri azaltmanıza yardımcı olacak daha fazla seçenek ekledik. Daha fazla bilgi için [Bkz. Web uygulaması güvenlik duvarı isteği boyut sınırları ve dışlama listeleri.](application-gateway-waf-configuration.md)|Aralık 2018|
|Otomatik ölçekleme, bölge artıklığı, statik VIP desteği      |v2 SKU ile Otomatik Ölçekleme, geliştirilmiş performans ve daha fazlası gibi birçok iyileştirme vardır. Azure [Uygulama Ağ Geçidi nedir?](overview.md)|Eylül 2018|
|Bağlantı boşaltma     |Bağlantı boşaltma, üyeleri arka uç havuzlarınızdan zarif bir şekilde kaldırmanızı sağlar. Daha fazla bilgi için [bkz.](features.md#connection-draining)|Eylül 2018|
|Özel hata sayfaları     |Özel hata sayfaları ile, web sitelerinizin geri kalanının biçimi içinde bir hata sayfası oluşturabilirsiniz. Bunu etkinleştirmek için [bkz.](custom-error.md)|Eylül 2018|
|Metrik Geliştirmeleri     |Gelişmiş ölçümlerle Uygulama Ağ Geçidinizin durumunu daha iyi görebilirsiniz. Uygulama Ağ Geçidi'nizdeki ölçümleri etkinleştirmek [için, Uygulama Ağ Geçidi için arka uç durumu, tanılama günlükleri ve ölçümlere](application-gateway-diagnostics.md)bakın.|Haziran 2018|

## <a name="next-steps"></a>Sonraki adımlar

Azure Uygulama Ağ Geçidi hakkında daha fazla bilgi için [bkz.](overview.md)
