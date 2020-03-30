---
title: Veri frm Azure IoT Hub gerçek zamanlı veri görselleştirme - Güç BI
description: Sensörden toplanan ve Azure IoT hub'ınıza gönderilen sıcaklık ve nem verilerini görselleştirmek için Power BI'yi kullanın.
author: robinsh
keywords: gerçek zamanlı veri görselleştirme, canlı veri görselleştirme, sensör veri görselleştirme
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 6/06/2019
ms.author: robinsh
ms.openlocfilehash: f0b909d10790511408e090546fd3359889ea5aca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73954638"
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a>Power BI kullanarak Azure IoT Hub'daki gerçek zamanlı sensör verilerini görselleştirin

![Uçuça diyagram](./media/iot-hub-live-data-visualization-in-power-bi/1_end-to-end-diagram.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Öğrenecekleriniz

Power BI kullanarak Azure IoT hub'ınızın aldığı gerçek zamanlı sensör verilerini nasıl görselleştireceğimizi öğrenirsiniz. IoT hub'ınızdaki verileri bir web uygulamasıyla görselleştirmek istiyorsanız, [Azure IoT Hub'daki gerçek zamanlı sensör verilerini görselleştirmek için bir web uygulaması kullanın'a](iot-hub-live-data-visualization-in-web-apps.md)bakın.

## <a name="what-you-do"></a>Ne yaparsınız

* Bir tüketici grubu ekleyerek IoT hub'ınızı veri erişimine hazırlayın.

* IoT merkezinizden Power BI hesabınıza veri aktarımı için bir Akış Analizi işi oluşturun, yapılandırın ve çalıştırın.

* Verileri görselleştirmek için bir Power BI raporu oluşturun ve yayımlayın.

## <a name="what-you-need"></a>Ne gerekiyor

* [Raspberry Pi online simülatör](iot-hub-raspberry-pi-web-simulator-get-started.md) öğretici veya cihaz öğreticiler birini tamamlayın; örneğin, [ahududu Pi düğüm ile.js](iot-hub-raspberry-pi-kit-node-get-started.md). Bu makaleler aşağıdaki gereksinimleri kapsamaktadır:
  
  * Etkin bir Azure aboneliği.
  * Aboneliğiniz altında bir Azure IoT hub'ı.
  * Azure IoT hub'ınıza ileti gönderen bir istemci uygulaması.

* Power BI hesabı. (Power[BI'yi ücretsiz deneyin](https://powerbi.microsoft.com/))

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Akış Analizi işi oluşturma, yapılandırma ve çalıştırma

Bir Stream Analytics işi oluşturarak başlayalım. İşi oluşturduktan sonra, verileri almak için kullanılan girişleri, çıktıları ve sorguyu tanımlarsınız.

### <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

1. Azure [portalında,](https://portal.azure.com) **Create a resource** > **Bir** > kaynak Nesnelerin İnterneti**Oluşturma Akış Analizi işini**seçin.

2. İş için aşağıdaki bilgileri girin.

   **İş adı**: İşin adı. Adın genel olarak benzersiz olması gerekir.

   **Kaynak grubu**: IoT hub'ınızın kullandığı kaynak grubunu kullanın.

   **Konum**: Kaynak grubunuzla aynı konumu kullanın.

   ![Azure'da Akış Analizi işi oluşturun](./media/iot-hub-live-data-visualization-in-power-bi/create-stream-analytics-job-azure.png)

3. **Oluştur'u**seçin.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Stream Analytics işine giriş ekleme

1. Akış Analizi işini açın.

2. **İş Topolojisi** **altında, Girişleri**seçin.

3. **Girişler** bölmesinde, **akış girişi ekle'yi**seçin ve açılan listeden **IoT Hub'ı** seçin. Yeni giriş bölmesine aşağıdaki bilgileri girin:

   **Giriş takma adı**: Giriş için benzersiz bir takma ad girin.

   **Aboneliğinizden IoT Hub sağlayın**: Bu radyo düğmesini seçin.

   **Abonelik**: Bu öğretici için kullandığınız Azure aboneliğini seçin.

   **IoT Hub**: Bu eğitim için kullandığınız IoT Hub'ını seçin.

   **Uç nokta**: **Mesajlaşma**'yı seçin.

   **Paylaşılan erişim ilkesi adı**: IoT hub'ınız için Akış Analizi işinin kullanmasını istediğiniz paylaşılan erişim ilkesinin adını seçin. Bu öğretici *için, hizmet*seçebilirsiniz. *Hizmet* ilkesi varsayılan olarak yeni IoT hub'larında oluşturulur ve IoT hub'ı tarafından açığa çıkarılan bulut tarafındaki uç noktalara gönderme ve alma izni verir. Daha fazla bilgi edinmek için [Erişim denetimi ve izinleri'ne](iot-hub-devguide-security.md#access-control-and-permissions)bakın.

   **Paylaşılan erişim ilkesi anahtarı**: Bu alan, paylaşılan erişim ilkesi adı için seçiminize göre otomatik olarak doldurulur.

   **Tüketici grubu**: Daha önce oluşturduğunuz tüketici grubunu seçin.

   Diğer tüm alanları varsayılanlarına bırakın.

   ![Azure'daki Akış Analizi işine giriş ekleme](./media/iot-hub-live-data-visualization-in-power-bi/add-input-to-stream-analytics-job-azure.png)

4. **Kaydet'i**seçin.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Stream Analytics işine çıkış ekleme

1. **İş Topolojisi**altında **Çıktıları**seçin.

2. **Çıktılar** bölmesinde **Ekle** ve **Güç BI'yi**seçin.

3. Power **BI - Yeni çıktı** bölmesinde, **Yetkilendirme'yi** seçin ve Power BI hesabınızda oturum açma istemlerini izleyin.

4. Power BI'de oturum imzaladıktan sonra aşağıdaki bilgileri girin:

   **Çıktı diğer adı**: Çıktı için benzersiz bir takma ad.

   **Grup Çalışma Alanı**: Hedef grup çalışma alanınızı seçin.

   **Dataset Adı**: Bir dataset adı girin.

   **Tablo Adı**: Tablo adı girin.

   ![Azure'daki Akış Analizi işine çıktı ekleme](./media/iot-hub-live-data-visualization-in-power-bi/add-output-to-stream-analytics-job-azure.png)

5. **Kaydet'i**seçin.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Stream Analytics işinin sorgusunu yapılandırma

1. **İş Topolojisi**'nin altında **Sorgu**'yu seçin.

2. `[YourInputAlias]` değerini işin giriş diğer adıyla değiştirin.

3. `[YourOutputAlias]` değerini işin çıkış diğer adıyla değiştirin.

   ![Azure'daki Akış Analizi işine sorgu ekleme](./media/iot-hub-live-data-visualization-in-power-bi/add-query-stream-analytics-job-azure.png)

4. **Kaydet'i**seçin.

### <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştırma

Akış Analizi işinde Genel **Bakış'ı**seçin ve ardından > **Şimdi** >  **Start****Başlat'ı**seçin. İş düzgün bir şekilde başlatıldıktan sonra, **Durduruldu** olan iş durumu **Çalışıyor** olarak değiştirilir.

![Azure'da Bir Akış Analizi işi çalıştırın](./media/iot-hub-live-data-visualization-in-power-bi/run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-to-visualize-the-data"></a>Verileri görselleştirmek için bir Power BI raporu oluşturma ve yayımlama

1. Örnek uygulamanın cihazınızda çalıştırdığından emin olun. Değilse, [cihazınızı Kur'a'nın](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started)altındaki öğreticilere başvurabilirsiniz.

2. [Power BI](https://powerbi.microsoft.com/en-us/) hesabınızda oturum açın.

3. Kullandığınız çalışma alanını seçin, **Çalışma Alanım.**

4. **Veri Kümeleri'ni**seçin.

   Akış Analizi işi için çıktıyı oluşturduğunuzda belirttiğiniz veri kümesini görmeniz gerekir.

5. Oluşturduğunuz veri kümesi için **Rapor Ekle'yi** (veri kümesi adının sağındaki ilk simge) seçin.

   ![Microsoft Power BI raporu oluşturma](./media/iot-hub-live-data-visualization-in-power-bi/start-power-bi.png)

6. Zamanla değişen gerçek zamanlı sıcaklığı göstermek için bir çizgi grafik oluşturun.

   1. Rapor oluşturma sayfasının **Görseller** bölmesine, satır grafiği eklemek için satır grafiği simgesini seçin.

   2. **Alanlar** bölmesinde, Stream Analytics işi için çıkış oluştururken belirttiğiniz tabloyu genişletin.

   3. **Görselleştirmeler** bölmesinde **EventEnqueuedUtcTime** öğesini **Eksen** üzerine sürükleyin.

   4. **temperature** öğesini **Değerler** üzerine sürükleyin.

      Bir çizgi grafik oluşturulur. Grafiğin x ekseninde UTC saat dilimine göre tarih ve saat görüntülenir. Grafiğin y ekseninde algılayıcıdan alınan sıcaklık görüntülenir.

      ![Microsoft Power BI raporuna sıcaklık için bir satır grafiği ekleme](./media/iot-hub-live-data-visualization-in-power-bi/power-bi-add-temp.png)

7. Zamanla değişen gerçek zamanlı nem oranını göstermek için bir çizgi grafik daha oluşturun. Bunu yapmak için yukarıdaki adımları izleyin ve **EventEnqueuedUtcTime'ı** x eksenine ve **nem** oranıy eksenine yerleştirin.

   ![Microsoft Power BI raporuna nem için bir çizgi grafiği ekleme](./media/iot-hub-live-data-visualization-in-power-bi/power-bi-add-humidity.png)

8. Raporu kaydetmek için **Kaydet'i** seçin.

9. Sol bölmedeki **Raporlar'ı** seçin ve ardından yeni oluşturduğunuz raporu seçin.

10. **Web'de Dosya** > **Yayımla'yı**seçin.

11. **Embed kodu oluştur'u**seçin ve ardından **Yayımla'yı**seçin.

Rapor erişimi için herkesle paylaşabileceğiniz rapor bağlantısı ve raporu blogunuza veya web sitenize entegre etmek için kullanabileceğiniz bir kod bağlantısı sağlanır.

![Microsoft Power BI raporu yayımlama](./media/iot-hub-live-data-visualization-in-power-bi/power-bi-publish.png)

Microsoft ayrıca, mobil cihazınızdaki Power BI panolarınızı ve raporlarınızı görüntülemek ve bunlarla etkileşimde olmak için [Power BI mobil uygulamalarını](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) da sunar.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT hub'ınızdaki gerçek zamanlı sensör verilerini görselleştirmek için Power BI'yi başarıyla kullandınız.

Azure IoT Hub'daki verileri görselleştirmenin başka bir yolu için, [Azure IoT Hub'daki gerçek zamanlı sensör verilerini görselleştirmek için bir web uygulaması kullanın'a](iot-hub-live-data-visualization-in-web-apps.md)bakın.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
