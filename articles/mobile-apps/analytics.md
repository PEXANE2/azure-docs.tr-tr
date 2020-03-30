---
title: Visual Studio App Center ve Azure hizmetleriyle mobil uygulama kullanımını ve kullanıcı davranışını anlama
description: Kullanıcıların mobil uygulamanızı nasıl kullandığını anlayarak akıllı iş kararları almanıza yardımcı olan App Center gibi hizmetler hakkında bilgi edinin.
author: codemillmatt
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: 4dc3cea4497c55dda0d8da2ca29201615dadff19
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80241068"
---
# <a name="analyze-and-understand-mobile-application-use"></a>Mobil uygulama kullanımını analiz edin ve anlayın
Kullanıcılarınızın uygulamalarınızı nasıl kullandığını ne kadar iyi anlıyorsunuz? Uygulamanızın kaç aktif kullanıcısı var ve kullanım zaman içinde nasıl değişiyor? Hangi özellikleri kullanıyorlar ve en çok hangileri kullanılıyor? Bu kullanıcılar nerede dayanır? Uygulamanın en son sürümünü kaç kullanıcı kullanıyor? Uygulamanızı başarılı bir işletmeye dönüştürmek için tüm bu soruları anlamak önemlidir. Bu tür kullanım analizi sorularını yanıtlamak için uygulamalarınızdan kullanım verileri toplamanız gerekir.

Verilere ne kadar çok bakarsanız, uygulamanızı geliştirmenin ve kullanıcılarınızı mutlu etmenin yollarını o kadar çok bulabilirsiniz. Eyleme geçirilebilir öngörüler bulmak ve kullanıcıları memnun etmek için verileri kullanmak önemlidir.

## <a name="importance-of-analytics"></a>Analitiğin önemi
- Kullanıcılarınızı, uygulamanızla nasıl etkileşimde olduklarını ve uygulamanızı yeniden ayarlamak ve işletmenizi büyütmek için harika deneyimler sağlamak için onları geri getiren şeyi anlayın.
- Uygulamanızı nasıl pazarlayıp müşterilerinize daha iyi hizmet verilebilen bilinçli kararlar almak için kullanım ölçümlerinizi izleyin.
- Uygulama performansınızı ölçün.
- Uygulamanızın hangi bölümlerini sürücü değeri ve performansını öğrenin.
- Karmaşa ve saklamayla ilgili konularhakkında veri odaklı öngörüler elde edin.

Mobil uygulama analizini etkinleştirmek için aşağıdaki hizmetleri kullanın.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Analytics,](/appcenter/analytics/) önemli olanşeylere odaklanarak hedef kitlenizi büyütmenizi sağlar. Kullanıcı oturumları, en iyi cihazlar, işletim sistemi sürümleri ve davranış analizi hakkında derin raporlama ve öngörüler sunar. Kapsamlı uygulama analitiği yle her şeyi izlemek için kolayca özel etkinlikler oluşturun.

   **Önemli özellikler**
   - Kullanım modellerini, kullanıcı benimsenmesini ve diğer etkileşim ölçümlerini ücretsiz olarak izleyin.
   - Özel etkinlikler aracılığıyla eğilimleri, kullanıcı davranışını ve etkileşimi tanımlayın.
   - Tek bir panoda uygulama kullanımı (günlük, haftalık, aylık), oturumlar, aygıt özellikleri ve kullanıcı demografisi hakkında ayrıntılı öngörüler ve kullanıma uygun ölçümler elde edin.
   - Sınırsız bekletme için tüm App Center Analytics verilerinizi sürekli olarak Azure'a aktarın. App Center Analytics, Azure Blob depolama sına ve Azure Uygulama Öngörülerine yapılan dışa aktarmayı destekler.
   - Bekletme, huni analizi ve kohortlar gibi daha da derin öngörüler için Azure Uygulama Öngörüleri ile tümleştirin.
   - Birkaç dakika içinde başlamak için tek satırlık SDK tümleştirmesini kullanın.
   - iOS, Android, macOS, tvOS, Xamarin, React Native, Unity ve Cordova için platform desteği kazanın.

   **Başvurular**
   - [App Center ile kaydolun](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
   - [App Center Analytics ile başlayın](/appcenter/analytics/)

## <a name="azure-monitor"></a>Azure İzleyici
Azure Monitor, performansı en üst düzeye çıkarmak ve mobil uygulamanızı izlemek için telemetri toplamak ve analiz etmek için araçlar sağlayan [Uygulama Öngörüleri'ni](/azure/azure-monitor/app/app-insights-overview)içerir. Uygulama Öngörüleri'ne ihracat ayarlamak için App Center Analytics'i kullanarak Uygulama Öngörüleri'nden yararlanabilirsiniz. Application Insights, Uygulama Merkezi'nin sağladığı analiz araçlarının ötesinde, uygulamalarınızdan özel olay telemetrisini sorgulayabilir, segmente edebilir, filtreleyebilir ve analiz edebilir.

**Önemli özellikler**
   - Özel olay telemetrinizi sorgulayın.
   - Güçlü segmentasyon özellikleriyle olay telemetrisini filtreleyin.
   - Uygulamanızdaki dönüşüm, bekletme ve gezinme desenlerini analiz edin. Şunu kullanabilirsiniz:
     - Dönüşüm oranlarını analiz etmek ve izlemek için huniler.
     - Uygulamanızın kullanıcıları zaman içinde ne kadar iyi tuttuğunu analiz etmek için bekletme.
     - Görselleştirmeleri ve metni paylaşılabilir bir raporda birleştirmek için çalışma kitapları.
     - Diğer analiz araçlarından kolayca başvurulabilmeleri için belirli kullanıcı gruplarının veya olayların adlarını vermek ve kaydetmek için kohortlar.

**Başvurular**
- [Azure portalında](https://portal.azure.com/)
- [Uygulama Merkezi ve Uygulama Öngörüleri ile mobil uygulamanızı analiz edin](/azure/azure-monitor/learn/mobile-center-quickstart)
- [Uygulama Öngörüleri ile Uygulama Merkezi Analizini Kullanma](/azure/azure-monitor/app/usage-overview)

## <a name="azure-playfab"></a>Azure PlayFab
[Azure PlayFab,](https://playfab.com/) dünya standartlarında bulutbağlantılı oyunlar oluşturmak için ihtiyaç duyduğunuz oyun hizmetleri, gerçek zamanlı analizler ve LiveOps ile eksiksiz bir arka uç platformu sunar. Bu hizmetler, oyun geliştiricileri için başlatılması gereken engelleri azaltır. Hem büyük hem de küçük stüdyolar kendi oyunları ile ölçeklendirmek maliyet-etkin geliştirme çözümleri sunuyoruz. Hizmetler, stüdyoların oyuncularla etkileşime girmenize, tutmana ve para kazanmaya yardımcı olabilir. PlayFab ile geliştiriciler, oyun oluşturmak ve işletmek, oyun verilerini analiz etmek ve genel oyun deneyimlerini geliştirmek için akıllı bulutu kullanabilir.

**Önemli özellikler**
   - Gerçek zamanlı panoları izleyin.
   - Oyununuzun performansını en iyi ölçümlerle değerlendirin.
   - Otomatik olarak oluşturulan raporlar aracılığıyla oyununuzun günlük ve aylık performansının özetlerini gözden geçirin. Raporları Game Manager'da görüntüleyebilir ve her gün indirebilir veya gelen kutunuza teslim edebilirsiniz.
   - Denemeleri çalıştırmak ve belirli bir değişken için en uygun ayarı belirlemek için A/B testini kullanın.
   - Oyuncuların otomatik gruplandırmalarını tanımlamak için oyuncular için segmentasyon kullanın.
    
**Başvurular**
- [PlayFab portalı](https://developer.playfab.com/en-US/sign-up)
- [Analiz](/gaming/playfab/#pivot=documentation&panel=analytics)
- [Hızlı başlangıçlar](/gaming/playfab/#pivot=documentation&panel=quickstarts) 
