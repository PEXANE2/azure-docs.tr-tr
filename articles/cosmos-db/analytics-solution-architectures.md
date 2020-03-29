---
title: Azure Cosmos DB'de küresel olarak dağıtılmış analizler kullanan çözümler.
description: Azure Cosmos DB'deki küresel olarak dağıtılan analizler kullanılarak oluşturulabilecek çözümler hakkında bilgi edinin.
author: markjbrown
ms.author: mjbrown
ms.topic: conceptual
ms.service: cosmos-db
ms.date: 09/26/2019
ms.reviewer: sngun
ms.openlocfilehash: d0b5042c1401a95cc75f4c01ace843659b6babfe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73681689"
---
# <a name="solutions-using-globally-distributed-analytics-in-azure-cosmos-db"></a>Azure Cosmos DB'de küresel olarak dağıtılmış analizler kullanan çözümler

Bu makalede, Azure Cosmos DB'de genel olarak dağıtılan analizler kullanılarak oluşturulabilen çözümler açıklanmaktadır.

## <a name="retail-and-consumer-goods"></a>Perakende ve tüketim malları

Azure Cosmos DB'de Spark desteğini kullanarak gerçek zamanlı öneriler ve teklifler sunabilirsiniz. Müşterilerin gerçek zamanlı kişiselleştirme ve ürün önerileri ile ihtiyaç duyulabilecek öğeleri keşfetmelerine yardımcı olabilirsiniz.

* Ürün kataloglarında gerçek zamanlı öneriler oluşturmak için Apache Spark çalışma zamanı tarafından sağlanan yerleşik Machine Learning desteğini kullanabilirsiniz.

* Yaşam boyu değeri hedefleyen öneriler sağlamak için tıklama akışı verisini, veri satın alma verilerini ve müşteri verilerini mayınlayabilirsiniz.

* Azure Cosmos DB'nin genel dağıtım özelliği kullanılarak, bölgelere yayılan yüksek hacimli ürün verileri milisaniye cinsinden analiz edilebilir.

* Coğrafi olarak dağıtılan kullanıcılar ve veriler için hızlı bir şekilde öngörüler alabilirsiniz. Doğru reklamı doğru kullanıcıya doğru zamanda sunarak promosyon dönüşüm oranını artırabilirsiniz.

* Dahili Spark akış özelliğinden, statik müşteri verileriyle birleştirerek canlı verileri zenginleştirebilirsiniz. Bu şekilde, daha kişiselleştirilmiş ve hedefli reklamlar gerçek zamanlı olarak ve müşterilerin ne yaptığıyla ilgili olarak sunabilirsiniz.

Aşağıdaki resim, Azure Cosmos DB Spark desteğinin fiyatlandırma ve promosyonları optimize etmek için nasıl kullanıldığını gösterir:

![Fiyatlandırma ve promosyonları optimize etmek için Azure Cosmos DB'nin Spark desteği](./media/analytics-solution-architectures/optimize-pricing-and-promotions.png)


Aşağıdaki resim, Azure Cosmos DB Spark desteğinin gerçek zamanlı öneri altyapısında nasıl kullanıldığını gösterir:

![Azure Cosmos DB Spark desteği gerçek zamanlı öneri motorunda](./media/analytics-solution-architectures/real-time-recommendation-engine.png)

## <a name="manufacturing-and-iot"></a>İmalat ve IoT

Azure Cosmos DB'nin dahili analiz platformu, küresel ölçekte milyonlarca cihazdan gelen IoT verilerinin gerçek zamanlı analizini etkinleştirmenize olanak tanır. Hava durumunu tahmin etme, tahmine dayalı analiz ler ve enerji optimizasyonları gibi modern yenilikler yapabilirsiniz.

* Azure Cosmos DB'yi kullanarak, gerçek zamanlı varlık ölçümleri ve hava durumu faktörleri gibi verileri mayınlayabilir, ardından alandaki bağlı aygıtların performansını optimize etmek için akıllı şebeke analitiği uygulayabilirsiniz. Akıllı şebeke analitiği, işletme maliyetlerini kontrol etmek, şebeke güvenilirliğini artırmak ve tüketicilere kişiselleştirilmiş enerji hizmetleri sunmak için anahtardır.

Aşağıdaki resim, Azure Cosmos DB'nin Spark desteğinin IoT aygıtlarından alınan ölçümleri okumak ve akıllı şebeke analitiğini uygulamak için nasıl kullanıldığını gösterir:

![IoT aygıtlarından ölçümleri okumak için Azure Cosmos DB'nin Kıvılcım desteği](./media/analytics-solution-architectures/read-metrics-from-iot-devices.png)

## <a name="predictive-maintenance"></a>Tahmine dayalı bakım

* Küçük sondaj kulelerinde kullanılan kompresörler gibi varlıkların derin su platformlarına tutulması karmaşık bir çabadır. Bu varlıklar dünya çapında bulunan ve veri petabayt üretmek. Azure Cosmos DB'yi kullanarak, büyük miktarda sensör telemetrisini işlemek, varlık parçalarını depolamak ve sensör eşleme verilerini işlemek için Spark akışını kullanan uçtan uca tahmine dayalı bir veri ardışık işlem akışı oluşturabilirsiniz.

* Varlık hatalarını gerçekleşmeden önce tahmin etmek ve hata oluşmadan önce bakım iş emirleri vermek için makine öğrenimi modelleri oluşturabilir ve dağıtabilirsiniz.

Aşağıdaki resim, Azure Cosmos DB'nin Spark desteğinin tahmine dayalı bir bakım sistemi oluşturmak için nasıl kullanıldığını gösterir:

![Azure Cosmos DB'nin öngörülü bir bakım sistemi oluşturmak için Spark desteği](./media/analytics-solution-architectures/predictive-maintenance-system.png)

Aşağıdaki resim, Azure Cosmos DB'nin Kıvılcım desteğinin gerçek zamanlı araç tanılama sistemi oluşturmak için nasıl kullanıldığını göstermektedir:

![Azure Cosmos DB'nin gerçek zamanlı araç tanılama sistemi oluşturmak için Spark desteği](./media/analytics-solution-architectures/real-time-vehicle-diagnostic-system.png)

## <a name="gaming"></a>Oyun

* Azure Cosmos DB, yerleşik Spark desteğiyle, mümkün olan en iyi oyun deneyimini oluşturmak için birkaç dakika içinde gelişmiş analitik ve makine öğrenimi modellerini kolayca oluşturmanıza, ölçeklendirmenize ve dağıtmanıza olanak tanır.

* Yüksek dönüşüm oranlarıelde etmek için ilgili kişiselleştirilmiş teklifler oluşturmak için oyuncu, satın alma ve davranışsal verileri analiz edebilirsiniz.

* Spark makine öğrenimini kullanarak, oyun telemetri verilerini analiz edebilir ve öngörüler elde edebilirsiniz. Yavaş yükleme sürelerini ve oyun içi sorunları tanılayabilir ve önleyebilirsiniz.

Aşağıdaki resim, Azure Cosmos DB'nin Spark desteğinin oyun analizinde nasıl kullanıldığını göstermektedir:

![Azure Cosmos DB'nin oyun analitiğinde Kıvılcım desteği](./media/analytics-solution-architectures/gaming-analytics.png)

## <a name="next-steps"></a>Sonraki adımlar

* Azure Cosmos DB'nin avantajları hakkında bilgi edinmek için [genel bakış](introduction.md) makalesine bakın.
* [MongoDB için Azure Cosmos DB API ile başlayın](mongodb-introduction.md)
* [Azure Cosmos DB Cassandra API ile başlayın](cassandra-introduction.md)
* [Azure Cosmos DB Gremlin API ile başlayın](graph-introduction.md)
* [Azure Cosmos DB Tablo API'si ile başlayın](table-introduction.md)
