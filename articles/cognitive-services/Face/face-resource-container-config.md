---
title: Kapsayıcıları yapılandırma - Yüz
titleSuffix: Azure Cognitive Services
description: Face kapsayıcı çalışma zamanı ortamı komut `docker run` bağımsız değişkenleri kullanılarak yapılandırılır. Hem gerekli hem de isteğe bağlı ayarlar vardır.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 11/07/2019
ms.author: dapine
ms.openlocfilehash: 3c78c9eb85c3a8be236be5c3a24bd877db204b6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76167971"
---
# <a name="configure-face-docker-containers"></a>Yüz Docker kaplarını yapılandırın

**Face** kapsayıcı çalışma zamanı ortamı komut `docker run` bağımsız değişkenleri kullanılarak yapılandırılır. Bu kapsayıcı birkaç isteğe bağlı ayarları ile birlikte birkaç gerekli ayarları vardır. Komutun birkaç [örneği](#example-docker-run-commands) mevcuttur. Kapsayıcıya özgü ayarlar fatura ayarlarıdır. 

## <a name="configuration-settings"></a>Yapılandırma ayarları

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting)ve [`Eula`](#eula-setting) ayarlar birlikte kullanılır ve her üçü için de geçerli değerler sağlamanız gerekir; aksi takdirde konteyneriniz çalışmayabaşlamaz. Bir kapsayıcıyı anında kullanmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için [Faturalandırma'ya](face-how-to-install-containers.md#billing)bakın.

## <a name="apikey-configuration-setting"></a>ApiKey yapılandırma ayarı

Ayar, `ApiKey` kapsayıcının fatura bilgilerini izlemek için kullanılan Azure kaynak anahtarını belirtir. ApiKey için bir değer belirtmeniz gerekir ve değer [`Billing`](#billing-configuration-setting) yapılandırma ayarı için belirtilen Bilişsel _Hizmetler_ kaynağı için geçerli bir anahtar olmalıdır.

Bu ayar aşağıdaki yerde bulunabilir:

* Azure portalı: **Bilişsel Hizmetler** Kaynak Yönetimi, **Anahtarlar** altında

## <a name="applicationinsights-setting"></a>ApplicationInsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Faturalandırma yapılandırma ayarı

Ayar, `Billing` kapsayıcının fatura bilgilerini ölçmede kullanılan _Azure'daki Bilişsel Hizmetler_ kaynağının bitiş noktası URI'yi belirtir. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve değer Azure'daki _Bilişsel Hizmetler_ kaynağı için geçerli bir bitiş noktası URI olmalıdır. Kapsayıcı her 10 ila 15 dakikada bir kullanımı bildirir.

Bu ayar aşağıdaki yerde bulunabilir:

* Azure portalı: **Bilişsel Hizmetlere** Genel Bakış, etiketli`Endpoint`

Örnekte gösterildiği _gibi, Yüz_ yönlendirmesini bitiş noktası URI'ye eklemeyi unutmayın. 

|Gerekli| Adı | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Evet| `Billing` | Dize | Faturalandırma uç noktası URI. FaturaLAMA URI'si edinme hakkında daha fazla bilgi için [gerekli parametreleri toplamaya](face-how-to-install-containers.md#gathering-required-parameters)bakın. Daha fazla bilgi ve bölgesel uç noktaların tam listesi [için, Bilişsel Hizmetler için Özel alt alan adları bölümüne](../cognitive-services-custom-subdomains.md)bakın. |

<!-- specific to face only -->

## <a name="cloudai-configuration-settings"></a>CloudAI yapılandırma ayarları

Bölümdeki `CloudAI` yapılandırma ayarları, kapsayıcınıza özgü kapsayıcıya özgü seçenekler sağlar. Aşağıdaki ayarlar ve `CloudAI` nesneler, bölümdeki Yüz kapsayıcısı için desteklenir

| Adı | Veri türü | Açıklama |
|------|-----------|-------------|
| `Storage` | Nesne | Yüz kapsayıcısı tarafından kullanılan depolama senaryosu. `Storage` Nesne için depolama senaryoları ve ilişkili ayarlar hakkında daha fazla bilgi için [bkz.](#storage-scenario-settings) |

### <a name="storage-scenario-settings"></a>Depolama senaryo ayarları

Yüz kapsayıcısı, depolanan lara bağlı olarak blob, önbellek, meta veri ve sıra verilerini depolar. Örneğin, büyük bir kişi grubunun eğitim dizinleri ve sonuçları blob verisi olarak depolanır. Yüz kapsayıcısı, bu tür verilerle etkileşimde ve depolamasırasında iki farklı depolama senaryosu sağlar:

* Bellek  
  Dört veri türü de bellekte depolanır. Dağıtılmadı, ısrarda da değiller. Yüz kapsayıcısı durdurulur veya kaldırılırsa, bu kapsayıcının depolama sındaki tüm veriler yok edilir.  
  Bu, Yüz kapsayıcısı için varsayılan depolama senaryosudur.
* Azure  
  Yüz kapsayıcısı, bu dört veri türünü kalıcı depolama alanına dağıtmak için Azure Depolama ve Azure Cosmos DB'yi kullanır. Blob ve sıra verileri Azure Depolama tarafından işlenir. Meta veriler ve önbellek verileri Azure Cosmos DB tarafından işlenir. Yüz kapsayıcısı durdurulur veya kaldırılırsa, söz sözlegelen kapsayıcının depolama sındaki tüm veriler Azure Depolama ve Azure Cosmos DB'de depolanır.  
  Azure depolama senaryosu tarafından kullanılan kaynaklar aşağıdaki ek gereksinimlere sahiptir
  * Azure Depolama kaynağı StorageV2 hesap türünü kullanmalıdır
  * Azure Cosmos DB kaynağı, MongoDB için Azure Cosmos DB'nin API'sini kullanmalıdır

Depolama senaryoları ve ilişkili yapılandırma `Storage` `CloudAI` ayarları, yapılandırma bölümü altında nesne tarafından yönetilir. Nesnede `Storage` aşağıdaki yapılandırma ayarları mevcuttur:

| Adı | Veri türü | Açıklama |
|------|-----------|-------------|
| `StorageScenario` | Dize | Kapsayıcı tarafından desteklenen depolama senaryosu. Aşağıdaki değerler kullanılabilir<br/>`Memory`- Varsayılan değer. Kapsayıcı, tek düğümlü, geçici kullanım için kalıcı olmayan, dağıtılmamış ve bellek içi depolama kullanır. Kapsayıcı durdurulur veya kaldırılırsa, bu kapsayıcının deposu yok edilir.<br/>`Azure`- Kapsayıcı depolama için Azure kaynaklarını kullanır. Kapsayıcı durdurulur veya kaldırılırsa, bu kapsayıcının depolama alanı kalıcıdır.|
| `ConnectionStringOfAzureStorage` | Dize | Kapsayıcı tarafından kullanılan Azure Depolama kaynağının bağlantı dizesi.<br/>Bu ayar yalnızca `Azure` yapılandırma ayarı `StorageScenario` için belirtilmişse geçerlidir. |
| `ConnectionStringOfCosmosMongo` | Dize | Kapsayıcı tarafından kullanılan Azure Cosmos DB kaynağıiçin MongoDB bağlantı dizesi.<br/>Bu ayar yalnızca `Azure` yapılandırma ayarı `StorageScenario` için belirtilmişse geçerlidir. |

Örneğin, aşağıdaki komut Azure depolama senaryosunu belirtir ve Yüz kapsayıcısı için veri depolamak için kullanılan Azure Depolama ve Cosmos DB kaynakları için örnek bağlantı dizeleri sağlar.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 CloudAI:Storage:StorageScenario=Azure CloudAI:Storage:ConnectionStringOfCosmosMongo="mongodb://samplecosmosdb:0123456789@samplecosmosdb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb" CloudAI:Storage:ConnectionStringOfAzureStorage="DefaultEndpointsProtocol=https;AccountName=sampleazurestorage;AccountKey=0123456789;EndpointSuffix=core.windows.net"
  ```

Depolama senaryosu giriş bağlarından ve çıktı bağlarından ayrı olarak işlenir. Tek bir kapsayıcı için bu özelliklerin bir birleşimini belirtebilirsiniz. Örneğin, aşağıdaki komut, çıkış yuvası olarak ana `D:\Output` bilgisayar makinesindeki klasöre bir Docker bağlama yuvası tanımlar, ardından Yüz kapsayıcı görüntüsünden bir kapsayıcıanlık olarak JSON formatında günlük dosyalarını çıkış yuvasına kaydeder. Komut ayrıca Azure depolama senaryosunu belirtir ve Yüz kapsayıcısı için veri depolamak için kullanılan Azure Depolama ve Cosmos DB kaynakları için örnek bağlantı dizeleri sağlar.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 --mount type=bind,source=D:\Output,destination=/output containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 Logging:Disk:Format=json CloudAI:Storage:StorageScenario=Azure CloudAI:Storage:ConnectionStringOfCosmosMongo="mongodb://samplecosmosdb:0123456789@samplecosmosdb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb" CloudAI:Storage:ConnectionStringOfAzureStorage="DefaultEndpointsProtocol=https;AccountName=sampleazurestorage;AccountKey=0123456789;EndpointSuffix=core.windows.net"
  ```

## <a name="eula-setting"></a>Eula ayarı

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Akıcı ayarlar

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>Http proxy kimlik bilgileri ayarları

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Oturum açma ayarları
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]

## <a name="mount-settings"></a>Montaj ayarları

Kapsayıcıya ve kapsayıcıdan veri okumak ve yazmak için bağlama bağlarını kullanın. Docker `--mount` [run](https://docs.docker.com/engine/reference/commandline/run/) komutundaki seçeneği belirterek bir giriş yuvası veya çıktı montajı belirtebilirsiniz.

Yüz kapları, eğitim veya hizmet verilerini depolamak için giriş veya çıktı montajları kullanmaz. 

Ana bilgisayar montaj konumunun tam sözdizimi ana bilgisayar işletim sistemine bağlı olarak değişir. Ayrıca, docker hizmet hesabı tarafından kullanılan izinler ile ana bilgisayar montaj konum izinleri arasındaki bir çakışma nedeniyle [ana bilgisayarın](face-how-to-install-containers.md#the-host-computer)montaj konumuna erişilemeyebilir. 

|İsteğe bağlı| Adı | Veri türü | Açıklama |
|-------|------|-----------|-------------|
|İzin verilmiyor| `Input` | Dize | Yüz kapları bunu kullanmaz.|
|İsteğe bağlı| `Output` | Dize | Çıkış montaj hedefi. Varsayılan değer: `/output`. Bu günlüklerin yeridir. Buna kapsayıcı günlükleri de dahildir. <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Örnek docker çalıştır komutları 

Aşağıdaki örnekler, komutların nasıl yazılabildiğini `docker run` ve kullanılacağını göstermek için yapılandırma ayarlarını kullanır.  Bir kez çalışırken, kapsayıcı [onu durdurunkadar](face-how-to-install-containers.md#stop-the-container) çalışmaya devam ediyor.

* **Satır devamı karakteri**: Aşağıdaki bölümlerdeki Docker komutları, `\`çizgi devamı karakteri olarak arka çizgiyi kullanır. Ana bilgisayar işletim sisteminizin gereksinimlerine göre bunu değiştirin veya kaldırın. 
* **Bağımsız değişken sırası**: Docker kapsayıcılarını çok iyi bilmediğiniz sürece bağımsız değişkenlerin sırasını değiştirmeyin.

{_argument_name_} 'yi kendi değerlerinizle değiştirin:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
| **{API_KEY}** | Azure `Face` Tuşları sayfasındaki `Face` kaynağın bitiş noktası anahtarı. | `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` |
| **{ENDPOINT_URI}** | Faturalandırma bitiş noktası değeri Azure `Face` Genel Bakış sayfasında kullanılabilir.| Açık örnekler için [gerekli parametreleri toplamaya](face-how-to-install-containers.md#gathering-required-parameters) bakın. |

[!INCLUDE [subdomains-note](../../../includes/cognitive-services-custom-subdomains-note.md)]

> [!IMPORTANT]
> `Eula`Kapsayıcıyı `Billing`çalıştırmak `ApiKey` için , ve seçenekler belirtilmelidir; aksi takdirde, kapsayıcı başlamaz.  Daha fazla bilgi için [Faturalandırma'ya](face-how-to-install-containers.md#billing)bakın.
> ApiKey değeri, Azure `Cognitive Services` Kaynak anahtarları sayfasındaki **Anahtardır.** 

## <a name="face-container-docker-examples"></a>Yüz konteyner Docker örnekleri

Aşağıdaki Docker örnekleri yüz konteyneri içindir. 

### <a name="basic-example"></a>Temel örnek 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={ENDPOINT_URI} \
  ApiKey={API_KEY} 
  ```

### <a name="logging-example"></a>Günlük örneği 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={ENDPOINT_URI} ApiKey={API_KEY} \
  Logging:Console:LogLevel:Default=Information
  ```

## <a name="next-steps"></a>Sonraki adımlar

* [Kapsayıcıların nasıl yüklenir ve çalıştırılabildiğini](face-how-to-install-containers.md) gözden geçirin
