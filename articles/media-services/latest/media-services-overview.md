---
title: Azure Media Services v3 'ye Genel Bakış | Microsoft Docs
description: Bu makalede Media Services için üst düzey bir genel bakış sağlanır ve daha fazla ayrıntı için makalelerin bağlantıları sağlanmaktadır.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: Azure Media Services, Stream, Broadcast, Live, OFFLINE
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 09/17/2019
ms.author: juliako
ms.custom: mvc
ms.openlocfilehash: c6359cf2401ff198b0242243dbf6dfdf2e35ce47
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72244030"
---
# <a name="azure-media-services-v3-overview"></a>Azure Media Services v3 genel bakış

Azure Media Services, yayın kalitesinde video akışı sağlayan, erişilebilirliği ve dağıtımı geliştiren, içeriği çözümleyen ve çok daha birçok çözüm oluşturmanıza olanak sağlayan bulut tabanlı bir platformdur. Bir uygulama geliştiricisi, bir devlet kurumu olan bir cep telefonu, bir eğlence Media Services şirketi ve en popüler mobil cihazlarda büyük kitlelere kadar üstün kalitede medya deneyimleri sunan uygulamalar oluşturmanıza yardımcı olur. ve tarayıcılar. 

Media Services v3 SDK 'Ları, [Media Services v3 Openapı belirtimini (Swagger)](https://aka.ms/ams-v3-rest-sdk)temel alır.

> [!NOTE]
> Şu anda v3 kaynaklarını yönetmek için Azure portal kullanamazsınız. [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref)veya desteklenen [SDK 'lardan](media-services-apis-overview.md#sdks)birini kullanın.

## <a name="what-can-i-do-with-media-services"></a>Media Services ile ne yapabilirim?

Media Services, bulutta çeşitli medya iş akışları oluşturmanıza olanak tanıdığından, Media Services ile neler yapabileceğinize ilişkin bazı örnekler aşağıda verilmiştir.  

* Birçok farklı biçimde tarayıcıda ve cihazlarda oynatılabilecek videoları çeşitli biçimlerde sunun. Çeşitli istemcilere (mobil cihazlar, TV, PC vb.) hem isteğe bağlı hem de canlı akış teslimi için, video ve ses içeriğinin uygun şekilde kodlanıp paketlenmesi gerekir. Bu içeriğin nasıl teslim edileceği ve akışının nasıl yapılacağını görmek için bkz. [hızlı başlangıç: kodlama ve akış dosyaları](stream-files-dotnet-quickstart.md).
* Canlı spor olaylarını futbol, bey, üniversite ve yüksek okul spor gibi büyük bir çevrimiçi kitleye akış ve daha fazlasını yapın. 
* Şehir, şehir ve yasama gövdeleri gibi genel toplantıları ve olayları yayınlayın.
* Kaydedilmiş videoları veya ses içeriğini analiz edin. Örneğin, daha yüksek müşteri memnuniyetini sağlamak için, kuruluşlar konuşmayı metne ve derleme arama dizinlerini ve panoları ayıklayabilir. Daha sonra, yaygın şikayetler, şikayet kaynakları ve diğer ilgili veriler hakkında zeka bilgileri ayıklayabilir.
* Abonelik video hizmeti oluşturun ve bir müşterinin (örneğin, bir film Studio), özel olarak telif haklı çalışmanın erişimini ve kullanımını kısıtlayaması gerektiğinde DRM korumalı içerik akışı yapın.
* Airdüzler, Trains ve otomobil oynatma işlemleri için çevrimdışı içerik sunun. Bir müşterinin, ağla bağlantısı kesilmek üzere tahmin edildiğinde kayıttan yürütmek için içeriği telefonlarına veya tabletlerine indirmesi gerekebilir.
* Konuşmayı metne dönüştürme, çok dilde çevirme vb. için Azure Media Services ve [Azure bilişsel hizmetler API'si](https://docs.microsoft.com/azure/#pivot=products&panel=ai) ile eğitim e-öğrenim video platformunu uygulayın. 
* Daha geniş bir hedef kitle (örneğin, işitme engelli kişiler veya farklı bir dilde okumak isteyen kişiler) için videolara alt yazı ve açıklamalı alt yazılar eklemek için [Azure bilişsel hizmetler API'si](https://docs.microsoft.com/azure/#pivot=products&panel=ai) ile birlikte Azure Media Services kullanın.
* Anında yüksek yükleri daha iyi işleyebilmek için büyük ölçeklemeye ulaşmak üzere Azure CDN etkinleştirin (örneğin, bir ürün başlatma olayının başlangıcı). 

## <a name="how-can-i-get-started-with-v3"></a>V3 'i kullanmaya nasıl başlamalıyım? 

İçeriği kodlama ve paketleme, Videoları isteğe bağlı olarak yayımlama, canlı yayın ve videoları Media Services v3 ile çözümleme hakkında bilgi edinin. Öğreticiler, API başvuruları ve diğer belgeler, milyonlarca kullanıcıya ölçeklenebilen isteğe bağlı ve canlı video veya ses akışlarını güvenli bir şekilde nasıl teslim etmek istediğinizi gösterir.

> [!TIP]
> Geliştirmeye başlamadan önce şunları gözden geçirin:<br/>* [temel kavramları](concepts-overview.md) (önemli kavramlar: paketleme, kodlama, koruma vb.)<br/>* [Media Services v3 API 'leri Ile geliştirme](media-services-apis-overview.md) (API 'lere erişme hakkında bilgiler, adlandırma kuralları vb.)

### <a name="quickstarts"></a>Hızlı Başlangıçlar  

Hızlı başlangıçlarda yeni müşterilerin Media Services hızla deneyebilmesinin temel gün 1 yönergeleri gösterilir.

* [Video dosyaları akışı-.NET](stream-files-dotnet-quickstart.md)
* [Video dosyaları akışı-CLı](stream-files-cli-quickstart.md)
* [Stream video Files-Node. js](stream-files-nodejs-quickstart.md)
    
### <a name="tutorials"></a>Öğreticiler 

Öğreticiler, bazı en Media Services görevlerden bazılarının senaryo tabanlı yordamlarını gösterir.

* [Uzak dosya ve akış videosunu kodla – REST](stream-files-tutorial-with-rest.md)
* [Karşıya yüklenen dosya ve akış videosunu kodla-.NET](stream-files-tutorial-with-api.md)
* [Canlı akış-.NET](stream-live-tutorial-with-api.md)
* [Videonuzu çözümleyin-.NET](analyze-videos-tutorial-with-api.md)
* [AES-128 dinamik şifreleme-.NET](protect-with-aes128.md)
    
### <a name="samples"></a>Örnekler

Azure Media Services kod örneklerine gitmek için [Bu örnek tarayıcıyı](https://docs.microsoft.com/samples/browse/?products=azure-media-services) kullanın.

### <a name="how-to-guides"></a>Nasıl yapılır kılavuzları

Makaleler, bir görevin nasıl tamamlanalınacağını gösteren kod örnekleri içerir. Bu bölümde, birçok örnek bulacaksınız, İşte bunlardan bazıları aşağıda verilmiştir:

* [Hesap oluşturma-CLı](create-account-cli-how-to.md)
* [Erişim API 'Leri-CLı](access-api-cli-how-to.md)
* [İş girişi olarak HTTPS ile kodlama-.NET](job-input-from-http-how-to.md)  
* [Olayları izleme-Portal](monitor-events-portal-how-to.md)
* [Multi-DRM-.NET ile dinamik olarak şifreleyin](protect-with-drm.md) 
* [Özel bir dönüşümle kodlama-CLı](custom-preset-cli-howto.md)

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde bulunun, güncelleştirmeleri al

Soru sormak, geri bildirimde bulunmak ve Media Services hakkında güncelleştirmeler almak için [Azure Media Services Community](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

[Temel kavramlar hakkında bilgi edinin](concepts-overview.md)

