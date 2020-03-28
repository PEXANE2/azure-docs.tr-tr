---
title: Bir uyarıda kök neden analizi yürütme - Azure | Microsoft Docs
description: Bu öğreticide Azure Time Series Insights kullanarak bir uyarıda kök neden analizinin nasıl yürütüleceğini öğreneceksiniz.
author: Philmea
ms.author: philmea
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/20/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: c3cb940583677d813873c07fbfa679fdcc1dff59
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77565477"
---
# <a name="tutorial-conduct-a-root-cause-analysis-on-an-alert"></a>Öğretici: Bir uyarıda kök neden analizi yürütme

Bu öğreticide, bir uyarının kök nedenini tanılamak için Uzaktan İzleme çözümü hızlandırıcısının nasıl kullanılacağını öğreneceksiniz. Uzaktan İzleme çözümü panosunda bir uyarı tetiklendiğini görecek ve sonra kök nedeni araştırmak için Azure Time Series Insights gezginini kullanacaksınız.

Öğreticide konum, rakım, hız ve yük sıcaklığı telemetri verilerini gönderen iki sanal tır cihazı kullanılmaktadır. Tırlar, Contoso adlı bir kuruluş tarafından yönetilmektedir ve Uzaktan İzleme çözümü hızlandırıcısına bağlıdır. Contoso operatörü olarak, tırlarınızdan birinin (delivery-truck-02) neden düşük sıcaklık uyarısı kaydettiğini anlamanız gerekir.

Bu öğreticide şunları yaptınız:

>[!div class="checklist"]
> * Panodaki cihazları filtreleme
> * Gerçek zamanlı telemetri verilerini görüntüleme
> * Time Series Insights gezgininde verileri keşfetme
> * Kök neden analizi yürütme
> * Öğrendiklerinize dayanarak yeni bir kural oluşturma

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

## <a name="choose-the-devices-to-display"></a>Görüntülenecek cihazları seçme

**Dashboard** (Pano) sayfasında görüntülenecek bağlı cihazları seçmek için filtreleri kullanın. Yalnızca **Truck** (Tır) cihazlarını görüntülemek için filtre açılan menüsünden yerleşik **Trucks** (Tırlar) filtresini seçin:

[![Gösterge panelindeki kamyonlar için filtre](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-trucks-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-trucks-expanded.png#lightbox)

Bir filtre uyguladığınızda, yalnızca filtre koşullarına uyan aygıtlar haritada ve **Pano'daki**telemetri panelinde görüntülenir. Çözüm hızlandırıcısına bağlı iki kamyon olduğunu görebilirsiniz, **kamyon-02**de dahil olmak üzere.

## <a name="view-real-time-telemetry"></a>Gerçek zamanlı telemetri verilerini görüntüleme

Çözüm hızlandırıcısı, **Dashboard** (Pano) sayfasındaki grafiğe gerçek zamanlı telemetri verilerini çizer. Varsayılan olarak grafik, zaman içinde değişiklik gösteren rakım telemetri verilerini göstermektedir:

[![Kamyon irtifa telemetri arsa](./media/iot-accelerators-remote-monitoring-root-cause-analysis/trucks-moving-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/trucks-moving-expanded.png#lightbox)

Tırların sıcaklık telemetri verilerini görüntülemek için **Telemetri paneli**’nde **Sıcaklık** seçeneğine tıklayın. Son 15 dakika boyunca her iki tırın da sıcaklığının nasıl değişiklik gösterdiğini görebilirsiniz. Uyarılar bölmesinde delivery-truck-02 için düşük sıcaklık uyarısının tetiklendiğini de görebilirsiniz.

[![Düşük sıcaklık uyarısı ile RM panosu](./media/iot-accelerators-remote-monitoring-root-cause-analysis/low-temp-alert-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/low-temp-alert-expanded.png#lightbox)

## <a name="explore-the-data"></a>Verileri keşfetme

Düşük sıcaklık alarmının nedenini belirlemek için, Time Series Insights gezgininde tır telemetri verilerini açın. Panodaki **Time Series Insights’ta Keşfet** bağlantılarından herhangi birine tıklayın:

[![TSI bağlantıları vurgulanan RM panosu](./media/iot-accelerators-remote-monitoring-root-cause-analysis/explore-tsi-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/explore-tsi-expanded.png#lightbox)

Gezgin başlatıldığında tüm cihazların listelendiğini görürsünüz:

[![TSI Explorer ilk görünümü](./media/iot-accelerators-remote-monitoring-root-cause-analysis/initial-tsi-view-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/initial-tsi-view-expanded.png#lightbox)

Filtre kutusuna **teslim kamyonu** yazarak aygıtları filtreleyin ve sol panelde **Ölçü** olarak **sıcaklığı** seçin:

[![TSI Explorer kamyon sıcaklığı](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-tsi-temp-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-tsi-temp-expanded.png#lightbox)

Uzaktan İzleme panosunda gördüğünüz görünümün aynısını görürsünüz. Ayrıca, artık uyarının içinde tetiklediği zaman çerçevesine daha yakın yakın laştırabilirsiniz:

[![TSI Explorer yakınlaştırma](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-zoom-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-zoom-expanded.png#lightbox)

Tırlardan gelen diğer telemetri akışlarına da ekleme yapabilirsiniz. Sol üst köşedeki **Ekle** düğmesini tıklatın. Yeni bir bölme görüntülenir:

[![Yeni bölmeli TSI Explorer](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-pane-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-pane-expanded.png#lightbox)

Yeni bölmede, yeni etiketin adını, öncekiyle eşleşecek şekilde **Cihazlar** olarak değiştirin. Görünümünüze yükseklik telemetrisini eklemek için **Ölçü** **Split By** ve **iothub-connection-device-id** değerini böl olarak **yükseklik** seçin:

[![Sıcaklık ve yükseklik ile TSI Explorer](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-altitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-altitude-expanded.png#lightbox)

## <a name="diagnose-the-alert"></a>Uyarıyı tanılama

Geçerli görünümdeki akışlara baktığınızda, iki kamyonun yükseklik profillerinin farklı olduğunu görebilirsiniz. Ayrıca tır yüksek bir rakıma ulaştığında **delivery-truck-02** tırında sıcaklık düşüşü gerçekleşir. Tırlar aynı rotayı izleyecek şekilde zamanlanmış olduğundan bu bulgu sizi şaşırtabilir.

Turların farklı yolculuk yolları izlediğine dair şüphenizi onaylamak için, **Ekle** düğmesini kullanarak yan panele başka bir bölme ekleyin. Yeni bölmede, yeni etiketin adını, öncekiyle eşleşecek şekilde **Cihazlar** olarak değiştirin. Boylam telemetri verilerini görünümünüze eklemek için **Ölçü** olarak **boylam** ve **Bölme ölçütü:** değeri olarak **iothub-connection-device-id** seçeneğini belirleyin. **Boylam** akışları arasındaki farka bakarak tırların farklı yolculuklar yaptığını görebilirsiniz:

[![Sıcaklık, yükseklik ve boylam ile TSI Explorer](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-longitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-longitude-expanded.png#lightbox)

## <a name="create-a-new-rule"></a>Yeni kural oluşturma

Kamyon güzergahları genellikle önceden optimize edilmiş olsa da, trafik düzeninin, hava durumunun ve diğer öngörülemeyen olayların gecikmelere neden olabileceğini ve kamyon sürücülerine en iyi kararlarına göre son dakika rota kararlarını bırakabileceğini fark esiniz. Ancak, araç içindeki varlıklarınızın sıcaklığı kritik olduğundan, Uzaktan İzleme çözümünüzde ek bir kural oluşturmalısınız. Bu kural, 1 dakikalık bir aralıktaki ortalama irtifa 350 feet'in üzerine çıkarsa bir uyarı aldığınızdan emin olmaktır:

[![Uzaktan İzleme kuralları sekmesi yükseklik kuralını belirler](./media/iot-accelerators-remote-monitoring-root-cause-analysis/new-rule-altitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/new-rule-altitude-expanded.png#lightbox)

Kurallar oluşturma ve kuralları düzenleme hakkında bilgi edinmek için, [cihaz sorunlarını algılama](iot-accelerators-remote-monitoring-automate.md) bölümünde yer alan önceki öğreticiye göz atın.

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir uyarının kök nedenini tanılamak için Uzaktan İzleme çözümü hızlandırıcısı ile Time Series Insights gezgininin nasıl kullanılacağı gösterildi. Çözüm hızlandırıcısını bağlı cihazlarınızdaki sorunları tanımlama ve düzeltme amacıyla kullanmayı öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [İzleme çözümünüze bağlı cihazlarla sorunları tanımlamak ve düzeltmek için cihaz uyarılarını kullanma](iot-accelerators-remote-monitoring-maintain.md)
