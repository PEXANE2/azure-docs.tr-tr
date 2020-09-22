---
title: Ölçüm Danışmanı tanıtımına keşfetmeye başlayın
titleSuffix: Azure Cognitive Services
description: Sunduğumuz tanıtımı kullanarak ölçüm Danışmanı Web arabirimi hakkında bilgi edinin
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: quickstart
ms.date: 09/14/2020
ms.author: aahi
ms.openlocfilehash: fbc73e20b8cc2baa1cc5c5a5b2f674fb1b2dde84
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948183"
---
# <a name="quickstart-explore-the-metrics-advisor-demo-with-example-data"></a>Hızlı başlangıç: örnek verilerle ölçüm Danışmanı tanıtımına göz atın

> [!Note]
> Ölçüm Danışmanı tanıtımı salt okunurdur ve verilerinizi eklemek mümkün olmayacaktır. Bu hizmeti verilerinizde kullanmak için önce [bir ölçüm Danışmanı kaynağı oluşturun](web-portal.md).

Ölçüm Danışmanı 'nda temel özellikleri keşfetmeye başlamak için bu makaleyi kullanın. Tam özellikli Web portalı hakkında bilgi sahibi olmanız için örnek veriler ve önceden ayarlanmış yapılandırmalara sahip bir demo sitesi sağlıyoruz.

Tanıtım sitesine gitmek için [Bu bağlantıya](https://aka.ms/MetricsAdvisor/Demo) tıklayın.

## <a name="view-the-available-sample-data"></a>Kullanılabilir örnek verileri görüntüleme

Tanıtım sitesinde oturum açtıktan sonra bir **veri akışı** sayfası görürsünüz. veri akışı, veri kaynağınızdan sorgulanan, zaman serisi verilerinin mantıksal bir grubudur. Ölçüm Danışmanı 'nda kullanılan hüküm ve kavramlar hakkında daha fazla bilgi için bkz. [Sözlük](../glossary.md). 

Azure SQL veritabanı veya Azure tablosu gibi farklı türlerde veri kaynaklarından alınan çeşitli veri akışları listelenmiştir. Her biri, ilişkili veri depolarına bağlanmak için biraz farklı yapılandırma ayarları kullanır.

:::image type="content" source="../media/sample-datafeeds.png" alt-text="Örnek veri listesi" lightbox="../media/sample-datafeeds.png":::

## <a name="explore-the-data-feed-configurations"></a>Veri akışı yapılandırmasını keşfet

*Örnek maliyet/gelir-şehir/kategori* veri akışına tıklayın. Akışa yönelik ayrıntıların birkaç bölümünü görürsünüz:

* Veri akışı adı ve alma durumu.
* Veri kaynağından sorgulanan ölçümlerin listesi. Örneğin, *Maliyet* ve *gelir*. 
* Veri akışı kullanılamaz hale geldiğinde oluşan uyarı geçmişi. 
* Veri akışı güncelleştirildiği zaman günlükleri.   
* Veri akışı bilgileri ve ayarları.

:::image type="content" source="../media/data-feed-view.png" alt-text="Veri akışı görünümü" lightbox="../media/data-feed-view.png":::


## <a name="view-time-series-visualizations-and-configurations"></a>Zaman serisi görselleştirmelerini ve konfigürasyonları görüntüleme

*Örnek maliyet/gelir-şehir/kategori* veri akışında *Maliyet* ölçüsüne tıklayın. İlgili zaman serisini, geçmiş ölçüm verilerine göre görselleştirmelerle birlikte boyutlara göre dilimleyerek görürsünüz. Ölçüm verilerinin etrafındaki mavi bant, ölçüm Danışmanı 'nın makine öğrenimi modellerinden beklenen değer aralığını temsil eder. Bu bandın dışında kalan noktalar, algılanan bozukluklar olan kırmızı noktalar olarak işaretlenir. 

:::image type="content" source="../media/series-visualization.png" alt-text="Seri görselleştirme" lightbox="../media/series-visualization.png":::

Anomali algılama, ölçüm Ayrıntıları sayfasının sol tarafındaki **yapılandırma ayarları saptanarak** yapılandırılabilir. Birden çok anomali algılama yöntemi mevcuttur ve bunları birleştirebilirsiniz. Ayrıca, farklı sensitivities deneyebilir, yönleri tespit edebilir ve diğer yapılandırmalara de sahip olabilirsiniz. **Yapılandırmaları algılamada** **Gelişmiş yapılandırma** bağlantısı, gruplar veya ayrı seriler üzerinde kullanılabilen daha karmaşık ve özelleştirilmiş algılama ayarları oluşturmanızı sağlar. 

Ayrıca, algılama algoritmasına geri bildirim sağlayarak anomali algılamayı ayarlayabilirsiniz. Anomali durumunu, mevsimsellik ve değişiklik noktası durumunu yapılandırmak için anomali ' e tıklayın ve **geri bildirim Ekle** bölmesini kullanın. Bu geri bildirim, gelecekteki noktalara yönelik olarak algılamaya eklenecektir.  

**Geri bildirim Ekle** bölmesinin en altında, olay **hub 'ına**bir bağlantı vardır ve bu, sizi olay Analizi sayfasına yönlendirir ve olayın kök nedenini analiz eder.  

:::image type="content" source="../media/incident-link.png" alt-text="Olay bağlantısı" lightbox="../media/incident-link.png":::

## <a name="explore-anomaly-detection-results-and-perform-root-cause-analysis"></a>Anomali algılama sonuçlarını keşfet ve kök neden analizi gerçekleştir

Bir anomali 'ten gelen **Olay Hub 'ına** tıkladığınızda, olayla ilgili tanılama öngörülerini içeren bir olay analiz sayfası görürsünüz; Örneğin, önem derecesi, ilgili anomali sayısı ve başlangıç/bitiş saati. **Kök nedeni** bölümü, olay ağacını analiz ederek otomatik önerileri görüntüler. bu durum, olayın kök nedeni olan sapma, dağıtım ve üst bozukluklar için katkı sağlar.

**Tanılama** bölümünde, olayın tanılanması için çeşitli sekmelerin yanı ve olay ağacı gösterilmektedir.

:::image type="content" source="../media/incident-diagnostic.png" alt-text="Olay tanılama" lightbox="../media/incident-diagnostic.png":::

Olayın temel nedenini işaret ederek, durum kötüleşmeden önce eyleme çıkabilir ve sorunu azaltabilirsiniz. Ayrıca, sunulan diğer tanılama özelliklerine tıklayarak daha fazla öngörü keşfedebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

- [Web portalını kullanma](web-portal.md)
- [REST API kullanma](rest-api.md)
- [Veri akışlarınızı ekleme](../how-tos/onboard-your-data.md)
    - [Veri akışlarını yönetme](../how-tos/manage-data-feeds.md)
    - [Farklı veri kaynakları için yapılandırma](../data-feeds-from-different-sources.md)
