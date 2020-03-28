---
title: Öğretici - Azure Monitör'de bir metrik grafiği oluşturma
description: Azure ölçümleri gezgini ile ilk metrik grafiğinizi nasıl oluşturacağınızı öğrenin.
author: bwren
ms.author: bwren
ms.subservice: metrics
ms.topic: tutorial
ms.date: 03/09/2020
ms.openlocfilehash: d8f72471dd3109bf584d18f032ec2f4d949a4993
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79082821"
---
# <a name="tutorial-create-a-metrics-chart-in-azure-monitor"></a>Öğretici: Azure Monitor'da bir metrik grafiği oluşturma
Metrics explorer, Azure portalındaki Azure Monitor'un metrik değerlerden grafikler oluşturmanıza, eğilimleri görsel olarak ilişkilendirmenize ve metrik değerlerdeki ani artışları ve düşüşleri araştırmanıza olanak tanıyan bir özelliğidir. Azure kaynaklarınızın sistem durumunu ve kullanımını araştırmak veya özel ölçümlerden grafikler çizmek için metrikler gezginini kullanın. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Grafik çizmek istediğiniz bir metrik seçin
> * Metrik değerlerin farklı toplamalarını gerçekleştirin
> * Grafik için zaman aralığını ve parçalılığı değiştirme

Aşağıda, bu makalede özetlenen yordamdan daha kapsamlı bir senaryo gösteren bir video yer almaktadır. Ölçümlerde yeniyseniz, önce bu makaleyi okumanızı ve daha sonra daha fazla ayrıntı yı görmek için videoyu görüntülemenizi öneririz. 

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4qO59]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için izlemek için bir Azure kaynağına ihtiyacınız var. Azure aboneliğinizde ölçümleri destekleyen herhangi bir kaynağı kullanabilirsiniz. Bir kaynağın ölçümleri destekleyip desteklemediğini belirlemek için Azure portalındaki menüsüne gidin ve menünün **İzleme** bölümünde Bir **Ölçümler** seçeneği olduğunu doğrulayın.


## <a name="log-in-to-azure"></a>Azure'da oturum açma
Azure portalına giriş [https://portal.azure.com](https://portal.azure.com)yapın.

## <a name="open-metrics-explorer-and-select-a-scope"></a>Ölçümler gezginini açın ve bir kapsam seçin
Azure Monitörü menüsünden veya Azure portalındaki bir kaynağın menüsünden metrikler gezginini açabilirsiniz. Kullandığınız seçenekten bağımsız olarak tüm kaynaklardan ölçümler kullanılabilir. 

1. **Azure Monitörü** menüsünden veya kaynak menüsünün **İzleme** bölümünden **Ölçümler'i** seçin.

1. Ölçümleri görmek istediğiniz kaynak olan **Kapsam'ı**seçin. Kaynak menüsünden ölçüler gezginini açtıysanız kapsam zaten doldurulur.

    ![Kapsam seçin](media/tutorial-metrics-explorer/scope-picker.png)

2. Kapsambirden fazlaysa Bir **Ad Alanı** seçin. Ad alanı, ölçümleri kolayca bulabilmeniz için bunları düzenlemenin bir yoludur. Örneğin, depolama hesaplarının Dosyalar, Tablolar, Blobs ve Kuyruklar ölçümleri depolamak için ayrı ad alanları vardır. Birçok kaynak türü yalnızca bir ad alanı vardır.

3. Seçili kapsam ve ad alanı için kullanılabilir ölçümler listesinden bir metrik seçin.

    ![Bir metrik seçin](media/tutorial-metrics-explorer/metric-picker.png)

4. İsteğe bağlı olarak, metrik **Toplama değiştirin.** Bu, grafik için zaman tanecikleri arasında metrik değerlerin nasıl toplanacağını tanımlar. Örneğin, zaman parçalı lık 15 dakika olarak ayarlanırsa ve toplama toplamı ayarlanırsa, grafikteki her nokta, her 15 dakikalık kesimde toplanan tüm değerlerin toplamı olur.

    ![Grafik](media/tutorial-metrics-explorer/chart.png)

5. Metrik **Ekle** düğmesini kullanın ve aynı grafikte çizilmiş birden çok ölçüm görmek istiyorsanız bu adımları yineleyin. Tek görünümdeki birden çok grafik için **Yeni grafik** düğmesini seçin.

## <a name="select-a-time-range-and-granularity"></a>Bir zaman aralığı ve parçalılık seçin

Varsayılan olarak, grafik en son 24 saatlik ölçüm verilerini gösterir. Grafiğin **Zaman aralığını** veya her veri noktasının zaman aralığını tanımlayan **Zaman parçalı lığını** değiştirmek için zaman seçiciyi kullanın. Grafik, belirtilen süre içinde tüm örneklenmiş değerleri hesaplamak için belirtilen toplama kullanır.

![Zaman aralığı panelini değiştirin](media/tutorial-metrics-explorer/time-picker.png)


Grafiğin başak veya daldırma gibi ilginç bir alanını araştırmak için **zaman fırçasını** kullanın. Fare işaretçisini alanın başına koyun, sol fare düğmesini tıklatın ve basılı tutun, alanın diğer tarafına sürükleyin ve düğmeyi bırakın. Grafik bu zaman aralığını yakınlaştırır. 

![Zaman fırçası](media/tutorial-metrics-explorer/time-brush.png)

## <a name="apply-dimension-filters-and-splitting"></a>Boyut filtrelerini ve bölmeyi uygulayın
Ölçümlerinizde ek analizler gerçekleştirmenize ve verilerinizdeki olası aykırıları belirlemenize olanak tanıyan gelişmiş özellikler için aşağıdaki referanslara bakın.

- [Filtreleme,](../platform/metrics-charts.md#apply-filters-to-charts) grafikte hangi boyut değerlerinin yer aldığınızı seçmenize olanak tanır. Örneğin, yalnızca *sunucu yanıt süresi* ölçümü çizerken başarılı istekleri göstermek isteyebilirsiniz. 

- [Grafiğin](../platform/metrics-charts.md#apply-splitting-to-a-chart) bir boyutun her değeri için ayrı çizgiler gösterip görüntülemediğini veya değerleri tek bir satıra toplayıp ayırmadığını bölme. Örneğin, tüm sunucu örneklerinde ortalama yanıt süresi için bir satır görmek isteyebilirsiniz veya her sunucu için ayrı satırlar isteyebilirsiniz. 

Filtreleme ve bölme uygulanan [grafiklerin örneklerine](../platform/metric-chart-samples.md) bakın.

## <a name="advanced-chart-settings"></a>Gelişmiş grafik ayarları

Grafik stilini, başlığı özelleştirebilir ve gelişmiş grafik ayarlarını değiştirebilirsiniz. Özelleştirme ile yapıldığında, çalışmanızı kaydetmek için bir panoya sabitle. Ölçümler uyarılarını da yapılandırabilirsiniz. Azure Monitor ölçümleri gezgininin bu ve diğer gelişmiş özellikleri hakkında bilgi edinmek için [Azure Ölçümleri](../platform/metrics-charts.md#lock-boundaries-of-chart-y-axis) Gezgini'nin Gelişmiş özelliklerine bakın.


## <a name="next-steps"></a>Sonraki adımlar
Azure Monitor'da ölçümlerle nasıl çalışacağınızı öğrendiğiniz için, proaktif uyarılar göndermek için ölçümleri nasıl kullanacağınızı öğrenin.

> [!div class="nextstepaction"]
> [Azure İzleyici'yi kullanarak ölçüm uyarıları oluşturma, görüntüleme ve yönetme](../platform/alerts-metric.md)

