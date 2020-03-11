---
title: Konuşma kapsayıcılarını yapılandırma
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti her kapsayıcıyı ortak bir yapılandırma çerçevesi sağlar; böylece, kapsayıcılarınız için depolama, günlüğe kaydetme ve telemetri ve güvenlik ayarlarını kolayca yapılandırabilir ve yönetebilirsiniz.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/09/2020
ms.author: dapine
ms.openlocfilehash: dd5a531e4a979cba9c2a766c7774762a0427ad02
ms.sourcegitcommit: b8d0d72dfe8e26eecc42e0f2dbff9a7dd69d3116
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79037317"
---
# <a name="configure-speech-service-containers"></a>Konuşma hizmeti kapsayıcılarını yapılandırma

Konuşma kapsayıcıları, müşterilerin hem güçlü bulut özellikleri hem de kenar yerinin avantajlarından yararlanmak için optimize edilmiş bir konuşma uygulaması mimarisi oluşturmasına imkan tanır. Artık destekduğumuz dört konuşma kapsayıcısı, **konuşmadan metne**, **özel konuşmadan metne**, **metinden konuşmaya**ve **özel metin okuma özelliğine**sahip.

**Konuşma** kapsayıcısı çalışma zamanı ortamı, `docker run` komutu bağımsız değişkenleri kullanılarak yapılandırılır. Bu kapsayıcıda bazı gerekli ayarlar ve bazı isteğe bağlı ayarlar vardır. Birkaç komuta [örnek](#example-docker-run-commands) vardır. Kapsayıcıya özgü ayarlar faturalandırma ayarlardır.

## <a name="configuration-settings"></a>Yapılandırma ayarları

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting)ve [`Eula`](#eula-setting) ayarları birlikte kullanılır ve üç tane için de geçerli değerler sağlamanız gerekir; Aksi takdirde Kapsayıcınız başlatılmaz. Bir kapsayıcı oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalandırma](speech-container-howto.md#billing).

## <a name="apikey-configuration-setting"></a>ApiKey yapılandırma ayarı

`ApiKey` ayarı, kapsayıcının fatura bilgilerini izlemek için kullanılan Azure Kaynak anahtarını belirtir. ApiKey için bir değer belirtmeniz gerekir ve değerin [`Billing`](#billing-configuration-setting) yapılandırma ayarı Için belirtilen _konuşma_ kaynağı için geçerli bir anahtar olması gerekir.

Bu ayar aşağıdaki yerde bulunabilir:

- Azure portal: **konuşma 'nın** kaynak yönetimi, **anahtarlar** altında

## <a name="applicationinsights-setting"></a>Applicationınsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Yapılandırma ayarı faturalama

`Billing` ayarı, Azure 'daki, kapsayıcının fatura bilgilerini ölçmek için kullanılan _konuşma_ kaynağının uç nokta URI 'sini belirtir. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve değer Azure 'da bir _konuşma_ kaynağı için geçerli bir uç nokta URI 'si olmalıdır. Kapsayıcı her 10 ila 15 dakikada bir kullanım raporu sağlar.

Bu ayar aşağıdaki yerde bulunabilir:

- Azure portal: **konuşmaya** genel bakış, etiketli `Endpoint`

| Gerekli | Adı | Veri türü | Açıklama |
| -------- | ---- | --------- | ----------- |
| Yes | `Billing` | Dize | Faturalama uç noktası URI 'SI. Faturalandırma URI 'sini alma hakkında daha fazla bilgi için bkz. [gerekli parametreleri toplama](speech-container-howto.md#gathering-required-parameters). Daha fazla bilgi ve bölgesel uç noktaların tamamen listesi için bkz. bilişsel [Hizmetler Için özel alt etki alanı adları](../cognitive-services-custom-subdomains.md). |

## <a name="eula-setting"></a>EULA'yı ayarlama

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Fluentd ayarları

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>HTTP proxy kimlik bilgileri ayarları

[!INCLUDE [Container shared HTTP proxy settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Günlük ayarları

[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]

## <a name="mount-settings"></a>Bağlama ayarları

Kullanım bağlama okumak ve kapsayıcı gelen ve giden veri yazmak için bağlar. [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunda `--mount` seçeneğini belirterek bir giriş bağlama veya çıkış bağlama belirtebilirsiniz.

Standart konuşma kapsayıcıları, eğitim veya hizmet verilerini depolamak için giriş veya çıkış taklarını kullanmaz. Ancak, özel konuşma kapsayıcıları, toplu takmaları kullanır.

Konak bağlama konumu söz dizimi konak işletim sistemine göre değişir. Ayrıca, Docker hizmeti hesabı ve konak bağlama konumu izinleri tarafından kullanılan izinler arasındaki bir çakışma nedeniyle [ana bilgisayarın](speech-container-howto.md#the-host-computer)bağlama konumu erişilebilir olmayabilir.

| İsteğe bağlı | Adı | Veri türü | Açıklama |
| -------- | ---- | --------- | ----------- |
| İzin verilmiyor | `Input` | Dize | Standart konuşma kapsayıcıları bunu kullanmaz. Özel konuşma kapsayıcıları, [birim bağlama](#volume-mount-settings)kullanır.                                                                                    |
| İsteğe bağlı | `Output` | Dize | Çıkış bağlama hedefi. Varsayılan değer: `/output`. Bu günlükler konumdur. Bu, kapsayıcı günlüklerini içerir. <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output` |

## <a name="volume-mount-settings"></a>Birim bağlama ayarları

Özel konuşma kapsayıcıları, özel modelleri kalıcı hale getirmek için [birim takmaları](https://docs.docker.com/storage/volumes/) kullanır. [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutuna `-v` (veya `--volume`) seçeneğini ekleyerek bir birim bağlama belirtebilirsiniz.

Özel modeller ilk kez, özel konuşma kapsayıcısı Docker Run komutunun bir parçası olarak yeni bir model eklendiğinde indirilir. Özel bir konuşma kapsayıcısı için aynı `ModelId` sıralı çalıştırmaları, daha önce indirilen modeli kullanır. Birim bağlama sağlanmazsa, özel modeller kalıcı olamaz.

Birim bağlama ayarı üç renkten `:` ayrılmış alandan oluşur:

1. İlk alan, ana makinedeki birimin adıdır, örneğin _C:\input_.
2. İkinci alan, kapsayıcıda Dizin, örneğin _/usr/local/modeller_.
3. Üçüncü alan (isteğe bağlı) virgülle ayrılmış seçenek listesidir, daha fazla bilgi için bkz. [birimleri kullanma](https://docs.docker.com/storage/volumes/).

### <a name="volume-mount-example"></a>Birim bağlama örneği

```bash
-v C:\input:/usr/local/models
```

Bu komut, _C:\ınput_ dizinini ana makine makinesini _/usr/local/modeller_ dizinine bağlar.

> [!IMPORTANT]
> Birim bağlama ayarları yalnızca **özel konuşma tanıma metin** ve **özel metin okuma** kapsayıcıları için geçerlidir. Standart **konuşmadan metne** ve **metinden konuşmaya** kapsayıcılar, toplu takmaları kullanmaz.

## <a name="example-docker-run-commands"></a>Örnek docker komutlarını çalıştırın

Aşağıdaki örnekler `docker run` komutlarının nasıl yazılacağını ve kullanılacağını göstermek için yapılandırma ayarlarını kullanır. Çalışan bir kez, kapsayıcıyı [durduruncaya](speech-container-howto.md#stop-the-container) kadar çalışmaya devam eder.

- **Satır devamlılık karakteri**: aşağıdaki bölümlerdeki Docker komutları, satır devamlılık karakteri olarak `\`ters eğik çizgi kullanır. Bu konak işletim sisteminin gereksinimlerine göre kaldırın veya değiştirin.
- **Bağımsız değişken sırası**: Docker Kapsayıcıları hakkında bilginiz yoksa bağımsız değişkenlerin sırasını değiştirmeyin.

{_Argument_name_} değerini kendi değerlerinizle değiştirin:

| Yer tutucu | Değer | Biçim veya örnek |
| ----------- | ----- | ----------------- |
| **{API_KEY}** | Azure `Speech` Keys sayfasında `Speech` kaynağının bitiş noktası anahtarı.   | `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`                                                                                  |
| **{ENDPOINT_URI}** | Faturalandırma uç noktası değeri, Azure `Speech` Genel Bakış sayfasında bulunur. | Açık örnekler için [gerekli parametreleri toplama](speech-container-howto.md#gathering-required-parameters) konusuna bakın. |

[!INCLUDE [subdomains-note](../../../includes/cognitive-services-custom-subdomains-note.md)]

> [!IMPORTANT]
> Kapsayıcıyı çalıştırmak için `Eula`, `Billing`ve `ApiKey` seçenekleri belirtilmelidir; Aksi takdirde, kapsayıcı başlatılmaz. Daha fazla bilgi için bkz. [faturalandırma](#billing-configuration-setting).
> ApiKey değeri, Azure konuşma kaynak anahtarları sayfasından alınan **anahtardır** .

## <a name="speech-container-docker-examples"></a>Konuşma kapsayıcısı Docker örnekleri

Aşağıdaki Docker örnekleri konuşma kapsayıcısına yöneliktir.

## <a name="speech-to-text"></a>[Konuşmayı metne dönüştürme](#tab/stt)

### <a name="basic-example-for-speech-to-text"></a>Konuşmayı metne dönüştürme için temel örnek

```Docker
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

### <a name="logging-example-for-speech-to-text"></a>Konuşmayı metne dönüştürme için günlüğe kaydetme örneği

```Docker
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
Logging:Console:LogLevel:Default=Information
```

## <a name="custom-speech-to-text"></a>[Özel Konuşma Tanıma metin](#tab/cstt)

### <a name="basic-example-for-custom-speech-to-text"></a>Özel Konuşma Tanıma metne yönelik temel örnek

```Docker
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

### <a name="logging-example-for-custom-speech-to-text"></a>Özel Konuşma Tanıma metin için günlüğe kaydetme örneği

```Docker
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
Logging:Console:LogLevel:Default=Information
```

## <a name="text-to-speech"></a>[Metin okuma](#tab/tss)

### <a name="basic-example-for-text-to-speech"></a>Metin okuma için temel örnek

```Docker
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

### <a name="logging-example-for-text-to-speech"></a>Metinden konuşmaya yazma için günlüğe kaydetme örneği

```Docker
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
Logging:Console:LogLevel:Default=Information
```

## <a name="custom-text-to-speech"></a>[Özel metin okuma](#tab/ctts)

### <a name="basic-example-for-custom-text-to-speech"></a>Özel metin okuma için temel örnek

```Docker
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

### <a name="logging-example-for-custom-text-to-speech"></a>Özel metin okuma için günlüğe kaydetme örneği

```Docker
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
Logging:Console:LogLevel:Default=Information
```

---

## <a name="next-steps"></a>Sonraki adımlar

- [Kapsayıcıları yüklemeyi ve çalıştırmayı](speech-container-howto.md) inceleyin
