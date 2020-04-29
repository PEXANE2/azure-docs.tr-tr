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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
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

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

## <a name="choose-the-devices-to-display"></a>Görüntülenecek cihazları seçme

**Dashboard** (Pano) sayfasında görüntülenecek bağlı cihazları seçmek için filtreleri kullanın. Yalnızca **Truck** (Tır) cihazlarını görüntülemek için filtre açılan menüsünden yerleşik **Trucks** (Tırlar) filtresini seçin:

[![Panodaki structuralks için filtre uygulama](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-trucks-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-trucks-expanded.png#lightbox)

Filtre uyguladığınızda, yalnızca filtre koşullarıyla eşleşen cihazlar haritada ve **panoda**telemetri panelinde görüntülenir. **Kamyon-02**dahil olmak üzere çözüm hızlandırıcısına bağlı iki structuralks olduğunu görebilirsiniz.

## <a name="view-real-time-telemetry"></a>Gerçek zamanlı telemetri verilerini görüntüleme

Çözüm hızlandırıcısı, **Dashboard** (Pano) sayfasındaki grafiğe gerçek zamanlı telemetri verilerini çizer. Varsayılan olarak grafik, zaman içinde değişiklik gösteren rakım telemetri verilerini göstermektedir:

[![Kamyon yüksekliği telemetri çizimi](./media/iot-accelerators-remote-monitoring-root-cause-analysis/trucks-moving-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/trucks-moving-expanded.png#lightbox)

Tırların sıcaklık telemetri verilerini görüntülemek için **Telemetri paneli**’nde **Sıcaklık** seçeneğine tıklayın. Son 15 dakika boyunca her iki tırın da sıcaklığının nasıl değişiklik gösterdiğini görebilirsiniz. Uyarılar bölmesinde delivery-truck-02 için düşük sıcaklık uyarısının tetiklendiğini de görebilirsiniz.

[![Düşük geçici uyarı içeren RM panosu](./media/iot-accelerators-remote-monitoring-root-cause-analysis/low-temp-alert-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/low-temp-alert-expanded.png#lightbox)

## <a name="explore-the-data"></a>Verileri keşfetme

Düşük sıcaklık alarmının nedenini belirlemek için, Time Series Insights gezgininde tır telemetri verilerini açın. Panodaki **Time Series Insights’ta Keşfet** bağlantılarından herhangi birine tıklayın:

[![TSI bağlantıları vurgulanmış olan RM panosu](./media/iot-accelerators-remote-monitoring-root-cause-analysis/explore-tsi-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/explore-tsi-expanded.png#lightbox)

Gezgin başlatıldığında tüm cihazların listelendiğini görürsünüz:

[![TSI Explorer ilk görünümü](./media/iot-accelerators-remote-monitoring-root-cause-analysis/initial-tsi-view-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/initial-tsi-view-expanded.png#lightbox)

Filtre kutusuna **teslim kamyonu** yazarak cihazları filtreleyin ve sol bölmedeki **Ölçü** olarak **sıcaklık** ' yi seçin:

[![TSI gezgin kamyonu sıcaklığı](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-tsi-temp-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-tsi-temp-expanded.png#lightbox)

Uzaktan Izleme panosunda gördüğünüz görünümü görürsünüz. Ayrıca, artık uyarının tetiklendiği zaman dilimine yaklaştırılmış olabilirsiniz:

[![TSI gezgin önizlemesi](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-zoom-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-zoom-expanded.png#lightbox)

Tırlardan gelen diğer telemetri akışlarına da ekleme yapabilirsiniz. Sol üst köşedeki **Ekle** düğmesine tıklayın. Yeni bir bölme görüntülenir:

[![Yeni bölme ile TSI Gezgini](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-pane-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-pane-expanded.png#lightbox)

Yeni bölmede, yeni etiketin adını, öncekiyle eşleşecek şekilde **Cihazlar** olarak değiştirin. Bir değere göre yükseklik telemetrisi eklemek için **Ölçü** ve **ıothub-bağlantı-cihaz kimliği** olarak **Böl** **değerini seçin:**

[![Sıcaklık ve yükseklik özellikli TSI Gezgini](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-altitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-altitude-expanded.png#lightbox)

## <a name="diagnose-the-alert"></a>Uyarıyı tanılama

Geçerli görünümdeki akışlara baktığınızda, iki structuralks için olan yükseklik profillerinin farklı olduğunu görebilirsiniz. Ayrıca tır yüksek bir rakıma ulaştığında **delivery-truck-02** tırında sıcaklık düşüşü gerçekleşir. Tırlar aynı rotayı izleyecek şekilde zamanlanmış olduğundan bu bulgu sizi şaşırtabilir.

Turların farklı yolculuk yolları izlediğine dair şüphenizi onaylamak için, **Ekle** düğmesini kullanarak yan panele başka bir bölme ekleyin. Yeni bölmede, yeni etiketin adını, öncekiyle eşleşecek şekilde **Cihazlar** olarak değiştirin. Boylam telemetri verilerini görünümünüze eklemek için **Ölçü** olarak **boylam** ve **Bölme ölçütü:** değeri olarak **iothub-connection-device-id** seçeneğini belirleyin. **Boylam** akışları arasındaki farka bakarak tırların farklı yolculuklar yaptığını görebilirsiniz:

[![Sıcaklık, yükseklik ve Boylam ile TSI Gezgini](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-longitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-longitude-expanded.png#lightbox)

## <a name="create-a-new-rule"></a>Yeni kural oluşturma

Kamyonun yolları tipik olarak iyileştirilirken, trafik desenlerinin, hava durumunun ve diğer öngörülemeyen olayların gecikmelerden oluşmasına ve en iyi kararlarına göre son dakika rota kararlarını kamyon sürücülerine bırakabileceğini fark etmiş olursunuz. Ancak, varlıkların içindeki varlıklarınızın sıcaklığı kritik olduğundan, uzaktan Izleme çözümünüzde ek bir kural oluşturmanız gerekir. Bu kural, 1 dakikalık bir Aralık üzerindeki ortalama yükseklik 350 fit 'in üzerine gittiğinde bir uyarı almanızı sağlamaktır:

[![Uzaktan Izleme kuralları sekmesi yükseklik kuralı ayarla](./media/iot-accelerators-remote-monitoring-root-cause-analysis/new-rule-altitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/new-rule-altitude-expanded.png#lightbox)

Kurallar oluşturma ve kuralları düzenleme hakkında bilgi edinmek için, [cihaz sorunlarını algılama](iot-accelerators-remote-monitoring-automate.md) bölümünde yer alan önceki öğreticiye göz atın.

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir uyarının kök nedenini tanılamak için Uzaktan İzleme çözümü hızlandırıcısı ile Time Series Insights gezgininin nasıl kullanılacağı gösterildi. Çözüm hızlandırıcısını bağlı cihazlarınızdaki sorunları tanımlama ve düzeltme amacıyla kullanmayı öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [İzleme çözümünüze bağlı cihazlarla sorunları tanımlamak ve düzeltmek için cihaz uyarılarını kullanma](iot-accelerators-remote-monitoring-maintain.md)
