---
title: Kapsayıcılar nasıl yüklenir ve çalıştırılır - Bilgisayar Lı Görüş
titleSuffix: Azure Cognitive Services
description: Bu geçiş eğitiminde Computer Vision için kapsayıcıları nasıl karşıdan yükleyin, yükler ve çalıştırın.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 03/17/2020
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: f8d19ded32f7f4a90b23106b6cec53418eef407e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79458044"
---
# <a name="install-and-run-read-containers-preview"></a>Okuma kapları yükleme ve çalıştırma (Önizleme)

Kapsayıcılar, BilgisayarLı Vizyon API'lerini kendi ortamınızda çalıştırmanızı sağlar. Kapsayıcılar belirli güvenlik ve veri yönetimi gereksinimleri için harikadır. Bu makalede, bir Computer Vision kapsayıcısını nasıl indireceğinizi, yükleyeceğinizi ve çalıştıracağınız öğrenilir.

Tek bir Docker konteyner, *Read*, Computer Vision için kullanılabilir. *Read* kapsayıcısı, makbuzlar, posterler ve kartvizitler gibi farklı yüzeylere ve arka planlara sahip çeşitli nesnelerin görüntülerinden *yazdırılan metni* algılamanızı ve ayıklamanızı sağlar. Ayrıca, *Read* kapsayıcısı resimlerdeki *el yazısı metni* algılar ve PDF, TIFF ve çok sayfalı dosya desteği sağlar. Daha fazla bilgi için [Okuma](concept-recognizing-text.md#read-api) API belgelerine bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Kapsayıcıları kullanmadan önce aşağıdaki ön koşulları karşılamanız gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Docker [Engine'in ana bilgisayara](#the-host-computer)yüklenmesi gerekir. Docker, [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms) üzerinde Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temel bilgileri ile ilgili giriş yapmak için [Docker’a genel bakış](https://docs.docker.com/engine/docker-overview/) bölümüne bakın.<br><br> Docker, kapsayıcıların Azure'a bağlanmasına ve fatura verilerini göndermesine izin verecek şekilde yapılandırılmalıdır. <br><br> **Windows'da**Docker, Linux kapsayıcılarını destekleyecek şekilde de yapılandırılmalıdır.<br><br>|
|Docker ile aşinalık | Docker kavramları hakkında, kayıt defterleri, depolar, konteynerler ve konteyner resimleri nin yanı sıra `docker` temel komutlar hakkında temel bilgilere sahip olmalısınız.| 
|Bilgisayarlı Vizyon kaynağı |Kapsayıcıyı kullanabilmek için aşağıdakilere sahip olmalısınız:<br><br>Bir Azure **Computer Vision** kaynağı ve ilişkili API bitiş noktası URI anahtarı. Her iki değer de kaynak için Genel Bakış ve Anahtarlar sayfalarında kullanılabilir ve kapsayıcıyı başlatmak için gereklidir.<br><br>**{API_KEY}**: **Keys** sayfasındaki mevcut iki kaynak anahtarından biri<br><br>**{ENDPOINT_URI}**: **Genel Bakış** sayfasında sağlanan bitiş noktası|

## <a name="request-access-to-the-private-container-registry"></a>Özel konteyner kayıt defterine erişim isteği

[!INCLUDE [Request access to public preview](../../../includes/cognitive-services-containers-request-access.md)]

[!INCLUDE [Gathering required container parameters](../containers/includes/container-gathering-required-parameters.md)]

### <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="advanced-vector-extension-support"></a>Gelişmiş Vektör Uzantısı desteği

**Ana bilgisayar,** docker kapsayıcısını çalıştıran bilgisayardır. Ana bilgisayar [Gelişmiş Vektör Uzantılarını](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#CPUs_with_AVX2) (AVX2) *desteklemelidir.* Aşağıdaki komutla Linux ana bilgisayarlarında AVX2 desteğini kontrol edebilirsiniz:

```console
grep -q avx2 /proc/cpuinfo && echo AVX2 supported || echo No AVX2 support detected
```
> [!WARNING]
> Ana bilgisayar AVX2 desteklemek için *gereklidir.* Konteyner AVX2 desteği olmadan doğru *çalışmaz.*

### <a name="container-requirements-and-recommendations"></a>Konteyner gereksinimleri ve öneriler

[!INCLUDE [Container requirements and recommendations](includes/container-requirements-and-recommendations.md)]

## <a name="get-the-container-image-with-docker-pull"></a>Konteyner görüntüsünü`docker pull`

Read için kapsayıcı görüntüler mevcuttur.

| Kapsayıcı | Konteyner Kayıt Defteri / Depo / Resim Adı |
|-----------|------------|
| Okuma | `containerpreview.azurecr.io/microsoft/cognitive-services-read:latest` |

Kapsayıcı [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) görüntüsünü indirmek için komutu kullanın.

### <a name="docker-pull-for-the-read-container"></a>Read konteyneri için Docker çekme

```bash
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-read:latest
```

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

## <a name="how-to-use-the-container"></a>Kapsayıcı nasıl kullanılır?

Kapsayıcı [ana bilgisayara](#the-host-computer)yüklendikten sonra, kapsayıcıyla çalışmak için aşağıdaki işlemi kullanın.

1. Gerekli fatura ayarlarıyla [kapsayıcıyı çalıştırın.](#run-the-container-with-docker-run) Komutun `docker run` daha fazla [örneği](computer-vision-resource-container-config.md) mevcuttur. 
1. [Kapsayıcının tahmin bitiş noktasını sorgula.](#query-the-containers-prediction-endpoint) 

## <a name="run-the-container-with-docker-run"></a>Kapsayıcıyı çalıştırın`docker run`

Kapsayıcıyı çalıştırmak için [docker run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. Değerleri ve `{API_KEY}` değerleri nasıl elde edineceklerine ilişkin ayrıntılar için [gerekli parametreleri](#gathering-required-parameters) `{ENDPOINT_URI}` toplamaya bakın.

Komut [örnekleri](computer-vision-resource-container-config.md#example-docker-run-commands) `docker run` mevcuttur.

```bash
docker run --rm -it -p 5000:5000 --memory 16g --cpus 8 \
containerpreview.azurecr.io/microsoft/cognitive-services-read \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Şu komut:

* Kapsayıcı görüntüsünden Read kapsayıcısını çalıştırın.
* 8 CPU çekirdeği ve 16 gigabayt (GB) bellek ayırır.
* TCP bağlantı noktası 5000'i açığa çıkarır ve kapsayıcı için sözde TTY ayırır.
* Çıktıktan sonra kapsayıcıotomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.

Komutun `docker run` daha fazla [örneği](./computer-vision-resource-container-config.md#example-docker-run-commands) mevcuttur. 

> [!IMPORTANT]
> `Eula`Kapsayıcıyı `Billing`çalıştırmak `ApiKey` için , ve seçenekler belirtilmelidir; aksi takdirde, kapsayıcı başlamaz.  Daha fazla bilgi için [Faturalandırma'ya](#billing)bakın.

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="query-the-containers-prediction-endpoint"></a>Kapsayıcının tahmin bitiş noktasını sorgula

Kapsayıcı REST tabanlı sorgu tahmin bitiş noktası API'leri sağlar. 

Kapsayıcı API'leri için ana bilgisayarı `http://localhost:5000`kullanın.

### <a name="asynchronous-read"></a>Asynchronous okuyun

Bilgisayar Görme `POST /vision/v2.0/read/core/asyncBatchAnalyze` hizmetinin ilgili REST işlemlerini nasıl kullandığına benzer şekilde görüntüyü eş senkronize okumak için birlikte ve `GET /vision/v2.0/read/operations/{operationId}` işlemleri kullanabilirsiniz. Asynchronous POST yöntemi, `operationId` http GET isteğine identifer olarak kullanılan bir yöntem döndürür.

Havalı UI'den tarayıcıda `asyncBatchAnalyze` genişletmek için "genişlet" seçeneğini belirleyin. Sonra **dosyayı seç'i seçin.** > **Choose file** Bu örnekte, aşağıdaki görüntüyü kullanacağız:

![sekmeler vs boşluklar](media/tabs-vs-spaces.png)

Eşzamanlı POST başarıyla çalıştırıldığında, bir HTTP **202** durum kodu döndürür. Yanıtın bir parçası olarak, `operation-location` istek için sonuç bitiş noktasını tutan bir üstbilgi vardır.

```http
 content-length: 0
 date: Fri, 13 Sep 2019 16:23:01 GMT
 operation-location: http://localhost:5000/vision/v2.0/read/operations/a527d445-8a74-4482-8cb3-c98a65ec7ef9
 server: Kestrel
```

Tam `operation-location` nitelikli URL ve bir HTTP GET üzerinden erişilir. Aşağıda, url'nin önceki resimden yürütülmesinden `operation-location` kaynaklanan JSON yanıtı vereme yanıtı veremehakkı ve

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

### <a name="synchronous-read"></a>Senkron okuma

Görüntüyü eşzamanlı `POST /vision/v2.0/read/core/Analyze` olarak okumak için işlemi kullanabilirsiniz. Görüntü bütünüyle okunduğunda, o zaman ve ancak o zaman API bir JSON yanıtı döndürmektedir. Bunun tek istisnası bir hata oluşursa. Bir hata oluştuğunda aşağıdaki JSON döndürülür:

```json
{
    status: "Failed"
}
```

JSON yanıt nesnesi, eşzamanlı sürümle aynı nesne grafiğine sahiptir. Bir JavaScript kullanıcısıysanız ve tür güvenliği istiyorsanız, JSON yanıtını nesne `AnalyzeResult` olarak atmak için aşağıdaki türler kullanılabilir.

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

Örnek kullanım örneğinde, <a href="https://aka.ms/ts-read-api-types" target="_blank" rel="noopener noreferrer">typeScript kum havuzuna <span class="docon docon-navigate-external x-hidden-focus"></span> bakın</a> ve kullanım kolaylığını görselleştirmek için **Çalıştır'ı** seçin.

## <a name="stop-the-container"></a>Kapsayıcıyı durdurma

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcıyı çıktı [yuvası](./computer-vision-resource-container-config.md#mount-settings) ve günlüğe kaydetme etkinken çalıştırıyorsanız, kapsayıcı kapsayıcıyı çalıştırırken veya çalıştırırken meydana gelen sorunları gidermeye yardımcı olan günlük dosyaları oluşturur.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Faturalandırma

Bilişsel Hizmetler kapsayıcıları, Azure hesabınızdaki ilgili kaynağı kullanarak fatura bilgilerini Azure'a gönderir.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için [yapı kaplarını yapıya kullanabilirsiniz.](./computer-vision-resource-container-config.md)

<!--blogs/samples/video course -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, Computer Vision kapsayıcılarını indirmek, yüklemek ve çalıştırmak için kavramları ve iş akışını öğrendiniz. Özet:

* Computer Vision, Docker için Read'i kapsülleden bir Linux konteyneri sağlar.
* Kapsayıcı görüntüleri Azure'daki "Kapsayıcı Önizleme" kapsayıcı kayıt defterinden indirilir.
* Konteyner görüntüleri Docker'da çalışır.
* Kabın ana bilgisayarı URI'yi belirterek Read kaplarında işlemleri aramak için REST API veya SDK'yı kullanabilirsiniz.
* Bir kapsayıcıyı anında kullanırken fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel Hizmetler kapsayıcıları ölçüm için Azure'a bağlı olmadan çalışma lisansı na sahip değildir. Müşterilerin, konteynerlerin fatura bilgilerini her zaman ölçüm hizmetiyle iletmesini sağlaması gerekir. Bilişsel Hizmetler kapsayıcıları müşteri verilerini (örneğin, analiz edilen resim veya metin) Microsoft'a göndermez.

## <a name="next-steps"></a>Sonraki adımlar

* Yapılandırma ayarları için [kapsayıcıları yapılandırmayı](computer-vision-resource-container-config.md) gözden geçirin
* Yazdırılan ve el yazısıyla yazılmış metni tanıma hakkında daha fazla bilgi edinmek için [Computer Vision'a genel bakışı](Home.md) gözden geçirin
* Kapsayıcı tarafından desteklenen yöntemler hakkında ayrıntılı bilgi için [BilgisayarLı Vizyon API'sine](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) bakın.
* Computer Vision işleviyle ilgili sorunları çözmek için [sık sorulan sorulara (SSS)](FAQ.md) bakın.
* Daha fazla [Bilişsel Hizmet Kapsayıcısı](../cognitive-services-container-support.md) Kullanın
