---
title: Azure ölçümleri gezgini ile başlarken
description: Azure ölçümleri gezgini ile ilk metrik grafiğinizi nasıl oluşturacağınızı öğrenin.
author: vgorbenko
services: azure-monitor
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: 920ff19b162843d1c2d631ad192ce1b3c4273a68
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248781"
---
# <a name="getting-started-with-azure-metrics-explorer"></a>Azure Ölçüm Gezgini'ni kullanmaya başlama

## <a name="where-do-i-start"></a>Nereden başlayacağım?
Azure Monitor ölçümleri gezgini, Microsoft Azure portalının grafikleri çizmeye, eğilimleri görsel olarak ilişkilendirmesine ve ölçümlerin değerlerindeki ani artışları ve düşüşleri araştırmasına olanak tanıyan bir bileşenidir. Kaynaklarınızın durumunu ve kullanımını araştırmak için metrikler gezginini kullanın. Aşağıdaki sırayla başlayın:

1. [Bir kaynak ve metrik seçin](#create-your-first-metric-chart) ve temel bir grafik bakın. Ardından, soruşturmanızla ilgili [bir zaman aralığı seçin.](#select-a-time-range)

1. [Boyut filtrelerini uygulamayı ve bölmeyi](#apply-dimension-filters-and-splitting)deneyin. Filtreler ve bölme, metrik segmentlerin genel metrik değere katkıda bulunduğuna çözümlemenize ve olası aykırıları belirlemenize olanak sağlar.

1. Panolara sabitlemeden önce grafiği özelleştirmek için [gelişmiş ayarları](#advanced-chart-settings) kullanın. Metrik değer bir eşiği aştığında veya altına düştüğünde uyarıları bildirimleri alacak şekilde [yapılandırın.](alerts-metric-overview.md)

## <a name="create-your-first-metric-chart"></a>İlk metrik grafiğinizi oluşturun

Kaynak, kaynak grubu, abonelik veya Azure Monitor görünümünden bir metrik grafik oluşturmak için **Ölçümler** sekmesini açın ve aşağıdaki adımları izleyin:

1. Kaynak seçiciyi kullanarak, ölçümleri görmek istediğiniz kaynağı seçin. (Belirli bir kaynak bağlamında **Ölçümleri** açtıysanız kaynak önceden seçilir).

    > ![Bir kaynak seçin](./media/metrics-getting-started/resource-picker.png)

2. Bazı kaynaklar için bir ad alanı seçmengerekir. Ad alanı, ölçümleri kolayca bulabilmeniz için bunları düzenlemenin bir yoludur. Örneğin, depolama hesaplarının Dosyalar, Tablolar, Blobs ve Kuyruklar ölçümleri depolamak için ayrı ad alanları vardır. Birçok kaynak türü yalnızca bir ad alanı vardır.

3. Kullanılabilir ölçümler listesinden bir metrik seçin.

    > ![Bir metrik seçin](./media/metrics-getting-started/metric-picker.png)

4. İsteğe bağlı olarak, metrik toplama değiştirebilirsiniz. Örneğin, grafiğinizin ölçümün minimum, en büyük veya ortalama değerlerini göstermesini isteyebilirsiniz.

> [!NOTE]
> Metrik **Ekle** düğmesini kullanın ve aynı grafikte çizilmiş birden çok ölçüm görmek istiyorsanız bu adımları yineleyin. Tek görünümdeki birden çok grafik için, üstteki **Grafik Ekle** düğmesini seçin.

## <a name="select-a-time-range"></a>Bir zaman aralığı seçin

Varsayılan olarak, grafik en son 24 saatlik ölçüm verilerini gösterir. Zaman aralığını değiştirmek, yakınlaştırmak veya grafiğinizi uzaklaştırmak için **zaman seçici** panelini kullanın. 

![Zaman aralığı panelini değiştirin](./media/metrics-getting-started/time-picker.png)

> [!NOTE]
> Grafiğin ilginç bir alanını (sivri veya dip) araştırmak için **zaman fırçasını** kullanın. Fare işaretçisini alanın başına koyun, sol fare düğmesini tıklatın ve basılı tutun, alanın diğer tarafına sürükleyin ve ardından düğmeyi bırakın. Grafik bu zaman aralığını yakınlaştırır. 

## <a name="apply-dimension-filters-and-splitting"></a>Boyut filtrelerini ve bölmeyi uygulayın

[Filtreleme](metrics-charts.md#apply-filters-to-charts) ve [bölme](metrics-charts.md#apply-splitting-to-a-chart) boyutları olan ölçümler için güçlü tanı araçlarıdır. Bu özellikler, çeşitli metrik segmentlerin ("boyut değerleri") ölçümün genel değerini nasıl etkilediğini gösterir ve olası aykırıları belirlemenize olanak sağlar.

- **Filtreleme,** grafikte hangi boyut değerlerinin yer aldığınızı seçmenize olanak tanır. Örneğin, *sunucu yanıt süresi* ölçümünü çizerken başarılı istekleri göstermek isteyebilirsiniz. Filtreyi *istek boyutunun başarısına* uygulamanız gerekir. 

- **Grafiğin** bir boyutun her değeri için ayrı çizgiler gösterip görüntülemediğini veya değerleri tek bir satıra toplayıp ayırmadığını bölme. Örneğin, tüm sunucu örneklerinde ortalama yanıt süresi için bir satır görebilir veya her sunucu için ayrı satırlar görebilirsiniz. Ayrı satırları görmek için *sunucu örneği* boyutuna bölme uygulamanız gerekir.

Filtreleme ve bölme uygulanan [grafiklerin örneklerine](metric-chart-samples.md) bakın. Makale, adımların grafikleri yapılandırmak için kullanıldığını gösterir.

## <a name="advanced-chart-settings"></a>Gelişmiş grafik ayarları

Grafik stilini, başlığı özelleştirebilir ve gelişmiş grafik ayarlarını değiştirebilirsiniz. Özelleştirme ile yapıldığında, çalışmanızı kaydetmek için bir panoya sabitle. Ölçümler uyarılarını da yapılandırabilirsiniz. Azure Monitor ölçümleri gezgininin bu ve diğer gelişmiş özellikleri hakkında bilgi edinmek için [ürün belgelerini](metrics-charts.md) izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Metrics Explorer'ın gelişmiş özellikleri hakkında bilgi edinin](metrics-charts.md)
* [Ölçüm Gezgini sorunlarını giderme](metrics-troubleshoot.md)
* [Azure hizmetleri için kullanılabilir ölçümlerin listesine bakın](metrics-supported.md)
* [Yapılandırılmış grafik örneklerine bakın](metric-chart-samples.md)
