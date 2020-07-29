---
title: FHıR için Azure IoT Bağlayıcısı (IoT Bağlayıcısı)-sorun giderme kılavuzu ve nasıl yapılır
description: Yaygın IoT Bağlayıcısı hata iletileri ve koşulları sorunlarını giderme ve eşleme dosyalarını kopyalama
services: healthcare-apis
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: conceptual
ms.date: 07/16/2020
ms.author: jasteppe
ms.openlocfilehash: eff1272318413da7855134b0a8a44dd0a0711a6c
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87285586"
---
# <a name="iot-connector-preview-troubleshooting-guide"></a>IoT Bağlayıcısı (Önizleme) sorun giderme kılavuzu

Bu makalede, yaygın IoT Bağlayıcısı (Önizleme) hata iletileri ve koşullarında sorun gidermeye yönelik adımlar sağlanmaktadır.  

Ayrıca, Azure portal dışında, görüntüleme ve arşivleme için IoT Bağlayıcısı (Önizleme) JSON eşleme dosyalarının kopyalarını oluşturmayı öğreneceksiniz.

## <a name="error-messages-and-fixes"></a>Hata iletileri ve düzeltmeler

|İleti   |Koşul  |Düzeltme         |
|----------|-----------|------------|
|Geçersiz eşleme adı, eşleme adı cihaz veya FHıR olmalıdır|Sağlanan eşleme türü cihaz veya FHıR değil|Desteklenen iki eşleme türünden birini kullanın (örneğin: cihaz veya FHıR)|
|Anahtar parametrelerini yeniden oluştur tanımlı değil|Anahtar isteğini yeniden üret|Yeniden oluşturma anahtar isteğine parametreleri ekleyin|
|Bu abonelikte sağlanabilen IoT Bağlayıcısı örneklerinin maksimum sayısına ulaştı|IoT Bağlayıcısı Abonelik kotasına ulaşıldı (varsayılan abonelik başına 2 ' dir)|Mevcut bağlayıcılardan birini silme, abonelik kotası başına (2) bağlayıcıya ulaşmayan farklı bir abonelik kullanın veya abonelik kotası artışı isteyin|
|IoT Bağlayıcısı için kaynak taşıma Azure API 'SI, FHıR kaynağı için desteklenmiyor|Bir veya daha fazla IoT Bağlayıcısı bulunan FHıR kaynağı için bir Azure API 'SI üzerinde taşıma işlemi yapılmaya çalışılıyor|Taşıma işlemlerini gerçekleştirmek/tamamlamak için mevcut bağlayıcıları silin|
|FHıR kaynağı için Azure API 'SI, özel bağlantı etkin.  Özel bağlantı IoT Bağlayıcısı ile desteklenmiyor|Özel bağlantısı etkin olan FHıR kaynağı için bir Azure API 'sine IoT Bağlayıcısı eklenmeye çalışılıyor|Özel bağlantısı etkinleştirilmemiş FHıR kaynağı (sürüm R4) için bir Azure API 'SI seçin veya oluşturun|
|IoT Bağlayıcısı sağlanmadı|Üst (IoT Bağlayıcısı) sağlanmadığından alt hizmetler (bağlantılar & eşlemeler) kullanılmaya çalışılıyor|IoT Bağlayıcısı sağlama|
|İstek desteklenmiyor|Belirli API isteği desteklenmiyor|Doğru API isteğini kullan|
|Hesap yok|IoT Bağlayıcısı ekleme girişimi ve FHıR kaynağı için Azure API 'SI yok|FHıR kaynağı için Azure API 'SI oluşturun ve sonra işlemi yeniden deneyin|
|FHıR kaynağı için Azure API 'SI IoT Bağlayıcısı için desteklenmez|FHıR kaynağı için Azure API 'sinin uyumsuz bir sürümüyle IoT Bağlayıcısı kullanılmaya çalışılıyor|FHıR kaynağı (sürüm R4) için yeni bir Azure API 'SI oluşturun veya FHıR kaynağı için mevcut bir Azure API 'SI kullanın (sürüm R4)

## <a name="creating-copies-of-the-iot-connector-preview-mapping-files"></a>IoT Bağlayıcısı (Önizleme) eşleme dosyalarının kopyalarını oluşturma
> [!NOTE]
> JSON, şu anda cihaz ve FHıR eşleme dosyaları için desteklenen tek biçimdir.

> [!TIP]
> IoT Bağlayıcısı eşleme dosyalarının kopyalanması, Azure Portal Web sitesi dışında düzenlenebilir ve arşivlenmek ve bir destek bileti açılırken Azure teknik desteği sağlamak için yararlı olabilir.
> 
> IoT bağlayıcı [cihazı ve FHıR eşleme JSON dosyaları](https://docs.microsoft.com/azure/healthcare-apis/iot-mapping-templates) hakkında daha fazla bilgi edinin

1. **"Eklentiler"** bölümünde FHIR kaynak panosu IÇIN Azure API 'sinin sol alt tarafındaki **"IoT Bağlayıcısı (Önizleme)"** öğesini seçin.

   :::image type="content" source="media/iot-troubleshoot/map-files-main-with-box.png" alt-text="IoT Bağlayıcısı" lightbox="media/iot-troubleshoot/map-files-main-with-box.png":::

2. Eşleme dosyalarını kopyalayacaksınız **"bağlayıcısını"** seçin.

   :::image type="content" source="media/iot-troubleshoot/map-files-select-connector-with-box.png" alt-text="IoT Bağlayıcısı" lightbox="media/iot-troubleshoot/map-files-select-connector-with-box.png":::

3. **"Cihaz eşlemeyi Yapılandır"** ı seçin.

   :::image type="content" source="media/iot-troubleshoot/map-files-select-device-with-box.png" alt-text="IoT Bağlayıcısı" lightbox="media/iot-troubleshoot/map-files-select-device-with-box.png":::

> [!NOTE]
> Bu işlem, **"FHıR eşlemeyi yapılandırma"** JSON içeriğini kopyalamak/kaydetmek için de kullanılabilir.

4. JSON içeriğini seçin ve bir kopyalama işlemi yapın (örneğin: Ctrl + c ' yi seçin). 

   :::image type="content" source="media/iot-troubleshoot/map-files-select-device-json-with-box.png" alt-text="IoT Bağlayıcısı" lightbox="media/iot-troubleshoot/map-files-select-device-json-with-box.png":::

5. Bir düzenleyici içindeki yeni bir dosyaya (örneğin: Visual Studio Code, Notepad) bir yapıştırma işlemi yapın ve dosyayı bir *. JSON uzantısıyla kaydedin.

> [!TIP]
> IoT Bağlayıcısı için bir [Azure teknik destek](https://azure.microsoft.com/support/create-ticket/) bileti açarsanız, sorun giderme sürecine yardımcı olması için eşleme dosyalarınızın kopyalarını eklediğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

IoT Bağlayıcısı hakkında sık sorulan sorulara göz atın

>[!div class="nextstepaction"]
>[IoT Bağlayıcısı SSS](fhir-faq.md#iot-connector-preview)


FHIR, HL7’nin kayıtlı ticari markasıdır ve HL7’nin izniyle kullanılır.