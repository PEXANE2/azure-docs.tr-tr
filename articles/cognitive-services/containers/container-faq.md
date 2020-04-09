---
title: Bilişsel Hizmetler konteynersık sorulan sorular (SSS)
titleSuffix: Azure Cognitive Services
description: Sık sorulan sorular ve cevaplar.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 8aae650065e8de11ccdc55a8a056c379a219fcb4
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80876515"
---
# <a name="azure-cognitive-services-containers-frequently-asked-questions-faq"></a>Azure Bilişsel Hizmetler kapları sık sorulan sorular (SSS)

## <a name="general-questions"></a>Genel sorular

**S: Neler kullanılabilir?**

**C:** [Azure Bilişsel Hizmetleri'ndeki kapsayıcı desteği,](../cognitive-services-container-support.md) geliştiricilerin Azure'da kullanılabilen ancak kapsayıcılaştırmanın [avantajlarıyla](../cognitive-services-container-support.md#features-and-benefits) kullanılabilen aynı akıllı API'leri kullanmasına olanak tanır. Kapsayıcı desteği şu anda Azure Bilişsel Hizmetleri'nin bir alt kümesi için önizlemede kullanılabilir:

> [!div class="checklist"]
> * [Anomali Algılayıcısı][ad-containers]
> * [Görüntü İşleme][cv-containers]
> * [Yüz][fa-containers]
> * [Form Tanıma][fr-containers]
> * [Language Understanding (LUIS)][lu-containers]
> * [Konuşma Hizmeti API’si][sp-containers]
> * [Metin Analizi][ta-containers]

**S: Bilişsel Hizmetler bulutu ile kapsayıcılar arasında bir fark var mı?**

**A:** Bilişsel Hizmetler kapsayıcıları Bilişsel Hizmetler bulutuna alternatiftir. Kapsayıcılar, ilgili bulut hizmetleriyle aynı özellikleri sunar. Müşteriler kapsayıcıları şirket içinde veya Azure'da dağıtabilir. Çekirdek AI teknolojisi, fiyatlandırma katmanları, API anahtarları ve API imzası kapsayıcı ve ilgili bulut hizmetleri arasında aynıdır. Kapsayıcıları bulut hizmeti eşdeğeri üzerinden seçmenin [özellikleri ve avantajları](../cognitive-services-container-support.md#features-and-benefits) aşağıda verilmiştir.

**S: Konteynerler tüm Bilişsel Hizmetler için kullanılabilir olacak ve biz beklememiz gereken kaplar sonraki kümesi nelerdir?**

**A:** Biz konteyner teklifleri olarak daha fazla Bilişsel Hizmetler kullanılabilir hale getirmek istiyorum. Yeni kapsayıcı sürümleri ve diğer Bilişsel Hizmetler duyuruları hakkında güncelleştirmeler almak için yerel Microsoft hesap yöneticinize başvurun.

**S: Bilişsel Hizmetler kapsayıcıları için Hizmet Düzeyi Sözleşmesi (SLA) ne olacak?**

**A:** Bilişsel Hizmetler kapları bir SLA yok.

Kaynakların Bilişsel Hizmetler kapsayıcı yapılandırmaları müşteriler tarafından denetlenir, bu nedenle Microsoft genel kullanılabilirlik (GA) için bir SLA sunmaz. Müşteriler kapsayıcıları şirket içinde dağıtmakta serbesttir, böylece ana ortamları tanımlarlar.

> [!IMPORTANT]
> Bilişsel Hizmetler Hizmet Düzeyi Sözleşmeleri hakkında daha fazla bilgi edinmek için [SLA sayfamızı ziyaret edin.](https://azure.microsoft.com/support/legal/sla/cognitive-services/v1_1/)

**S: Bu kapsayıcılar egemen bulutlarda kullanılabilir mi?**

**A:** Herkes "egemen bulut" terimini bilmiyor, bu yüzden tanımla başlayalım:

> "Egemen bulut", [Azure Hükümeti,](../../azure-government/documentation-government-welcome.md) [Azure Almanya](../../germany/germany-welcome.md)ve Azure [China 21Vianet](https://docs.microsoft.com/azure/china/overview-operations) bulutlarından oluşur.

Ne yazık ki, Bilişsel Hizmetler kapsayıcıları yerel egemen bulutlarda *desteklenmez.* Kapsayıcılar bu bulutlarda çalıştırılabilir, ancak bunlar genel buluttan çekilir ve kullanım verilerini genel bitiş noktasına göndermeleri gerekir.

### <a name="versioning"></a>Sürüm Oluşturma

**S: Kapsayıcılar en son sürüme nasıl güncellenir?**

**A:** Müşteriler dağıttıkları kapsayıcıları ne zaman güncelleştireceklerini seçebilirler. Kapsayıcılar, en son sürümü `latest` belirtmek için standart [Docker etiketleri](https://docs.docker.com/engine/reference/commandline/tag/) ile işaretlenir. Müşterilerin, bir resim güncelleştirildiğinde nasıl bilgilendirileceklerine ilişkin ayrıntılar için azure [konteyner kayıt defteri webhook'ları,](../../container-registry/container-registry-webhook.md) kullanıma alınan konteynerlerin en son sürümünü piyasaya sürüldüklerinde çekmelerini öneririz.
 
**S: Hangi sürümler desteklenecek?**

**A:** Kapsayıcının geçerli ve son ana sürümü desteklenir. Ancak, müşterilerin en son teknolojiyi elde etmek için güncel kalmalarını öneririz.
 
**S: Güncelleştirmeler nasıl sürümlenir?**

**A:** Ana sürüm değişiklikleri, API imzada bir kesme değişikliği olduğunu gösterir. Bunun genellikle ilgili Bilişsel Hizmet bulut teklifindeki önemli sürüm değişiklikleriyle çakışacağını tahmin ediyoruz. Küçük sürüm değişiklikleri hata düzeltmeleri, model güncelleştirmeleri veya API imzabir kesme değişiklik yapmaz yeni özellikler gösterir.

## <a name="technical-questions"></a>Teknik sorular

**S: IoT cihazlarında Bilişsel Hizmetler kapsayıcılarını nasıl çalıştırmalıyım?**

İster güvenilir bir internet bağlantınız olsun, ister bant genişliği maliyetinde tasarruf etmek ister. Veya düşük gecikme süresi gereksinimleri varsa veya yerinde analiz edilmesi gereken hassas verilerle ilgileniyorsanız, [Azure IoT Edge bilişsel hizmetler kapsayıcıları ile](https://azure.microsoft.com/blog/running-cognitive-services-on-iot-edge/) bulut ile tutarlılık sağlar.

**S: Ürün geri bildirimi ni ve özellik önerilerini nasıl sağlarım?**

**A:** Müşteriler endişelerini herkese açık bir şekilde [dile getirmeleri](https://cognitive.uservoice.com/) ve potansiyel sorunların çakıştığı durumlarda aynı şeyi yapan diğer lerini oylayabilmek için teşvik edilir. Kullanıcı ses aracı hem ürün geri bildirimi hem de özellik önerileri için kullanılabilir.

**S: Destek için kiminle iletişim kuruyorum?**

**A:** Müşteri destek kanalları, Bilişsel Hizmetler bulut teklifiyle aynıdır. Tüm Bilişsel Hizmetler kapsayıcıları bize ve topluluk destek müşterilerine yardımcı olacak günlük özellikleri içerir. Ek destek için aşağıdaki seçeneklere bakın.

### <a name="customer-support-plan"></a>Müşteri destek planı

Müşteriler, destek için kime başvuracaklarını görmek için [Azure destek planına](https://azure.microsoft.com/support/plans/) başvurmalıdır.

### <a name="azure-knowledge-center"></a>Azure bilgi merkezi

Müşteriler, soruları yanıtlamak ve sorunları desteklemek için [Azure bilgi merkezini](https://azure.microsoft.com/resources/knowledge-center/) keşfetmekte özgürdür.

### <a name="stack-overflow"></a>Stack Overflow

> [Stack Overflow](https://en.wikipedia.org/wiki/Stack_Overflow) profesyonel ve meraklısı programcılar için bir soru ve cevap sitesidir.

İhtiyaçlarınıza uygun olası sorular ve yanıtlar için aşağıdaki etiketleri keşfedin.

* [Azure Bilişsel Hizmetler](https://stackoverflow.com/questions/tagged/azure-cognitive-services)
* [Microsoft Bilişsel](https://stackoverflow.com/questions/tagged/microsoft-cognitive)

**S: Faturalandırma nasıl çalışır?**

**A:** Müşteriler, Bilişsel Hizmetler bulutuna benzer şekilde tüketime göre ücretlendirilir. Kapsayıcıların ölçüm verilerini Azure'a gönderecek şekilde yapılandırılması gerekir ve hareketler buna göre faturalandırılır. Barındırılan ve şirket içi hizmetlerde kullanılan kaynaklar, her iki kullanıma göre sayma, katmanlı fiyatlandırma ile tek kotaya eklenir. Daha fazla ayrıntı için, ilgili teklifin fiyatlandırma sayfasına bakın.

* [Anomali Algılayıcısı][ad-containers-billing]
* [Görüntü İşleme][cv-containers-billing]
* [Yüz][fa-containers-billing]
* [Form Tanıma][fr-containers-billing]
* [Language Understanding (LUIS)][lu-containers-billing]
* [Konuşma Hizmeti API’si][sp-containers-billing]
* [Metin Analizi][ta-containers-billing]

> [!IMPORTANT]
> Bilişsel Hizmetler kapsayıcıları ölçüm için Azure'a bağlı olmadan çalışma lisansı na sahip değildir. Müşterilerin, konteynerlerin fatura bilgilerini her zaman ölçüm hizmetiyle iletmesini sağlaması gerekir. Bilişsel Hizmetler kapsayıcıları müşteri verilerini Microsoft'a göndermez.
 
**S: Konteynerler için geçerli destek garantisi nedir?**

**A:** Önizlemeler için garanti yoktur. Kapsayıcılar genel kullanılabilirlik (GA) olarak resmen duyurulduğunda Microsoft'un kurumsal yazılımlar için standart garantisi geçerli olacaktır.
 
**S: Internet bağlantısı kaybolduğunda Bilişsel Hizmetler kapsayıcılarına ne olur?**

**A:** Bilişsel Hizmetler kapsayıcıları ölçüm için Azure'a bağlı olmadan çalışma *lisansı na sahip değildir.* Müşterilerin kapsayıcıların ölçüm hizmetiyle her zaman iletişim kurmasını sağlaması gerekir.

**S: Kapsayıcı Azure'a bağlanmadan ne kadar süre çalışabilir?**

**A:** Bilişsel Hizmetler kapsayıcıları ölçüm için Azure'a bağlı olmadan çalışma *lisansı na sahip değildir.* Müşterilerin kapsayıcıların ölçüm hizmetiyle her zaman iletişim kurmasını sağlaması gerekir.
 
**S: Bu kapsayıcıları çalıştırmak için geçerli donanım nedir?**

**A:** Bilişsel Hizmetler kapsayıcıları, x64 Linux Docker Containers'ı destekleyen uyumlu linux düğümü, VM ve kenar aygıtıçalıştırabilen x64 tabanlı kapsayıcılardır. Hepsi CPU işlemcileri gerektirir. Her konteyner teklifi için minimum ve önerilen yapılandırmalar aşağıda mevcuttur:

* [Anomali Algılayıcısı][ad-containers-recommendations]
* [Görüntü İşleme][cv-containers-recommendations]
* [Yüz][fa-containers-recommendations]
* [Form Tanıma][fr-containers-recommendations]
* [Language Understanding (LUIS)][lu-containers-recommendations]
* [Konuşma Hizmeti API’si][sp-containers-recommendations]
* [Metin Analizi][ta-containers-recommendations]
 
**S: Bu kapsayıcılar şu anda Windows'da desteklenmiş mi?**

**A:** Bilişsel Hizmetler kapsayıcıları Linux kapsayıcılarıdır, ancak Windows'daki Linux kapları için bazı destek vardır. Windows'daki Linux kapsayıcıları hakkında daha fazla bilgi için [Docker belgelerine](https://blog.docker.com/2017/09/preview-linux-containers-on-windows/)bakın.
 
**S: Konteynerleri nasıl keşfederim?**

**A:** Bilişsel Hizmetler kapsayıcıları Azure portalı, Docker hub'ı ve Azure konteyner kayıt defterleri gibi çeşitli konumlarda kullanılabilir. En son kapsayıcı konumları için [konteyner depolarına ve resimlere](../cognitive-services-container-support.md#container-repositories-and-images)bakın.

**S: Bilişsel Hizmetler kapsayıcıları AWS ve Google teklifleri ile karşılaştırıldığında nasıl?**

**A:** Microsoft, önceden eğitilmiş AI modellerini, müşteriler bir bulut hizmeti kullanıyormuş gibi işlem başına basit faturalandırmalı kaplarda hareket ettiren ilk bulut sağlayıcısıdır. Microsoft, karma bulutun müşterilere daha fazla seçenek sağladığına inanıyor.

**S: Kapsayıcıların hangi uyumluluk sertifikaları vardır?**

**A:** Bilişsel hizmet kapları herhangi bir uyumluluk sertifikaları yok

**S: Bilişsel Hizmetler kapları hangi bölgelerde kullanılabilir?**

**A:** Kapsayıcılar herhangi bir bölgede çalıştırılabilir, ancak anahtara ihtiyaç duyarlar ve ölçüm için Azure'u geri arayabilirler. Bulut Hizmeti için desteklenen tüm bölgeler, kapsayıcıölçüm çağrısı için desteklenir.

[!INCLUDE [Containers next steps](includes/containers-next-steps.md)]

[ad-containers]: ../anomaly-Detector/anomaly-detector-container-howto.md
[cv-containers]: ../computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: ../face/face-how-to-install-containers.md
[fr-containers]: ../form-recognizer/form-recognizer-container-howto.md
[lu-containers]: ../luis/luis-container-howto.md
[sp-containers]: ../speech-service/speech-container-howto.md
[ta-containers]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md

[ad-containers-billing]: ../anomaly-Detector/anomaly-detector-container-howto.md#billing
[cv-containers-billing]: ../computer-vision/computer-vision-how-to-install-containers.md#billing
[fa-containers-billing]: ../face/face-how-to-install-containers.md#billing
[fr-containers-billing]: ../form-recognizer/form-recognizer-container-howto.md#billing
[lu-containers-billing]: ../luis/luis-container-howto.md#billing
[sp-containers-billing]: ../speech-service/speech-container-howto.md#billing
[ta-containers-billing]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md#billing

[ad-containers-recommendations]: ../anomaly-Detector/anomaly-detector-container-howto.md#container-requirements-and-recommendations
[cv-containers-recommendations]: ../computer-vision/computer-vision-how-to-install-containers.md#container-requirements-and-recommendations
[fa-containers-recommendations]: ../face/face-how-to-install-containers.md#container-requirements-and-recommendations
[fr-containers-recommendations]: ../form-recognizer/form-recognizer-container-howto.md#container-requirements-and-recommendations
[lu-containers-recommendations]: ../luis/luis-container-howto.md#container-requirements-and-recommendations
[sp-containers-recommendations]: ../speech-service/speech-container-howto.md#container-requirements-and-recommendations
[ta-containers-recommendations]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md#container-requirements-and-recommendations
