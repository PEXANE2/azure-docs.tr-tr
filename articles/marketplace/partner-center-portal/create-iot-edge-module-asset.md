---
title: IoT Edge modülü teknik varlıklarınızı hazırlayın - Azure Marketi
description: Azure Marketi'nde yayınlamadan önce Nesnelerin İnterneti (IoT) Edge modülü teknik varlıklarınızın karşılaması gereken teknik ve yapılandırma gereksinimleri hakkında bilgi edinin.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/03/2020
ms.openlocfilehash: e83c70424c131e5324192b07e65321d63bf06e2e
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80674221"
---
# <a name="prepare-your-iot-edge-module-technical-assets"></a>IoT Edge modülü teknik varlıklarınızı hazırlayın

> [!IMPORTANT]
> IoT Edge modül tekliflerinizi Bulut İş Ortağı Portalı'ndan İş Ortağı Merkezi'ne taşıyoruz. Tekliflerin izlene kadar, lütfen [IoT Edge modülü teknik varlıklarınızı](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-create-technical-assets) Bulut İş Ortağı Portalı için hazırlayın'daki talimatları uygulayın.

Bu makalede, Nesnelerin İnterneti (IoT) Edge modülü teknik varlıklarının Azure Marketi'nde yayımlanmadan önce karşılaması gereken gereksinimler açıklanmaktadır.

## <a name="get-started"></a>başlarken

IoT Edge modülü, Docker uyumlu bir kapsayıcıdır ve IoT Edge aygıtında çalışır.

- IoT Edge modülleri hakkında daha fazla bilgi edinmek için Azure [IoT Edge modüllerini anlayın.](https://aka.ms/UnderstandAzureIoTEdgemodules)
- IoT Edge modül geliştirmenize başlamak için [bkz.](https://aka.ms/DevelopyourownIoTEdgemodules)

## <a name="technical-requirements"></a>Teknik gereksinimler

IoT Edge modülünüz, Azure Marketi'nde onaylanmak ve yayımlanmak için aşağıdaki teknik gereksinimleri karşılamalıdır.

### <a name="platform-support"></a>Platform desteği

IoT Edge modülünüz aşağıdaki platform seçeneklerinden birini desteklemelidir:

#### <a name="tier-1-platforms-supported-by-iot-edge"></a>IoT Edge tarafından desteklenen Tier 1 platformları

Modülünüz, IoT Edge tarafından desteklenen tüm Tier 1 platformlarını desteklemelidir [(Azure IoT Edge desteğinde](https://aka.ms/AzureIoTEdgesupport)kaydedildi). Daha iyi bir müşteri deneyimi sağladığı ndan bu seçeneği öneririz. Bu kriterleri karşılayan modüller sergilenecektir. Bu platform seçeneğini kullanan bir modül şunları yapmalı:

- [GitHub Bildirim aracıyla](https://aka.ms/GitHubmanifest-tool)oluşturulmuş manifesto etiketleri olan en son etiket ve sürüm etiketi (örneğin, 1.0.1) sağlayın.

- [Azure IoT Edge Sertifikalı aygıt kataloğuna](https://catalog.azureiotsolutions.com/) **Yararlı bağlantılar** bölümüne bir bağlantı eklemek için [İş Merkezi'ndeki](https://partner.microsoft.com/dashboard/commercial-marketplace) teklif listeleme sekmesini kullanın. Azure https://aka.ms/iot-edge-certified [IoT Edge Aygıt Kataloğu'na](https://catalog.azureiotsolutions.com/)giderilen yeniden yönlendirme bağlantısını kullanabilirsiniz.

#### <a name="a-subset-of-tier-1-platforms-supported-by-iot-edge"></a>IoT Edge tarafından desteklenen Tier 1 platformlarının bir alt kümesi

Modülünüz, IoT Edge tarafından desteklenen Tier 1 platformlarının bir alt kümesini (en az bir) desteklemelidir [(Azure IoT Edge desteğinde](https://aka.ms/AzureIoTEdgesupport)kaydedildi). Bu platform seçeneğini kullanan bir modül şunları yapmalı:

- Birden fazla platform desteklenirse, GitHub [bildirim aracıyla](https://github.com/estesp/manifest-tool) oluşturulmuş bildirim etiketleri olan en son bir etiket ve sürüm etiketi (örneğin, 1.0.1) sağlayın. Bildirim etiketleri yalnızca bir platform desteklendiğinde isteğe bağlıdır.
- [Azure IoT Edge Sertifikalı aygıt kataloğundan](https://catalog.azureiotsolutions.com/)en az bir IoT Edge aygıtına **Yararlı bağlantılar** bölümüne bağlantı eklemek için [İş Merkezi'ndeki](https://partner.microsoft.com/dashboard/commercial-marketplace) teklif kaydı sekmesini kullanın.

:::image type="content" source="media/iot-edge-module-technical-assets-offer-listing.png" alt-text="Bu, İş Ortağı Merkezi'ndeki Teklif Listeleme bölümünün bir resmidir":::

### <a name="device-dimensions"></a>Cihaz boyutları

Hedeflenen IoT Edge aygıtlarında IoT Edge modül boyutları (CPU, RAM, depolama ve GPU gibi) aşağıdaki gereksinimleri karşılamalıdır:

- Modül, [Azure IoT Edge Sertifikalı aygıt kataloğundan](https://catalog.azureiotsolutions.com/)en az bir IoT Edge aygıtıyla çalışmalıdır.

- Minimum donanım gereksinimleri, teklifin açıklamasındaki son paragraf olarak belgelenmelidir [(İş Ortağı Merkezi'ndeki](https://partner.microsoft.com/dashboard/commercial-marketplace)teklif listesi sekmesi altında). İsteğe bağlı olarak, önerilen donanım gereksinimlerini önemli ölçüde farklıysa da listeleyebilirsiniz. Örneğin, teklif açıklamanızın sonuna aşağıdaki bölümü ekleyin:

Bu HTML metnini kopyalayın veya düzenleme penceresinde karşılık gelen zengin metin işlevlerini kullanın.

```html
<p><u>Minimum hardware requirements:</u> Linux x64 and arm32 OS, 1GB of RAM, 500 Mb of storage</p>
```

### <a name="configuration"></a>Yapılandırma

Bir IoT Edge aygıtına dağıtımı olabildiğince basit hale getirmek için modülünüz varsayılan yapılandırma ayarlarını içermelidir. Bu [bilgiler, Ortak Merkezi'ndeki](https://partner.microsoft.com/dashboard/commercial-marketplace)plan için **Teknik yapılandırma** sayfasında sağlanabilir. Kapsayıcı, kenar Hub'ı ve IoT Hub ile iletişimi etkinleştirmek için IoT Edge Modülü SDK'yı da içerebilir.

#### <a name="default-configuration"></a>Varsayılan yapılandırma

IoT Edge [modülleri, İş Ortağı Merkezi'ndeki](https://partner.microsoft.com/dashboard/commercial-marketplace)plan için **Teknik yapılandırma** sayfasında sağlanan varsayılan ayarlarla başlayabilmelidir. Aşağıdaki varsayılan ayarlar kullanılabilir:

- Varsayılan **rotalar**
- Varsayılan **modül ikiz istenilen özellikler**
- Varsayılan **ortam değişkenleri**
- Varsayılan **kapsayıcı seçenekleri oluşturmak**

Varsayılan değer için gerekli olan bir parametrenin (örneğin, müşterinin sunucusunun IP adresi) mantıklı olmadığı bir senaryoda, varsayılan değer olarak bir parametre ekleyin. Bu değer büyük harfle ve parantez içinde kapalıdır. Bu örnek için, aşağıdaki varsayılan ortam değişkenini ayarlarsınız:

```
ServerIPAddress = <MY_SERVER_IP_ADDRESS>
```

#### <a name="configuration-documentation"></a>Yapılandırma belgeleri

Bir IoT Edge modülünün tüm yapılandırma ayarları açıkça belgelenmelidir. Örneğin, yollarının, istenen ikiz özelliklerinin, ortam değişkenlerinin, createOptions'un nasıl kullanılacağını belgelemeniz gerekir. Belgelerinize bir bağlantı sağlamanız veya teklifinizin veya plan açıklamanızın bir parçası haline getirmeniz gerekir. Bu bilgileri [İş Ortağı Merkezi'ndeki](https://partner.microsoft.com/dashboard/commercial-marketplace) **Teklif listesi** ve **Plan listesi** sayfasında sağlayabilirsiniz.

#### <a name="tags-and-versioning"></a>Etiketler ve sürüm

Müşteriler kolayca bir modül dağıtmak ve otomatik olarak pazardan güncellemeleri almak gerekir (bir geliştirici senaryosunda). Ayrıca test ettikleri tam sürümü kullanabilmeli ve dondurabilmeli (üretim senaryosunda).

Bu müşteri beklentilerini karşılamak ve pazarda yayınlanacak, IoT Edge modülleri aşağıdaki gereksinimleri karşılamalıdır

- Desteklenen tüm platformlarda en son sürüme işaret eden bir en son etiket ekleyin.
- X, Y ve Z'nin tümseger olduğu X.Y.Z formunda sürüm etiketleri yapın.
- Desteklenen tüm platformlarda belirli bir sürüme işaret eden 1.0.1 gibi bir "sürüm" etiketi ekleyin.
- Değiştirilmemelidir, çünkü 1.0.1 gibi "sürüm" etiketlerini güncelleştirmeyin.

> [!NOTE]
> İsteğe bağlı olarak, sürüm 2.0 ve 1.0 gibi "yuvarlanan sürüm" etiketleri içerebilir. Bu, paralel olarak birden çok ana sürümü nidamı destekler.

### <a name="telemetry"></a>Telemetri

IoT Modülü SDK'yı kullanan modüller, telemetri amacıyla benzersiz modül tanımlayıcısını PublisherId.OfferId.SkuId'e ayarlamalıdır. Benzersiz bir tanımlayıcı, Azure Marketi'nin çalışan modül örneklerinin sayısını belirlemesine yardımcı olur.

ProductInfo'yu bu tanımlayıcıya ayarlamak için IoT Modülü SDK'larından aşağıdaki yöntemlerden birini kullanın:

- [C #](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.productinfo?view=azure-dotnet#Microsoft_Azure_Devices_Client_DeviceClient_ProductInfo)
- [C](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
- [Python](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
- [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.productinfo?view=azure-java-stable)

IoT Modülü SDK'sını kullanmayan modüller için, İndirme sayısı gibi İş Ortağı Merkezi aracılığıyla daha az kesin öngörüler mevcuttur.

### <a name="security"></a>Güvenlik

IoT Edge modülleri [ayrıcalıklı modüllerden](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities)kaçınmalıdır. Bunun yerine mümkün olduğunca ana bilgisayara en az ayrıcalıklı erişim isteyin.

### <a name="module-iot-sdk"></a>Modül IoT SDK

IoT Modülü SDK dahil sertifikasyon için bir ön koşul değildir. Ancak, IoT Modülü SDK dahil olmak üzere daha iyi bir kullanıcı deneyimi sağlayabilir. Örneğin, Bulut'a yönlendirmeyi veya ileti göndermeyi desteklemek için.

IoT Modülü SDK çalışan modül örneklerinin sayısı hakkında telemetri verileri almak için gereklidir.

## <a name="recertification-process"></a>Resertifikasyon süreci

Ortaklar, modüllerini etkileyen bir kırılma olduğunda, örneğin:

- IoT Edge tarafından desteklenen Tier 1 OS/arch destek matrisi
- IoT Modülü SDK
- IoT Edge çalışma zamanı
- IoT Edge modülü sertifika yönergeleri

İş ortakları, tekliflerini [Ortak Merkezi'nde](https://partner.microsoft.com/dashboard/commercial-marketplace)yeniden yayınlayarak tekliflerini güncellemeli ve yeniden düzenlemelidir.

Teklifiniz, yeni resim etiketleri eklemek gibi güncellerseniz de yeniden onaylanır.

## <a name="host-module-in-azure-container-registry"></a>Azure Konteyner Kayıt Defteri'nde ana bilgisayar modülü

IoT Edge modülünüzü Azure Marketi'ne yüklemek için öncelikle bir [Azure Kapsayıcı Kayıt Defteri'nde](https://azure.microsoft.com/services/container-registry/) (ACR) barındırmanız gerekir. Modül, bir bildirim etiketiyle başvurulan resim etiketleri de dahil olmak üzere yayımlamak istediğiniz tüm etiketleri içermelidir. Daha fazla bilgi için, öğreticiye bakın [bir Azure kapsayıcı kayıt defteri oluşturun ve bir kapsayıcı resmine itin.](https://docs.microsoft.com/azure/container-instances/container-instances-tutorial-prepare-acr)

## <a name="next-steps"></a>Sonraki adımlar

- [IoT Edge modül teklifinizi oluşturun](https://aka.ms/AzureCreateIoT)