---
title: 'Hızlı başlangıç: Önizleme demo ortamını keşfedin - Azure Time Series Insights | Microsoft Dokümanlar'
description: Azure Zaman Serisi Öngörüleri Önizleme demo ortamının temel özelliklerini keşfedin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 02/07/2020
ms.openlocfilehash: dc4a8da69b0398c6487008c106a9f5bcdb8a885e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77110244"
---
# <a name="quickstart-explore-the-azure-time-series-insights-preview-demo-environment"></a>Hızlı başlangıç: Azure Zaman Serisi Öngörüleri Önizleme demo ortamını keşfedin

Bu hızlı başlangıç, Azure Zaman Serisi Öngörüleri Önizleme ortamıyla başlamanızı sağlar. Ücretsiz demoda, Time Series Insights Preview'a eklenen temel özellikleri gezersiniz.

Zaman Serisi Öngörüler Önizleme demo ortamı, iki rüzgar türbini çiftliği işleten contoso adlı bir senaryo şirketi içerir. Her çiftlikte 10 türbin vardır. Her türbinin, azure IoT Hub'a her dakika veri bildiren 20 sensörü vardır. Sensörler hava koşulları, bıçak perdesi ve yaw konumu hakkında bilgi toplar. Jeneratör performansı, şanzıman davranışı ve güvenlik monitörleri hakkında da bilgi kaydedilir.

Bu hızlı başlangıçta, Contoso verilerinde işlem yapılabilir öngörüler bulmak için Time Series Insights'ı nasıl kullanacağınızı öğrenirsiniz. Ayrıca, kritik hataları daha iyi tahmin etmek ve bakım yapmak için kısa bir kök neden çözümlemesi yaparsınız.

> [!IMPORTANT]
> Hesabınız yoksa ücretsiz bir [Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

## <a name="explore-the-time-series-insights-explorer-in-a-demo-environment"></a>Zaman Serisi Öngörüler kaşifini bir demo ortamında keşfedin

Zaman Serisi Öngörüleri Önizleme gezgini geçmiş verileri ve kök neden çözümlemesi gösterir. Başlamak için:

1.  [Contoso Rüzgar Çiftliği demo](https://insights.timeseries.azure.com/preview/samples) ortamına gidin.  

1. Sizden istenirse, Azure hesap kimlik bilgilerinizi kullanarak Time Series Insights explorer'da oturum açın.

## <a name="work-with-historical-data"></a>Geçmiş verilerle çalışma

1. **Contoso Bitki 1,** rüzgar türbini **W7**bakmak .  

   1. Görünüm aralığını **1/1/17 20:00:00.00 ile 3/10/17 20:00:00.00 (UTC) olarak değiştirin.**
   1. Bir sensör seçmek için, **Contoso Bitki 1** > **W7** > **Jeneratör Sistemi** > **GeneratorSpeed**seçin. Ardından, gösterilen değerleri gözden geçirin.

      [![Contoso Bitki 1 W7](media/v2-update-quickstart/quick-start-generator-speed.png)](media/v2-update-quickstart/quick-start-generator-speed.png#lightbox)

1. Son zamanlarda, Contoso rüzgar türbini **W7**bir yangın bulundu. Yangına neyin yol açtığı konusunda görüşler farklılık gösteriyor. Time Series Insights'ta, yangın sırasında etkinleştirilen yangın uyarı sensörü görüntülenir.

   1. Görünüm aralığını **3/9/17 20:00:00.00 ile 3/10/17 20:00:00.00 (UTC) olarak değiştirin.**
   1. **Güvenlik Sistemi** > **FireAlert**seçin.

      [![Contoso rüzgar türbini W7 bir yangın bulundu](media/v2-update-quickstart/quick-start-fire-alert.png)](media/v2-update-quickstart/quick-start-fire-alert.png#lightbox)

1. Ne olduğunu anlamak için yangın saatinde diğer olayları gözden geçirin. Petrol basıncı ve aktif uyarılar yangından hemen önce fırladı.

   1. **Pitch System** > **HidrolikOilPressure'ı**seçin.
   1. **Pitch System** > **ActiveWarning'i**seçin.

      [![Diğer olayları aynı anda gözden geçirme](media/v2-update-quickstart/quick-start-active-warning.png)](media/v2-update-quickstart/quick-start-active-warning.png#lightbox)

1. Yağ basıncı ve aktif uyarı sensörleri yangından hemen önce fırladı. Görüntülenen zaman serisini, yangına yol açan diğer işaretleri incelemek için genişletin. Her iki sensör de zaman içinde sürekli olarak dalgalandı. Dalgalanmalar kalıcı ve endişe verici bir patern gösteriyor.

    * Görünüm aralığını **2/24/17 20:00:00.00 ile 3/10/17 20:00:00.00 (UTC) olarak değiştirin.**

      [![Yağ basıncı ve aktif uyarı sensörleri de fırladı](media/v2-update-quickstart/quick-start-view-range.png)](media/v2-update-quickstart/quick-start-view-range.png#lightbox)

1. İki yıllık tarihi veriler incelenirken, aynı sensör dalgalanmalarına sahip başka bir yangın olayı ortaya çıkıyor.

    * Görünüm aralığını **1/1/16 ile 12/31/17** (tüm veriler) olarak değiştirin.

      [![Tarihsel desenleri arayın](media/v2-update-quickstart/quick-start-expand-view-range.png)](media/v2-update-quickstart/quick-start-expand-view-range.png#lightbox)

Time Series Insights ve sensör telemetrisini kullanarak, tarihsel verilerde gizli uzun vadeli bir eğilim keşfettik. Bu yeni öngörülerle şunları yapabiliriz:

* Gerçekte ne olduğunu açıkla.
* Sorunu düzeltin.
* Daha iyi uyarı bildirim sistemlerini yerleştirin.

## <a name="root-cause-analysis"></a>Kök neden analizi

1. Bazı senaryolar, verilerdeki ipuçlarını ortaya çıkarmak için gelişmiş analizler gerektirir. **6/25**tarihindeki yel değirmeni **W6'yı** seçin.

    1. Görünüm aralığını **6/1/17 20:00:00.00 ile 7/1/17 20:00:00.00 (UTC) olarak değiştirin.**
    1. **Contoso Tesisi 1** > **W6** > **Emniyet Sistemi** > **VoltajActuatorSwitchWarning**seçin.

       [![Görüş aralığını değiştirin ve W6'yı seçin](media/v2-update-quickstart/quick-start-voltage-switch-warning.png)](media/v2-update-quickstart/quick-start-voltage-switch-warning.png#lightbox)

1. Uyarı, jeneratörden gelen voltajla ilgili bir sorunu gösterir. Jeneratörün genel güç çıkışı geçerli aralıkta normal parametreler dahilindedir. Aralığımızı artırarak, başka bir model ortaya çıkar. Bir teslimat bellidir.

    1. **VoltageAkOratörSwitchWarning** sensörünü çıkarın.
    1. **Jeneratör Sistemi** > **ActivePower**seçin.
    1. Aralığı 3d olarak **değiştirin.**

       [![Aralığı 3d olarak değiştirin](media/v2-update-quickstart/quick-start-interval-change.png)](media/v2-update-quickstart/quick-start-interval-change.png#lightbox)

1. Zaman aralığını genişleterek, sorunun durdurulup durdurulmadığını veya devam edip etmediğini belirleyebiliriz.

    * Süreyi 60 güne uzatın.

      [![Süreyi 60 güne kadar uzatın](media/v2-update-quickstart/quick-start-expand-interval-range.png)](media/v2-update-quickstart/quick-start-expand-interval-range.png#lightbox)

1. Diğer sensör veri noktaları daha fazla bağlam sağlamak için eklenebilir. Ne kadar çok sensör görüntülediğimiz, sorunu anlamamız o kadar dolgun. Gerçek değerleri görüntülemek için bir işaretçi bırakalım. 

    1. **Jeneratör Sistemi**seçin ve sonra üç sensör seçin: **GridVoltagePhase1**, **GridVoltagePhase2**, ve **GridVoltagePhase3**.
    1. Görünür alandaki son veri noktasına bir işaretçi bırakın.

       [![İşaretçi bırak](media/v2-update-quickstart/quick-start-drop-marker.png)](media/v2-update-quickstart/quick-start-drop-marker.png#lightbox)

    Voltaj sensörlerinden ikisi karşılaştırılabilir ve normal parametreler dahilinde çalışıyor. **GridVoltagePhase3** sensörü suçlu gibi görünüyor.

1. Son derece bağlamsal veriler eklendi, faz 3 bırakma daha da sorun olarak görünür. Şimdi, uyarının nedeni hakkında iyi bir ipucumuz var. Konuyu bakım ekibimize havale etmeye hazırız.  

    * Ekranı, aynı grafik ölçeğindeki tüm **Jeneratör Sistemi** sensörlerini yerle bir etmek için değiştirin.

      [![Ekranı her şeyi içerecek şekilde değiştirme](media/v2-update-quickstart/quick-start-generator-system.png)](media/v2-update-quickstart/quick-start-generator-system.png#lightbox)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlatmayı tamamladığınızda, oluşturduğunuz kaynakları temizleyin:

1. [Azure portalındaki](https://portal.azure.com)sol menüden **Tüm kaynakları**seçin ve Azure Zaman Serisi Öngörüleri kaynak grubunu bulun.
1. **Sil'i** seçerek tüm kaynak grubunu (ve içindeki tüm kaynakları) silin veya her kaynağı tek tek kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

Kendi Zaman Serisi Öngörüleri Önizleme ortamınızı oluşturmaya hazırsınız. Başlamak için:

> [!div class="nextstepaction"]
> [Zaman Serisi Öngörüleri Önizleme ortamınızı planlayın](time-series-insights-update-plan.md)

Demoyu ve özelliklerini kullanmayı öğrenin:

> [!div class="nextstepaction"]
> [Zaman Serisi Öngörüler Önizleme kaşifi](time-series-insights-update-explorer.md)