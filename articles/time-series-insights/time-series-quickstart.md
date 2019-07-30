---
title: 'Hızlı Başlangıç: Gezgin Azure Time Series Insights | Microsoft Docs'
description: Bu hızlı başlangıçta, çok sayıda IoT verilerini görselleştirmek için Web tarayıcınızda Azure Time Series Insights Gezginini kullanmaya nasıl başlacağınız gösterilmektedir. Bir tanıtım ortamında temel özelliklere göz atın.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 07/29/2019
ms.openlocfilehash: 94402953a7168b6518d0a50f81f75ee63da8c902
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638867"
---
# <a name="quickstart-explore-azure-time-series-insights"></a>Hızlı Başlangıç: Azure Time Series Insights’ı keşfedin

Bu Azure Time Series Insights gezgin hızlı başlangıcı, ücretsiz bir tanıtım ortamında Time Series Insights kullanmaya başlamanıza yardımcı olur. Bu özellik sayesinde, Web tarayıcınızı kullanarak büyük hacimlerinizde çok sayıda IoT verisi ve genel kullanıma sunulan tura yönelik önemli özellikleri görselleştirme hakkında bilgi edineceksiniz.

Azure Time Series Insights, milyarlarca IoT olayını aynı anda keşfedip analiz etmeyi kolaylaştıran tam olarak yönetilen bir analiz, depolama ve görselleştirme hizmetidir. IoT çözümünüzü hızla doğrulayabilmeniz ve görev açısından kritik cihazlara maliyetli kapalı kalma süresi oluşmasını önlemek için verilerinizin genel bir görünümünü sunar. Azure Time Series Insights, gizli eğilimleri keşfetmenize, anormallikleri belirlemenize ve neredeyse gerçek zamanlı olarak kök neden analizleri yürütmenize yardımcı olur.

Daha fazla esneklik için, güçlü [REST API 'leri](./time-series-insights-update-tsq.md) ve [istemci SDK 'sı](./tutorial-create-tsi-sample-spa.md)aracılığıyla önceden var olan bir uygulamaya Azure Time Series Insights ekleyebilirsiniz. API 'Leri, zaman serisi verilerini depolamak, sorgulamak ve tercih ettiğiniz bir istemci uygulamasında kullanmak için kullanabilirsiniz. Ayrıca, mevcut uygulamanıza Kullanıcı arabirimi bileşenleri eklemek için istemci SDK 'sını de kullanabilirsiniz.

Bu Time Series Insights gezgin hızlı başlangıcı, artık genel olarak kullanılabilen özelliklerin kılavuzlu bir gezintisini sunmaktadır.

## <a name="prepare-the-demo-environment"></a>Tanıtım ortamını hazırlama

1. Henüz bir tane oluşturmadıysanız, [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

1. Tarayıcınızda [genel kullanılabilirlik tanıtımı](https://insights.timeseries.azure.com/demo)' na gidin.

1. İstenirse, Azure hesabı kimlik bilgilerinizi kullanarak Time Series Insights Gezgininde oturum açın.

1. Time Series Insights hızlı tur sayfası görüntülenir. Hızlı tura başlamak için **İleri ' yi** seçin.

   [![Ileri Seç](media/quickstart/quickstart1.png)](media/quickstart/quickstart1.png#lightbox)

## <a name="explore-the-demo-environment"></a>Tanıtım ortamını keşfet

1. **Zaman seçim paneli** görüntülenir. Görselleştirilecek zaman aralığını seçmek için bu paneli kullanın.

   [![Zaman seçimi bölmesi](media/quickstart/quickstart2.png)](media/quickstart/quickstart2.png#lightbox)

1. Bir zaman dilimi seçin ve bölgeye sürükleyin. Ardından **Ara**' yı seçin.

   [![Zaman dilimi seçin](media/quickstart/quickstart3.png)](media/quickstart/quickstart3.png#lightbox)

   Time Series Insights, belirttiğiniz zaman aralığı için bir grafik görselleştirmesi görüntüler. Çizgi grafik içinde çeşitli eylemler gerçekleştirebilirsiniz. Örneğin, filtreleyebilir, sabitleyebilir, sıralayabilir ve yığın oluşturabilirsiniz.

   **Zaman seçimi paneline**dönmek için gösterilen aşağı oku seçin:

   [![Grafik](media/quickstart/quickstart4.png)](media/quickstart/quickstart4.png#lightbox)

1. Yeni bir arama terimi eklemek için **terimler panelinde** **Ekle** ' yi seçin.

   [![Öğe Ekle](media/quickstart/quickstart5.png)](media/quickstart/quickstart5.png#lightbox)

1. Grafikte bir bölgeyi seçip bölgeye sağ tıklayabilir ve **Olayları Keşfet**’i seçebilirsiniz.

   [![Olayları keşfet](media/quickstart/quickstart6.png)](media/quickstart/quickstart6.png#lightbox)

   Ham verilerinizin bir Kılavuzu, araştırırken bulunan bölgeden görüntülenir.

   [![Izgara görünümü](media/quickstart/quickstart7.png)](media/quickstart/quickstart7.png#lightbox)

## <a name="select-and-filter-data"></a>Verileri seçme ve filtreleme

1. Grafikteki değerleri değiştirmek için koşullarınızı düzenleyin. Farklı değer türlerini çapraz ilişkilendirmek için başka bir terim ekleyin.

   [![Terim ekleyin](media/quickstart/quickstart8.png)](media/quickstart/quickstart8.png#lightbox)

1. İmprovised serisi filtreleme için **filtre serisi** kutusuna bir filtre terimi girin. Bu hızlı başlangıçta, bir istasyona ait sıcaklığı ve basıncı karşılıklı olarak ilişkilendirmek için **Station5** terimini girin.

   [![Diziyi filtrele](media/quickstart/quickstart9.png)](media/quickstart/quickstart9.png#lightbox)

Hızlı başlangıcı bitirdikten sonra örnek veri kümesiyle deneme yaparak farklı görselleştirmeler oluşturabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiyi tamamladığınıza göre, oluşturduğunuz kaynakları temizleyin:

1. [Azure Portal](https://portal.azure.com)sol menüden **tüm kaynaklar**' ı seçin, Azure Time Series Insights kaynak grubunuzu bulun.
1. Her kaynağı ayrı ayrı **Sil** veya Kaldır ' a tıklayarak kaynak grubunun tamamını (ve içinde yer alan tüm kaynakları) silin.

## <a name="next-steps"></a>Sonraki adımlar

Kendi Time Series Insights ortamınızı oluşturmaya hazırsınız:
> [!div class="nextstepaction"]
> [Time Series Insights ortamınızı planlayın](time-series-insights-environment-planning.md)
