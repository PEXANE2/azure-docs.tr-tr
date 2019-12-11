---
title: Azure IoT Hub DPS ile bağlantıyı Tanıla ve sorunlarını giderme
description: Azure IoT Hub cihaz sağlama hizmeti (DPS) için cihaz bağlantısıyla sık karşılaşılan hataları tanılamayı ve sorun gidermeyi öğrenin
author: xujing-ms
manager: nberdy
ms.service: iot-dps
services: iot-dps
ms.topic: conceptual
ms.date: 09/09/2019
ms.author: xujing
ms.openlocfilehash: ffe20ff80e26bc5564b9379ea21ca99e2890b519
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74974828"
---
# <a name="troubleshooting-with-azure-iot-hub-device-provisioning-service"></a>Azure IoT Hub cihaz sağlama hizmeti ile sorun giderme

Kanıtlama hataları, kayıt hataları vb. gibi birçok olası başarısızlık noktası olduğundan, IoT cihazlarına yönelik bağlantı sorunlarının giderilmesi zor olabilir. Bu makalede, [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/overview)aracılığıyla cihaz bağlantısı sorunlarını algılamaya ve gidermeye yönelik yönergeler sunulmaktadır.

## <a name="using-azure-monitor-to-view-metrics-and-set-up-alerts"></a>Ölçümleri görüntülemek ve uyarıları ayarlamak için Azure Izleyici 'yi kullanma

Aşağıdaki yordamda IoT Hub cihaz sağlama hizmeti ölçümünde uyarının nasıl görüntüleneceği ve ayarlanacağı açıklanır. 

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.

2. IoT Hub cihaz sağlama hizmetinize gidin.

3. **Ölçümler**’i seçin.

4. İstediğiniz ölçümü seçin. 
   <br />Şu anda DPS için üç ölçüm vardır:

    | Ölçüm Adı | Açıklama |
    |-------|------------|
    | Kanıtlama denemeleri | Cihaz sağlama hizmeti ile kimlik doğrulamaya çalıştı cihaz sayısı|
    | Kayıt denemeleri | Başarılı kimlik doğrulamasından sonra IoT Hub kaydolmaya çalışılan cihazların sayısı|
    | Cihaz atandı | IoT Hub başarıyla atanan cihazların sayısı|

5. Ölçümün görsel görünümünü oluşturmak için istenen toplama yöntemini seçin. 

6. Ölçüm uyarısı ayarlamak için, ölçüm dikey penceresinin sağ üst köşesindeki **Yeni uyarı kuralları** ' nı seçin, benzer şekilde **Uyarı** dikey penceresine gidebilir ve **Yeni uyarı kuralları**' nı seçebilirsiniz.

7. **Koşul Ekle**' yi seçin, ardından komut istemlerini izleyerek istediğiniz ölçüm ve eşiği seçin.

Daha fazla bilgi edinmek için bkz. [Microsoft Azure klasik uyarılar nelerdir?](../azure-monitor/platform/alerts-overview.md)

## <a name="using-log-analytic-to-view-and-resolve-errors"></a>Hataları görüntülemek ve çözmek için log analitik kullanma

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.

2. IoT Hub 'ınıza gidin.

3. **Tanılama ayarları**' nı seçin.

4. Seçin **tanılamayı Aç**.

5. İstenen günlüklerin toplanmasını sağlar.

    | Günlük Adı | Açıklama |
    |-------|------------|
    | DeviceOperations Işlemleri | Cihaz bağlantı olaylarıyla ilgili Günlükler |
    | ServiceOperations | Service SDK kullanımıyla ilgili olay günlükleri (ör. kayıt grupları oluşturma veya güncelleştirme)|

6. **Log Analytics gönder** ' i açın ([bkz. fiyatlandırma](https://azure.microsoft.com/pricing/details/log-analytics/)). 

7. Cihaz sağlama hizmeti kaynağı altındaki Azure portal **Günlükler** sekmesine gidin.

8. Son olayları görüntülemek için **Çalıştır** ' a tıklayın.

9. Sonuçlar varsa, hata hakkında daha fazla ayrıntı almak için `OperationName`, `ResultType`, `ResultSignature`ve `ResultDescription` (hata iletisi) arayın.


## <a name="common-error-codes"></a>Sık kullanılan hata kodları
Sık karşılaşılan hataları anlamak ve çözmek için bu tabloyu kullanın.

| Hata Kodu| Açıklama | HTTP Durum Kodu |
|-------|------------|------------|
| 400 | İsteğin gövdesi geçerli değil; Örneğin, ayrıştırılamıyor veya nesne doğrulanamıyor.| 400 Hatalı biçim |
| 401 | Yetkilendirme belirteci doğrulanamıyor; Örneğin, zaman aşımına uğradı veya isteğin URI 'SI için uygulanmaz. Bu hata kodu, TPM kanıtlama akışının bir parçası olarak cihazlara de döndürülür. | 401 Yetkisiz|
| 404 | Cihaz sağlama hizmeti örneği veya bir kaynak (örn. bir kayıt) yok. |404 Bulunamadı |
| 412 | İstekteki ETag, RFC7232 başına mevcut kaynağın ETag 'i ile eşleşmiyor. | 412 Önkoşul başarısız oldu |
| 429 | İşlemler, hizmet tarafından kısıtlanıyor. Belirli hizmet limitleri için bkz. [cihaz sağlama hizmeti sınırlarını IoT Hub](https://docs.microsoft.com/azure/azure-subscription-service-limits#iot-hub-device-provisioning-service-limits). | 429 çok fazla istek |
| 500 | Bir iç hata oluştu. | 500 İç Sunucu Hatası|
