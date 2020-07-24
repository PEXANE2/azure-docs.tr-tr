---
title: Module ikizi JSON şeması-Azure
description: Bu konu, IoT Edge üzerindeki canlı video analizlerinin Module ikizi JSON şemasını açıklar.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 8bd86bdc2c8de9ee586e785db2074fa772100420
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87053066"
---
# <a name="module-twin-json-schema"></a>Module ikizi JSON şeması

Cihaz TWINS, meta veriler, konfigürasyonlar ve koşullar dahil olmak üzere cihaz durum bilgilerini depolayan JSON belgelerdir. Azure IoT Hub, IoT Hub'a bağladığınız her cihaz için bir cihaz çifti tutar. Ayrıntılı açıklama için bkz. [IoT Hub modül TWINS 'ı anlama ve kullanma](../../iot-hub/iot-hub-devguide-module-twins.md)

Bu konu, IoT Edge üzerindeki canlı video analizlerinin Module ikizi JSON şemasını açıklar.

> [!NOTE]
> Media Services kaynaklarına ve Media Services API'sine erişim yetkisi almak için önce kimliğinizi doğrulamanız gerekir. Daha fazla bilgi için bkz. [Azure MEDIA SERVICES API 'Sine erişme](../latest/media-services-apis-overview.md#accessing-the-azure-media-services-api).

## <a name="module-twin-properties"></a>Module ikizi özellikleri

IoT Edge üzerindeki canlı video analizi, aşağıdaki modülün ikizi özelliklerini kullanıma sunar. 

|Özellik |Gerekli |Dinamik |Açıklama |
|---|---|---|---|
|applicationDataDirectory |Evet |Hayır |Kalıcı yapılandırma için bağlı bir birimin yolu. |
|Azudüzeltici Aservicesarmid |Evet |Hayır |Media Services hesabı için benzersiz Azure Kaynak Yönetimi tanımlayıcısı.|
|Aadtenantıd |Evet |Hayır |Müşteri Azure AD kiracı KIMLIĞI.|
|Aadserviceprincıpalappıd |Yes |Yes |Müşteri Azure AD AppID oluşturdu.|
|aadServicePrincipalCertificate |Yes<sup>*</sup>  |Yes |Müşteri Azure AD AppID sertifikası oluşturdu.|
|aadServicePrincipalPassword |Yes<sup>*</sup>  |Yes |Müşteri Azure AD AppID parolası oluşturdu.|
|aadEndpoint |Hayır |Hayır |Buluta özel Azure AD uç noktası. <br/>Varsayılanını`https://login.microsoftonline.com` |
|Aadresourceıd |Hayır |Hayır |Buluta özel Azure AD hedef kitlesi/kaynak KIMLIĞI <br/>Varsayılanını`https://management.core.windows.net/` |
|armEndpoint |Hayır |Hayır |Buluta özgü Azure Kaynak uç noktasını yönetme. <br/>Varsayılanını`https://management.azure.com/` |
|diagnosticsLevel |Hayır |Evet |Olaylar ayrıntı düzeyi: <br/>Bilgi & # x02758; Uyarı & # x02758; Hata & # x02758; Kritik & # x02758; Seçim |
|diagnosticsEventsOutputName |Hayır |Evet |Tanılama olayları için hub çıkışı. <br/>(Boş, tanılama yayımlanmadığı anlamına gelir)|
|operationalEventsOutputName|Hayır|Evet|İşletimsel olaylar için Merkez çıkışı.<br/>(Boş, işlemsel olayların yayımlanmadığı anlamına gelir)
|logLevel|Hayır|Evet|Aşağıdakilerden biri: <br/>& # x000B7; Seçeneini<br/>& # x000B7; Bilgi (varsayılan)<br/>& # x000B7; Warning<br/>& # x000B7; Hatayla<br/>& # x000B7; Seçim|
|Günlüğe kaydetme kategorileri|Hayır|Evet|Aşağıdakilerin virgülle ayrılmış listesi: uygulama, MediaPipeline, olaylar <br/>Varsayılan: uygulama, olaylar|
|debugLogsDirectory|Hayır|Evet|Hata ayıklama günlükleri dizini. Mevcut Günlükler oluşturulmuşsa, mevcut değilse hata ayıklama günlükleri devre dışıdır.

<sup>*</sup>Hizmet sorumlusu sertifikası veya parola sağlamalısınız. 

Dinamik özellikler modülün yeniden başlatılmasına gerek kalmadan güncelleştirilemeyebilir. [MEDIA SERVICES API 'ye erişim sağlama](../latest/access-api-howto.md) makalesindeki yönergeyi izleyerek bu özelliklerden bazıları için değerleri elde edebilirsiniz. 

İsteğe bağlı tanılama ayarlarının rolü hakkında daha fazla bilgi için [izleme ve günlüğe kaydetme](monitoring-logging.md) hakkındaki makaleye bakın.

```
{ 
    "properties.desired": { 
        // Required 
        "applicationDataDirectory": "/var/lib/azuremediaservices", 
        "azureMediaServicesArmId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/<rgname>/providers/microsoft.media/mediaservices/<ams_account>", 
        "aadTenantId": "00000000-0000-0000-0000-000000000000", 
        "aadServicePrincipalAppId": "00000000-0000-0000-0000-000000000000", 
        "aadServicePrincipalPassword": "{Service principal password}", 

        // Optional API Access 
        "aadEndpoint": "https://<aad-endpoint>", 
        "aadResourceId": "https://management.core.windows.net/", 
        "armEndpoint": "https://management.azure.com/", 
        
        // Optional Diagnostics 
        "diagnosticsEventsOutputName": "lvaEdgeDiagnostics",
        "operationalEventsOutputName": "lvaEdgeOperational",
        "logLevel": "Information",
        "logCategories": "Application,Events"
    } 
} 
```

## <a name="next-steps"></a>Sonraki adımlar

[Doğrudan yöntemler](direct-methods.md)
