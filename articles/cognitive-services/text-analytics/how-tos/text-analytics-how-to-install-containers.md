---
title: Kapsayıcıları yükleyip çalıştırma-Metin Analizi
titleSuffix: Azure Cognitive Services
description: İndirme, yükleme ve bu izlenecek yol öğreticide metin analizi için kapsayıcıları çalıştırmak nasıl.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: dapine
ms.openlocfilehash: f658e8d0f820ccec513b5665fc1ce94c083c3b3e
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68703537"
---
# <a name="install-and-run-text-analytics-containers"></a>Metin Analizi kapsayıcıları yükleyip çalıştırma

Kapsayıcılar, kendi ortamınızda metin analitik API 'Leri çalıştırmanızı sağlar ve belirli güvenlik ve veri idare gereksinimleriniz için harika bir yöntemdir. Metin Analizi kapsayıcıları ham metin üzerinde gelişmiş doğal dil işleme sağlar ve üç ana işlev içerir: yaklaşım analizi, anahtar ifade ayıklama ve dil algılama. Varlık bağlama Şu anda kapsayıcıda desteklenmiyor.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Metin Analizi kapsayıcılarından herhangi birini çalıştırmak için konak bilgisayar ve kapsayıcı ortamları olmalıdır.

## <a name="preparation"></a>Hazırlık

Metin analizi kapsayıcıları kullanmadan önce aşağıdaki gereksinimleri karşılaması gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Bir [ana bilgisayarda](#the-host-computer)Docker altyapısının yüklü olması gerekir. Docker, [MacOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/)ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms)'ta Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).<br><br> Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır. <br><br> **Windows 'da**Docker 'ın de Linux kapsayıcılarını destekleyecek şekilde yapılandırılması gerekir.<br><br>|
|Docker ile benzerlik | Kayıt defterleri, depolar, kapsayıcılar ve kapsayıcı görüntüleri gibi Docker kavramlarının yanı sıra temel `docker` komutlar hakkında bilgi sahibi olmanız gerekir.| 
|Metin Analizi kaynağı |Kapsayıcısını kullanabilmeniz için şunları yapmanız gerekir:<br><br>İlişkili API anahtarını ve uç nokta URI 'sini almak için bir Azure [metin analizi kaynağı](../../cognitive-services-apis-create-account.md) . Her iki değer de Azure portal Metin Analizi genel bakış ve anahtarlar sayfalarında bulunur ve kapsayıcıyı başlatmak için gereklidir.<br><br>**{API_KEY}** : **Anahtarlar** sayfasındaki kullanılabilir iki kaynak anahtardan biri<br><br>**{ENDPOINT_URI}** : **Genel bakış** sayfasında belirtilen bitiş noktası|

[!INCLUDE [Gathering required parameters](../../containers/includes/container-gathering-required-parameters.md)]

### <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

Aşağıdaki tabloda açıklanmıştır en düşük ve önerilen CPU çekirdekleri, en az 2.6 gigahertz (GHz) veya daha hızlı ve her bir metin analizi kapsayıcısı için ayrılacak gigabayt (GB) bellek.

| Kapsayıcı | Minimum | Önerilen | TPS<br>(En düşük, en yüksek)|
|-----------|---------|-------------|--|
|Anahtar İfade Ayıklama | 1 çekirdek, 2 GB bellek | 1 çekirdek, 4 GB bellek |15, 30|
|Dil Algılama | 1 çekirdek, 2 GB bellek | 1 çekirdek, 4 GB bellek |15, 30|
|Yaklaşım Analizi 2. x | 1 çekirdek, 2 GB bellek | 1 çekirdek, 4 GB bellek |15, 30|
|Yaklaşım Analizi 3. x | 1 çekirdek, 2 GB bellek | 4 çekirdek, 4 GB bellek |15, 30|

* Her çekirdek en az 2,6 gigahertz (GHz) veya daha hızlı olmalıdır.
* TPS-saniye başına işlem

Çekirdek ve bellek, `--cpus` `docker run` komutunun bir parçası `--memory` olarak kullanılan ve ayarlarına karşılık gelir.

## <a name="get-the-container-image-with-docker-pull"></a>Kapsayıcı görüntüsünü al`docker pull`

Microsoft kapsayıcı kayıt defterinden kapsayıcı görüntülerini metin analizi için kullanılabilir.

| Kapsayıcı | Havuz |
|-----------|------------|
|Anahtar İfade Ayıklama | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
|Dil Algılama | `mcr.microsoft.com/azure-cognitive-services/language` |
|Yaklaşım Analizi 2. x| `mcr.microsoft.com/azure-cognitive-services/sentiment` |
|Yaklaşım Analizi 3. x| `containerpreview.azurecr.io/microsoft/cognitive-services-sentiment-v3.0` |

Microsoft Container Registry bir kapsayıcı görüntüsünü indirmek için [komutunukullanın.`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/)

Metin analizi kapsayıcılar için kullanılabilir etiketler tam bir açıklaması için aşağıdaki kapsayıcıların Docker Hub bakın:

* [Anahtar ifade ayıklama](https://go.microsoft.com/fwlink/?linkid=2018757)
* [Dil algılama](https://go.microsoft.com/fwlink/?linkid=2018759)
* [Yaklaşım analizi](https://go.microsoft.com/fwlink/?linkid=2018654)

Bir kapsayıcı görüntüsünü indirmek için [komutunukullanın.`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/)

### <a name="docker-pull-for-the-key-phrase-extraction-container"></a>Anahtar tümceciği ayıklama kapsayıcısı için Docker Pull

```
docker pull mcr.microsoft.com/azure-cognitive-services/keyphrase:latest
```

### <a name="docker-pull-for-the-language-detection-container"></a>Dil algılama kapsayıcısı için Docker Pull

```
docker pull mcr.microsoft.com/azure-cognitive-services/language:latest
```

### <a name="docker-pull-for-the-sentiment-2x-container"></a>Yaklaşım 2. x kapsayıcısı için Docker Pull

```
docker pull mcr.microsoft.com/azure-cognitive-services/sentiment:latest
```

### <a name="docker-pull-for-the-sentiment-3x-container"></a>Yaklaşım 3. x kapsayıcısı için Docker Pull

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-sentiment-v3.0:latest
```

[!INCLUDE [Tip for using docker list](../../../../includes/cognitive-services-containers-docker-list-tip.md)]

## <a name="how-to-use-the-container"></a>Kapsayıcıyı kullanma

Kapsayıcı [ana bilgisayardan](#the-host-computer)olduktan sonra, kapsayıcında çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı](#run-the-container-with-docker-run)gerekli faturalandırma ayarlarıyla çalıştırın. `docker run` Komuta daha fazla [örnek](../text-analytics-resource-container-config.md#example-docker-run-commands) kullanılabilir.
1. Kapsayıcının, [v2](#query-the-containers-v2-prediction-endpoint) veya [v3](#query-the-containers-v3-prediction-endpoint)için tahmin uç noktasını sorgulayın.

## <a name="run-the-container-with-docker-run"></a>Kapsayıcıyı ile çalıştırma`docker run`

Üç kapsayıcının herhangi birini çalıştırmak için [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. `{Endpoint_URI}` [](#gathering-required-parameters) Ve`{API_Key}` değerlerini alma hakkında ayrıntılar için gerekli parametreleri toplama bölümüne bakın.

Komut örnekleri mevcuttur. [](../text-analytics-resource-container-config.md#example-docker-run-commands) `docker run`

### <a name="run-v2-container-example-of-docker-run-command"></a>Docker Run komutunun v2 kapsayıcısını çalıştırma örneği

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/keyphrase \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Bu komut:

* Kapsayıcı görüntüsünden bir anahtar tümceciği kapsayıcısı çalıştırır
* Bir CPU çekirdeği ve 4 gigabayt (GB) bellek ayırır
* 5000 numaralı TCP bağlantı noktasını kullanıma sunar ve sahte TTY için kapsayıcı ayırır.
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.

### <a name="run-v3-container-example-of-docker-run-command"></a>Docker Run komutu için v3 kapsayıcı örneğini Çalıştır

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
containerpreview.azurecr.io/microsoft/cognitive-services-sentiment-v3.0 \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Bu komut:

* Kapsayıcı görüntüsünden bir anahtar tümceciği kapsayıcısı çalıştırır
* 4 CPU çekirdeği ve 4 gigabayt (GB) bellek ayırır
* 5000 numaralı TCP bağlantı noktasını kullanıma sunar ve sahte TTY için kapsayıcı ayırır.
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](#billing).

[!INCLUDE [Running multiple containers on the same host](../../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

## <a name="query-the-containers-v2-prediction-endpoint"></a>Kapsayıcının v2 tahmin uç noktasını sorgulama

Kapsayıcı, REST tabanlı sorgu tahmin uç noktası API 'Leri sağlar.

Kapsayıcı API 'leri için `https://localhost:5000`Konağı kullanın.

## <a name="query-the-containers-v3-prediction-endpoint"></a>Kapsayıcının v3 tahmin uç noktasını sorgulama

Kapsayıcı, REST tabanlı sorgu tahmin uç noktası API 'Leri sağlar.

Kapsayıcı API 'leri için `https://localhost:5000`Konağı kullanın.

### <a name="v3-api-request-post-body"></a>V3 API isteği GÖNDERI gövdesi

Aşağıdaki JSON, bir v3 API isteğinin GÖNDERI gövdesi örneğidir:

```json
{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Hello world. This is some input text that I love."
    },
    {
      "language": "en",
      "id": "2",
      "text": "It's incredibly sunny outside! I'm so happy."
    }
  ]
}
```

### <a name="v3-api-response-body"></a>V3 API yanıt gövdesi

Aşağıdaki JSON, bir v3 API isteğinin GÖNDERI gövdesi örneğidir:

```json
{
    "documents": [
        {
            "id": "1",
            "sentiment": "positive",
            "documentScores": {
                "positive": 0.98570585250854492,
                "neutral": 0.0001625834556762,
                "negative": 0.0141316400840878
            },
            "sentences": [
                {
                    "sentiment": "neutral",
                    "sentenceScores": {
                        "positive": 0.0785155147314072,
                        "neutral": 0.89702343940734863,
                        "negative": 0.0244610067456961
                    },
                    "offset": 0,
                    "length": 12
                },
                {
                    "sentiment": "positive",
                    "sentenceScores": {
                        "positive": 0.98570585250854492,
                        "neutral": 0.0001625834556762,
                        "negative": 0.0141316400840878
                    },
                    "offset": 13,
                    "length": 36
                }
            ]
        },
        {
            "id": "2",
            "sentiment": "positive",
            "documentScores": {
                "positive": 0.89198976755142212,
                "neutral": 0.103382371366024,
                "negative": 0.0046278294175863
            },
            "sentences": [
                {
                    "sentiment": "positive",
                    "sentenceScores": {
                        "positive": 0.78401315212249756,
                        "neutral": 0.2067587077617645,
                        "negative": 0.0092281140387058
                    },
                    "offset": 0,
                    "length": 30
                },
                {
                    "sentiment": "positive",
                    "sentenceScores": {
                        "positive": 0.99996638298034668,
                        "neutral": 0.0000060341349126,
                        "negative": 0.0000275444017461
                    },
                    "offset": 31,
                    "length": 13
                }
            ]
        }
    ],
    "errors": []
}
```

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Kapsayıcıyı durdur

[!INCLUDE [How to stop the container](../../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcıyı bir çıkış [bağlaması](../text-analytics-resource-container-config.md#mount-settings) ve günlüğü etkin olarak çalıştırırsanız kapsayıcı, kapsayıcıyı başlatırken veya çalıştırırken oluşan sorunları gidermek için yararlı olan günlük dosyaları oluşturur.

## <a name="billing"></a>Faturalandırma

Metin Analizi kapsayıcıları Azure hesabınızdaki bir _metin analizi_ kaynak kullanarak faturalandırma bilgilerini Azure 'a gönderir. 

[!INCLUDE [Container's Billing Settings](../../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](../text-analytics-resource-container-config.md).

<!--blogs/samples/video course -->

[!INCLUDE [Discoverability of more container information](../../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, kavramlar ve indirme, yükleme ve metin analizi kapsayıcıları çalıştırmak için iş akışı öğrendiniz. Özet:

* Metin analizi, anahtar ifade ayıklama, dil algılama ve yaklaşım analizi Kapsüllenen üç Linux kapsayıcıları için Docker, sağlar.
* Kapsayıcı görüntülerini azure'da Microsoft kapsayıcı kayıt defteri (MCR) alanından indirilir.
* Docker kapsayıcı görüntüleri çalıştırın.
* Metin analizi-kapsayıcılarında işlemleri ana kapsayıcısının URI belirterek çağırmak için REST API veya SDK'sını kullanabilirsiniz.
* Bir kapsayıcı örneği oluşturulurken, fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcılar, Microsoft müşteri verilerini (örneğin, görüntü veya metin analiz edilen) göndermeyin.

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [kapsayıcıları yapılandırma](../text-analytics-resource-container-config.md) yapılandırma ayarları
* İşlevlerle ilgili sorunları çözmek için [sık sorulan sorular (SSS)](../text-analytics-resource-faq.md) bölümüne bakın.
