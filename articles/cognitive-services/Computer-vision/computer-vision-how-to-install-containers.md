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
ms.date: 09/03/2020
ms.author: aahi
ms.custom: seodec18
ms.openlocfilehash: bc55ab2697d8278bd975f618d17804499ba0128d
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90982095"
---
# <a name="install-and-run-read-containers-preview"></a>Okuma kapsayıcıları yükleyip çalıştırma (Önizleme)

[!INCLUDE [container hosting on the Microsoft Container Registry](../containers/includes/gated-container-hosting.md)]

Kapsayıcılar, Görüntü İşleme API’lerini kendi ortamınızda çalıştırmanızı sağlar. Kapsayıcılar, belirli güvenlik ve veri idare gereksinimleri için çok kullanışlıdır. Bu makalede bir Görüntü İşleme kapsayıcısını indirme, yükleme ve çalıştırmayı öğreneceksiniz.

*Okuma* kapsayıcısı, çeşitli nesnelerin görüntülerini, alış irsaliyeleri, posterler ve iş kartları gibi farklı yüzeylerle ve arka planlarla *algılayıp ayıklamanızı* sağlar. Ayrıca, *okuma* kapsayıcısı resimlerde *el yazısı metın* ALGıLAR ve PDF, TIFF ve çok sayfalı dosya desteği sağlar. Daha fazla bilgi için bkz. [okuma](concept-recognizing-text.md#read-api) API 'si belgeleri.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/cognitive-services/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Kapsayıcıları kullanmadan önce aşağıdaki önkoşulları karşılamanız gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Bir [ana bilgisayarda](#the-host-computer)Docker altyapısının yüklü olması gerekir. Docker, [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms) üzerinde Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temel bilgileri ile ilgili giriş yapmak için [Docker’a genel bakış](https://docs.docker.com/engine/docker-overview/) bölümüne bakın.<br><br> Kapsayıcıların Azure 'a bağlanıp faturalandırma verilerini göndermesini sağlamak için Docker yapılandırılmalıdır. <br><br> **Windows 'da**Docker 'ın de Linux kapsayıcılarını destekleyecek şekilde yapılandırılması gerekir.<br><br>|
|Docker ile benzerlik | Kayıt defterleri, depolar, kapsayıcılar ve kapsayıcı görüntüleri gibi Docker kavramlarının yanı sıra temel komutlar hakkında bilgi sahibi olmanız gerekir `docker` .| 
|Görüntü İşleme kaynağı |Kapsayıcısını kullanabilmeniz için şunları yapmanız gerekir:<br><br>Uç nokta URI 'SI olan bir Azure **görüntü işleme** kaynağı ve ilişkili API anahtarı. Her iki değer de kaynak için genel bakış ve anahtarlar sayfalarında bulunur ve kapsayıcıyı başlatmak için gereklidir.<br><br>**{API_KEY}**: **anahtarlar** sayfasında kullanılabilir iki kaynak anahtardan biri<br><br>**{ENDPOINT_URI}**: **genel bakış** sayfasında belirtilen bitiş noktası|

## <a name="request-approval-to-run-the-container"></a>Kapsayıcıyı çalıştırmak için onay isteyin

Kapsayıcıyı çalıştırmak için onay istemek üzere [istek formunu](https://aka.ms/cognitivegate) doldurun ve iletin. 

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
| Okuma 3,0-Önizleme | `mcr.microsoft.com/azure-cognitive-services/vision/read:3.0-preview` |
| Okuma 3,1-Önizleme | `mcr.microsoft.com/azure-cognitive-services/vision/read:3.1-preview` |

[`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/)Bir kapsayıcı görüntüsünü indirmek için komutunu kullanın.

### <a name="docker-pull-for-the-read-container"></a>Okuma kapsayıcısı için Docker çekme

# <a name="version-31-preview"></a>[Sürüm 3,1-Önizleme](#tab/version-3-1)

```bash
docker pull mcr.microsoft.com/azure-cognitive-services/vision/read:3.1-preview
```

# <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

```bash
docker pull mcr.microsoft.com/azure-cognitive-services/vision/read:3.0-preview
```

---

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

## <a name="how-to-use-the-container"></a>Kapsayıcıyı kullanma

Kapsayıcı [ana bilgisayardan](#the-host-computer)olduktan sonra, kapsayıcında çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı](#run-the-container-with-docker-run)gerekli faturalandırma ayarlarıyla çalıştırın. Komuta [examples](computer-vision-resource-container-config.md) daha fazla örnek `docker run` kullanılabilir. 
1. [Kapsayıcının tahmin uç noktasını sorgulayın](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Kapsayıcıyı ile çalıştırma `docker run`

Kapsayıcıyı çalıştırmak için [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. Ve değerlerini alma hakkında ayrıntılar için [gerekli parametreleri toplama](#gathering-required-parameters) bölümüne bakın `{ENDPOINT_URI}` `{API_KEY}` .

Komut [örnekleri](computer-vision-resource-container-config.md#example-docker-run-commands) `docker run` mevcuttur.

# <a name="version-31-preview"></a>[Sürüm 3,1-Önizleme](#tab/version-3-1)

```bash
docker run --rm -it -p 5000:5000 --memory 18g --cpus 8 \
mcr.microsoft.com/azure-cognitive-services/vision/read:3.1-preview \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Şu komut:

* Kapsayıcı görüntüsünden okuma kapsayıcısını çalıştırır.
* 8 CPU çekirdeği ve 18 gigabayt (GB) bellek ayırır.
* TCP bağlantı noktası 5000 ' i gösterir ve kapsayıcı için bir sözde TTY ayırır.
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.

# <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

```bash
docker run --rm -it -p 5000:5000 --memory 18g --cpus 8 \
mcr.microsoft.com/azure-cognitive-services/vision/read:3.0-preview \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}

```

Şu komut:

* Kapsayıcı görüntüsünden okuma kapsayıcısını çalıştırır.
* 8 CPU çekirdeği ve 18 gigabayt (GB) bellek ayırır.
* TCP bağlantı noktası 5000 ' i gösterir ve kapsayıcı için bir sözde TTY ayırır.
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.

---


Komuta [examples](./computer-vision-resource-container-config.md#example-docker-run-commands) daha fazla örnek `docker run` kullanılabilir. 

> [!IMPORTANT]
> `Eula` `Billing` `ApiKey` Kapsayıcıyı çalıştırmak için, ve seçenekleri belirtilmelidir; Aksi takdirde kapsayıcı başlatılmaz.  Daha fazla bilgi için bkz. [faturalandırma](#billing).

Daha yüksek aktarım hızına ihtiyacınız varsa (örneğin, çok sayfalı dosyaları işlerken), [Azure depolama](https://docs.microsoft.com/azure/storage/common/storage-account-create) ve [Azure kuyruğu](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction)kullanarak [bir Kubernetes kümesine](deploy-computer-vision-on-premises.md)birden çok v 3.0 veya v 3.1 kapsayıcısı dağıtmanızı düşünün.

İşleme için görüntüleri depolamak üzere Azure Storage kullanıyorsanız, kapsayıcıyı çağırırken kullanılacak bir [bağlantı dizesi](https://docs.microsoft.com/azure/storage/common/storage-configure-connection-string) oluşturabilirsiniz.

Bağlantı dizenizi bulmak için:

1. Azure portal **depolama hesaplarına** gidin ve hesabınızı bulun.
2. Sol gezinti listesindeki **erişim tuşları** ' na tıklayın.
3. Bağlantı dizeniz, **bağlantı dizesinin** altında bulunur

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="query-the-containers-prediction-endpoint"></a>Kapsayıcının tahmin uç noktasını sorgulama

Kapsayıcı REST tabanlı sorgu tahmin uç noktası API’lerini sağlar. 

# <a name="version-31-preview"></a>[Sürüm 3,1-Önizleme](#tab/version-3-1)

Kapsayıcı API’leri için `http://localhost:5000` konağını kullanın. Swagger yolunu şurada görebilirsiniz: `http://localhost:5000/swagger/vision-v3.0-preview-read/swagger.json` .

# <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

Kapsayıcı API’leri için `http://localhost:5000` konağını kullanın. Swagger yolunu şurada görebilirsiniz: `http://localhost:5000/swagger/vision-v3.1-preview-read/swagger.json` .

---

### <a name="asynchronous-read"></a>Zaman uyumsuz okuma


# <a name="version-31-preview"></a>[Sürüm 3,1-Önizleme](#tab/version-3-1)

`POST /vision/v3.1/read/analyze` `GET /vision/v3.1/read/operations/{operationId}` Görüntü işleme hizmetinin ilgili Rest işlemlerini nasıl kullandığına benzer şekilde bir görüntüyü zaman uyumsuz olarak okumak için konser içindeki ve işlemlerini kullanabilirsiniz. Zaman uyumsuz POST yöntemi, `operationId` HTTP GET isteğine tanımlayıcı olarak kullanılan bir döndürür.


Swagger kullanıcı arabiriminden, `asyncBatchAnalyze` tarayıcıda genişletmek için öğesini seçin. Ardından **deneyin**  >  **dosyayı**seçin öğesini seçin. Bu örnekte, aşağıdaki görüntüyü kullanacağız:

![sekmeler vs alanları](media/tabs-vs-spaces.png)

Zaman uyumsuz GÖNDERI başarıyla çalıştırıldığında, bir **HTTP 202** durum kodu döndürür. Yanıtın bir parçası olarak, `operation-location` isteğin sonuç uç noktasını tutan bir üst bilgi vardır.

```http
 content-length: 0
 date: Fri, 04 Sep 2020 16:23:01 GMT
 operation-location: http://localhost:5000/vision/v3.1/read/operations/a527d445-8a74-4482-8cb3-c98a65ec7ef9
 server: Kestrel
```

, `operation-location` Tam URL 'dir ve bır http get aracılığıyla erişilir. Önceki görüntüden URL 'yi yürütmenin JSON yanıtı aşağıda verilmiştir `operation-location` :

```json
{
  "status": "succeeded",
  "createdDateTime": "2020-09-02T10:30:14Z",
  "lastUpdatedDateTime": "2020-09-02T10:30:15Z",
  "analyzeResult": {
    "version": "3.1.0",
    "readResults": [
      {
        "page": 1,
        "angle": 2.12,
        "width": 502,
        "height": 252,
        "unit": "pixel",
        "language": "",
        "lines": [
          {
            "boundingBox": [58, 42, 314, 59, 311, 123, 56, 121],
            "text": "Tabs vs",
            "appearance": {
              "style": "handwriting",
              "styleConfidence": 0.999
            },
            "words": [
              {
                "boundingBox": [85, 45, 242, 62, 241, 122, 83, 123],
                "text": "Tabs",
                "confidence": 0.981
              },
              {
                "boundingBox": [258, 64, 314, 72, 314, 123, 256, 123],
                "text": "vs",
                "confidence": 0.958
              }
            ]
          },
          {
            "boundingBox": [286, 171, 415, 165, 417, 197, 287, 201],
            "text": "paces",
            "appearance": {
              "style": "print",
              "styleConfidence": 0.603
            },
            "words": [
              {
                "boundingBox": [303, 175, 415, 167, 415, 198, 306, 199],
                "text": "paces",
                "confidence": 0.918
              }
            ]
          }
        ]
      }
    ]
  }
}
```

# <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

`POST /vision/v3.0/read/analyze` `GET /vision/v3.0/read/operations/{operationId}` Görüntü işleme hizmetinin ilgili Rest işlemlerini nasıl kullandığına benzer şekilde bir görüntüyü zaman uyumsuz olarak okumak için konser içindeki ve işlemlerini kullanabilirsiniz. Zaman uyumsuz POST yöntemi, `operationId` HTTP GET isteğine tanımlayıcı olarak kullanılan bir döndürür.

Swagger kullanıcı arabiriminden, `asyncBatchAnalyze` tarayıcıda genişletmek için öğesini seçin. Ardından **deneyin**  >  **dosyayı**seçin öğesini seçin. Bu örnekte, aşağıdaki görüntüyü kullanacağız:

![sekmeler vs alanları](media/tabs-vs-spaces.png)

Zaman uyumsuz GÖNDERI başarıyla çalıştırıldığında, bir **HTTP 202** durum kodu döndürür. Yanıtın bir parçası olarak, `operation-location` isteğin sonuç uç noktasını tutan bir üst bilgi vardır.

```http
 content-length: 0
 date: Fri, 04 Sep 2020 16:23:01 GMT
 operation-location: http://localhost:5000/vision/v3.0/read/operations/a527d445-8a74-4482-8cb3-c98a65ec7ef9
 server: Kestrel
```

, `operation-location` Tam URL 'dir ve bır http get aracılığıyla erişilir. Önceki görüntüden URL 'yi yürütmenin JSON yanıtı aşağıda verilmiştir `operation-location` :

```json
{
  "status": "succeeded",
  "createdDateTime": "2020-09-02T10:24:49Z",
  "lastUpdatedDateTime": "2020-09-02T10:24:50Z",
  "analyzeResult": {
    "version": "3.0.0",
    "readResults": [
      {
        "page": 1,
        "angle": 2.12,
        "width": 502,
        "height": 252,
        "unit": "pixel",
        "language": "",
        "lines": [
          {
            "boundingBox": [58, 42, 314, 59, 311, 123, 56, 121],
            "text": "Tabs vs",
            "words": [
              {
                "boundingBox": [85, 45, 242, 62, 241, 122, 83, 123],
                "text": "Tabs",
                "confidence": 0.981
              },
              {
                "boundingBox": [258, 64, 314, 72, 314, 123, 256, 123],
                "text": "vs",
                "confidence": 0.958
              }
            ]
          },
          {
            "boundingBox": [286, 171, 415, 165, 417, 197, 287, 201],
            "text": "paces",
            "words": [
              {
                "boundingBox": [303, 175, 415, 167, 415, 198, 306, 199],
                "text": "paces",
                "confidence": 0.918
              }
            ]
          }
        ]
      }
    ]
  }
}
```

---

> [!IMPORTANT]
> Bir yük dengeleyicinin arkasında birden çok okuma kapsayıcısı dağıtırsanız (örneğin, Docker Compose veya Kubernetes), bir dış önbelleğiniz olması gerekir. İşlem kapsayıcısı ve GET isteği kapsayıcısı aynı olamaz, çünkü bir dış önbellek sonuçları depolar ve kapsayıcılar arasında paylaşır. Önbellek ayarları hakkında daha fazla bilgi için bkz. [görüntü işleme Docker kapsayıcılarını yapılandırma](https://docs.microsoft.com/azure/cognitive-services/computer-vision/computer-vision-resource-container-config).

### <a name="synchronous-read"></a>Zaman uyumlu okuma

Bir görüntüyü eşzamanlı olarak okumak için aşağıdaki işlemi kullanabilirsiniz. 

# <a name="version-31-preview"></a>[Sürüm 3,1-Önizleme](#tab/version-3-1)

`POST /vision/v3.1/read/syncAnalyze` 

# <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

`POST /vision/v3.0/read/SyncAnalyze`

---

Resim tamamen okunsa ve yalnızca API bir JSON yanıtı döndürüyor. Bunun tek istisnası bir hata meydana gelir. Bir hata oluştuğunda aşağıdaki JSON döndürülür:

```json
{
    "status": "Failed"
}
```

JSON yanıt nesnesi, zaman uyumsuz sürümle aynı nesne grafiğine sahiptir. Bir JavaScript kullanıcısı ve tür güvenliği istiyorsanız, JSON yanıtını dönüştürmek için TypeScript kullanmayı düşünün.

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
