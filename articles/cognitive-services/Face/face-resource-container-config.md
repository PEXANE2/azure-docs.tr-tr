---
title: Kapsayıcıları yapılandırma-yüz tanıma API 'SI
titleSuffix: Azure Cognitive Services
description: Kapsayıcılar için yapılandırma ayarları.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: dapine
ms.openlocfilehash: 752613becb92711866d520e6fcd46ed3a320353f
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70860263"
---
# <a name="configure-face-docker-containers"></a>Yüz Docker kapsayıcılarını yapılandırma

**Yüz** kapsayıcı çalışma zamanı ortamı, `docker run` komut bağımsız değişkenleri kullanılarak yapılandırılır. Bu kapsayıcıda bazı gerekli ayarlar ve bazı isteğe bağlı ayarlar vardır. Birkaç [örnekler](#example-docker-run-commands) komutu kullanılabilir. Kapsayıcıya özgü ayarlar faturalandırma ayarlardır. 

## <a name="configuration-settings"></a>Yapılandırma ayarları

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), Ve [ `Eula` ](#eula-setting) ayarları birlikte kullanılır ve bunları; Aksi takdirde, tüm üç için geçerli değerler sağlamanız gerekir kapsayıcınızı başlatılamıyor. Bir kapsayıcı örneği oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalama](face-how-to-install-containers.md#billing).

## <a name="apikey-configuration-setting"></a>ApiKey yapılandırma ayarı

`ApiKey` Ayar kapsayıcısı için fatura bilgileri izlemek için kullanılan Azure kaynak anahtarını belirtir. Apikey için bir değer belirtmeniz gerekir ve değerin [`Billing`](#billing-configuration-setting) yapılandırma ayarı için belirtilen bilişsel _Hizmetler_ kaynağı için geçerli bir anahtar olması gerekir.

Bu ayar aşağıdaki yerde bulunabilir:

* Azure portal: Bilişsel **Hizmetler** Kaynak yönetimi, **anahtarlar** altında

## <a name="applicationinsights-setting"></a>Applicationınsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Yapılandırma ayarı faturalama

Ayar, Azure üzerinde bulunan bilişsel hizmetler kaynağının, kapsayıcının fatura bilgilerini ölçmek için kullanılan uç nokta URI 'sini belirtir. `Billing` Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve Azure 'daki bilişsel _Hizmetler_ kaynağı için değer geçerli bir uç nokta URI 'si olmalıdır. Kapsayıcı her 10 ila 15 dakikada bir kullanım raporu sağlar.

Bu ayar aşağıdaki yerde bulunabilir:

* Azure portal: Bilişsel **Hizmetler** Genel bakış, etiketli`Endpoint`

_Yüz_ yönlendirmeyi, örnekte gösterildiği gibi uç nokta URI 'sine eklemeyi unutmayın. 

|Gerekli| Ad | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Evet| `Billing` | Dize | Faturalandırma uç noktası URI'si<br><br>Örnek:<br>`Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0` |

<!-- specific to face only -->

## <a name="cloudai-configuration-settings"></a>CloudAI yapılandırma ayarları

Yapılandırma ayarlarında `CloudAI` bölümü kapsayıcınıza benzersiz kapsayıcı özgü seçenekler sağlar. Aşağıdaki ayarları ve nesneleri yüz kapsayıcıda desteklenir `CloudAI` bölümü

| Ad | Veri türü | Açıklama |
|------|-----------|-------------|
| `Storage` | Nesne | Yüz tanıma kapsayıcı tarafından kullanılan depolama senaryo. Depolama senaryoları ve için ilişkili ayarları hakkında daha fazla bilgi için `Storage` nesne, bkz: [depolama senaryo ayarları](#storage-scenario-settings) |

### <a name="storage-scenario-settings"></a>Depolama senaryosu ayarları

Yüz tanıma kapsayıcı, blob, önbellek, meta verileri ve sıra veri, ne saklanan bağlı olarak depolar. Örneğin, eğitim dizinleri ve sonuçları bir büyük kişi grubu için blob veri depolanır. Yüz tanıma kapsayıcı ile etkileşim kurulurken iki farklı depolama senaryoları ve bu tür verilerin depolama sağlar:

* Bellek  
  Tüm dört veri türlerini bellekte depolanır. Dağılımı ya da bunların kalıcıdır. Yüz tanıma kapsayıcı durdurulmuş veya kaldırılmış, tüm bu kapsayıcı için depolama birimindeki verileri bozulur.  
  Yüz tanıma kapsayıcısı için varsayılan depolama senaryosu budur.
* Azure  
  Yüz tanıma kapsayıcı, bu dört veri türleri arasında kalıcı depolama dağıtmak için Azure depolama ve Azure Cosmos DB kullanır. BLOB ve kuyruk verileri Azure Depolama tarafından işlenir. Meta veriler ve önbellek verileri Azure Cosmos DB tarafından işlenir. Yüz tanıma kapsayıcı durdurulmuş veya kaldırılmış, tüm veriler bu kapsayıcı için depolama alanında kalır Azure depolama ve Azure Cosmos DB içinde depolanan.  
  Azure depolama senaryo tarafından kullanılan kaynakları aşağıdaki ek gereksinimlere sahiptir.
  * Azure depolama kaynağı StorageV2 hesap türü kullanmalısınız.
  * Azure Cosmos DB kaynağı MongoDB için Azure Cosmos DB API 'sini kullanmalıdır

Tarafından yönetilen depolama senaryolar ve ilişkili yapılandırma ayarları `Storage` altında nesne `CloudAI` yapılandırma bölümü. Aşağıdaki yapılandırma ayarları kullanılabilir `Storage` nesnesi:

| Ad | Veri türü | Açıklama |
|------|-----------|-------------|
| `StorageScenario` | Dize | Kapsayıcı tarafından desteklenen depolama senaryo. Aşağıdaki değerler kullanılabilir<br/>`Memory` -Varsayılan değer. Kapsayıcı, tek bir düğüm, geçici kullanım için kalıcı olmayan, olmayan dağıtılmış ve bellek içi depolama kullanır. Bu kapsayıcı için depolama kapsayıcısı durdurulmuş veya kaldırılmış olmadığını yok edilir.<br/>`Azure` -Kapsayıcı, depolama için Azure kaynaklarını kullanır. Bu kapsayıcı için depolama kapsayıcısı durdurulmuş veya kaldırılmış olmadığını kalıcı hale getirilir.|
| `ConnectionStringOfAzureStorage` | Dize | Kapsayıcı tarafından kullanılan Azure depolama kaynağı için bağlantı dizesi.<br/>Bu ayar, yalnızca aşağıdaki durumlarda geçerlidir `Azure` için belirtilen `StorageScenario` yapılandırma ayarı. |
| `ConnectionStringOfCosmosMongo` | Dize | Kapsayıcı tarafından kullanılan Azure Cosmos DB kaynak için MongoDB bağlantı dizesi.<br/>Bu ayar, yalnızca aşağıdaki durumlarda geçerlidir `Azure` için belirtilen `StorageScenario` yapılandırma ayarı. |

Örneğin, aşağıdaki komutu, Azure depolama senaryosu belirtir ve örnek bağlantı dizeleri yüz kapsayıcı verilerini depolamak için kullanılan Azure depolama ve Cosmos DB kaynakları sağlar.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 CloudAI:Storage:StorageScenario=Azure CloudAI:Storage:ConnectionStringOfCosmosMongo="mongodb://samplecosmosdb:0123456789@samplecosmosdb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb" CloudAI:Storage:ConnectionStringOfAzureStorage="DefaultEndpointsProtocol=https;AccountName=sampleazurestorage;AccountKey=0123456789;EndpointSuffix=core.windows.net"
  ```

Depolama senaryosu alanından ayrı olarak yönetilir takar giriş ve çıkış takar. Tek bir kapsayıcı için bu özellikleri birleşimi belirtebilirsiniz. Örneğin, aşağıdaki komutu için Docker bağlama bağlama tanımlar `D:\Output` klasörü çıkış bağlama olarak konak makinedeki sonra JSON biçiminde çıktı bağlama için günlük dosyalarını kaydetme yüz kapsayıcı görüntüsünden bir kapsayıcı oluşturur. Komut ayrıca Azure depolama senaryosu belirtir ve örnek bağlantı dizeleri yüz kapsayıcı verilerini depolamak için kullanılan Azure depolama ve Cosmos DB kaynakları sağlar.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 --mount type=bind,source=D:\Output,destination=/output containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 Logging:Disk:Format=json CloudAI:Storage:StorageScenario=Azure CloudAI:Storage:ConnectionStringOfCosmosMongo="mongodb://samplecosmosdb:0123456789@samplecosmosdb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb" CloudAI:Storage:ConnectionStringOfAzureStorage="DefaultEndpointsProtocol=https;AccountName=sampleazurestorage;AccountKey=0123456789;EndpointSuffix=core.windows.net"
  ```

## <a name="eula-setting"></a>EULA'yı ayarlama

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Fluentd ayarları

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>Http proxy kimlik bilgileri ayarları

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Günlük ayarları
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]

## <a name="mount-settings"></a>Bağlama ayarları

Kullanım bağlama okumak ve kapsayıcı gelen ve giden veri yazmak için bağlar. Bir giriş bağlama belirtin veya çıkış bağlama belirterek `--mount` seçeneğini [docker run](https://docs.docker.com/engine/reference/commandline/run/) komutu.

Yüz kapsayıcıları, eğitim veya hizmet verilerini depolamak için giriş veya çıkış taklarını kullanmaz. 

Konak bağlama konumu söz dizimi konak işletim sistemine göre değişir. Ayrıca, Docker hizmeti hesabı ve konak bağlama konumu izinleri tarafından kullanılan izinler arasındaki bir çakışma nedeniyle [ana bilgisayarın](face-how-to-install-containers.md#the-host-computer)bağlama konumu erişilebilir olmayabilir. 

|İsteğe Bağlı| Name | Veri türü | Açıklama |
|-------|------|-----------|-------------|
|İzin verilmedi| `Input` | Dize | Yüz kapsayıcıları bunu kullanmaz.|
|İsteğe Bağlı| `Output` | Dize | Çıkış bağlama hedefi. Varsayılan değer `/output` şeklindedir. Bu günlükler konumdur. Bu, kapsayıcı günlüklerini içerir. <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Örnek docker komutlarını çalıştırın 

Aşağıdaki örnekler, yazma ve kullanma göstermek için yapılandırma ayarlarını kullanır. `docker run` komutları.  Kapsayıcıyı çalıştıran sonra dek çalıştırmaya devam [Durdur](face-how-to-install-containers.md#stop-the-container) bu.

* **Satır devamlılık karakteri**: Aşağıdaki bölümlerdeki Docker komutları, satır devamlılık karakteri olarak ters eğik `\`çizgi kullanır. Bu konak işletim sisteminin gereksinimlerine göre kaldırın veya değiştirin. 
* **Bağımsız değişken sırası**: Docker Kapsayıcıları hakkında bilginiz yoksa bağımsız değişkenlerin sırasını değiştirmeyin.

Yerine {_argument_name_} kendi değerlerinizle:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
|{API_KEY} | Bilişsel hizmetler kaynağının bitiş noktası anahtarı. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{ENDPOINT_URI} | Uç nokta URL değeri.|`https://myresourcename.cognitive.microsoft.com/face/v1.0`|

[!INCLUDE [subdomains-note](../../../includes/cognitive-services-custom-subdomains-note.md)]

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](face-how-to-install-containers.md#billing).
> Apikey değeri, Azure `Cognitive Services` kaynak anahtarları sayfasından alınan **anahtardır** . 

## <a name="face-container-docker-examples"></a>Yüz kapsayıcı Docker örnekleri

Aşağıdaki Docker örnekleri, yüz kapsayıcısı için verilmiştir. 

### <a name="basic-example"></a>Temel örnek 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={ENDPOINT_URI} \
  ApiKey={API_KEY} 
  ```

### <a name="logging-example"></a>Günlüğe kaydetme örneği 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={ENDPOINT_URI} ApiKey={API_KEY} \
  Logging:Console:LogLevel:Default=Information
  ```

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [yükleme ve kapsayıcıları çalıştırın](face-how-to-install-containers.md)
