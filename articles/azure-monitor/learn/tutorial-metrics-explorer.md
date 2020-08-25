---
title: Öğretici-Azure Izleyici 'de ölçüm grafiği oluşturma
description: Azure Ölçüm Gezgini ile ilk ölçüm grafiğinizi oluşturmayı öğrenin.
author: bwren
ms.author: bwren
ms.subservice: metrics
ms.topic: tutorial
ms.date: 03/09/2020
ms.openlocfilehash: d8f72471dd3109bf584d18f032ec2f4d949a4993
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "79082821"
---
# <a name="tutorial-create-a-metrics-chart-in-azure-monitor"></a>Öğretici: Azure Izleyici 'de ölçüm grafiği oluşturma
Ölçüm değerleri, ölçüm değerlerinden grafikler oluşturmanıza, eğilimleri görsel olarak ilişkilendirmenize ve ölçüm değerlerinde ani artışları ve DIP 'leri araştırmanıza olanak tanıyan Azure portal Azure Izleyici 'nin bir özelliğidir. Azure kaynaklarınızın sistem durumunu ve kullanımını araştırmak veya özel ölçülerden grafikler çizmek için ölçüm gezginini kullanın. 

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Grafik çizmek istediğiniz bir ölçüm seçin
> * Farklı ölçüm değerleri toplamaları gerçekleştirin
> * Grafik için zaman aralığını ve ayrıntı düzeyini değiştirme

Aşağıda, bu makalede özetlenen yordamdan daha kapsamlı bir senaryo gösteren bir video verilmiştir. Ölçümler hakkında yeni bir durumda olduğunuzda, önce bu makaleyi okuyup daha sonra daha fazla ayrıntı görmek için videoyu görüntülemenizi öneririz. 

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4qO59]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlayabilmeniz için izlemeniz gereken bir Azure kaynağınız olması gerekir. Azure aboneliğinizde ölçümleri destekleyen herhangi bir kaynağı kullanabilirsiniz. Bir kaynağın ölçümleri destekleyip desteklemediğini anlamak için Azure portal menüsüne gidin ve menünün **izleme** bölümünde bir **ölçümler** seçeneği olduğunu doğrulayın.


## <a name="log-in-to-azure"></a>Azure'da oturum açma
Üzerinde Azure portal oturum açın [https://portal.azure.com](https://portal.azure.com) .

## <a name="open-metrics-explorer-and-select-a-scope"></a>Ölçüm Gezgini 'ni açın ve bir kapsam seçin
Ölçüm Gezgini 'ni Azure Izleyici menüsünden ya da Azure portal bir kaynağın menüsünden açabilirsiniz. Kullandığınız seçenekten bağımsız olarak tüm kaynakların ölçümleri kullanılabilir. 

1. **Azure izleyici** menüsünde veya bir kaynağın menüsünün **izleme** bölümünden **ölçümler** ' i seçin.

1. Ölçümlerini görmek istediğiniz kaynak olan **kapsamı**seçin. Ölçüm Gezginini bir kaynağın menüsünden açtıysanız kapsam zaten doldurulmuştur.

    ![Kapsam seçin](media/tutorial-metrics-explorer/scope-picker.png)

2. Kapsamda birden fazla tane varsa bir **ad alanı** seçin. Ad alanı, ölçümleri kolayca bulabilmeniz için, ölçümleri düzenlemenin bir yoludur. Örneğin, depolama hesaplarının dosya, tablo, blob 'Lar ve sıra ölçümlerini depolamak için ayrı ad alanları vardır. Birçok kaynak türünün yalnızca bir ad alanı vardır.

3. Seçili kapsam ve ad alanı için kullanılabilir ölçümler listesinden bir ölçüm seçin.

    ![Ölçüm seçin](media/tutorial-metrics-explorer/metric-picker.png)

4. İsteğe bağlı olarak, ölçüm **toplamayı**değiştirin. Bu, ölçüm değerlerinin grafik için zaman parçalı olarak nasıl toplandığını tanımlar. Örneğin, zaman ayrıntı düzeyi 15 dakikaya ayarlanmışsa ve toplama Sum olarak ayarlandıysa, grafikteki her bir nokta, her 15 dakikalık segment üzerinde tüm toplanan değerlerin toplamı olur.

    ![Grafik](media/tutorial-metrics-explorer/chart.png)

5. Aynı grafikte birden çok ölçüm çizilmek istiyorsanız **ölçüm Ekle** düğmesini kullanın ve bu adımları tekrarlayın. Tek bir görünümdeki birden çok grafik için **yeni grafik** düğmesini seçin.

## <a name="select-a-time-range-and-granularity"></a>Zaman aralığı ve ayrıntı düzeyi seçin

Varsayılan olarak, grafik en son 24 saat ölçüm verilerini gösterir. Her bir veri noktasının zaman aralığını tanımlayan grafik veya zaman **ayrıntı düzeyi** için zaman **aralığını** değiştirmek üzere saat seçiciyi kullanın. Grafik, belirlenen zaman düzeyinde tüm örneklenmiş değerleri hesaplamak için belirtilen toplamayı kullanır.

![Zaman aralığı Bölmesini Değiştir](media/tutorial-metrics-explorer/time-picker.png)


Grafiğin ani veya DIP gibi ilginç bir alanını araştırmak için **zaman fırçası** 'nı kullanın. Fare işaretçisini alanın başlangıcına koyun, sol fare düğmesine tıklayın ve basılı tutun, alanın diğer tarafına sürükleyin ve düğmeyi bırakın. Grafik bu zaman aralığında yakınlaşacaktır. 

![Zaman fırçası](media/tutorial-metrics-explorer/time-brush.png)

## <a name="apply-dimension-filters-and-splitting"></a>Boyut filtrelerini uygulama ve bölme
Ölçümleriniz üzerinde ek analizler gerçekleştirmenize ve verilerinizde olası aykırı değerleri tanımlamanızı sağlayan gelişmiş özellikler için aşağıdaki referanslara bakın.

- [Filtreleme](../platform/metrics-charts.md#apply-filters-to-charts) , grafiğe hangi boyut değerlerinin ekleneceğini seçmenizi sağlar. Örneğin, bir *sunucu yanıt süresi* ölçümünü grafikleme sırasında yalnızca başarılı istekleri göstermek isteyebilirsiniz. 

- Grafiğe, grafiğin her bir boyutun değeri için ayrı satırlar görüntüleyip görüntülemediğini veya değerleri tek bir satıra toplamayacağını [denetler.](../platform/metrics-charts.md#apply-splitting-to-a-chart) Örneğin, tüm sunucu örneklerinde ortalama yanıt süresi için bir satır görmek isteyebilirsiniz veya her bir sunucu için ayrı satırlar istemeniz gerekebilir. 

Filtreleme ve bölme uygulanmış [grafiklerin örneklerine](../platform/metric-chart-samples.md) bakın.

## <a name="advanced-chart-settings"></a>Gelişmiş grafik ayarları

Grafik stilini, başlığı özelleştirebilir ve gelişmiş grafik ayarlarını değiştirebilirsiniz. Özelleştirme ile işiniz bittiğinde çalışmanızı kaydetmek için bir panoya sabitleyin. Ayrıca, ölçüm uyarılarını yapılandırabilirsiniz. Azure Izleyici Ölçüm Gezgini 'nin bu ve diğer gelişmiş özellikleri hakkında bilgi edinmek için bkz. [azure Ölçüm Gezgini gelişmiş özellikleri](../platform/metrics-charts.md#lock-boundaries-of-chart-y-axis) .


## <a name="next-steps"></a>Sonraki adımlar
Azure Izleyici 'de ölçümlerle nasıl çalışacağınızı öğrendiğinize göre, öngörülü uyarılar göndermek için ölçümleri nasıl kullanacağınızı öğrenin.

> [!div class="nextstepaction"]
> [Azure İzleyici'yi kullanarak ölçüm uyarıları oluşturma, görüntüleme ve yönetme](../platform/alerts-metric.md)

