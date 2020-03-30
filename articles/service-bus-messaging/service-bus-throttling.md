---
title: Azure Hizmet Veri Servisi Azaltma Bölümüne Genel Bakış | Microsoft Dokümanlar
description: Hizmet Veri Servisi azaltma - Standart ve Premium katmanlara genel bakış.
services: service-bus-messaging
author: axisc
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 10/01/2019
ms.author: aschhab
ms.openlocfilehash: f852ad70b2eb97e2b8b3e40d086e98b3836c3592
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77598298"
---
# <a name="throttling-operations-on-azure-service-bus"></a>Azure Hizmet Veri Yolunda azaltma işlemleri

Bulut yerel çözümleri, iş yükünüzle ölçeklendirilebilen sınırsız kaynak kavramı sunar. Bu kavram bulutta şirket içi sistemlerden daha doğru olsa da, bulutta hala var olan sınırlamalar vardır.

Bu sınırlamalar, bu makalede belirtildiği gibi hem Standart hem de Premium katmanlarda istemci uygulama isteklerinin daraltılmasına neden olabilir. 

## <a name="throttling-in-azure-service-bus-standard-tier"></a>Azure Hizmet Veri Veri Mes'de Azaltma Standart katmanında azaltma

Azure Hizmet Veri Servisi Standart katmanı, istediğiniz kadar öde fiyatlandırma modeliyle çok kiracılı bir kurulum olarak çalışır. Burada, aynı kümedeki birden çok ad alanı ayrılan kaynakları paylaşır. Standart katman, düşük üretim sistemleriyle birlikte geliştirici, test ve QA ortamları için önerilen seçimdir.

Geçmişte, Azure Hizmet Veri Servisi'nde kesinlikle kaynak kullanımına bağlı kaba azaltma sınırları vardı. Ancak, azaltma mantığını hassaslaştırmak ve bu kaynakları paylaşan tüm ad boşluklarına öngörülebilir azaltma davranışı sağlamak için bir fırsat vardır.

Aynı kaynakları kullanan tüm Azure Hizmet Veri Hizmeti Veri Merkezi Standart ad alanlarında kaynakların adil kullanımını ve dağıtımını sağlamak amacıyla, azaltma mantığı kredi tabanlı olarak değiştirilmiştir.

> [!NOTE]
> Azaltmanın Azure Hizmet Veri Servisi veya herhangi bir bulut yerel hizmeti için ***yeni olmadığını*** unutmayın.
>
> Kredi tabanlı azaltma, çeşitli ad alanlarının kaynakları çok kiracılı Standart katman ortamında paylaşma biçimini yeniden rafine etmek ve böylece kaynakları paylaşan tüm ad alanlarının adil kullanımını sağlamaktır.

### <a name="what-is-credit-based-throttling"></a>Kredi tabanlı azaltma nedir?

Kredi tabanlı azaltma, belirli bir ad alanında belirli bir zaman diliminde gerçekleştirilebilecek işlem sayısını sınırlar. 

Aşağıda kredi tabanlı azaltma için iş akışı - 

  * Her zaman döneminin başlangıcında, her ad alanına belirli sayıda kredi sağlarız.
  * Gönderen veya alıcı istemci uygulamaları tarafından gerçekleştirilen işlemler bu kredilere göre sayılır (ve kullanılabilir kredilerden çıkarılır).
  * Krediler tükenirse, sonraki işlemler bir sonraki zaman diliminin başlangıcına kadar daraltılır.
  * Krediler bir sonraki zaman diliminin başında yenilenir.

### <a name="what-are-the-credit-limits"></a>Kredi limitleri nelerdir?

Kredi limitleri şu anda her saniye (ad alanı başına) '1000' kredi olarak ayarlanır.

Tüm işlemler eşit oluşturulmaz. Burada her bir operasyonların kredi maliyetleri vardır - 

| İşlem | Kredi maliyeti|
|-----------|-----------|
| Veri işlemleri (Gönder, SendAsync, Al, ReceiveAsync, Peek) |İleti başına 1 kredi |
| Yönetim işlemleri (Oluşturma, Okuma, Güncelleme, Kuyruklarda Silme, Konular, Abonelikler, Filtreler) | 10 kredi |

> [!NOTE]
> Bir Konu'ya gönderirken, her iletinin Abonelik'te kullanıma sunulmadan önce filtre(ler) karşı değerlendirilir.
> Her filtre değerlendirmesi de kredi limitine (yani filtre başına 1 kredi lik değerlendirme) göre sayılır.
>

### <a name="how-will-i-know-that-im-being-throttled"></a>Gazla dolandırıldığımı nereden bileceğim?

İstemci uygulama istekleri daraltıldığında, aşağıdaki sunucu yanıtı uygulamanız tarafından alınır ve günlüğe kaydedilir.

```
The request was terminated because the entity is being throttled. Error code: 50009. Please wait 2 seconds and try again.
```

### <a name="how-can-i-avoid-being-throttled"></a>Nasıl boğazlanmaktan kaçınabilirim?

Paylaşılan kaynaklarla, bu kaynakları paylaşan çeşitli Hizmet Veri Günü ad alanlarında bir tür adil kullanım uygulamak önemlidir. Azaltma, tek bir iş yükündeki ani artışların aynı kaynaklardaki diğer iş yüklerinin daraltılmasına neden olmamasını sağlar.

Makalenin daha sonralarında da belirtildiği gibi, istemci SDK'leri (ve diğer Azure PaaS teklifleri) varsayılan yeniden deneme ilkesine sahip olduğundan, daraltMa riski yoktur. Herhangi bir daraltılmış istekler üstel geri leme ile yeniden denenecek ve sonunda kredi ler yenilendiğinde geçecek.

Anlaşılır bir şekilde, bazı uygulamalar daraltılmasına karşı hassas olabilir. Bu durumda, [geçerli Hizmet Veri Servisi Standart ad alanınızı Premium'a geçirmeniz](service-bus-migrate-standard-premium.md)önerilir. 

Geçişte, Hizmet Veri Yolunda ad alanınıza özel kaynaklar tahsis edebilir ve iş yüklerinizde bir artış olduğunda kaynakları uygun şekilde ölçeklendirebilir ve daraltılma olasılığını azaltabilirsiniz. Ayrıca, iş yükünüz normal düzeylere düştüğünde, ad alanınıza ayrılan kaynakları küçültebilirsiniz.

## <a name="throttling-in-azure-service-bus-premium-tier"></a>Azure Hizmet Veri Hizmetleri Premium katmanında azaltma

[Azure Hizmet Veri Servisi Premium](service-bus-premium-messaging.md) katmanı, mesajlaşma birimleri açısından müşteri tarafından her ad alanı kurulumuna özel kaynaklar ayırır. Bu özel kaynaklar öngörülebilir üretim ve gecikme sağlar ve yüksek iş üretimi veya hassas üretim sınıfı sistemleri için önerilir.

Ayrıca, Premium katmanı, müşterilerin iş yükünde ani artışlar yaşadıklarında iş kapasitelerini ölçeklendirmelerine de olanak tanır.

### <a name="how-does-throttling-work-in-service-bus-premium"></a>Service Bus Premium'da azaltma nasıl çalışır?

Service Bus Premium için özel kaynak tahsisi ile, azaltma tamamen ad alanına ayrılan kaynakların sınırlamaları tarafından yönlendirilir.

İstek sayısı geçerli kaynakların hizmet edebileceğinden fazlaysa, istekler daraltılacaktır.

### <a name="how-will-i-know-that-im-being-throttled"></a>Gazla dolandırıldığımı nereden bileceğim?

Azure Hizmet Veri Yolu Premium'da azaltmayı belirlemenin çeşitli yolları vardır - 
  * **Azaltılmış İstekler,** kaç isteğin azaltıldığını belirlemek için [Azure Monitör İsteği ölçümlerinde](service-bus-metrics-azure-monitor.md#request-metrics) yer gösterir.
  * Yüksek **CPU Kullanımı,** geçerli kaynak ayırmanın yüksek olduğunu ve geçerli iş yükü azaltmazsa isteklerin azaltılabilir olduğunu gösterir.
  * Yüksek **Bellek Kullanımı,** geçerli kaynak ayırmanın yüksek olduğunu ve geçerli iş yükü azaltmazsa isteklerin azaltılabilir olduğunu gösterir.

### <a name="how-can-i-avoid-being-throttled"></a>Nasıl boğazlanmaktan kaçınabilirim?

Service Bus Premium ad alanı zaten özel kaynaklara sahip olduğundan, iş yükünde bir artış olması durumunda (veya beklentiyle) ad alanınıza ayrılan İleti birimi sayısını artırarak daraltma olasılığını azaltabilirsiniz.

Yukarıdaki ölçümlerdeki değişikliklertarafından tetiklenebilen [runbook'lar](../automation/automation-create-alert-triggered-runbook.md) oluşturularak yukarı/aşağı ölçekleme elde edilebilir.

## <a name="faqs"></a>SSS

### <a name="how-does-throttling-affect-my-application"></a>Azaltma uygulamamı nasıl etkiler?

Bir istek azaltıldığında, kaynakların izin verdiğinden daha fazla istekle karşı karşıya olduğundan hizmetin meşgul olduğunu gösterir. Aynı işlem birkaç dakika sonra tekrar denenmişse, hizmet geçerli iş yükü boyunca çalıştıktan sonra, istek yerine getirilebilir.

Azaltma, herhangi bir bulut yerel hizmetinin beklenen davranışı olduğundan, yeniden deneme mantığını Azure Hizmet Veri Servisi SDK'nın kendisine kurduk. Varsayılan değer, her seferinde aynı isteğin daraltılmadığından emin olmak için üstel bir geri lemeyle otomatik yeniden deneme olarak ayarlanır.

Varsayılan yeniden deneme mantığı her işlem için geçerliolacaktır.

### <a name="does-throttling-result-in-data-loss"></a>Azaltma veri kaybına neden olur mu?

Azure Hizmet Veri Servisi kalıcılık için optimize edilebiyi, Hizmet Veri Servisi'ne gönderilen tüm verilerin hizmet isteğinin başarısını kabul etmeden önce depolamaya adadığından emin oluruz.

İstek Servis Veri Servisi tarafından başarıyla 'ACK' (kabul) edildikten sonra, Servis Veri Servisi'nin isteği başarıyla işlediği anlamına gelir. Servis Veri Servisi bir 'NACK' (hata) döndürürse, Servis Veri Servisi'nin isteği işleyemediği ve istemci uygulamasının isteği yeniden denemesi gerektiği anlamına gelir.

Ancak, bir istek azaltıldığında, hizmet kaynak sınırlamaları nedeniyle isteği şu anda kabul edemeyeceğini ve işleyemeyeceğini ima eder. Servis Veri Servisi isteğe bakmadığından, bu herhangi bir veri kaybı anlamına **gelmez.** Bu durumda, Hizmet Veri Servisi SDK'nın varsayılan yeniden deneme ilkesine güvenerek, isteğin sonunda işlenmesini sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi ve Servis Veri Servisi mesajlaşması kullanma örnekleri için aşağıdaki gelişmiş konulara bakın:

* [Service Bus mesajlaşma hizmetine genel bakış](service-bus-messaging-overview.md)
* [Hızlı başlangıç: Azure portalı ve .NET kullanarak iletileri gönderme ve alma](service-bus-quickstart-portal.md)
* [Öğretici: Azure portalı ve konuları/abonelikleri kullanarak envanter güncelleştirme](service-bus-tutorial-topics-subscriptions-portal.md)

