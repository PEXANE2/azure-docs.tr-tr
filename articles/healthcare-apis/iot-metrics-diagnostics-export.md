---
title: Tanılama ayarları aracılığıyla FHIR (Önizleme) ölçümleri için Azure IoT bağlayıcısını dışarı aktarma
description: Bu makalede, Tanılama ayarları aracılığıyla FHıR (Önizleme) ölçümleri için Azure IoT Bağlayıcısı 'nın nasıl dışarı aktarılacağı açıklanmaktadır
services: healthcare-apis
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: how-to
ms.date: 10/16/2020
ms.author: jasteppe
ms.openlocfilehash: c81dcdd2e79f5d89a0766415b47ad118874e5ad2
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92210417"
---
# <a name="export-azure-iot-connector-for-fhir-preview-metrics-through-diagnostic-settings"></a>Tanılama ayarları aracılığıyla FHIR (Önizleme) ölçümleri için Azure IoT bağlayıcısını dışarı aktarma

Bu makalede, FHıR * ölçüm günlükleri için Azure IoT bağlayıcısını nasıl dışarı aktaracağınızı öğreneceksiniz. Ölçüm günlüğünü sağlayan özellik Azure portal [**Tanılama ayarlarınızdaki**](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings) bir özelliktir. 

> [!TIP]
> Denetim günlüğünü ayarlamak için [fhır Için Azure API ve FHıR Için Azure IoT Bağlayıcısı ' nda tanılama günlüğünü etkinleştirme](enable-diagnostic-logging.md#enable-diagnostic-logging-in-azure-api-for-fhir) ' deki yönergeleri izleyin.

## <a name="enable-metric-logging-for-the-azure-iot-connector-for-fhir-preview"></a>FHıR için Azure IoT Bağlayıcısı için ölçüm günlüğünü etkinleştirme (Önizleme)
1. FHıR için Azure IoT Bağlayıcısı için ölçüm günlüğünü etkinleştirmek üzere Azure portal için Azure API 'nizi seçin 

2. **Tanılama ayarlarına** gidin 

3. **+ Tanılama ayarı Ekle** ' yi seçin

   :::image type="content" source="media/iot-metrics-export/diagnostic-settings-main.png" alt-text="IoT Connector1" lightbox="media/iot-metrics-export/diagnostic-settings-main.png"::: 

4. **Tanılama ayarı adı** iletişim kutusuna bir ad girin.

5. Tanılama günlüklerinizi erişmek için kullanmak istediğiniz yöntemi seçin:

    1. Denetim veya el ile inceleme için **bir depolama hesabına arşivleme** . Kullanmak istediğiniz depolama hesabının zaten oluşturulmuş olması gerekir.
    2. Üçüncü taraf bir hizmet veya özel analitik çözüm tarafından giriş için **Olay Hub 'ına akış** . Bu adımı yapılandırmadan önce bir olay hub 'ı ad alanı ve Olay Hub 'ı ilkesi oluşturmanız gerekir.
    3. Azure Izleyici 'de Log Analytics çalışma alanına **akış** . Bu seçeneği seçmeden önce günlüklerinizi analiz çalışma alanınızı oluşturmanız gerekecektir.

6. FHıR için Azure IoT Bağlayıcısı için **hata, trafik ve gecikme süresini** ve Fhır için Azure API 'si için yakalamak istediğiniz ek ölçüm kategorilerini seçin.

7. **Kaydet**’i seçin

   :::image type="content" source="media/iot-metrics-export/diagnostic-setting-add.png" alt-text="IoT Connector1" lightbox="media/iot-metrics-export/diagnostic-setting-add.png":::

> [!Note] 
> İlk ölçüm günlüklerinin tercih ettiğiniz depoda görüntülenmesi 15 dakika kadar sürebilir.  
 
Tanılama günlükleri ile çalışma hakkında daha fazla bilgi için bkz. [Azure kaynak günlüğü belgeleri](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-overview)

## <a name="conclusion"></a>Sonuç 
Ölçüm günlüklerine erişim sağlamak, izleme ve sorun giderme için gereklidir.  FHıR için Azure IoT Bağlayıcısı, bu eylemleri ölçüm günlükleri aracılığıyla yapmanıza olanak sağlar. 

## <a name="next-steps"></a>Sonraki adımlar

FHıR için Azure IoT Bağlayıcısı hakkında sık sorulan sorulara göz atın.

>[!div class="nextstepaction"]
>[FHıR SSS için Azure IoT Bağlayıcısı](fhir-faq.md#azure-iot-connector-for-fhir-preview)

* Azure portal, FHıR için Azure IoT Bağlayıcısı, IoT Bağlayıcısı (Önizleme) olarak adlandırılır.

FHIR, HL7’nin kayıtlı ticari markasıdır ve HL7’nin izniyle kullanılır.
