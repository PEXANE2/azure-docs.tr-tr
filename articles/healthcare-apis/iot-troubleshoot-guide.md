---
title: FHıR için Azure IoT Bağlayıcısı (Önizleme)-sorun giderme kılavuzu ve nasıl yapılır
description: FHıR (Önizleme) hata iletileri ve koşulları için genel Azure IoT Bağlayıcısı sorunlarını giderme ve eşleme dosyalarını kopyalama
services: healthcare-apis
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: conceptual
ms.date: 07/30/2020
ms.author: jasteppe
ms.openlocfilehash: 8c372a865e34b2cbd1b5b3e6d8619c3ef0f438e0
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87460431"
---
# <a name="azure-iot-connector-for-fhir-preview-troubleshooting-guide"></a>FHıR için Azure IoT Bağlayıcısı (Önizleme) sorun giderme kılavuzu

Bu makalede, FHıR hata iletileri ve koşulları için genel Azure IoT Bağlayıcısı sorunlarını gidermeye yönelik adımlar sağlanmaktadır.  

Ayrıca, Azure IoT bağlayıcısının, Azure portal dışında düzenlenmek ve arşivlemek üzere veya bir destek bileti açarken Azure teknik desteği sağlamak için Azure IoT Bağlayıcısı 'nın kopyalarını oluşturmayı öğreneceksiniz. 

## <a name="error-messages-and-fixes"></a>Hata iletileri ve düzeltmeler

|İleti   |Koşul  |Düzeltme         |
|----------|-----------|------------|
|Geçersiz eşleme adı, eşleme adı cihaz veya FHıR olmalıdır|Sağlanan eşleme türü cihaz veya FHıR değil|Desteklenen iki eşleme türünden birini kullanın (örneğin: cihaz veya FHıR)|
|Anahtar parametrelerini yeniden oluştur tanımlı değil|Anahtar isteğini yeniden üret|Yeniden oluşturma anahtar isteğine parametreleri ekleyin|
|Bu abonelikte sağlanabilen IoT Bağlayıcısı * örneklerinin maksimum sayısına ulaştı|FHıR abonelik kotasına yönelik Azure IoT Connector 'a ulaşıldı (abonelik başına varsayılan değer (2))|FHıR için Azure IoT bağlayıcısının mevcut örneklerinden birini silin, abonelik kotasına ulaşmayan veya abonelik kotası artışı talep eden farklı bir abonelik kullanın|
|IoT Bağlayıcısı için kaynak taşıma Azure API 'SI, FHıR kaynağı için desteklenmiyor|FHıR için Azure IoT bağlayıcısının bir veya daha fazla örneği olan FHıR kaynağı için bir Azure API 'SI üzerinde taşıma işlemi yapılmaya çalışılıyor|FHıR için Azure IoT bağlayıcısının mevcut örneklerini silme ve taşıma işlemini tamamlamak için|
|IoT Bağlayıcısı sağlanmadı|Üst öğe (FHıR için Azure IoT Bağlayıcısı) sağlanmadığından alt hizmetler (bağlantılar & eşlemeler) kullanılmaya çalışılıyor|FHıR için Azure IoT Bağlayıcısı sağlama|
|İstek desteklenmiyor|Belirli API isteği desteklenmiyor|Doğru API isteğini kullan|
|Hesap yok|FHAR için Azure IoT Bağlayıcısı ekleme girişimi ve FHıR kaynağı için Azure API 'SI yok|FHıR kaynağı için Azure API 'SI oluşturun ve sonra işlemi yeniden deneyin|
|FHıR kaynağı için Azure API 'SI IoT Bağlayıcısı için desteklenmez|Fhir kaynağı için Azure API 'sinin uyumsuz bir sürümüyle FHıR için bir Azure IoT Bağlayıcısı kullanılmaya çalışılıyor|FHıR kaynağı (sürüm R4) için yeni bir Azure API 'SI oluşturun veya FHıR kaynağı için mevcut bir Azure API 'SI kullanın (sürüm R4)

## <a name="creating-copies-of-the-azure-iot-connector-for-fhir-preview-mapping-files"></a>FHIR (Önizleme) eşleme dosyaları için Azure IoT bağlayıcısının kopyalarını oluşturma
FHıR eşleme dosyaları için Azure IoT bağlayıcısının kopyalanması, Azure portal web sitesinin dışında düzenlenebilir ve arşivlenmek ve bir destek bileti açılırken Azure teknik desteği sağlamak için yararlı olabilir.

> [!NOTE]
> JSON, şu anda cihaz ve FHıR eşleme dosyaları için desteklenen tek biçimdir.

> [!TIP]
> FHıR cihazı için Azure IoT Bağlayıcısı [ve fhır eşleme JSON dosyaları](https://docs.microsoft.com/azure/healthcare-apis/iot-mapping-templates) hakkında daha fazla bilgi edinin

1. **"Eklentiler"** bölümünde FHIR kaynak panosu IÇIN Azure API 'sinin sol alt tarafındaki **"IoT Bağlayıcısı (Önizleme)"** öğesini seçin.

   :::image type="content" source="media/iot-troubleshoot/map-files-main-with-box.png" alt-text="IoT Bağlayıcısı" lightbox="media/iot-troubleshoot/map-files-main-with-box.png":::

2. Eşleme dosyalarını kopyalayacaksınız **"bağlayıcısını"** seçin.

   :::image type="content" source="media/iot-troubleshoot/map-files-select-connector-with-box.png" alt-text="IoT Bağlayıcısı" lightbox="media/iot-troubleshoot/map-files-select-connector-with-box.png":::

> [!NOTE]
> Bu işlem, **"FHıR eşlemesini yapılandırma"** JSON içeriğini kopyalamak ve kaydetmek için de kullanılabilir.

3. **"Cihaz eşlemeyi Yapılandır"** ı seçin.

    :::image type="content" source="media/iot-troubleshoot/map-files-select-device-with-box.png" alt-text="IoT Bağlayıcısı" lightbox="media/iot-troubleshoot/map-files-select-device-with-box.png":::

4. JSON içeriğini seçin ve bir kopyalama işlemi yapın (örneğin: Ctrl + c ' yi seçin). 

   :::image type="content" source="media/iot-troubleshoot/map-files-select-device-json-with-box.png" alt-text="IoT Bağlayıcısı" lightbox="media/iot-troubleshoot/map-files-select-device-json-with-box.png":::

5. Bir düzenleyici içindeki yeni bir dosyaya (örneğin: Visual Studio Code, Notepad) bir yapıştırma işlemi yapın ve dosyayı bir *. JSON uzantısıyla kaydedin.

> [!TIP]
> FHıR için Azure IoT Bağlayıcısı için bir [Azure teknik destek](https://azure.microsoft.com/support/create-ticket/) bileti açarsanız, sorun giderme işlemine yardımcı olması için eşleme dosyalarınızın kopyalarını eklediğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

FHıR için Azure IoT Bağlayıcısı hakkında sık sorulan sorulara göz atın.

>[!div class="nextstepaction"]
>[FHıR için Azure IoT Bağlayıcısı (Önizleme) SSS](fhir-faq.md#iot-connector-preview)

* Azure portal, FHıR için Azure IoT Bağlayıcısı, IoT Bağlayıcısı (Önizleme) olarak adlandırılır.

FHIR, HL7’nin kayıtlı ticari markasıdır ve HL7’nin izniyle kullanılır.