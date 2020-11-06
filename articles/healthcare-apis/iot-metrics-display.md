---
title: FHIR için Azure IoT Bağlayıcısı (önizleme) Ölçümlerini görüntüleme ve yapılandırma
description: Bu makalede, FHıR (Önizleme) ölçümleri için Azure IoT bağlayıcısını görüntüleme ve yapılandırma açıklanmaktadır
services: healthcare-apis
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: how-to
ms.date: 10/29/2020
ms.author: jasteppe
ms.openlocfilehash: 1cdae789b8286be408735fff92e2de46e28ce514
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93394297"
---
# <a name="view-and-configure-azure-iot-connector-for-fhir-preview-metrics"></a>FHIR için Azure IoT Bağlayıcısı (önizleme) Ölçümlerini görüntüleme ve yapılandırma 

Bu makalede, FHıR * ölçümleri için Azure IoT bağlayıcısını görüntüleme ve yapılandırma hakkında bilgi edineceksiniz. 

> [!TIP]
> Ölçüm verilerinin dışarı aktarılmasını ayarlamayı öğrenmek için [Tanılama ayarları aracılığıyla Azure IoT bağlayıcısını dışarı aktarma (Önizleme) ölçümlerinde](./iot-metrics-diagnostics-export.md) bulunan yönergeleri izleyin.

## <a name="view-metrics-for-azure-iot-connector-for-fhir-preview"></a>FHıR için Azure IoT Bağlayıcısı ölçümlerini görüntüleme (Önizleme)
1. IoT bağlayıcılarının ölçümlerini görüntülemek için Azure portal Azure API 'nizi seçin. 

2. **Ölçümlere** git 

3. **IoT Bağlayıcısı** sekmesini seçin.

   :::image type="content" source="media/iot-metrics-display/iot-metrics-main.png" alt-text="IoT Connector1" lightbox="media/iot-metrics-display/iot-metrics-main.png"::: 

4. Ölçümlerini görüntülemek için bir IoT Bağlayıcısı seçin (örneğin: FHıR hizmeti için bu Azure API 'siyle ilişkili olan (4) IoT bağlayıcıları vardır).

   :::image type="content" source="media/iot-metrics-display/iot-metrics-select-connector.png" alt-text="IoT Connector2" lightbox="media/iot-metrics-display/iot-metrics-select-connector.png"::: 

> [!NOTE]
> **Özel** sekme, IoT Bağlayıcısı ölçümlerini görüntülemek için belirli zaman/tarih birleşimleri oluşturulmasına olanak sağlar.

5. Görüntülenecek IoT Bağlayıcısı ölçümlerinin zaman dilimini seçin (örneğin, 1 saat, 24 saat, 7 gün veya özel).

   :::image type="content" source="media/iot-metrics-display/iot-metrics-select-time.png" alt-text="IoT Connector3" lightbox="media/iot-metrics-display/iot-metrics-select-time.png"::: 
 
## <a name="metrics-types-for-azure-iot-connector-for-fhir-preview"></a>FHıR için Azure IoT Bağlayıcısı için ölçüm türleri (Önizleme) 
Görüntülenen IoT Bağlayıcısı ölçümleri aşağıdaki gibidir:

|Ölçüm türü|Ölçüm amacı| 
|-----------|--------------|
|Gelen Ileti sayısı|Alınan ham gelen ileti sayısı (örneğin: cihaz olayları).|
|Normalleştirilmiş Ileti sayısı|Normalleştirilmiş iletilerin sayısı.|
|Ileti grubu sayısı|Belirlenen zaman penceresinde iletileri toplanan grupların sayısı.|
|Ortalama normalleştirilmiş aşama gecikmesi|Normalleştirme aşamasının ortalama gecikme süresi. Normalleştir, ham gelen iletilerde normalleştirmeyi gerçekleştirmaktır.|
|Ortalama Grup aşaması gecikmesi|Grup aşamasının ortalama gecikme süresi. Grup aşaması, normalleştirilmiş iletilerde arabelleğe alma, toplama ve gruplandırma gerçekleştirmaktır.| 
|Toplam hata sayısı|Toplam hata sayısı.| 

## <a name="focusing-and-configuring-azure-iot-connector-for-fhir-preview-metrics"></a>FHıR (Önizleme) ölçümleri için Azure IoT bağlayıcısını odaklama ve yapılandırma
Bu örnekte, **gelen ileti** ölçümlerinin sayısına odaklanacağız.

1. Odaklanmak istediğiniz zaman bir nokta seçin.

   :::image type="content" source="media/iot-metrics-display/iot-metrics-focus.png" alt-text="IoT Connector4" lightbox="media/iot-metrics-display/iot-metrics-focus.png"::: 

2. Bu ekrandan, daha fazla özelleştirme için **ölçüm ekleyebilir** , **filtre ekleyebilir** ve **bölme uygulayabilirsiniz** . 

   :::image type="content" source="media/iot-metrics-display/iot-metrics-add-options.png" alt-text="IoT Connector5" lightbox="media/iot-metrics-display/iot-metrics-add-options.png"::: 

## <a name="conclusion"></a>Sonuç 
Veri düzlemi ölçümlerine erişim sağlamak, izleme ve sorun giderme için gereklidir.  FHıR için Azure IoT Bağlayıcısı, bu işlemleri ölçümler aracılığıyla yapma konusunda yardımcı olur. 

## <a name="next-steps"></a>Sonraki adımlar

FHıR için Azure IoT Bağlayıcısı hakkında sık sorulan sorulara göz atın.

>[!div class="nextstepaction"]
>[FHıR SSS için Azure IoT Bağlayıcısı](fhir-faq.md)

* Azure portal, FHıR için Azure IoT Bağlayıcısı, IoT Bağlayıcısı (Önizleme) olarak adlandırılır.

FHIR, HL7’nin kayıtlı ticari markasıdır ve HL7’nin izniyle kullanılır.