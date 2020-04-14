---
title: 'Quickstart: Azure Time Series Insights explorer - Azure Time Series Öngörüleri | Microsoft Dokümanlar'
description: Azure Zaman Serisi Öngörüleri kaşifi ile nasıl başlayın. Büyük hacimli IoT verilerini görselleştirin ve ortamınızın temel özelliklerini gezin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 04/13/2020
ms.openlocfilehash: a566c55d2ac51efc5ff2650ad682f10dae2bdc31
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81272007"
---
# <a name="quickstart-explore-azure-time-series-insights"></a>Hızlı Başlangıç: Azure Time Series Insights’ı Keşfedin

Bu Azure Zaman Serisi Öngörüleri explorer quickstart ücretsiz bir gösteri ortamında Time Series Insights ile başlamak yardımcı olur. Bu hızlı başlangıçta, büyük hacimli IoT verilerini görselleştirmek için web tarayıcınızı nasıl kullanacağınızı ve genellikle şu anda kullanılabilen tur temel özelliklerini öğrenirsiniz.

Azure Time Series Öngörüleri, milyarlarca IoT olayını aynı anda keşfetmeyi ve analiz etmeyi kolaylaştıran, tam olarak yönetilen bir analiz, depolama ve görselleştirme hizmetidir. IoT çözümünüzü hızlı bir şekilde doğrulayabilmeniz ve görev açısından kritik aygıtlara pahalıya mal olan kapalı kalma sürelerini önleyebilmeniz için verilerinizin genel görünümünü sağlar. Azure Time Series Öngörüleri, gizli eğilimleri keşfetmenize, anormallikleri tespit etmenize ve kök neden analizini neredeyse gerçek zamanlı olarak gerçekleştirmenize yardımcı olur.

Ek esneklik için, güçlü [REST API'leri](./time-series-insights-update-tsq.md) ve [istemci SDK'sı](https://github.com/microsoft/tsiclient)aracılığıyla önceden varolan bir uygulamaya Azure Time Series Öngörüleri ekleyebilirsiniz. API'leri, zaman serisi verilerini istediğiniz bir istemci uygulamasında depolamak, sorgulamak ve kullanmak için kullanabilirsiniz. Ayrıca, varolan uygulamanıza Kullanıcı Arabirimi bileşenleri eklemek için istemci SDK'yı da kullanabilirsiniz.

Bu Zaman Serisi Insights explorer quickstart genel kullanılabilirlik özellikleri rehberli bir tur sunuyor.

> [!IMPORTANT]
> Daha önce bir hesap oluşturmadıysanız ücretsiz bir [Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

## <a name="prepare-the-demo-environment"></a>Demo ortamını hazırlayın

1. Tarayıcınızda, [genel kullanılabilirlik demosu](https://insights.timeseries.azure.com/demo)gidin.

1. İstenirse, Azure hesap kimlik bilgilerinizi kullanarak Time Series Insights explorer'da oturum açın.

1. Zaman Serisi Öngörüleri hızlı tur sayfası görüntülenir. Hızlı tura başlamak için **İleri'yi** seçin.

   [![Quickstart karşılama - Sonraki'ni seçin](media/quickstart/quickstart-welcome.png)](media/quickstart/quickstart-welcome.png#lightbox)

## <a name="explore-the-demo-environment"></a>Demo ortamını keşfedin

1. **Zaman seçim paneli** görüntülenir. Görselleştirilecek zaman aralığını seçmek için bu paneli kullanın.

   [![Zaman seçim paneli](media/quickstart/quickstart-time-selection-panel.png)](media/quickstart/quickstart-time-selection-panel.png#lightbox)

1. Bir zaman dilimi seçin ve bölgede sürükleyin. Ardından **Ara'yı**seçin.

   [![Zaman aralığı seçin](media/quickstart/quickstart-select-time.png)](media/quickstart/quickstart-select-time.png#lightbox)

   Time Series Insights, belirttiğiniz zaman aralığı için bir grafik görselleştirmesi görüntüler. Çizgi grafiği içinde çeşitli eylemler yapabilirsiniz. Örneğin, filtreleyebilir, sabitleyebilir, sıralayabilir ve yığınlayabilirsiniz.

   Zaman seçim **paneline**dönmek için aşağıdaki gibi aşağı ok'u seçin:

   [![Grafik](media/quickstart/quickstart-select-down-arrow.png)](media/quickstart/quickstart-select-down-arrow.png#lightbox)

1. Yeni bir arama terimi eklemek için **Şartlar paneline** **Ekle'yi** seçin.

   [![Arama terimleri paneli ekleme](media/quickstart/quickstart-add-terms.png)](media/quickstart/quickstart-add-terms.png#lightbox)

1. Grafikte bir bölgeyi seçip bölgeye sağ tıklayabilir ve **Olayları Keşfet**’i seçebilirsiniz.

   [![Olayları Keşfet](media/quickstart/quickstart-explore-events.png)](media/quickstart/quickstart-explore-events.png#lightbox)

   Ham verilerinizin ızgarası, keşfetmekte olduğunuz bölgeden görüntüler.

   [![Olayları keşfedin - ızgara veri görünümü](media/quickstart/quickstart-explore-events-grid-data.png)](media/quickstart/quickstart-explore-events-grid-data.png#lightbox)

## <a name="select-and-filter-data"></a>Verileri seçme ve filtreleme

1. Grafikteki değerleri değiştirmek için terimlerinizi değiştirin. Farklı değer türlerini ilişkilendirmek için başka bir terim ekleyin.

   [![Terim ekleme](media/quickstart/quickstart-add-a-term.png)](media/quickstart/quickstart-add-a-term.png#lightbox)

1. Seçili tüm arama terimlerini görüntülemek için **Filtre serisi** kutusunu boş bırakın veya doğaçlama seri filtreleme için **Filtre serisi** kutusuna bir filtre terimi girin.

   [![Seriyi filtrele](media/quickstart/quickstart-filter-series.png)](media/quickstart/quickstart-filter-series.png#lightbox)

   Bu hızlı başlangıçta, bir istasyona ait sıcaklığı ve basıncı karşılıklı olarak ilişkilendirmek için **Station5** terimini girin.

Hızlı başlangıcı bitirdikten sonra örnek veri kümesiyle deneme yaparak farklı görselleştirmeler oluşturabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiyi tamamladığınızda, oluşturduğunuz kaynakları temizleyin:

1. [Azure portalındaki](https://portal.azure.com)sol menüden **Tüm kaynakları**seçin, Azure Zaman Serisi Öngörüleri kaynak grubunuzu bulun.
1. **Sil'i** seçerek tüm kaynak grubunu (ve içindeki tüm kaynakları) silin veya her kaynağı tek tek kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

Kendi Time Series Öngörüleri ortamınızı oluşturmaya hazırsınız:
> [!div class="nextstepaction"]
> [Time Series Insights ortamınızı planlayın](time-series-insights-environment-planning.md)
