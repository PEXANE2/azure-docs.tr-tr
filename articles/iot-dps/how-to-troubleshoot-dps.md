---
title: Azure IoT Hub DPS ile bağlantı bağlantı larını tanılama ve sorun giderme
description: Azure IoT Hub Aygıt Sağlama Hizmeti (DPS) için aygıt bağlantısıyla sık karşılaşılan hataları tanılamayı ve sorun gidermeyi öğrenin
author: xujing-ms
manager: nberdy
ms.service: iot-dps
services: iot-dps
ms.topic: conceptual
ms.date: 09/09/2019
ms.author: xujing
ms.openlocfilehash: 3cbab09c6b50abb590cfe9f2720713a8fa547aa7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75646481"
---
# <a name="troubleshooting-with-azure-iot-hub-device-provisioning-service"></a>Azure IoT Hub Aygıt Sağlama Hizmeti ile Sorun Giderme

IoT aygıtları için bağlantı sorunlarını gidermek zor olabilir, çünkü attestation hataları, kayıt hataları gibi birçok olası hata noktası vardır. Bu makalede, [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview)üzerinden aygıt bağlantısı sorunlarının nasıl algılandığı ve sorun giderilen yol hakkında kılavuz verilmektedir.

## <a name="using-azure-monitor-to-view-metrics-and-set-up-alerts"></a>Ölçümleri görüntülemek ve uyarıları ayarlamak için Azure Monitörünü kullanma

Aşağıdaki yordam, IoT Hub Aygıt Sağlama Hizmeti ölçümünde nasıl görüntülenir ve uyarı ayarlanır. 

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. IoT Hub Cihaz Sağlama Hizmetinize göz atın.

3. **Ölçümler**’i seçin.

4. İstenilen ölçütünü seçin. 
   <br />Şu anda DPS için üç ölçüm vardır:

    | Metrik Adı | Açıklama |
    |-------|------------|
    | Attestation girişimleri | Aygıt Sağlama Hizmeti ile kimlik doğrulamaya çalışan aygıt sayısı|
    | Kayıt denemeleri | Başarılı kimlik doğrulamadan sonra IoT Hub'a kaydolmaya çalışan aygıt sayısı|
    | Atanan aygıt | IoT Hub'ına başarıyla atanan aygıt sayısı|

5. Ölçümün görsel görünümünü oluşturmak için istenen toplama yöntemini seçin. 

6. Bir metrik uyarısı ayarlamak için, metrik bıçağın sağ üst kısmından **Yeni uyarı kuralları** nı seçin, benzer şekilde **Alert** blade'e gidebilir ve Yeni **uyarı kurallarını**seçebilirsiniz.

7. **Koşul Ekle'yi**seçin, ardından istemleri izleyerek istediğiniz metrik ve eşiği seçin.

Daha fazla bilgi edinmek için Microsoft [Azure'daki klasik uyarılar nelerdir?](../azure-monitor/platform/alerts-overview.md)

## <a name="using-log-analytic-to-view-and-resolve-errors"></a>Hataları görüntülemek ve çözmek için Log Analytic'i kullanma

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. IoT hub'ınıza göz atın.

3. **Tanılama ayarlarını**seçin.

4. **Tanılamayı Aç'ı**seçin.

5. İstenilen günlüklerin toplanmasını etkinleştirin.

    | Günlük Adı | Açıklama |
    |-------|------------|
    | Cihaz İşlemleri | Aygıt bağlantısı olaylarıile ilgili günlükler |
    | Hizmet İşlemleri | Hizmet SDK'nın kullanımıyla ilgili olay günlükleri (örn. kayıt grupları oluşturma veya güncelleme)|

6. Günlük **Analitiğine Gönder'i** açın ([fiyatlandırmaya bakın).](https://azure.microsoft.com/pricing/details/log-analytics/) 

7. Aygıt Sağlama Hizmeti kaynağı altında Azure portalında **Günlükler** sekmesine gidin.

8. Son olayları görüntülemek için **Çalıştır'ı** tıklatın.

9. Sonuç varsa, hata `OperationName`hakkında `ResultType` `ResultSignature`daha `ResultDescription` fazla ayrıntı almak için , , , ve (hata iletisi) arayın.


## <a name="common-error-codes"></a>Genel hata kodları
Sık karşılaşılan hataları anlamak ve gidermek için bu tabloyu kullanın.

| Hata Kodu| Açıklama | HTTP Durum Kodu |
|-------|------------|------------|
| 400 | İsteğin gövdesi geçerli değildir; örneğin, ayrıştıolamaz veya nesne doğrulanamaz.| 400 Kötü biçim |
| 401 | Yetkilendirme belirteci doğrulanamaz; örneğin, süresi dolmuş veya isteğin URI için geçerli değildir. Bu hata kodu, TPM attestation akışının bir parçası olarak aygıtlara da döndürülür. | 401 Yetkisiz|
| 404 | Aygıt Sağlama Hizmeti örneği veya bir kaynak (örn. bir kayıt) yok. |404 Bulunamadı |
| 412 | İstekteki ETag, RFC7232'ye göre varolan kaynağın ETag'ı ile eşleşmez. | 412 Ön koşul başarısız oldu |
| 429 | Operasyonlar hizmet tarafından azaltılıyor. Belirli hizmet sınırları [için, IoT Hub Aygıt Sağlama Hizmeti sınırlarına](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#iot-hub-device-provisioning-service-limits)bakın. | 429 Çok fazla istek |
| 500 | Bir iç hata oluştu. | 500 İç Sunucu Hatası|
