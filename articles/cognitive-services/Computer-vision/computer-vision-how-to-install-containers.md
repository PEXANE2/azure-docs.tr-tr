---
title: Kapsayıcıları yüklemek ve çalıştırmak-Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Bu adım adım öğreticide Görüntü İşleme için kapsayıcıları indirme, yükleme ve çalıştırma.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 10/03/2019
ms.author: dapine
ms.custom: seodec18
ms.openlocfilehash: 7c137572fadd07254343b7b4c34b5a63534b9d88
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71936994"
---
# <a name="install-and-run-computer-vision-containers"></a>Görüntü İşleme kapsayıcıları yükleyip çalıştırma

Kapsayıcılar, Görüntü İşleme API 'Lerini kendi ortamınızda çalıştırmanızı sağlar. Kapsayıcılar, belirli güvenlik ve veri idare gereksinimleri için harika. Bu makalede bir Görüntü İşleme kapsayıcısını indirme, yükleme ve çalıştırma hakkında bilgi edineceksiniz.

Görüntü İşleme için iki Docker kapsayıcısı mevcuttur: *metin tanıma* ve *okuma*. *Metin tanıma* kapsayıcısı, çeşitli nesnelerin görüntülerini, alış irsaliyeleri, posterler ve iş kartları gibi farklı yüzeylerle ve arka planlarla *algılayıp ayıklamanızı* sağlar. Ancak *okuma* kapsayıcısı, ancak Ayrıca resimlerde *el yazısı metinleri* ALGıLAR ve PDF/TIFF/çok sayfalı destek sağlar. Daha fazla bilgi için bkz. [okuma API 'si](concept-recognizing-text.md#read-api) belgeleri.

> [!IMPORTANT]
> Metin Tanıma kapsayıcısı, okuma kapsayıcısının yararına kullanım dışı bırakılıyor. Okuma kapsayıcısı, Metin Tanıma kapsayıcısının öncülü olan bir üst kümesidir ve tüketiciler, okuma kapsayıcısını kullanarak geçiş yapılmalıdır. Her iki kapsayıcı yalnızca Ingilizce ile çalışır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Prerequisites

Kapsayıcıları kullanmadan önce aşağıdaki önkoşulları karşılamanız gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Bir [ana bilgisayarda](#the-host-computer)Docker altyapısının yüklü olması gerekir. Docker, [MacOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/)ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms)'ta Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temelleri hakkında bilgi için bkz. [Docker genel bakış](https://docs.docker.com/engine/docker-overview/).<br><br> Kapsayıcıların Azure 'a bağlanıp faturalandırma verilerini göndermesini sağlamak için Docker yapılandırılmalıdır. <br><br> **Windows 'da**Docker 'ın de Linux kapsayıcılarını destekleyecek şekilde yapılandırılması gerekir.<br><br>|
|Docker ile benzerlik | Kayıt defterleri, depolar, kapsayıcılar ve kapsayıcı görüntüleri gibi Docker kavramlarının yanı sıra temel `docker` komutlarının bir bilgisine sahip olmanız gerekir.| 
|Görüntü İşleme kaynağı |Kapsayıcısını kullanabilmeniz için şunları yapmanız gerekir:<br><br>Uç nokta URI 'SI olan bir Azure **görüntü işleme** kaynağı ve ilişkili API anahtarı. Her iki değer de kaynak için genel bakış ve anahtarlar sayfalarında bulunur ve kapsayıcıyı başlatmak için gereklidir.<br><br>**{API_KEY}** : **anahtarlar** sayfasında kullanılabilir iki kaynak anahtardan biri<br><br>**{ENDPOINT_URI}** : **genel bakış** sayfasında belirtilen bitiş noktası|

[!INCLUDE [Gathering required container parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="request-access-to-the-private-container-registry"></a>Özel kapsayıcı kayıt defterine erişim isteme

[!INCLUDE [Request access to public preview](../../../includes/cognitive-services-containers-request-access.md)]

### <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

[!INCLUDE [Container requirements and recommendations](includes/container-requirements-and-recommendations.md)]

## <a name="get-the-container-image-with-docker-pull"></a>@No__t ile kapsayıcı görüntüsünü al-0

# <a name="readtabread"></a>[Read](#tab/read)

Okuma için kapsayıcı görüntüleri kullanılabilir.

| Kapsayıcı | Container Registry/depo/görüntü adı |
|-----------|------------|
| Oku | `containerpreview.azurecr.io/microsoft/cognitive-services-read:latest` |

# <a name="recognize-texttabrecognize-text"></a>[Metin Tanıma](#tab/recognize-text)

Metin Tanıma için kapsayıcı görüntüleri kullanılabilir.

| Kapsayıcı | Container Registry/depo/görüntü adı |
|-----------|------------|
| Metin Tanıma | `containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest` |

***

Bir kapsayıcı görüntüsünü indirmek için [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) komutunu kullanın.

# <a name="readtabread"></a>[Read](#tab/read)

### <a name="docker-pull-for-the-read-container"></a>Okuma kapsayıcısı için Docker çekme

```bash
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-read:latest
```

# <a name="recognize-texttabrecognize-text"></a>[Metin Tanıma](#tab/recognize-text)

### <a name="docker-pull-for-the-recognize-text-container"></a>Metin Tanıma kapsayıcısı için Docker Pull

```bash
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest
```

***

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

## <a name="how-to-use-the-container"></a>Kapsayıcıyı kullanma

Kapsayıcı [ana bilgisayardan](#the-host-computer)olduktan sonra, kapsayıcında çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı](#run-the-container-with-docker-run)gerekli faturalandırma ayarlarıyla çalıştırın. @No__t-1 komutuna daha fazla [örnek](computer-vision-resource-container-config.md) kullanılabilir. 
1. [Kapsayıcının tahmin uç noktasını sorgulayın](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Kapsayıcıyı @no__t ile çalıştırma-0

Kapsayıcıyı çalıştırmak için [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. @No__t-1 ve `{API_KEY}` değerlerini alma hakkında ayrıntılar için [gerekli parametreleri toplama](#gathering-required-parameters) bölümüne bakın.

@No__t-1 komutunun [örnekleri](computer-vision-resource-container-config.md#example-docker-run-commands) mevcuttur.

# <a name="readtabread"></a>[Read](#tab/read)

```bash
docker run --rm -it -p 5000:5000 --memory 16g --cpus 8 \
containerpreview.azurecr.io/microsoft/cognitive-services-read \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Bu komut:

* Kapsayıcı görüntüsünden okuma kapsayıcısını çalıştırır.
* 8 CPU çekirdeği ve 16 gigabayt (GB) bellek ayırır.
* TCP bağlantı noktası 5000 ' i gösterir ve kapsayıcı için bir sözde TTY ayırır.
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.

# <a name="recognize-texttabrecognize-text"></a>[Metin Tanıma](#tab/recognize-text)

```bash
docker run --rm -it -p 5000:5000 --memory 16g --cpus 8 \
containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Bu komut:

* Kapsayıcı görüntüsünden Metin Tanıma kapsayıcısını çalıştırır.
* 8 CPU çekirdeği ve 16 gigabayt (GB) bellek ayırır.
* TCP bağlantı noktası 5000 ' i gösterir ve kapsayıcı için bir sözde TTY ayırır.
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.

***

@No__t-1 komutuna daha fazla [örnek](./computer-vision-resource-container-config.md#example-docker-run-commands) kullanılabilir. 

> [!IMPORTANT]
> Kapsayıcıyı çalıştırmak için `Eula`, `Billing` ve `ApiKey` seçeneklerinin belirtilmesi gerekir; Aksi takdirde, kapsayıcı başlatılmaz.  Daha fazla bilgi için bkz. [faturalandırma](#billing).

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="query-the-containers-prediction-endpoint"></a>Kapsayıcının tahmin uç noktasını sorgulama

Kapsayıcı, REST tabanlı sorgu tahmin uç noktası API 'Leri sağlar. 

Kapsayıcı API 'Leri için `http://localhost:5000` olan Konağı kullanın.

# <a name="readtabread"></a>[Read](#tab/read)

### <a name="asynchronous-read"></a>Zaman uyumsuz okuma

Görüntü işleme hizmetinin ilgili Rest işlemlerini nasıl kullandığına benzer şekilde bir görüntüyü zaman uyumsuz olarak okumak için, `POST /vision/v2.0/read/core/asyncBatchAnalyze` ve `GET /vision/v2.0/read/operations/{operationId}` işlemlerini konser içinde kullanabilirsiniz. Zaman uyumsuz POST yöntemi, HTTP GET isteğine tanımlayıcı olarak kullanılan bir `operationId` döndürür.

Swagger kullanıcı arabiriminden, tarayıcıda genişletmek için `asyncBatchAnalyze` ' ı seçin. Ardından @no__t **deneyin**-1**Dosya Seç**' i seçin. Bu örnekte, aşağıdaki görüntüyü kullanacağız:

![sekmeler vs alanları](media/tabs-vs-spaces.png)

Zaman uyumsuz GÖNDERI başarıyla çalıştırıldığında, bir **HTTP 202** durum kodu döndürür. Yanıtın bir parçası olarak, isteğin sonuç uç noktasını tutan `operation-location` üstbilgisi vardır.

```http
 content-length: 0
 date: Fri, 13 Sep 2019 16:23:01 GMT
 operation-location: http://localhost:5000/vision/v2.0/read/operations/a527d445-8a74-4482-8cb3-c98a65ec7ef9
 server: Kestrel
```

@No__t-0 tam URL 'dir ve bir HTTP GET aracılığıyla erişilir. Önceki görüntüden `operation-location` URL 'sini yürütmenin JSON yanıtı aşağıda verilmiştir:

```json
{
  "status": "Succeeded",
  "recognitionResults": [
    {
      "page": 1,
      "clockwiseOrientation": 2.42,
      "width": 502,
      "height": 252,
      "unit": "pixel",
      "lines": [
        {
          "boundingBox": [
            56,
            39,
            317,
            50,
            313,
            134,
            53,
            123
          ],
          "text": "Tabs VS",
          "words": [
            {
              "boundingBox": [
                90,
                43,
                243,
                53,
                243,
                123,
                94,
                125
              ],
              "text": "Tabs",
              "confidence": "Low"
            },
            {
              "boundingBox": [
                259,
                55,
                313,
                62,
                313,
                122,
                259,
                123
              ],
              "text": "VS"
            }
          ]
        },
        {
          "boundingBox": [
            221,
            148,
            417,
            146,
            417,
            206,
            227,
            218
          ],
          "text": "Spaces",
          "words": [
            {
              "boundingBox": [
                230,
                148,
                416,
                141,
                419,
                211,
                232,
                218
              ],
              "text": "Spaces"
            }
          ]
        }
      ]
    }
  ]
}
```

### <a name="synchronous-read"></a>Zaman uyumlu okuma

Bir görüntüyü eşzamanlı olarak okumak için `POST /vision/v2.0/read/core/Analyze` işlemini kullanabilirsiniz. Resim tamamen okunsa ve yalnızca API bir JSON yanıtı döndürüyor. Bunun tek istisnası bir hata meydana gelir. Bir hata oluştuğunda aşağıdaki JSON döndürülür:

```json
{
    status: "Failed"
}
```

JSON yanıt nesnesi, zaman uyumsuz sürümle aynı nesne grafiğine sahiptir. Bir JavaScript kullanıcısı ve tür güvenliği istiyorsanız, JSON yanıtını `AnalyzeResult` nesnesi olarak dönüştürmek için aşağıdaki türler kullanılabilir.

```typescript
export interface AnalyzeResult {
    status: Status;
    recognitionResults?: RecognitionResult[] | null;
}

export enum Status {
    NotStarted = 0,
    Running = 1,
    Failed = 2,
    Succeeded = 3
}

export enum Unit {
    Pixel = 0,
    Inch = 1
}

export interface RecognitionResult {
    page?: number | null;
    clockwiseOrientation?: number | null;
    width?: number | null;
    height?: number | null;
    unit?: Unit | null;
    lines?: Line[] | null;
}

export interface Line {
    boundingBox?: number[] | null;
    text: string;
    words?: Word[] | null;
}

export interface Word {
  boundingBox?: number[] | null;
  text: string;
  confidence?: string | null;
}
```

Örnek kullanım örneği için, [burada TypeScript korumalı alanı](https://aka.ms/ts-read-api-types) ' na bakın ve kullanım kolaylığını görselleştirmek Için "Çalıştır" ı seçin.

# <a name="recognize-texttabrecognize-text"></a>[Metin Tanıma](#tab/recognize-text)

### <a name="asynchronous-text-recognition"></a>Zaman uyumsuz metin tanıma

@No__t-0 ve `GET /vision/v2.0/textOperations/*{id}*` işlemlerini konser içinde, görüntü işleme hizmetinin ilgili Rest işlemlerini nasıl kullandığına benzer şekilde, görüntüde yazılı metni zaman uyumsuz olarak tanımak için kullanabilirsiniz. Metin Tanıma kapsayıcısı, el ile yazılmış metni değil, şu anda yalnızca yazdırılmış metni tanır, bu nedenle Görüntü İşleme hizmeti işlemi için normalde belirtilen `mode` parametresi Metin Tanıma kapsayıcısı tarafından yok sayılır.

### <a name="synchronous-text-recognition"></a>Zaman uyumlu metin tanıma

Bir görüntüdeki yazdırılmış metni eşzamanlı olarak tanımak için `POST /vision/v2.0/recognizeTextDirect` işlemini kullanabilirsiniz. Bu işlem zaman uyumlu olduğundan, bu işlem için istek gövdesi `POST /vision/v2.0/recognizeText` işlemiyle aynıdır, ancak bu işlemin yanıt gövdesi, `GET /vision/v2.0/textOperations/*{id}*` işlemi tarafından döndürülen ile aynıdır.

***

## <a name="stop-the-container"></a>Kapsayıcıyı durdur

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcıyı bir çıkış [bağlaması](./computer-vision-resource-container-config.md#mount-settings) ve günlüğü etkin olarak çalıştırırsanız kapsayıcı, kapsayıcıyı başlatırken veya çalıştırırken oluşan sorunları gidermek için yararlı olan günlük dosyaları oluşturur.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Ödeme

Bilişsel hizmetler kapsayıcıları, Azure hesabınızdaki ilgili kaynağı kullanarak faturalandırma bilgilerini Azure 'a gönderir.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](./computer-vision-resource-container-config.md).

<!--blogs/samples/video course -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, Görüntü İşleme kapsayıcıları indirmek, yüklemek ve çalıştırmak için kavramları ve iş akışını öğrendiniz. Özet:

* Görüntü İşleme, Docker için hem Metin Tanıma hem de okunan bir Linux kapsayıcısı sağlar.
* Kapsayıcı görüntüleri, Azure 'daki "kapsayıcı önizlemesi" kapsayıcı kayıt defterinden indirilir.
* Kapsayıcı görüntüleri Docker 'da çalışır.
* Metin Tanıma işlemleri çağırmak ya da kapsayıcının ana bilgisayar URI 'sini belirterek kapsayıcıları okumak için REST API veya SDK kullanabilirsiniz.
* Bir kapsayıcıyı örnekledikten sonra faturalandırma bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel hizmetler kapsayıcıları, ölçüm için Azure 'a bağlı kalmadan çalıştırılmak üzere lisanslanmaz. Müşterilerin, ödeme bilgilerini her zaman ölçüm hizmetiyle iletişimine olanak tanımak için kapsayıcıların etkinleştirilmesi gerekir. Bilişsel hizmetler kapsayıcıları, müşteri verilerini (örneğin, çözümlenmekte olan resim veya metin) Microsoft 'a göndermez.

## <a name="next-steps"></a>Sonraki adımlar

* Yapılandırma ayarları için [kapsayıcıları](computer-vision-resource-container-config.md) yapılandırmayı gözden geçir
* Basılan ve el yazısı metni tanıma hakkında daha fazla bilgi edinmek için [görüntü işleme genel bakışı](Home.md) inceleyin
* Kapsayıcı tarafından desteklenen yöntemler hakkındaki ayrıntılar için [görüntü işleme API'si](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) başvurun.
* Görüntü İşleme işlevlerle ilgili sorunları çözmek için [sık sorulan sorular (SSS)](FAQ.md) bölümüne bakın.
* Daha fazla bilişsel [Hizmetler kapsayıcısı](../cognitive-services-container-support.md) kullanın
