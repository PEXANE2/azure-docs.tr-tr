---
title: Kullanıcı analizi araçlarını giderme - Azure Uygulama Öngörüleri
description: Sorun giderme kılavuzu - Application Insights ile site ve uygulama kullanımını analiz etme.
ms.topic: conceptual
author: NumberByColors
ms.author: daviste
ms.date: 07/11/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 8d2e573f34895207a455838b5fc64f95560943d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670925"
---
# <a name="troubleshoot-user-behavior-analytics-tools-in-application-insights"></a>Application Insights'ta kullanıcı davranışı analizi araçlarını giderme
[Uygulama Öngörüleri'nde kullanıcı davranışı analizi araçları](usage-overview.md)hakkında sorularınız mı var: [Kullanıcılar, Oturumlar, Etkinlikler,](usage-segmentation.md) [Huniler,](usage-funnels.md) [Kullanıcı Akışları,](usage-flows.md) [Bekletme](usage-retention.md)veya Kohortlar? İşte bazı cevaplar.

## <a name="counting-users"></a>Kullanıcıları Sayma
**Kullanıcı davranışı analizi araçları, uygulamamın tek bir kullanıcısı/oturumu olduğunu gösterir, ancak uygulamamın birçok kullanıcısı/oturumu olduğunu biliyorum. Bu yanlış sayıları nasıl düzeltebilirim?**

Application Insights'taki tüm telemetri etkinlikleri, standart özelliklerinden ikisi olarak anonim bir [kullanıcı kimliğine](../../azure-monitor/app/data-model-context.md) ve [oturum kimliğine](../../azure-monitor/app/data-model-context.md) sahiptir. Varsayılan olarak, tüm kullanım analizi araçları bu t'lere dayalı olarak kullanıcıları ve oturumları sayar. Bu standart özellikler uygulamanızın her kullanıcısı ve oturumu için benzersiz ahkilerle doldurulmuyorsa, kullanım analizi araçlarında kullanıcı ve oturum sayısının yanlış olduğunu görürsünüz.

Bir web uygulamasını izliyorsanız, en kolay çözüm Uygulama [Öngörüleri JavaScript SDK'yı](../../azure-monitor/app/javascript.md) uygulamanıza eklemek ve komut dosyası snippet'inin izlemek istediğiniz her sayfaya yüklendiğinden emin olmaktır. JavaScript SDK otomatik olarak anonim kullanıcı ve oturum kimlikleri oluşturur, ardından uygulamanızdan gönderilen telemetri olaylarını bu kimliklerle doldurur.

Bir web hizmetini (kullanıcı arabirimi yok) izliyorsanız, hizmetinizin benzersiz kullanıcı ve oturum kavramlarına göre [anonim kullanıcı kimliği ve oturum kimliği özelliklerini dolduran bir telemetri başlatma laştırıcı oluşturun.](usage-send-user-context.md)

Uygulamanız kimlik [doğrulaması kullanıcı kimlikleri](../../azure-monitor/app/api-custom-events-metrics.md#authenticated-users)gönderiyorsa, Kullanıcılar aracındaki kimlik doğrulamalı kullanıcı kimliklerine göre sayabilirsiniz. "Göster" açılır açılır düşüşünde "Kimlik doğrulaması yapılan kullanıcıları" seçin.

Kullanıcı davranışı analizi araçları şu anda anonim kullanıcı kimliği, kimlik doğrulaması kullanıcı kimliği veya oturum kimliği dışındaki özelliklere dayalı olarak kullanıcıları veya oturumları saymayı desteklemez.

## <a name="naming-events"></a>Olayları Adlandırma
**Uygulamamda binlerce farklı sayfa görünümü ve özel etkinlik adı var. Aralarında ayrım yapmak zordur ve kullanıcı davranışı analizi araçları genellikle yanıt vermez hale gelir. Bu adlandırma sorunlarını nasıl düzeltebilirim?**

Kullanıcı davranışı analizi araçları boyunca sayfa görünümü ve özel olay adları kullanılır. Olayları iyi adlandırma, bu araçlardan değer elde etmek için çok önemlidir. Amaç, çok az, aşırı genel ada sahip olmak ("Düğme tıklandı") ile çok fazla, aşırı özel ada sahip olmak ("http:\//www.contoso.com/index"' düğmesini tıklattılar) arasındaki dengedir.

Uygulamanızın gönderdiği sayfa görünümünde ve özel etkinlik adlarında değişiklik yapmak için uygulamanızın kaynak kodunu değiştirmeniz ve yeniden dağıtmanız gerekir. **Application Insights'taki tüm telemetri verileri 90 gün boyunca saklanır ve silinemez,** bu nedenle olay adlarında yaptığınız değişikliklerin tam olarak ortaya çıkması 90 gün sürer. Ad değişikliği yaptıktan sonraki 90 gün boyunca, hem eski hem de yeni olay adları telemetrinizde gösterilecek, bu nedenle sorguları ayarlayın ve ekipleriniz içinde buna göre iletişim kurun.

Uygulamanız çok fazla sayfa görüntüleme adı gönderiyorsa, bu sayfa görünümü adlarının kodda el ile belirtilip belirtilmediklerini veya Application Insights JavaScript SDK tarafından otomatik olarak gönderilip gönderilmediğini kontrol edin:

* Sayfa görünümü adları API kullanılarak kodolarak el ile [ `trackPageView` belirtilirse,](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)adı daha az spesifik olacak şekilde değiştirin. URL'yi sayfa görünümü adına koymak gibi yaygın hatalardan kaçının. Bunun yerine, API'nin `trackPageView` URL parametresini kullanın. Sayfa görünümü adındaki diğer ayrıntıları özel özelliklere taşıyın.

* Application Insights JavaScript SDK otomatik olarak sayfa görüntüleme adları gönderiyorsa, sayfalarınızın başlıklarını değiştirebilir veya sayfa görünümü adlarını el ile gönderecek şekilde geçiş yapabilirsiniz. SDK varsayılan olarak her sayfanın [başlığını](https://developer.mozilla.org/docs/Web/HTML/Element/title) sayfa görünümü adı olarak gönderir. Daha genel olmak için başlıklarınızı değiştirebilir, ancak SEO ve bu değişikliğin sahip olabileceği diğer etkileri dikkatli olun. `trackPageView` API ile sayfa görünümü adlarını el ile belirtmek otomatik olarak toplanan adları geçersiz kılar, böylece sayfa başlıklarını değiştirmeden telemetride daha fazla genel ad gönderebilirsiniz.   

Uygulamanız çok fazla özel etkinlik adı gönderiyorsa, koddaki adı daha az spesifik olacak şekilde değiştirin. Yine, URL'leri ve diğer sayfa başına veya dinamik bilgileri doğrudan özel olay adlarına koymaktan kaçının. Bunun yerine, bu ayrıntıları API ile `trackEvent` özel olay özel özelliklerine taşıyın. Örneğin, bunun `appInsights.trackEvent("Edit button clicked on http://www.contoso.com/index")`yerine , biz `appInsights.trackEvent("Edit button clicked", { "Source URL": "http://www.contoso.com/index" })`gibi bir şey öneririz .

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıcı davranışı analizi araçlarına genel bakış](usage-overview.md)

## <a name="get-help"></a>Yardım alın
* [Stack Overflow](https://stackoverflow.com/questions/tagged/ms-application-insights)

