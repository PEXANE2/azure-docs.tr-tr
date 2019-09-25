---
title: 'Hızlı Başlangıç: Azure Time Series Insights Preview demo ortamını keşfet | Microsoft Docs'
description: Azure Time Series Insights Preview demo ortamını anlamak için hızlı başlangıç.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: dpalled
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 09/23/2019
ms.openlocfilehash: 633eb00b479c6d2e2bf233b42aff7d393b110fd2
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71258425"
---
# <a name="quickstart-explore-the-azure-time-series-insights-preview-demo-environment"></a>Hızlı Başlangıç: Azure Time Series Insights Preview demo ortamını keşfet

Bu hızlı başlangıç, Azure Time Series Insights Preview ortamını kullanmaya başlamanızı ister. Ücretsiz gösteride, Time Series Insights önizlemeye eklenen önemli özellikler turuna katılın.

Time Series Insights Preview demo ortamı, iki rüzgar türbin grubu çalıştıran bir senaryo şirketi olan contoso içerir. Her grupta 10 türbines vardır. Her türbin, verileri her dakika Azure IoT Hub rapor eden 20 sensöre sahiptir. Algılayıcılar, hava durumu koşulları, dikey pencere sıklığı ve yaw konumu hakkında bilgi toplar. Üretici performansı, Gearbox davranışı ve güvenlik izleyicileri ile ilgili bilgiler de kaydedilir.

Bu hızlı başlangıçta, contoso verilerinde eyleme dönüştürülebilir Öngörüler bulmak için Time Series Insights kullanmayı öğreneceksiniz. Ayrıca kritik sorunları daha iyi tahmin etmek ve bakım gerçekleştirmek için kısa bir kök neden analizi de gerçekleştirirsiniz.

> [!IMPORTANT]
> Ücretsiz bir [Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

## <a name="explore-the-time-series-insights-explorer-in-a-demo-environment"></a>Tanıtım ortamında Time Series Insights Gezginini keşfet

Time Series Insights önizleme Gezgini geçmiş verileri ve kök nedenler analizini gösterir. Kullanmaya başlamak için:

1.  [Contoso rüzgar grubu tanıtım](https://insights.timeseries.azure.com/preview/samples) ortamına gidin.  

1. İstenirse, Azure hesabı kimlik bilgilerinizi kullanarak Time Series Insights Gezgini ' nde oturum açın.

## <a name="work-with-historical-data"></a>Geçmiş verilerle çalışma

1. **Contoso bitki 1**' de rüzgar türbin **W7**bölümüne bakın.  

   1. Görünüm aralığını **3/10/17 20:00 1/1/17 20:00 (UTC)** olarak değiştirin.
   1. Bir algılayıcı seçmek için **contoso bitki 1** > **W7** > **Generator sistem** > **generatorspeed**' i seçin. Ardından, gösterilen değerleri gözden geçirin.

      [![Contoso tesis 1 ' de W7](media/v2-update-quickstart/quickstart-one.png)](media/v2-update-quickstart/quickstart-one.png#lightbox)

1. Contoso, Rüzgar Turbin **W7**içinde bir yangın buldu. Opın ons, yangın ne olduğuna ilişkin farklılık gösterir. Time Series Insights, yangın uyarı algılayıcısının yangın sırasında etkinleştirildiğini görebiliriz.

   1. Görünüm aralığını **3/10/17 20:00 3/9/17 20:00 (UTC)** olarak değiştirin.
   1. **Güvenlik sistemi** > **firealert**öğesini seçin.

      [![Contoso, Rüzgar Turbin W7 'de bir yangın buldu](media/v2-update-quickstart/quickstart-two.png)](media/v2-update-quickstart/quickstart-two.png#lightbox)

1. Ne olduğunu anlamak için yangın süresi etrafında diğer olayları gözden geçirin. Yağ baskısı ve etkin uyarılar, yalnızca yangın 'tan önce.

   1. **Sıklık sistem** > **Hidrolik licoilbasıncı**' nı seçin.
   1. **Aralıklı sistem** > **activewarning**öğesini seçin.

      [![Aynı anda diğer olayları gözden geçirme](media/v2-update-quickstart/quickstart-three.png)](media/v2-update-quickstart/quickstart-three.png#lightbox)

1. Yağ basıncı ve etkin uyarı sensörleri, yangın 'tan önce. Daha önce harekete geçmiş olan diğer işaretleri görmek için görüntülenen zaman serisini genişletin. Her iki algılayıcı zaman içinde sürekli olarak dalgalanılır. Dalgalanmalar kalıcı ve worrisme deseninin olduğunu gösterir.

    * Görünüm aralığını **3/10/17 20:00 2/24/17 20:00 (UTC)** olarak değiştirin.

      [![Yağ basıncı ve etkin uyarı algılayıcıları Ayrıca spıked](media/v2-update-quickstart/quickstart-four.png)](media/v2-update-quickstart/quickstart-four.png#lightbox)

1. İki yıllık geçmiş verileri incelemek, aynı algılayıcı dalgalanmalarına sahip başka bir yangın olayını ortaya çıkarır.

    * Görünüm aralığını **1/1/16, 12/31/17** (tüm veriler) olarak değiştirin.

      [![Geçmiş desenleri ara](media/v2-update-quickstart/quickstart-five.png)](media/v2-update-quickstart/quickstart-five.png#lightbox)

Time Series Insights ve algılayıcı telemetrimizi kullanarak, geçmiş verilerde uzun süreli ve sorunlu bir eğilim ekledik. Bu yeni Öngörüler sayesinde şunları yapabilirsiniz:

* Gerçekten ne olduğunu açıklayın.
* Sorunu giderin.
* Üst uyarı bildirim sistemlerini yerine koyun.

## <a name="root-cause-analysis"></a>Kök neden analizi

1. Bazı senaryolarda verilerde hafif ipuçları ortaya çıkarmak için gelişmiş analizler gerekir. **6/25**tarihinde Windmill **W6** seçin.

    1. Görünüm aralığını **7/1/17 20:00 6/1/17 20:00 (UTC)** olarak değiştirin.
    1. **Contoso bitki 1** > **W6** > **güvenlik**sistemiVoltageActuatorSwitchWarning > ' nı seçin.

       [![Görünüm aralığını değiştirin ve W6 seçin](media/v2-update-quickstart/quickstart-six.png)](media/v2-update-quickstart/quickstart-six.png#lightbox)

1. Uyarı, Oluşturucu tarafından çıkış gerilimi ile ilgili bir sorun olduğunu gösterir. Oluşturucunun genel güç çıkışı, geçerli aralıktaki normal parametreler içinde çalışıyor. Zaman sıklığımızı artırarak başka bir model ortaya çıktı. Kesin bir bırakma kapalı olur.

    1. **VoltageActuatorSwitchWarning** algılayıcısı 'nı kaldırın.
    1. **Generator sistem** > **activepower**öğesini seçin.
    1. Aralığı **3B**olarak değiştirin.

       [![Aralığı 3B olarak değiştirme](media/v2-update-quickstart/quickstart-seven.png)](media/v2-update-quickstart/quickstart-seven.png#lightbox)

1. Zaman aralığını genişleterek, sorunun durdurulup durdurulmadığını veya devam edip etmediğini belirleyebiliriz.

    * Zaman aralığını 60 güne genişletin.

      [![Süreyi 60 güne uzat](media/v2-update-quickstart/quickstart-eight.png)](media/v2-update-quickstart/quickstart-eight.png#lightbox)

1. Diğer algılayıcı veri noktaları, daha fazla bağlam sağlamak için eklenebilir. Daha fazla algılayıcı, sorunun anlaşılmasından daha fazla bilgi sahibi. Şimdi gerçek değerleri görmek için bir işaret bırakın. 

    1. **Oluşturucu sistemi**' ni seçin ve ardından üç algılayıcı seçin: **GridVoltagePhase1**, **GridVoltagePhase2**ve **GridVoltagePhase3**.
    1. Görünür alandaki son veri noktasında bir işaretleyici bırakın.

       [![İşaretleyici bırak](media/v2-update-quickstart/quickstart-nine.png)](media/v2-update-quickstart/quickstart-nine.png#lightbox)

    Voltaj sensörlerinden ikisi, çalışan comparably ve normal parametreler içinde. **GridVoltagePhase3** sensöri olduğu anlaşılıyor.

1. Yüksek düzeyde bağlamsal veriler eklendiğinde, 3. aşama devre, sorun için daha da büyük görünür. Şimdi, uyarının nedenine yönelik iyi bir lider sunuyoruz. Sorunu, bakım ekibimize başvuracak şekilde hazırlıyoruz.  

    * Görüntüyü aynı grafik ölçeğinde tüm **Oluşturucu sistem** algılayıcılarının kaplaması için değiştirin.

      [![Görüntüyü her şeyi içerecek şekilde değiştirin](media/v2-update-quickstart/quickstart-ten.png)](media/v2-update-quickstart/quickstart-ten.png#lightbox)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiyi tamamladığınıza göre, oluşturduğunuz kaynakları temizleyin:

1. [Azure Portal](https://portal.azure.com)sol menüden **tüm kaynaklar**' ı seçin, Azure Time Series Insights kaynak grubunuzu bulun.
1. Her kaynağı ayrı ayrı **Sil** veya Kaldır ' a tıklayarak kaynak grubunun tamamını (ve içinde yer alan tüm kaynakları) silin.

## <a name="next-steps"></a>Sonraki adımlar

Kendi Time Series Insights önizleme ortamınızı oluşturmaya hazırsınız. Başlamak için:

> [!div class="nextstepaction"]
> [Time Series Insights Preview ortamınızı planlayın](time-series-insights-update-plan.md)

Tanıtım ve özellikleri arasında gezinmeyi öğrenin:

> [!div class="nextstepaction"]
> [Time Series Insights önizleme Gezgini](time-series-insights-update-explorer.md)
