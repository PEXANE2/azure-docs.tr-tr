---
title: FHıR için Azure IoT Bağlayıcısı (Önizleme)-sorun giderme kılavuzu ve nasıl yapılır
description: Genel Azure IoT Bağlayıcısı 'nda FHıR (Önizleme) hata iletileri ve koşulları için sorun giderme ve eşleme dosyalarını kopyalama
services: healthcare-apis
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: troubleshooting
ms.date: 08/07/2020
ms.author: jasteppe
ms.openlocfilehash: 088d1e409f14fdba02311d1ff17eb655f6e41ad3
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88053465"
---
# <a name="azure-iot-connector-for-fhir-preview-troubleshooting-guide"></a>FHıR için Azure IoT Bağlayıcısı (Önizleme) sorun giderme kılavuzu

Bu makalede, FHıR * hata iletileri ve koşulları için genel Azure IoT Bağlayıcısı sorunlarını gidermeye yönelik adımlar sağlanmaktadır.  

Ayrıca, FHıR dönüştürme eşlemeleri JSON (örneğin: cihaz ve FHıR) için Azure IoT bağlayıcısının kopyalarını oluşturmayı öğreneceksiniz.  

Dönüştürme ve arşivleme için JSON kopyalarını Azure portal dışında kullanarak değiştirebilirsiniz.  

> [!TIP]
> FHıR için Azure IoT Bağlayıcısı için bir [Azure teknik destek](https://azure.microsoft.com/support/create-ticket/) bileti açarsanız, sorun giderme işlemine yardımcı olmak için dönüştürme eşleme JSON 'larınızın kopyalarını eklediğinizden emin olun.

## <a name="error-messages-and-fixes-for-azure-iot-connector-for-fhir-preview"></a>FHıR için Azure IoT Bağlayıcısı hata iletileri ve düzeltmeleri (Önizleme)

|İleti|Görüntülenemeyecek|Koşul|Düzeltme| 
|-------|---------|---------|---|
|Geçersiz eşleme adı, eşleme adı cihaz veya FHıR olmalıdır.|API|Sağlanan eşleme türü cihaz veya FHıR değil.|Desteklenen iki eşleme türünden birini kullanın (örneğin: cihaz veya FHıR).|
|Doğrulama başarısız oldu. Gerekli bilgiler eksik veya geçersiz.|API ve Azure portal|Gerekli bilgi veya öğe için bir dönüştürme eşlemesinin kaydedilmeye çalışılıyor.|Eksik dönüştürme eşleme bilgilerini veya öğesini ekleyin ve dönüştürme eşlemesini tekrar kaydetmeyi deneyin.|
|Anahtar parametrelerini yeniden oluştur tanımlı değil.|API|Anahtar isteğini yeniden üret.|Yeniden oluşturma anahtar isteğine parametreleri ekleyin.|
|Bu abonelikte sağlanabilen IoT Bağlayıcısı örneklerinin maksimum sayısına ulaşıldı.|API ve Azure portal|FHıR abonelik kotasına ulaşılan Azure IoT Bağlayıcısı (abonelik başına varsayılan değer (2)).|FHıR için Azure IoT bağlayıcısının mevcut örneklerinden birini silin.  Abonelik kotasına ulaşmamış farklı bir abonelik kullanın.  Abonelik kotası artışı iste.|
|IoT Bağlayıcısı için Azure API 'SI için kaynak taşıma özelliği, FHıR kaynağı için desteklenmiyor.|API ve Azure portal|FHIR için Azure IoT bağlayıcısının bir veya daha fazla örneği olan FHıR kaynağı için bir Azure API 'SI üzerinde taşıma işlemi yapılmaya çalışılıyor.|Taşıma işlemini yapmak için FHAR Azure IoT bağlayıcısının mevcut örneklerini silin.|
|IoT Bağlayıcısı sağlanmadı.|API|Üst öğe (FHıR için Azure IoT Bağlayıcısı) sağlanmadığından alt hizmetler (bağlantılar & eşlemeler) kullanılmaya çalışılıyor.|FHıR için bir Azure IoT Bağlayıcısı sağlayın.|
|İstek desteklenmiyor.|API|Belirli API isteği desteklenmiyor.|Doğru API isteğini kullanın.|
|Hesap yok.|API|FHAR için Azure IoT Bağlayıcısı ekleme girişimi ve FHıR için Azure API kaynağı yok.|FHıR kaynağı için Azure API 'SI oluşturun ve ardından işlemi yeniden deneyin.|
|FHıR kaynağı için Azure API 'SI IoT Bağlayıcısı için desteklenmez.|API|FHıR için Azure API 'sinin uyumsuz bir sürümüyle fhır için bir Azure IoT Bağlayıcısı kullanılmaya çalışılıyor.|FHıR kaynağı (sürüm R4) için yeni bir Azure API 'SI oluşturun veya FHıR kaynağı için mevcut bir Azure API 'SI (sürüm R4) kullanın.

##  <a name="why-is-my-azure-iot-connector-for-fhir-preview-data-not-showing-up-in-azure-api-for-fhir"></a>Azure IoT Bağlayıcım, fhır için Azure API 'de görünmüyor.

|Olası sorunlar  |Düzeltmeler            |
|------------------|-----------------|
|Verilerin işlenmesi devam ediyor.|Veriler, toplu iş (YAKLAŞıK 15 dakikada bir) için Azure API 'sine yöneliktir.  Verilerin hala işlenmesine ve verilerin FHıR için Azure API 'sinde kalıcı olması için ek süre gerekebilir.|
|Cihaz dönüştürme eşleme JSON 'ı yapılandırılmadı.|Uyumsuz cihaz dönüştürme eşlemesi JSON 'ı yapılandırın ve kaydedin.|
|FHıR dönüştürme eşlemesi JSON yapılandırılmadı.|Uygun FHıR dönüştürme eşlemesi JSON 'ı yapılandırın ve kaydedin.|
|Cihaz iletisinde cihaz eşlemesinde tanımlanmış bir beklenen ifade yok.|Cihaz iletisinde tanımlanan, cihaz eşlemesinde eşleşen belirteçlerde tanımlanan JsonPath ifadelerin doğrulanması.|
|FHıR için Azure API 'sinde bir cihaz kaynağı oluşturulmadı (çözüm türü: yalnızca arama) *.|FHıR için Azure API 'sinde geçerli bir cihaz kaynağı oluşturun. Cihaz kaynağının gelen iletide belirtilen cihaz tanımlayıcısıyla eşleşen bir tanımlayıcı içerdiğinden emin olun.|
|FHıR için Azure API 'sinde bir hasta kaynağı oluşturulmadı (çözüm türü: yalnızca arama) *.|FHıR için Azure API 'sinde geçerli bir hasta kaynağı oluşturun.|
|Device. hasta başvurusu ayarlanmadı veya başvuru geçersiz (çözüm türü: yalnızca arama) *.|Cihaz kaynağının bir hasta kaynağına geçerli bir [başvuru](https://www.hl7.org/fhir/device-definitions.html#Device.patient) içerdiğinden emin olun.| 

* Başvuru hızlı başlangıç: FHıR çözüm türleri için Azure IoT bağlayıcısının işlevsel bir açıklaması için [Azure Portal kullanarak Azure IoT bağlayıcısını (Önizleme) dağıtma](iot-fhir-portal-quickstart.md#create-new-azure-iot-connector-for-fhir-preview) (örneğin: arama veya oluşturma).

## <a name="creating-copies-of-the-azure-iot-connector-for-fhir-preview-conversion-mapping-json"></a>Azure IoT bağlayıcısının, FHIR (Önizleme) dönüştürme eşlemesi JSON için kopyalarını oluşturma
FHıR eşleme dosyaları için Azure IoT bağlayıcısının kopyalanması, Azure portal web sitesinin dışında düzenlenebilir ve arşivlenmek üzere yararlı olabilir.

Sorun gidermeye yardımcı olmak üzere bir destek bileti açılırken, eşleme dosya kopyaları Azure teknik desteği 'ne sağlanmalıdır.

> [!NOTE]
> JSON, şu anda cihaz ve FHıR eşleme dosyaları için desteklenen tek biçimdir.

> [!TIP]
> FHıR cihazı için Azure IoT Bağlayıcısı [ve fhır dönüştürme eşleme JSON](https://docs.microsoft.com/azure/healthcare-apis/iot-mapping-templates) hakkında daha fazla bilgi edinin

1. **"Eklentiler"** bölümünde FHIR kaynak panosu IÇIN Azure API 'sinin sol alt tarafındaki **"IoT Bağlayıcısı (Önizleme)"** öğesini seçin.

   :::image type="content" source="media/iot-troubleshoot/map-files-main-with-box.png" alt-text="IoT Bağlayıcısı" lightbox="media/iot-troubleshoot/map-files-main-with-box.png":::

2. JSON dönüştürme eşlemesini içinden kopyalayacaksınız **"bağlayıcısını"** seçin.

   :::image type="content" source="media/iot-troubleshoot/map-files-select-connector-with-box.png" alt-text="IoT Bağlayıcısı" lightbox="media/iot-troubleshoot/map-files-select-connector-with-box.png":::

> [!NOTE]
> Bu işlem, **"FHıR eşlemesini yapılandırma"** JSON içeriğini kopyalamak ve kaydetmek için de kullanılabilir.

3. **"Cihaz eşlemeyi Yapılandır"** ı seçin.

    :::image type="content" source="media/iot-troubleshoot/map-files-select-device-with-box.png" alt-text="IoT Bağlayıcısı" lightbox="media/iot-troubleshoot/map-files-select-device-with-box.png":::

4. JSON içeriğini seçin ve bir kopyalama işlemi yapın (örneğin: Ctrl + c ' yi seçin). 

   :::image type="content" source="media/iot-troubleshoot/map-files-select-device-json-with-box.png" alt-text="IoT Bağlayıcısı" lightbox="media/iot-troubleshoot/map-files-select-device-json-with-box.png":::

5. Bir düzenleyici içindeki yeni bir dosyaya (örneğin: Visual Studio Code, Notepad) bir yapıştırma işlemi yapın ve dosyayı bir *. JSON uzantısıyla kaydedin.

> [!TIP]
> FHıR için Azure IoT Bağlayıcısı için bir [Azure teknik destek](https://azure.microsoft.com/support/create-ticket/) bileti açarsanız, sorun giderme işlemine yardımcı olmak için dönüştürme eşleme JSON 'larınızın kopyalarını eklediğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

FHıR için Azure IoT Bağlayıcısı hakkında sık sorulan sorulara göz atın.

>[!div class="nextstepaction"]
>[FHıR SSS için Azure IoT Bağlayıcısı](fhir-faq.md#azure-iot-connector-for-fhir-preview)

* Azure portal, FHıR için Azure IoT Bağlayıcısı, IoT Bağlayıcısı (Önizleme) olarak adlandırılır.

FHIR, HL7’nin kayıtlı ticari markasıdır ve HL7’nin izniyle kullanılır.
