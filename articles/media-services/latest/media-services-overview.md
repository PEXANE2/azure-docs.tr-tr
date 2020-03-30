---
title: Azure Media Services v3'e genel bakış
titleSuffix: Azure Media Services
description: Hızlı başlangıçlar, öğreticiler ve kod örneklerine bağlantılar içeren Azure Medya Hizmetleri v3'e üst düzey bir genel bakış.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, akış, yayın, canlı, çevrimdışı
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 03/09/2020
ms.author: juliako
ms.custom: mvc
ms.openlocfilehash: bd3890757377525cf9c178866a2a2fbc0791b9de
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79461019"
---
# <a name="azure-media-services-v3-overview"></a>Azure Media Services v3'e genel bakış

Azure Media Services, yayın kalitesinde video akışı elde etmenizi, erişilebilirlik ve dağıtımı iyileştirmenizi, içerikleri analiz etmenizi ve daha fazlasını yapmanızı sağlayan çözümler derlemenize olanak tanıyan bulut tabanlı bir platformdur. İster bir uygulama geliştiricisi, ister çağrı merkezi, bir devlet kurumu veya bir eğlence şirketi olun, Media Services günümüzün en popüler mobil cihazlarında ve tarayıcılarında büyük kitlelere olağanüstü kalitede medya deneyimleri sunan uygulamalar oluşturmanıza yardımcı olur.

Medya Hizmetleri v3 SDK'lar [Medya Hizmetleri v3 OpenAPI Belirtim (Swagger)](https://aka.ms/ams-v3-rest-sdk)dayanmaktadır.

> [!NOTE]
> Şu anda, [Azure portalını](https://portal.azure.com/) şu şekilde kullanabilirsiniz: Medya Hizmetleri v3 [Live Events'i](live-events-outputs-concept.md)yönetmek, v3 [Varlıklarını](assets-concept.md)görüntülemek (yönetmemek) veya [API'lere erişim hakkında bilgi almak.](access-api-portal.md) Diğer tüm yönetim görevleri için (örneğin, [Dönüşümler ve İşler ve](transforms-jobs-concept.md) [İçerik koruması),](content-protection-overview.md) [REST API,](https://docs.microsoft.com/rest/api/media/) [CLI](https://aka.ms/ams-v3-cli-ref)veya desteklenen [SDK'lardan](media-services-apis-overview.md#sdks)birini kullanın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="compliance-privacy-and-security"></a>Uyumluluk, Gizlilik ve Güvenlik

Önemli bir hatırlatma olarak, Azure Medya Hizmetlerini kullanımınızda geçerli tüm yasalara uymanız gerekir ve Medya Hizmetlerini veya herhangi bir Azure hizmetini başkalarının haklarını ihlal eden veya başkalarına zarar verebilecek şekilde kullanamazsınız.

Medya Hizmetlerine herhangi bir video/resim yüklemeden önce, video/görüntüyü kullanmadan önce, yasaların gerektirdiği durumlarda, video/görüntüdeki bireylerin (varsa) verilerinin Medya Hizmetleri ve Azure'da kullanımı, işlenmesi ve depolanması için gerekli tüm izinler de dahil olmak üzere, videoyu/görüntüyü kullanmak için gereken tüm haklara sahip olmalısınız. Bazı yargı bölgeleri, biyometrik veriler gibi belirli veri kategorilerinin toplanması, çevrimiçi işlenmesi ve depolanması için özel yasal gereklilikler uygulayabilir. Özel yasal gerekliliklere tabi tüm verilerin işlenmesi ve saklanması için Medya Hizmetleri ve Azure'u kullanmadan önce, sizin için geçerli olabilecek bu tür yasal gerekliliklere uygunluğu sağlamalısınız.

Medya Hizmetlerinde uyumluluk, gizlilik ve güvenlik hakkında bilgi edinmek için lütfen Microsoft [Güven Merkezi'ni](https://www.microsoft.com/trust-center/?rtc=1)ziyaret edin. Microsoft'un gizlilik yükümlülükleri, verilerinizin nasıl silindigini içeren veri işleme ve saklama uygulamaları için lütfen Microsoft' un [Gizlilik Bildirimini,](https://privacy.microsoft.com/PrivacyStatement) [Çevrimiçi Hizmetler Koşulları'nı](https://www.microsoft.com/licensing/product-licensing/products?rtc=1) ("OST") ve [Veri İşleme Eki 'ni](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=67) ("DPA") inceleyin. Medya Hizmetlerini kullanarak, OST, DPA ve Gizlilik Bildirimi'ne bağlı olmayı kabul edeyim.
 
## <a name="what-can-i-do-with-media-services"></a>Media Services ile ne yapabilirim?

Medya Hizmetleri, bulutta çeşitli medya iş akışları oluşturmanıza olanak tanır. Medya Hizmetleri ile yapabilecekleriniz hakkında bazı örnekler şunlardır:

* Videoları, çeşitli tarayıcılarda ve cihazlarda oynatılabilmesi için çeşitli biçimlerde sunma. Çeşitli istemcilere (mobil cihazlar, TV, PC vb.) hem isteğe bağlı hem de canlı akış teslimatı için, video ve ses içeriğinin uygun şekilde kodlanması ve paketlemesi gerekir. Bu tür içeriklerin nasıl sunulacağını ve akışa alınacağını görmek için bkz. [Hızlı Başlangıç: Dosyaları kodlama ve akışa alma](stream-files-dotnet-quickstart.md).
* Canlı spor etkinliklerini futbol, beyzbol, kolej ve lise sporları gibi büyük bir çevrimiçi kitleye ve daha fazlasına aktarın.
* Belediye binaları, belediye meclisi toplantıları ve yasama organları gibi halka açık toplantı ve etkinlikleri yayınla.
* Kaydedilen videoları veya ses içeriğini analiz edin. Örneğin, daha yüksek müşteri memnuniyeti elde etmek için kuruluşlar, konuşmayı metne dönüştürebilir ve arama dizinleri ve panolar derleyebilir. Daha sonra genel şikayetlerden, şikayet kaynaklarından ve diğer ilişkili verilerden istihbarat çıkarabilir.
* Bir müşterinin (örneğin, bir filmi stüdyosu), telif hakkıyla korunan bir çalışmaya erişimi ve kullanım yetkisini kısıtlaması gerektiğinde bir abonelik video hizmeti oluşturun ve DRM korumalı içeriği akışa alın.
* Uçak, tren ve otomobillerde kayıttan yürütülmesi için çevrimdışı içerik sunun. Bir müşterinin, ağ bağlantısının kesileceğini tahmin ettiğinde kayıttan yürütülmesi için içeriği telefonuna veya tabletine indirmesi gerekebilir.
* Konuşma-metin altyazı, çoklu dillere çeviri ve benzeri için Azure Medya Hizmetleri ve [Azure Bilişsel Hizmetler API'leri](https://docs.microsoft.com/azure/?pivot=products&panel=ai) ile eğitici bir e-öğrenme video platformu uygulayın.
* Daha geniş bir kitleye (örneğin, işitme engelli kişiler veya farklı bir dilde okumak isteyen kişiler) hizmet vermek için videolara altyazı ve altyazı eklemek için Azure Medya Hizmetleri'ni [Azure Bilişsel Hizmetler API'leriyle](https://docs.microsoft.com/azure/?pivot=products&panel=ai) birlikte kullanın.
* Anlık yüksek yükleri (örneğin, bir ürün başlatma etkinliğinin başlaması) daha iyi işlemek için azure CDN'yi etkinleştirin.

## <a name="how-can-i-get-started-with-v3"></a>v3’ü kullanmaya nasıl başlayabilirim? 

Medya Hizmetleri v3 ile içeriği nasıl kodlayıp paketlendirecek, isteğe bağlı video akışı, canlı yayın ve analiz etme öğrenin. Öğreticiler, API başvuruları ve diğer belgeler, güvenli bir biçimde milyonlarca kullanıcıya ölçeklendirilebilen, isteğe bağlı ve canlı video veya ses akışları sağlama ile ilgili bilgiler içerir.

> [!TIP]
> Geliştirmeye başlamadan önce şunları gözden geçirin:<br/>* [Temel kavramlar](concepts-overview.md) (paketleme, kodlama ve koruma gibi önemli kavramları kuluçkaya yaslar)<br/>* [Media Services v3 API'leri ile geliştirme](media-services-apis-overview.md) (API'lere erişim, adlandırma kuralları vb. hakkında bilgi içerir)

### <a name="sdks"></a>SDK’lar

[Azure Media Services v3 istemci SDK'ları](media-services-apis-overview.md#sdks)ile geliştirmeye başlayın.

### <a name="quickstarts"></a>Hızlı Başlangıçlar  

Hızlı başlangıçlar, yeni müşterilerin Medya Hizmetlerini hızla denemeleri için temel gün-1 yönergelerini gösterir.

* [Stream video dosyaları - .NET](stream-files-dotnet-quickstart.md)
* [Stream video dosyaları - CLI](stream-files-cli-quickstart.md)
* [Video dosyalarını akışla aktarma - Node.js](stream-files-nodejs-quickstart.md)

### <a name="tutorials"></a>Öğreticiler

Öğreticiler, en iyi Medya Hizmetleri görevlerinden bazıları için senaryo tabanlı yordamları gösterir.

* [Uzak dosyayı kodlama ve videoyu akışla aktarma – REST](stream-files-tutorial-with-rest.md)
* [Karşıya yüklenen dosyayı kodlama ve videoyu akışla aktarma – .NET](stream-files-tutorial-with-api.md)
* [Canlı akış - .NET](stream-live-tutorial-with-api.md)
* [Videonuzu analiz etme - .NET](analyze-videos-tutorial-with-api.md)
* [AES-128 dinamik şifreleme - .NET](protect-with-aes128.md)

### <a name="samples"></a>Örnekler

Azure Medya Hizmetleri kod örneklerine göz atmak için [bu örnekleri tarayıcıyı](https://docs.microsoft.com/samples/browse/?products=azure-media-services) kullanın.

### <a name="how-to-guides"></a>Nasıl yapılır kılavuzları

Nasıl yapIlir kılavuzları, bir görevin nasıl tamamlanır olduğunu gösteren kod örnekleri içerir. Bu bölümde birçok örnek bulacaksınız. Bunlardan birkaçı şunlardır:

* [Hesap oluşturma - CLI](create-account-cli-how-to.md)
* [API'lere erişim - CLI](access-api-cli-how-to.md)
* [Https ile iş girişi olarak kodlayın - .NET](job-input-from-http-how-to.md)  
* [Olayları izleme - Portal](monitor-events-portal-how-to.md)
* [Multi-DRM ile dinamik olarak şifreleyin - .NET](protect-with-drm.md) 
* [Özel bir dönüşümle kodlama - CLI](custom-preset-cli-howto.md)

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde, güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

[Temel kavramlar hakkında bilgi edinin](concepts-overview.md)
