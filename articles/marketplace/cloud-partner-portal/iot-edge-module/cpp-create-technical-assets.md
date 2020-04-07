---
title: Azure IoT Edge modülü teknik varlıkları oluşturma | Azure Marketi
description: Bir IoT Edge modülü için teknik varlıkları oluşturun.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: cd9cd9f3986ef1944d9f9119296af5512b352d1d
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80744307"
---
# <a name="prepare-your-iot-edge-module-technical-assets"></a>IoT Edge modülü teknik varlıklarınızı hazırlayın

>[!Important]
>30 Mart 2020'den itibaren, IoT Edge modül tekliflerinizi İş Merkezi'ne taşımaya başlayacağız. Geçişten sonra, Tekliflerinizi İş Ortağı Merkezi'nde oluşturur ve yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Bir IoT Edge modülü teklifi oluştur'daki](https://aka.ms/AzureIoTTechAsset) yönergeleri izleyin.

Bu makalede, Azure Marketi'nde yayımlanmadan önce IoT Edge modülü teknik varlıklarınızın karşılaması gereken gereksinimler açıklanmaktadır.

## <a name="understanding-iot-edge-modules-and-getting-started"></a>IoT Edge modüllerini anlama ve

IoT Edge modülü, Docker uyumlu bir kapsayıcıdır ve IoT Edge aygıtında çalışmak üzere üretilmiştir.

- IoT Edge modülleri hakkında daha fazla bilgi edinmek için Azure [IoT Edge modüllerini anlayın.](https://docs.microsoft.com/azure/iot-edge/iot-edge-modules)
- IoT Edge modül geliştirmenize başlamak [için, IoT Edge modüllerini geliştirmek için gereksinimleri ve araçları](https://docs.microsoft.com/azure/iot-edge/module-development)görün.

## <a name="technical-requirements"></a>Teknik gereksinimler

IoT Edge modülünüzün Azure Marketi'nde sertifikalandırılabilmesi ve yayımlanabilmesi için aşağıdaki teknik gereksinimlerin karşılanması gerekir.

### <a name="platform-support"></a>Platform desteği

IoT Edge modülünüz aşağıdaki platform seçeneklerinden birini desteklemelidir.

#### <a name="tier-1-platforms-supported-by-iot-edge"></a>IoT Edge tarafından desteklenen Tier 1 platformları

IoT Edge tarafından desteklenen tüm Tier 1 platformlarını destekleyin [(Azure IoT Edge desteğinde](https://docs.microsoft.com/azure/iot-edge/support)kaydedildi). Daha iyi bir müşteri deneyimi sağladığı ndan bu seçeneği öneririz. Bu kriterleri karşılayan modüller sergilenecektir. Bu platform seçeneğini kullanan bir modül şunları yapmalı:

- GitHub `latest` [bildirim aracıyla](https://github.com/estesp/manifest-tool)oluşturulmuş bildirim `1.0.1`etiketleri olan bir etiket ve sürüm etiketi (örneğin,) sağlayın.
- [Uyumlu IoT Edge sertifikalı aygıtlara](https://aka.ms/iot-edge-certified)bağlantı eklemek için [Market sekmesini](./cpp-marketplace-tab.md) kullanın. Bu `https://aka.ms/iot-edge-certified`bağlantı, müşterilerin sertifikalı aygıtlara göz atabileceği veya arama yapabileceği bir web sitesine gider. Bu web sitesi, [Azure IoT Edge Sertifikalı](https://catalog.azureiotsolutions.com/) aygıt kataloğu olarak da bilinir.

#### <a name="a-subset-of-tier-1-platforms-supported-by-iot-edge"></a>IoT Edge tarafından desteklenen Tier 1 platformlarının bir alt kümesi
  
IoT Edge tarafından desteklenen Tier 1 platformlarının bir alt kümesini (en az bir) destekleyin [(Azure IoT Edge desteğinde](https://docs.microsoft.com/azure/iot-edge/support)kaydedildi). Bu platform seçeneğini kullanan bir modül şunları yapmalı:

- Birden `latest` fazla platform desteklenirse, `1.0.1`GitHub [bildirim aracıyla](https://github.com/estesp/manifest-tool) oluşturulmuş bildirim etiketleri olan bir etiket ve sürüm etiketi (örneğin,) sağlayın. Bildirim etiketleri yalnızca bir platform desteklendiğinde isteğe bağlıdır.
- [Azure IoT Edge Sertifikalı](https://catalog.azureiotsolutions.com/) aygıt kataloğunda en az bir IoT Edge aygıtına bağlantı sağlamak için Market [sekmesini](./cpp-marketplace-tab.md) kullanın.

### <a name="device-dimensions"></a>Cihaz boyutları

Hedeflenen IoT Edge cihazlarındaki IoT Edge modül boyutları (CPU/RAM/Storage/GPU/vb.) aşağıdaki gereksinimleri karşılamalıdır:

- Modülün [Azure IoT Edge Sertifikalı](https://catalog.azureiotsolutions.com/) aygıt kataloğunda en az bir **IoT Edge sertifikalı cihazla çalışması** gerekir.
- **Minimum donanım gereksinimleri,** teklifin açıklamasındaki son paragraf olarak belgelenmelidir [(Market sekmesi](./cpp-marketplace-tab.md)altında). İsteğe bağlı olarak, önerilen donanım gereksinimlerini önemli ölçüde farklıysa da listeleyebilirsiniz. Örneğin, teklif açıklamanızın sonuna aşağıdaki bölümü ekleyin:

  ```html
    <p><u>Minimum hardware requirements:</u> Linux x64 and arm32  OS, 1GB of RAM, 500 Mb of storage</p>
  ```

### <a name="configuration"></a>Yapılandırma

Ayrıca, bir IoT Edge aygıtına dağıtımı mümkün olduğunca doğrudan ileri ye getirmek için varsayılan yapılandırma ayarlarını da içerir. Kapsayıcı ayrıca edgeHub ve IoT Hub ile iletişimi etkinleştirmek için IoT Edge Modülü SDK'yı da içerebilir.

#### <a name="default-configuration"></a>Varsayılan yapılandırma

IoT Edge [modülleri, Bulut İş Ortağı portalının SKU sekmesinde](./cpp-skus-tab.md)sağlanan varsayılan ayarlarla başlayabilmeli. Aşağıdaki varsayılan ayarlar kullanılabilir:

- Varsayılan **rotalar**
- Varsayılan **ikiz istenilen özellikler**
- Varsayılan **ortam değişkenleri**
- Varsayılan **createOptions**

Varsayılan değer için gerekli olan parametrenin (örneğin, müşterinin sunucusunun IP adresi) anlamlı olmadığı bir senaryoda, varsayılan değer olarak bir parametre eklersiniz. Bu değer parantez içinde ve büyük harfle kapatılır. Bu örnek için, aşağıdaki varsayılan ortam değişkenini ayarlarsınız:

```
    ServerIPAddress = <MY_SERVER_IP_ADDRESS>
```

#### <a name="configuration-documentation"></a>Yapılandırma belgeleri

Bir IoT Edge modülünün tüm yapılandırma ayarları açıkça belgelenmelidir (rotalarının nasıl kullanılacağı, ikiz istenilen özellikler, ortam değişkenleri, createOptions vb.) Belgelerinize bir bağlantı sağlayın veya belgeler teklif/sku açıklamanızın bir parçası olmalıdır.

### <a name="tags-and-versioning"></a>Etiketler ve sürüm

Müşteriler kolayca bir modül dağıtmak ve otomatik olarak pazardan güncellemeleri almak gerekir (bir geliştirici senaryosunda.) Ayrıca test ettikleri tam sürümü kullanabilmeli ve dondurabilmeli (üretim senaryosunda.)

Bu müşteri beklentilerini karşılamak ve pazarda yayınlanacak, IoT Edge modülleri aşağıdaki gereksinimleri karşılamalıdır:

- Desteklenen tüm `latest` platformlarda en son sürümü işaret eden bir bildirim etiketi ekleyin.
- Sürüm etiketleri X, Y ve Z'nin tümseger olduğu X.Y.Z formunda olmalıdır.
- Desteklenen tüm platformlarda belirli `1.0.1`bir sürüme işaret eden bir "sürüm" etiketi ekleyin.
- Değişmez olmaları `1.0.1`gerektiğinden, "sürüm" etiketlerini güncellemeyin.

>[!Note]
>İsteğe bağlı olarak, sürüm gibi "yuvarlanan sürüm" etiketleri `2.0` içerebilir. `1.0` Bu, paralel olarak birden çok ana sürümü nidamı destekler.

### <a name="telemetry"></a>Telemetri

IoT Modülü SDK'sını kullanan modüller, telemetri `PublisherId.OfferId.SkuId` amacıyla benzersiz modül tanımlayıcısını ayarlamalıdır. Benzersiz bir tanımlayıcı, Azure Marketi'nin çalışan modül örneklerinin sayısını belirlemesini sağlar.

 Bu tanımlayıcıyı ayarlamak `ProductInfo` için IoT Modülü SDK'larından aşağıdaki yöntemleri kullanın:

- [C\#](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.productinfo?view=azure-dotnet#Microsoft_Azure_Devices_Client_DeviceClient_ProductInfo) 
- [C](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
- [Python](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
- [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.productinfo?view=azure-java-stable)

IoT Modülü SDK'sını kullanmayan modüller için, indirme sayısı gibi Bulut İş Ortağı Portalı üzerinden daha az kesin öngörüler mevcuttur.

### <a name="security"></a>Güvenlik

IoT Edge modülleri ana bilgisayara mümkün olduğunca en az ayrıcalıklı erişimi istemelidir. [Ayrıcalıklı modüllerden](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) kaçınılmalıdır.

### <a name="module-iot-sdk"></a>Modül IoT SDK

IoT Modülü SDK dahil sertifikasyon için bir ön koşul değildir. Ancak, IoT Modülü SDK dahil olmak üzere daha iyi bir kullanıcı deneyimi sağlayabilir. Örneğin, Bulut'a yönlendirmeyi veya ileti göndermeyi desteklemek için.

IoT Modülü SDK çalışan modül örneklerinin sayısı hakkında telemetri verileri almak için gereklidir.


## <a name="recertification-process"></a>Resertifikasyon süreci

<!-- Add legal time windows-->
Ortaklar, modüllerini etkileyen bir kırılma olduğunda, örneğin:

- IoT Edge tarafından desteklenen Tier 1 os/arch destek matrisi
- IoT Modülü SDK
- IoT Edge Çalışma Süresi
- IoT Edge modülü sertifika yönergeleri

İş ortaklarının modüllerini güncellemeleri ve Bulut İş Ortağı Portalı aracını kullanarak bunları yeniden onaylamaları gerekir.

## <a name="host-your-iot-edge-module-in-an-azure-container-registry"></a>IoT Edge modülünüzü Azure Kapsayıcı Kayıt Defteri'nde barındırın

IoT Edge modülünüzü Bulut İş Ortağı Portalı'na yüklemek için öncelikle bir [Azure Kapsayıcı Kayıt Defteri'nde](https://azure.microsoft.com/services/container-registry/) (ACR) barındırmanız gerekir. Modül, bir bildirim etiketiyle başvurulan resim etiketleri de dahil olmak üzere yayımlamak istediğiniz tüm etiketleri içermelidir.


## <a name="next-steps"></a>Sonraki adımlar

- [IoT Edge modül teklifinizi oluşturun](./cpp-create-offer.md)
