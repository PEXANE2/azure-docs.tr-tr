---
title: Kapsayıcıları yüklemek ve çalıştırmak-Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Bu adım adım öğreticide Görüntü İşleme için kapsayıcıları indirme, yükleme ve çalıştırma.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 05/05/2020
ms.author: aahi
ms.custom: seodec18
ms.openlocfilehash: 13d483507092892187bc13dd23bfa51ed516c890
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89441162"
---
# <a name="install-and-run-read-containers-preview"></a>Okuma kapsayıcıları yükleyip çalıştırma (Önizleme)

Kapsayıcılar, Görüntü İşleme API’lerini kendi ortamınızda çalıştırmanızı sağlar. Kapsayıcılar, belirli güvenlik ve veri idare gereksinimleri için çok kullanışlıdır. Bu makalede bir Görüntü İşleme kapsayıcısını indirme, yükleme ve çalıştırmayı öğreneceksiniz.

Tek bir Docker kapsayıcısı olan *Read*, görüntü işleme için kullanılabilir. *Okuma* kapsayıcısı, çeşitli nesnelerin görüntülerini, alış irsaliyeleri, posterler ve iş kartları gibi farklı yüzeylerle ve arka planlarla *algılayıp ayıklamanızı* sağlar. Ayrıca, *okuma* kapsayıcısı resimlerde *el yazısı metın* ALGıLAR ve PDF, TIFF ve çok sayfalı dosya desteği sağlar. Daha fazla bilgi için bkz. [okuma](concept-recognizing-text.md#read-api) API 'si belgeleri.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/cognitive-services/) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Kapsayıcıları kullanmadan önce aşağıdaki önkoşulları karşılamanız gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Bir [ana bilgisayarda](#the-host-computer)Docker altyapısının yüklü olması gerekir. Docker, [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms) üzerinde Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temel bilgileri ile ilgili giriş yapmak için [Docker’a genel bakış](https://docs.docker.com/engine/docker-overview/) bölümüne bakın.<br><br> Kapsayıcıların Azure 'a bağlanıp faturalandırma verilerini göndermesini sağlamak için Docker yapılandırılmalıdır. <br><br> **Windows 'da**Docker 'ın de Linux kapsayıcılarını destekleyecek şekilde yapılandırılması gerekir.<br><br>|
|Docker ile benzerlik | Kayıt defterleri, depolar, kapsayıcılar ve kapsayıcı görüntüleri gibi Docker kavramlarının yanı sıra temel komutlar hakkında bilgi sahibi olmanız gerekir `docker` .| 
|Görüntü İşleme kaynağı |Kapsayıcısını kullanabilmeniz için şunları yapmanız gerekir:<br><br>Uç nokta URI 'SI olan bir Azure **görüntü işleme** kaynağı ve ilişkili API anahtarı. Her iki değer de kaynak için genel bakış ve anahtarlar sayfalarında bulunur ve kapsayıcıyı başlatmak için gereklidir.<br><br>**{API_KEY}**: **anahtarlar** sayfasında kullanılabilir iki kaynak anahtardan biri<br><br>**{ENDPOINT_URI}**: **genel bakış** sayfasında belirtilen bitiş noktası|

## <a name="request-access-to-the-private-container-registry"></a>Özel kapsayıcı kayıt defterine erişim isteme

Kapsayıcıya erişim istemek için [istek formunu](https://aka.ms/cognitivegate) doldurun ve iletin. 

[!INCLUDE [Request access to public preview](../../../includes/cognitive-services-containers-request-access.md)]

[!INCLUDE [Gathering required container parameters](../containers/includes/container-gathering-required-parameters.md)]

### <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="advanced-vector-extension-support"></a>Gelişmiş vektör uzantısı desteği

**Ana** bilgisayar, Docker kapsayıcısını çalıştıran bilgisayardır. Ana bilgisayar *must support* [Gelişmiş vektör uzantılarını](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#CPUs_with_AVX2) desteklemelidir (AVX2). Aşağıdaki komutla Linux konaklarda AVX2 desteğini denetleyebilirsiniz:

```console
grep -q avx2 /proc/cpuinfo && echo AVX2 supported || echo No AVX2 support detected
```

> [!WARNING]
> Ana bilgisayar, AVX2 desteklemek için *gereklidir* . Kapsayıcı, AVX2 desteği olmadan *düzgün çalışmaz.*

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

[!INCLUDE [Container requirements and recommendations](includes/container-requirements-and-recommendations.md)]

## <a name="get-the-container-image-with-docker-pull"></a>Kapsayıcı görüntüsünü al `docker pull`

Okuma için kapsayıcı görüntüleri kullanılabilir.

| Kapsayıcı | Container Registry/depo/görüntü adı |
|-----------|------------|
| Okuma | `containerpreview.azurecr.io/microsoft/cognitive-services-read:2.0` |

[`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/)Bir kapsayıcı görüntüsünü indirmek için komutunu kullanın.

### <a name="docker-pull-for-the-read-container"></a>Okuma kapsayıcısı için Docker çekme

```bash
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-read:2.0
```

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

## <a name="how-to-use-the-container"></a>Kapsayıcıyı kullanma

Kapsayıcı [ana bilgisayardan](#the-host-computer)olduktan sonra, kapsayıcında çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı](#run-the-container-with-docker-run)gerekli faturalandırma ayarlarıyla çalıştırın. Komuta [examples](computer-vision-resource-container-config.md) daha fazla örnek `docker run` kullanılabilir. 
1. [Kapsayıcının tahmin uç noktasını sorgulayın](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Kapsayıcıyı ile çalıştırma `docker run`

Kapsayıcıyı çalıştırmak için [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. Ve değerlerini alma hakkında ayrıntılar için [gerekli parametreleri toplama](#gathering-required-parameters) bölümüne bakın `{ENDPOINT_URI}` `{API_KEY}` .

Komut [örnekleri](computer-vision-resource-container-config.md#example-docker-run-commands) `docker run` mevcuttur.

```bash
docker run --rm -it -p 5000:5000 --memory 16g --cpus 8 \
containerpreview.azurecr.io/microsoft/cognitive-services-read \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Şu komut:

* Kapsayıcı görüntüsünden okuma kapsayıcısını çalıştırır.
* 8 CPU çekirdeği ve 16 gigabayt (GB) bellek ayırır.
* TCP bağlantı noktası 5000 ' i gösterir ve kapsayıcı için bir sözde TTY ayırır.
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.

Komuta [examples](./computer-vision-resource-container-config.md#example-docker-run-commands) daha fazla örnek `docker run` kullanılabilir. 

> [!IMPORTANT]
> `Eula` `Billing` `ApiKey` Kapsayıcıyı çalıştırmak için, ve seçenekleri belirtilmelidir; Aksi takdirde kapsayıcı başlatılmaz.  Daha fazla bilgi için bkz. [faturalandırma](#billing).

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="query-the-containers-prediction-endpoint"></a>Kapsayıcının tahmin uç noktasını sorgulama

Kapsayıcı REST tabanlı sorgu tahmin uç noktası API’lerini sağlar. 

Kapsayıcı API’leri için `http://localhost:5000` konağını kullanın.

### <a name="asynchronous-read"></a>Zaman uyumsuz okuma

`POST /vision/v2.0/read/core/asyncBatchAnalyze` `GET /vision/v2.0/read/operations/{operationId}` Görüntü işleme hizmetinin ilgili Rest işlemlerini nasıl kullandığına benzer şekilde bir görüntüyü zaman uyumsuz olarak okumak için konser içindeki ve işlemlerini kullanabilirsiniz. Zaman uyumsuz POST yöntemi, `operationId` HTTP GET isteğine tanımlayıcı olarak kullanılan bir döndürür.

Swagger kullanıcı arabiriminden, `asyncBatchAnalyze` tarayıcıda genişletmek için öğesini seçin. Ardından **deneyin**  >  **dosyayı**seçin öğesini seçin. Bu örnekte, aşağıdaki görüntüyü kullanacağız:

![sekmeler vs alanları](media/tabs-vs-spaces.png)

Zaman uyumsuz GÖNDERI başarıyla çalıştırıldığında, bir **HTTP 202** durum kodu döndürür. Yanıtın bir parçası olarak, `operation-location` isteğin sonuç uç noktasını tutan bir üst bilgi vardır.

```http
 content-length: 0
 date: Fri, 13 Sep 2019 16:23:01 GMT
 operation-location: http://localhost:5000/vision/v2.0/read/operations/a527d445-8a74-4482-8cb3-c98a65ec7ef9
 server: Kestrel
```

, `operation-location` Tam URL 'dir ve bır http get aracılığıyla erişilir. Önceki görüntüden URL 'yi yürütmenin JSON yanıtı aşağıda verilmiştir `operation-location` :

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
          "boundingBox": [ 56, 39, 317, 50, 313, 134, 53, 123 ],
          "text": "Tabs VS",
          "words": [
            {
              "boundingBox": [ 90, 43, 243, 53, 243, 123, 94, 125 ],
              "text": "Tabs",
              "confidence": "Low"
            },
            {
              "boundingBox": [ 259, 55, 313, 62, 313, 122, 259, 123 ],
              "text": "VS"
            }
          ]
        },
        {
          "boundingBox": [ 221, 148, 417, 146, 417, 206, 227, 218 ],
          "text": "Spaces",
          "words": [
            {
              "boundingBox": [ 230, 148, 416, 141, 419, 211, 232, 218 ],
              "text": "Spaces"
            }
          ]
        }
      ]
    }
  ]
}
```

> [!IMPORTANT]
> Bir yük dengeleyicinin arkasında birden çok okuma kapsayıcısı dağıtırsanız (örneğin, Docker Compose veya Kubernetes), bir dış önbelleğiniz olması gerekir. İşlem kapsayıcısı ve GET isteği kapsayıcısı aynı olamaz, çünkü bir dış önbellek sonuçları depolar ve kapsayıcılar arasında paylaşır. Önbellek ayarları hakkında daha fazla bilgi için bkz. [görüntü işleme Docker kapsayıcılarını yapılandırma](https://docs.microsoft.com/azure/cognitive-services/computer-vision/computer-vision-resource-container-config).

### <a name="synchronous-read"></a>Zaman uyumlu okuma

`POST /vision/v2.0/read/core/Analyze`İşlemi eşzamanlı olarak bir görüntüyü okumak için kullanabilirsiniz. Resim tamamen okunsa ve yalnızca API bir JSON yanıtı döndürüyor. Bunun tek istisnası bir hata meydana gelir. Bir hata oluştuğunda aşağıdaki JSON döndürülür:

```json
{
    "status": "Failed"
}
```

JSON yanıt nesnesi, zaman uyumsuz sürümle aynı nesne grafiğine sahiptir. Bir JavaScript kullanıcısı ve tür güvenliği isterseniz, JSON yanıtını bir nesne olarak dönüştürmek için aşağıdaki türler kullanılabilir `AnalyzeResult` .

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

export enum Confidence {
    High = 0,
    Low = 1
}

export interface Word {
  boundingBox?: number[] | null;
  text: string;
  confidence?: Confidence | null;
}
```

Örnek kullanım örneği için, <a href="https://aka.ms/ts-read-api-types" target="_blank" rel="noopener noreferrer">burada <span class="docon docon-navigate-external x-hidden-focus"></span> TypeScript korumalı alanı</a> ' na bakın ve kullanım kolaylığını görselleştirmek için **Çalıştır** ' ı seçin.

## <a name="stop-the-container"></a>Kapsayıcıyı durdurma

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

Bu makalede, Görüntü İşleme kapsayıcıları indirmek, yüklemek ve çalıştırmak için kavramları ve iş akışını öğrendiniz. Özet:

* Görüntü İşleme, Docker için bir Linux kapsayıcısı sağlar ve bu da kapsülleme okur.
* Kapsayıcı görüntüleri, Azure 'daki "kapsayıcı önizlemesi" kapsayıcı kayıt defterinden indirilir.
* Kapsayıcı görüntüleri Docker 'da çalışır.
* Kapsayıcının ana bilgisayar URI 'sini belirterek okuma kapsayıcılarındaki işlemleri çağırmak için REST API veya SDK kullanabilirsiniz.
* Bir kapsayıcıyı örnekledikten sonra faturalandırma bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel hizmetler kapsayıcıları, ölçüm için Azure 'a bağlı kalmadan çalıştırılmak üzere lisanslanmaz. Müşterilerin, ödeme bilgilerini her zaman ölçüm hizmetiyle iletişimine olanak tanımak için kapsayıcıların etkinleştirilmesi gerekir. Bilişsel hizmetler kapsayıcıları, müşteri verilerini (örneğin, çözümlenmekte olan resim veya metin) Microsoft 'a göndermez.

## <a name="next-steps"></a>Sonraki adımlar

* Yapılandırma ayarları için [kapsayıcıları](computer-vision-resource-container-config.md) yapılandırmayı gözden geçir
* Basılan ve el yazısı metni tanıma hakkında daha fazla bilgi edinmek için [görüntü işleme genel bakışı](overview.md) inceleyin
* Kapsayıcı tarafından desteklenen yöntemler hakkındaki ayrıntılar için [görüntü işleme API'si](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) başvurun.
* Görüntü İşleme işlevlerle ilgili sorunları çözmek için [sık sorulan sorular (SSS)](FAQ.md) bölümüne bakın.
* Daha fazla bilişsel [Hizmetler kapsayıcısı](../cognitive-services-container-support.md) kullanın
