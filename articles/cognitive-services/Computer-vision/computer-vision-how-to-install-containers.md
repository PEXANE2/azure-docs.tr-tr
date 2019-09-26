---
title: Kapsayıcıları yüklemek ve çalıştırmak-Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: İndirme, yükleme ve bu izlenecek yol öğreticide görüntü işleme için kapsayıcıları çalıştırmak nasıl.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: dapine
ms.custom: seodec18
ms.openlocfilehash: 98330e88b0b94c488fd968d8fc18806ec6908b26
ms.sourcegitcommit: 9fba13cdfce9d03d202ada4a764e574a51691dcd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71316206"
---
# <a name="install-and-run-computer-vision-containers"></a>Görüntü İşleme kapsayıcıları yükleyip çalıştırma

Kapsayıcılar, Görüntü İşleme API 'Lerini kendi ortamınızda çalıştırmanızı sağlar. Kapsayıcılar, belirli güvenlik ve veri idare gereksinimleri için harika. Bu makalede bir Görüntü İşleme kapsayıcısını indirme, yükleme ve çalıştırma hakkında bilgi edineceksiniz.

Görüntü İşleme için iki Docker kapsayıcısı mevcuttur: *Metin tanıma* ve *okuma*. *Metin tanıma* kapsayıcısı, çeşitli nesnelerin görüntülerini, alış irsaliyeleri, posterler ve iş kartları gibi farklı yüzeylerle ve arka planlarla *algılayıp ayıklamanızı* sağlar. Ancak *okuma* kapsayıcısı, ancak Ayrıca resimlerde *el yazısı metinleri* ALGıLAR ve PDF/TIFF/çok sayfalı destek sağlar. Daha fazla bilgi için bkz. [okuma API 'si](concept-recognizing-text.md#read-api) belgeleri.

> [!IMPORTANT]
> Metin Tanıma kapsayıcısı, okuma kapsayıcısının yararına kullanım dışı bırakılıyor. Okuma kapsayıcısı, Metin Tanıma kapsayıcısının öncülü olan bir üst kümesidir ve tüketiciler, okuma kapsayıcısını kullanarak geçiş yapılmalıdır. Her iki kapsayıcı yalnızca Ingilizce ile çalışır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Kapsayıcıları kullanmadan önce aşağıdaki önkoşulları karşılamanız gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Bir [ana bilgisayarda](#the-host-computer)Docker altyapısının yüklü olması gerekir. Docker, [MacOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/)ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms)'ta Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).<br><br> Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır. <br><br> **Windows 'da**Docker 'ın de Linux kapsayıcılarını destekleyecek şekilde yapılandırılması gerekir.<br><br>|
|Docker ile benzerlik | Kayıt defterleri, depolar, kapsayıcılar ve kapsayıcı görüntüleri gibi Docker kavramlarının yanı sıra temel `docker` komutlar hakkında bilgi sahibi olmanız gerekir.| 
|Görüntü İşleme kaynağı |Kapsayıcısını kullanabilmeniz için şunları yapmanız gerekir:<br><br>Uç nokta URI 'SI olan bir Azure **görüntü işleme** kaynağı ve ilişkili API anahtarı. Her iki değer de kaynak için genel bakış ve anahtarlar sayfalarında bulunur ve kapsayıcıyı başlatmak için gereklidir.<br><br>**{API_KEY}** : **Anahtarlar** sayfasındaki kullanılabilir iki kaynak anahtardan biri<br><br>**{ENDPOINT_URI}** : **Genel bakış** sayfasında belirtilen bitiş noktası|

[!INCLUDE [Gathering required container parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="request-access-to-the-private-container-registry"></a>Özel kapsayıcı kayıt defterine erişim isteği

[!INCLUDE [Request access to public preview](../../../includes/cognitive-services-containers-request-access.md)]

[!INCLUDE [Gathering required parameters](../containers/includes/container-gathering-required-parameters.md)]

### <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

[!INCLUDE [Container requirements and recommendations](includes/container-requirements-and-recommendations.md)]

## <a name="get-the-container-image-with-docker-pull"></a>Kapsayıcı görüntüsünü al`docker pull`

# <a name="readtabread"></a>[Okuma](#tab/read)

Okuma için kapsayıcı görüntüleri kullanılabilir.

| Kapsayıcı | Container Registry/depo/görüntü adı |
|-----------|------------|
| Okuma | `containerpreview.azurecr.io/microsoft/cognitive-services-read:latest` |

# <a name="recognize-texttabrecognize-text"></a>[Metin tanıma](#tab/recognize-text)

Metin Tanıma için kapsayıcı görüntüleri kullanılabilir.

| Kapsayıcı | Container Registry/depo/görüntü adı |
|-----------|------------|
| Metin Tanıma | `containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest` |

***

Bir kapsayıcı görüntüsünü indirmek için [komutunukullanın.`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/)

# <a name="readtabread"></a>[Okuma](#tab/read)

### <a name="docker-pull-for-the-read-container"></a>Okuma kapsayıcısı için Docker çekme

```bash
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-read:latest
```

# <a name="recognize-texttabrecognize-text"></a>[Metin tanıma](#tab/recognize-text)

### <a name="docker-pull-for-the-recognize-text-container"></a>Metin Tanıma kapsayıcısı için Docker Pull

```bash
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest
```

***

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

## <a name="how-to-use-the-container"></a>Kapsayıcıyı kullanma

Kapsayıcı [ana bilgisayardan](#the-host-computer)olduktan sonra, kapsayıcında çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı](#run-the-container-with-docker-run)gerekli faturalandırma ayarlarıyla çalıştırın. `docker run` Komuta daha fazla [örnek](computer-vision-resource-container-config.md) kullanılabilir. 
1. [Kapsayıcının tahmin uç noktasını sorgulayın](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Kapsayıcıyı ile çalıştırma`docker run`

Kapsayıcıyı çalıştırmak için [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. `{ENDPOINT_URI}` [](#gathering-required-parameters) Ve`{API_KEY}` değerlerini alma hakkında ayrıntılar için gerekli parametreleri toplama bölümüne bakın.

Komut örnekleri mevcuttur. [](computer-vision-resource-container-config.md#example-docker-run-commands) `docker run`

# <a name="readtabread"></a>[Okuma](#tab/read)

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

# <a name="recognize-texttabrecognize-text"></a>[Metin tanıma](#tab/recognize-text)

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

`docker run` Komuta daha fazla [örnek](./computer-vision-resource-container-config.md#example-docker-run-commands) kullanılabilir. 

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](#billing).

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="query-the-containers-prediction-endpoint"></a>Kapsayıcının tahmin uç noktasını sorgulama

Kapsayıcı, REST tabanlı sorgu tahmin uç noktası API 'Leri sağlar. 

Kapsayıcı API 'leri için `http://localhost:5000`Konağı kullanın.

# <a name="readtabread"></a>[Okuma](#tab/read)

### <a name="asynchronous-read"></a>Zaman uyumsuz okuma

Görüntü işleme hizmetinin ilgili Rest `POST /vision/v2.0/read/core/asyncBatchAnalyze` işlemlerini `GET /vision/v2.0/read/operations/{operationId}` nasıl kullandığına benzer şekilde bir görüntüyü zaman uyumsuz olarak okumak için konser içindeki ve işlemlerini kullanabilirsiniz. Zaman uyumsuz post yöntemi, http get `operationId` isteğine tanımlayıcı olarak kullanılan bir döndürür.

Swagger kullanıcı arabiriminden, tarayıcıda genişletmek `asyncBatchAnalyze` için öğesini seçin. Ardından **deneyin** > **dosyayı**seçin öğesini seçin. Bu örnekte, aşağıdaki görüntüyü kullanacağız:

![sekmeler vs alanları](media/tabs-vs-spaces.png)

Zaman uyumsuz GÖNDERI başarıyla çalıştırıldığında, bir **HTTP 202** durum kodu döndürür. Yanıtın bir parçası olarak, isteğin sonuç uç noktasını `operation-location` tutan bir üst bilgi vardır.

```http
 content-length: 0
 date: Fri, 13 Sep 2019 16:23:01 GMT
 operation-location: http://localhost:5000/vision/v2.0/read/operations/a527d445-8a74-4482-8cb3-c98a65ec7ef9
 server: Kestrel
```

, `operation-location` Tam URL 'dir ve bir http get aracılığıyla erişilir. Önceki görüntüden `operation-location` URL 'yi yürütmenin JSON yanıtı aşağıda verilmiştir:

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

`POST /vision/v2.0/read/core/Analyze` İşlemi eşzamanlı olarak bir görüntüyü okumak için kullanabilirsiniz. Resim tamamen okunsa ve yalnızca API bir JSON yanıtı döndürüyor. Bunun tek istisnası bir hata meydana gelir. Bir hata oluştuğunda aşağıdaki JSON döndürülür:

```json
{
    status: "Failed"
}
```

JSON yanıt nesnesi, zaman uyumsuz sürümle aynı nesne grafiğine sahiptir. Bir JavaScript kullanıcısı ve tür güvenliği isterseniz, JSON yanıtını bir `AnalyzeResult` nesne olarak dönüştürmek için aşağıdaki türler kullanılabilir.

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

# <a name="recognize-texttabrecognize-text"></a>[Metin tanıma](#tab/recognize-text)

### <a name="asynchronous-text-recognition"></a>Zaman uyumsuz metin tanıma

Kullanabileceğiniz `POST /vision/v2.0/recognizeText` ve `GET /vision/v2.0/textOperations/*{id}*` işlemlerini zaman uyumsuz olarak yazdırılan metin, görüntü işleme hizmeti bu karşılık gelen REST işlemlerini nasıl kullandığı için benzer resim tanımak için uyumlu bir şekilde. Metni Tanı kapsayıcı yazdırılan metin, el yazısı olmayan metin şu anda yalnızca tanır. böylece `mode` normalde görüntü işleme hizmeti işlemi metni tanı kapsayıcı tarafından göz ardı edilir için belirtilen parametre.

### <a name="synchronous-text-recognition"></a>Zaman uyumlu metin tanıma

Kullanabileceğiniz `POST /vision/v2.0/recognizeTextDirect` görüntüdeki basılı metin eş zamanlı olarak tanıyacak şekilde işlemi. Bu işlem zaman uyumlu olduğundan, bu işlem için istek gövdesi `POST /vision/v2.0/recognizeText` işlemle aynıdır, ancak bu işlemin yanıt gövdesi `GET /vision/v2.0/textOperations/*{id}*` işlem tarafından döndürülen ile aynıdır.

***

## <a name="stop-the-container"></a>Kapsayıcıyı durdur

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcıyı bir çıkış [bağlaması](./computer-vision-resource-container-config.md#mount-settings) ve günlüğü etkin olarak çalıştırırsanız kapsayıcı, kapsayıcıyı başlatırken veya çalıştırırken oluşan sorunları gidermek için yararlı olan günlük dosyaları oluşturur.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Faturalandırma

Bilişsel hizmetler kapsayıcıları, Azure hesabınızdaki ilgili kaynağı kullanarak faturalandırma bilgilerini Azure 'a gönderir.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](./computer-vision-resource-container-config.md).

<!--blogs/samples/video course -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, kavramlar ve indirme, yükleme ve görüntü işleme kapsayıcıları çalıştırmak için iş akışı öğrendiniz. Özet:

* Görüntü İşleme, Docker için hem Metin Tanıma hem de okunan bir Linux kapsayıcısı sağlar.
* Kapsayıcı görüntüleri, Azure 'daki "kapsayıcı önizlemesi" kapsayıcı kayıt defterinden indirilir.
* Docker kapsayıcı görüntüleri çalıştırın.
* Metin Tanıma işlemleri çağırmak ya da kapsayıcının ana bilgisayar URI 'sini belirterek kapsayıcıları okumak için REST API veya SDK kullanabilirsiniz.
* Bir kapsayıcı örneği oluşturulurken, fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcıları, müşteri verilerini (örneğin, çözümlenmekte olan resim veya metin) Microsoft 'a göndermez.

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [kapsayıcıları yapılandırma](computer-vision-resource-container-config.md) yapılandırma ayarları
* Gözden geçirme [görüntü işleme genel bakış](Home.md) yazdırılan ve el yazısı metinleri tanıma hakkında daha fazla bilgi edinmek için
* Başvurmak [görüntü işleme API'si](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) kapsayıcı tarafından desteklenen yöntemleri hakkında ayrıntılar için.
* Başvurmak [sık sorulan sorular (SSS)](FAQ.md) görüntü işleme işlevselliği ile ilgili sorunları gidermek için.
* Daha fazla bilişsel [Hizmetler kapsayıcısı](../cognitive-services-container-support.md) kullanın
