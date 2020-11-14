---
title: FHIR için Azure IoT Bağlayıcısı (önizleme) ölçümlerini görüntüleme ve yapılandırma
description: Bu makalede, FHıR (Önizleme) ölçümleri için Azure IoT bağlayıcısını görüntüleme ve yapılandırma açıklanmaktadır.
services: healthcare-apis
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: how-to
ms.date: 11/13/2020
ms.author: jasteppe
ms.openlocfilehash: 79a7b18986f4bed503cd5493a7d74325a13fe535
ms.sourcegitcommit: 9826fb9575dcc1d49f16dd8c7794c7b471bd3109
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2020
ms.locfileid: "94630524"
---
# <a name="display-and-configure-azure-iot-connector-for-fhir-preview-metrics"></a>FHIR için Azure IoT Bağlayıcısı (önizleme) ölçümlerini görüntüleme ve yapılandırma 

Bu makalede, hızlı sağlık birlikte çalışabilirlik kaynakları (FHıR&#174;) * ölçümleri için Azure IoT bağlayıcısını görüntülemeyi ve yapılandırmayı öğreneceksiniz.

> [!TIP]
> Ölçüm verilerinin dışarı aktarılmasını nasıl ayarlayacağınızı öğrenmek için [Tanılama ayarları aracılığıyla FHIR (Önizleme) Için Azure IoT bağlayıcısını dışarı aktarma](./iot-metrics-diagnostics-export.md)bölümündeki yönergeleri izleyin.

## <a name="display-metrics-for-azure-iot-connector-for-fhir-preview"></a>FHıR için Azure IoT Bağlayıcısı için ölçümleri görüntüleme (Önizleme)

1. Azure portal oturum açın ve ardından FHıR hizmeti için Azure API 'nizi seçin. 

2. Sol bölmede **ölçümler** ' i seçin. 

3. **IoT Bağlayıcısı** sekmesini seçin.

   :::image type="content" source="media/iot-metrics-display/iot-metrics-main.png" alt-text="Gelen ve normalleştirilmiş ileti sayısını görüntüleyen çizgi grafikleriyle ' IoT Bağlayıcısı ' bölmesinin ekran görüntüsü." lightbox="media/iot-metrics-display/iot-metrics-main.png"::: 

4. Ölçümlerini görüntülemek için bir IoT Bağlayıcısı seçin. Örneğin, FHıR hizmeti için bu Azure API 'siyle ilişkili dört IoT Bağlayıcısı ( *bağlayıcı 1* , *bağlayıcı 2* vb.) vardır.

   :::image type="content" source="media/iot-metrics-display/iot-metrics-select-connector.png" alt-text="IoT Bağlayıcısı sekmeleri 1, 2, 3 ve 4 görüntüleyen ' IoT Bağlayıcısı ' bölmesinin ekran görüntüsü." lightbox="media/iot-metrics-display/iot-metrics-select-connector.png"::: 

5. Göstermek istediğiniz IoT Bağlayıcısı ölçümlerinin zaman aralığını (örneğin, **1 saat** , **24 saat** , **7 gün** veya **özel** ) seçin. **Özel** sekmesini seçerek, IoT Bağlayıcısı ölçümlerini görüntülemek için belirli zaman/tarih kombinasyonları oluşturabilirsiniz.

   :::image type="content" source="media/iot-metrics-display/iot-metrics-select-time.png" alt-text="' Bağlayıcı 1 ' için bir ' 1 saat ' zaman aralığı çizgi grafiği görüntüleyen ' IoT Bağlayıcısı ' bölmesinin ekran görüntüsü." lightbox="media/iot-metrics-display/iot-metrics-select-time.png"::: 
 
## <a name="metric-types-for-azure-iot-connector-for-fhir-preview"></a>FHıR için Azure IoT Bağlayıcısı için ölçüm türleri (Önizleme) 

> [!TIP]
> FHıR için Azure IoT Bağlayıcısı 'nda veri akışı hakkında bilgi edinmek için, hata iletileri ve düzeltmeler hakkında daha fazla bilgi edinmek üzere [FHıR Için Azure IoT bağlayıcısını (Önizleme) veri akışını](./iot-data-flow.md) ve [Fhır Için Azure IoT Bağlayıcısı (Önizleme) sorun giderme kılavuzunu](./iot-troubleshoot-guide.md) görüntüleyin.

Görüntülenebilecek IoT Bağlayıcısı ölçümleri aşağıdaki tabloda listelenmiştir:

|Ölçüm türü|Ölçüm amacı| 
|-----------|--------------|
|Gelen Ileti sayısı|Alınan ham gelen ileti sayısını (örneğin, cihaz olayları) görüntüler.|
|Normalleştirilmiş Ileti sayısı|Normalleştirilmiş ileti sayısını görüntüler.|
|Ileti grubu sayısı|Belirlenen zaman penceresinde toplanmış iletileri olan grup sayısını görüntüler.|
|Ortalama normalleştirilmiş aşama gecikmesi|Normalleştirilmiş aşamanın ortalama gecikme süresini görüntüler. Normalleştirilmiş aşama, ham gelen iletilerde normalleştirmeyi gerçekleştirir.|
|Ortalama Grup aşaması gecikmesi|Grup aşamasının ortalama gecikme süresini görüntüler. Grup aşaması, normalleştirilmiş iletileri arabelleğe alma, toplama ve gruplama işlemlerini gerçekleştirir.| 
|Toplam hata sayısı|Toplam hata sayısını görüntüler.| 

## <a name="focus-on-and-configure-azure-iot-connector-for-fhir-preview-metrics"></a>FHıR (Önizleme) ölçümleri için Azure IoT bağlayıcısını odaklayın ve yapılandırın

Bu örnekte, **gelen ileti** ölçümünün sayısına odaklanalım.

1. Odaklanmak istediğiniz zaman bir nokta seçin.

   :::image type="content" source="media/iot-metrics-display/iot-metrics-focus.png" alt-text="' Gelen Ileti sayısı ' ölçüm bölmesinin, çizgi grafiğinde tek bir zaman noktasını vurgulayan ekran görüntüsü." lightbox="media/iot-metrics-display/iot-metrics-focus.png"::: 

2. **Gelen Ileti sayısı** bölmesinde ölçüm **Ekle** , **Filtre Ekle** veya **bölmeyi Uygula** ' yı seçerek ölçümü daha fazla özelleştirebilirsiniz. 

   :::image type="content" source="media/iot-metrics-display/iot-metrics-add-options.png" alt-text="' Ölçüm Ekle, ' ' Filtre Ekle, ' ve ' bölmeyi Uygula ' düğmelerini vurgulayan ' gelen Ileti sayısı ' ölçüm bölmesinin ekran görüntüsü." lightbox="media/iot-metrics-display/iot-metrics-add-options.png"::: 

## <a name="conclusion"></a>Sonuç 
Veri düzlemi ölçümlerine erişim sağlamak, izleme ve sorun giderme için gereklidir. FHıR için Azure IoT Bağlayıcısı, bu işlemleri ölçümler aracılığıyla size yardımcı olur. 

## <a name="next-steps"></a>Sonraki adımlar

FHıR için Azure IoT Bağlayıcısı hakkında sık sorulan soruların yanıtlarını alın.

>[!div class="nextstepaction"]
>[FHıR için Azure IoT Bağlayıcısı SSS](fhir-faq.md)

* Azure portal, FHıR için Azure IoT Bağlayıcısı, IoT Bağlayıcısı (Önizleme) olarak adlandırılır. FHıR, HL7 'in tescilli ticari markasıdır ve HL7 izniyle kullanılır. 
