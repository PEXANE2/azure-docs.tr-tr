---
title: Docker konteynerler - LUIS
titleSuffix: Azure Cognitive Services
description: LUIS konteyneri eğitimli veya yayınlanmış uygulamanızı bir docker konteynerine yükler ve konteynerin API uç noktalarından sorgu tahminlerine erişim sağlar.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/08/2019
ms.author: dapine
ms.openlocfilehash: 308a474970db54022e5351fdf349d9572fbafb0d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219934"
---
# <a name="install-and-run-luis-docker-containers"></a>LUIS docker konteynerleri yükleyin ve çalıştırın
 
Dil Anlama (LUIS) kapsayıcısı eğitimli veya yayınlanmış Dil Anlama modelinizi yükler. Luis [uygulaması](https://www.luis.ai)olarak docker konteyneri, kapsayıcının API uç noktalarından sorgu tahminlerine erişim sağlar. Kapsayıcıdan sorgu günlüklerini toplayabilir ve uygulamanın tahmin doğruluğunu artırmak için bunları Dil Bilgisi uygulamasına geri yükleyebilirsiniz.

Aşağıdaki video bu kapsayıcının kullanılmasını gösterir.

[![Bilişsel Hizmetler için Konteyner gösteri](./media/luis-container-how-to/luis-containers-demo-video-still.png)](https://aka.ms/luis-container-demo)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

LUIS kapsayıcısını çalıştırmak için aşağıdaki ön koşullara dikkat edin:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Docker [Engine'in ana bilgisayara](#the-host-computer)yüklenmesi gerekir. Docker, [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms) üzerinde Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temel bilgileri ile ilgili giriş yapmak için [Docker’a genel bakış](https://docs.docker.com/engine/docker-overview/) bölümüne bakın.<br><br> Docker, kapsayıcıların Azure'a bağlanmasına ve fatura verilerini göndermesine izin verecek şekilde yapılandırılmalıdır. <br><br> **Windows'da**Docker, Linux kapsayıcılarını destekleyecek şekilde de yapılandırılmalıdır.<br><br>|
|Docker ile aşinalık | Docker kavramları hakkında, kayıt defterleri, depolar, konteynerler ve konteyner resimleri nin yanı sıra `docker` temel komutlar hakkında temel bilgilere sahip olmalısınız.| 
|Azure `Cognitive Services` kaynağı ve LUIS [paketli uygulama](luis-how-to-start-new-app.md) dosyası |Kapsayıcıyı kullanabilmek için aşağıdakilere sahip olmalısınız:<br><br>* Bilişsel _Hizmetler_ Azure kaynağı ve ilişkili faturalandırma anahtarı faturalandırma bitiş noktası URI. Her iki değer de kaynak için Genel Bakış ve Anahtarlar sayfalarında kullanılabilir ve kapsayıcıyı başlatmak için gereklidir. <br>* İlgili App ID ile konteynere monte edilmiş bir giriş olarak paketlenmiş eğitimli veya yayınlanmış bir uygulama. Paketlenmiş dosyayı LUIS portalından veya Yazarap'lardan alabilirsiniz. Eğer [yazma API'lerden](#authoring-apis-for-package-file)LUIS paketli uygulama alıyorsanız, ayrıca _Yazma Anahtarı_gerekir.<br><br>Bu gereksinimler komut satırı bağımsız değişkenlerini aşağıdaki değişkenlere geçirmek için kullanılır:<br><br>**{AUTHORING_KEY}**: Bu anahtar, paketlenmiş uygulamayı buluttaki LUIS hizmetinden almak ve sorgu günlüklerini buluta geri yüklemek için kullanılır. Biçim şöyledir: `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.<br><br>**{APP_ID}**: Bu kimlik, Uygulamayı seçmek için kullanılır. Biçim şöyledir: `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`.<br><br>**{API_KEY}**: Bu anahtar kapsayıcıyı başlatmak için kullanılır. Bitiş noktası anahtarını iki yerde bulabilirsiniz. Bunlardan ilki, Bilişsel _Hizmetler_ kaynağının anahtarlar listesindeki Azure portalıdır. Bitiş noktası anahtarı, Keys ve Endpoint ayarları sayfasındaki LUIS portalında da mevcuttur. Başlangıç anahtarını kullanmayın.<br><br>**{ENDPOINT_URI}**: Genel Bakış sayfasında verilen bitiş noktası.<br><br>[Yazma anahtarı ve bitiş noktası anahtarının](luis-boundaries.md#key-limits) farklı amaçları var. Bunları birbirinin yerine kullanmayın. |

[!INCLUDE [Gathering required container parameters](../containers/includes/container-gathering-required-parameters.md)]

### <a name="authoring-apis-for-package-file"></a>Paket dosyası için API yazma

Paket uygulamalar için API yazma:

* [Yayınlanan paket API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-packagepublishedapplicationasgzip)
* [Yayınlanmamış, yalnızca eğitimli paket API'si](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-packagetrainedapplicationasgzip)

### <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Konteyner gereksinimleri ve öneriler

Bu kapsayıcı ayarlar için minimum ve önerilen değerleri destekler:

|Kapsayıcı| Minimum | Önerilen | Tps<br>(Minimum, Maksimum)|
|-----------|---------|-------------|--|
|LUIS|1 çekirdekli, 2 GB bellek|1 çekirdekli, 4 GB bellek|20, 40|

* Her çekirdek en az 2,6 gigahertz (GHz) veya daha hızlı olmalıdır.
* TPS - saniyedeki işlemler

Çekirdek ve bellek, `--cpus` `--memory` komutun bir parçası olarak `docker run` kullanılan ve ayarlara karşılık gelir.

## <a name="get-the-container-image-with-docker-pull"></a>Konteyner görüntüsünü`docker pull`

Depodan [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) bir kapsayıcı görüntüsü indirmek `mcr.microsoft.com/azure-cognitive-services/luis` için komutu kullanın:

```
docker pull mcr.microsoft.com/azure-cognitive-services/luis:latest
```

Önceki komutta kullanılan gibi `latest` kullanılabilir etiketlerin tam açıklaması için Docker Hub'daki [LUIS](https://go.microsoft.com/fwlink/?linkid=2043204) bölümüne bakın.

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

## <a name="how-to-use-the-container"></a>Kapsayıcı nasıl kullanılır?

Kapsayıcı [ana bilgisayara](#the-host-computer)yüklendikten sonra, kapsayıcıyla çalışmak için aşağıdaki işlemi kullanın.

![Dil Anlama (LUIS) kapsayıcısı kullanma süreci](./media/luis-container-how-to/luis-flow-with-containers-diagram.jpg)

1. LUIS portalından veya LUIS API'lerden konteyner için [ihracat paketi.](#export-packaged-app-from-luis)
1. Paket dosyasını [ana bilgisayarda](#the-host-computer)gerekli **giriş** dizinine taşıyın. LUIS paket dosyasını yeniden adlandırmayın, değiştirmeyin, üzerine yazmayın veya sıkıştırmayın.
1. Gerekli _giriş montaj_ ve fatura ayarları ile [kapsayıcı çalıştırın.](#run-the-container-with-docker-run) Komutun `docker run` daha fazla [örneği](luis-container-configuration.md#example-docker-run-commands) mevcuttur. 
1. [Kapsayıcının tahmin bitiş noktasını sorgulama.](#query-the-containers-prediction-endpoint) 
1. Kapsayıcı ile yaptığınızda, LUIS portalındaki çıkış yuvasından [uç nokta günlüklerini aktarın](#import-the-endpoint-logs-for-active-learning) ve kapsayıcıyı [durdurun.](#stop-the-container)
1. Uygulamayı geliştirmek için LUIS portalının [etkin öğrenimini](luis-how-to-review-endpoint-utterances.md) **Gözden Geçir uç nokta lı sözlerle** sayfada kullanın.

Kapsayıcıda çalışan uygulama değiştirilemez. Kapsayıcıdaki uygulamayı değiştirmek için LUIS portalı'nı kullanarak LUIS hizmetindeki uygulamayı değiştirmeniz veya [LUIS](https://www.luis.ai) [yazaraplarını](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f)kullanmanız gerekir. Sonra tren ve / veya yayımlamak, sonra yeni bir paket indirmek ve konteyner tekrar çalıştırın.

Konteynerin içindeki LUIS uygulaması, LUIS hizmetine geri aktarılamaz. Yalnızca sorgu günlükleri yüklenebilir. 

## <a name="export-packaged-app-from-luis"></a>LUIS'ten paket uygulama ihracatı

LUIS konteynerkullanıcı söyleyiştahmin sorguları yanıtlamak için eğitimli veya yayınlanmış LUIS uygulaması gerektirir. LUIS uygulamasını almak için eğitimli veya yayınlanmış paket API'yi kullanın. 

Varsayılan `docker run` konum, `input` komutu çalıştırdığınız yerle ilgili alt dizinidir.  

Paket dosyasını bir dizine yerleştirin ve docker konteynerini çalıştırdığınızda bu dizine giriş yuvası olarak başvurun. 

### <a name="package-types"></a>Paket türleri

Giriş montaj dizini, uygulamanın **Üretim,** **Evreleme**ve **Sürümlü** modellerini aynı anda içerebilir. Tüm paketler monte edilmiştir.

|Paket Türü|Sorgu Bitiş Noktası API|Sorgu kullanılabilirliği|Paket dosya adı biçimi|
|--|--|--|--|
|Sürümlü|GET, POST|Yalnızca konteyner|`{APP_ID}_v{APP_VERSION}.gz`|
|Hazırlama|GET, POST|Azure ve kapsayıcı|`{APP_ID}_STAGING.gz`|
|Üretim|GET, POST|Azure ve kapsayıcı|`{APP_ID}_PRODUCTION.gz`|

> [!IMPORTANT]
> LUIS paket dosyalarını yeniden adlandırmayın, değiştirmeyin, üzerine yazmayın veya sıkıştırmayın.

### <a name="packaging-prerequisites"></a>Ambalaj ön koşulları

BIR LUIS uygulamasını paketlemeden önce aşağıdakileri yapmalısınız:

|Ambalaj Gereksinimleri|Ayrıntılar|
|--|--|
|Azure _Bilişsel Hizmetler_ kaynak örneği|Desteklenen bölgeler şunlardır:<br><br>Batı ABD`westus`( )<br>Batı Avrupa`westeurope`( )<br>Avustralya Doğu`australiaeast`( )|
|Eğitimli veya yayınlanmış LUIS uygulaması|[Desteklenmeyen bağımlılıklar][unsupported-dependencies]olmadan. |
|[Ana bilgisayarın](#the-host-computer)dosya sistemine erişim |Ana bilgisayar bir [giriş montaj](luis-container-configuration.md#mount-settings)izin vermelidir.|
  
### <a name="export-app-package-from-luis-portal"></a>LUIS portalından uygulama paketi ihracatı

LUIS [portalı,](https://www.luis.ai) eğitimli veya yayınlanan uygulamapaketini dışa aktarma olanağı sağlar.

### <a name="export-published-apps-package-from-luis-portal"></a>LUIS portalından yayınlanan uygulama paketini dışa aktarma

Yayınlanan uygulamanın paketi **Uygulamalarım** listesi sayfasından edinilebilir. 

1. LUIS [portalında](https://www.luis.ai)oturum açın.
1. Listedeki uygulama adının solundaki onay kutusunu seçin. 
1. Listenin üstündeki bağlamsal araç çubuğundan **Dışa Aktar** öğesini seçin.
1. **Kapsayıcı için Dışa Aktar 'ı (GZIP) seçin.**
1. **Üretim yuvası** veya **Evreleme yuvası**ortamını seçin.
1. Paket tarayıcıdan indirilir.

![Kap için yayınlanan paketi Uygulama sayfasının Dışa Aktar menüsünden dışa aktarma](./media/luis-container-how-to/export-published-package-for-container.png)

### <a name="export-versioned-apps-package-from-luis-portal"></a>LUIS portalından sürümlü uygulama paketini dışa aktarma

Sürümlü uygulamanın paketi **Sürümler** listesi sayfasından edinilebilir.

1. LUIS [portalında](https://www.luis.ai)oturum açın.
1. Listeden uygulamaya tıklayın. 
1. Uygulamanın navigasyon çubuğunda **Yönet'i** seçin.
1. Sol gezinti çubuğunda **Sürümler'i** seçin.
1. Listedeki sürüm adının solundaki onay kutusunu seçin.
1. Listenin üstündeki bağlamsal araç çubuğundan **Dışa Aktar** öğesini seçin.
1. **Kapsayıcı için Dışa Aktar 'ı (GZIP) seçin.**
1. Paket tarayıcıdan indirilir.

![Kapsayıcı için eğitilen paketi Sürümler sayfasının Dışa Aktar menüsünden dışa aktarma](./media/luis-container-how-to/export-trained-package-for-container.png)

### <a name="export-published-apps-package-from-api"></a>API'den yayınlanan uygulama paketini dışa aktarma

Daha önce [yayınladığınız](luis-how-to-publish-app.md)bir LUIS uygulamasını paketlemek için aşağıdaki REST API yöntemini kullanın. HTTP belirtiminin altındaki tabloyu kullanarak API çağrısındayer tutucular için kendi uygun değerlerinizi değiştirme.

```http
GET /luis/api/v2.0/package/{APP_ID}/slot/{SLOT_NAME}/gzip HTTP/1.1
Host: {AZURE_REGION}.api.cognitive.microsoft.com
Ocp-Apim-Subscription-Key: {AUTHORING_KEY}
```

| Yer tutucu | Değer |
|-------------|-------|
| **{APP_ID}** | Yayınlanan LUIS uygulamasının uygulama kimliği. |
| **{SLOT_NAME}** | Yayınlanan LUIS uygulamasının ortamı. Aşağıdaki değerlerden birini kullanın:<br/>`PRODUCTION`<br/>`STAGING` |
| **{AUTHORING_KEY}** | Yayınlanan LUIS uygulaması için LUIS hesabının yazar anahtarı.<br/>Yazarlık anahtarınızı LUIS portalındaki **Kullanıcı Ayarları** sayfasından alabilirsiniz. |
| **{AZURE_REGION}** | Uygun Azure bölgesi:<br/><br/>`westus`- Batı ABD<br/>`westeurope`- Batı Avrupa<br/>`australiaeast`- Avustralya Doğu |

Yayınlanan paketi indirmek [için, burada API belgelerine][download-published-package]bakın. Başarıyla karşıdan yüklenirse, yanıt bir LUIS paket dosyasıdır. Dosyayı kapsayıcının giriş yuvası için belirtilen depolama konumuna kaydedin. 

### <a name="export-versioned-apps-package-from-api"></a>SÜRÜMlü uygulama paketini API'den dışa aktarma

Önceden [eğitmiş](luis-how-to-train.md)olduğunuz bir LUIS uygulamasını paketlemek için aşağıdaki REST API yöntemini kullanın. HTTP belirtiminin altındaki tabloyu kullanarak API çağrısındayer tutucular için kendi uygun değerlerinizi değiştirme.

```http
GET /luis/api/v2.0/package/{APP_ID}/versions/{APP_VERSION}/gzip HTTP/1.1
Host: {AZURE_REGION}.api.cognitive.microsoft.com
Ocp-Apim-Subscription-Key: {AUTHORING_KEY}
```

| Yer tutucu | Değer |
|-------------|-------|
| **{APP_ID}** | Eğitimli LUIS uygulamasının uygulama kimliği. |
| **{APP_VERSION}** | Eğitimli LUIS uygulamasının uygulama sürümü. |
| **{AUTHORING_KEY}** | Yayınlanan LUIS uygulaması için LUIS hesabının yazar anahtarı.<br/>Yazarlık anahtarınızı LUIS portalındaki **Kullanıcı Ayarları** sayfasından alabilirsiniz. |
| **{AZURE_REGION}** | Uygun Azure bölgesi:<br/><br/>`westus`- Batı ABD<br/>`westeurope`- Batı Avrupa<br/>`australiaeast`- Avustralya Doğu |

Sürümdeki paketi indirmek [için, burada API belgelerine][download-versioned-package]bakın. Başarıyla karşıdan yüklenirse, yanıt bir LUIS paket dosyasıdır. Dosyayı kapsayıcının giriş yuvası için belirtilen depolama konumuna kaydedin. 

## <a name="run-the-container-with-docker-run"></a>Kapsayıcıyı çalıştırın`docker run`

Kapsayıcıyı çalıştırmak için [docker run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. Değerleri ve `{API_KEY}` değerleri nasıl elde edineceklerine ilişkin ayrıntılar için [gerekli parametreleri](#gathering-required-parameters) `{ENDPOINT_URI}` toplamaya bakın.

Komut [örnekleri](luis-container-configuration.md#example-docker-run-commands) `docker run` mevcuttur.

```console
docker run --rm -it -p 5000:5000 ^
--memory 4g ^
--cpus 2 ^
--mount type=bind,src=c:\input,target=/input ^
--mount type=bind,src=c:\output\,target=/output ^
mcr.microsoft.com/azure-cognitive-services/luis ^
Eula=accept ^
Billing={ENDPOINT_URI} ^
ApiKey={API_KEY}
```

* Bu örnek, Windows'da `C:` herhangi bir izin çakışmasını önlemek için sürücü üzerindeki dizin kullanır. Giriş dizini olarak belirli bir dizini kullanmanız gerekiyorsa, docker hizmeti ne izin verebilirsiniz. 
* Docker konteynerleri aşina değilseniz bağımsız değişkenlerin sırasını değiştirmeyin.
* Farklı bir işletim sistemi kullanıyorsanız, sisteminiz için doğru konsol/terminal, bağlar için klasör sözdizimini ve satır devam karakterini kullanın. Bu örnekler, satır devam karakteri `^`olan bir Windows konsolu varsayalım. Kapsayıcı bir Linux işletim sistemi olduğundan, hedef montaj Linux tarzı klasör sözdizimini kullanır.

Şu komut:

* LUIS kapsayıcı görüntüsünden bir kapsayıcı çalıştırın
* *C:\input'taki*giriş yuvasından LUIS uygulamasını yükler , konteyner ana bilgisayarda bulunan
* İki CPU çekirdeği ve 4 gigabayt (GB) bellek ayırır
* TCP bağlantı noktası 5000'i açığa çıkarır ve kapsayıcı için sözde TTY ayırır
* Konteyner ana bilgisayarda bulunan *C:\output'ta*çıkış montajıiçin konteyner ve LUIS günlüklerini kaydeder
* Çıktıktan sonra kapsayıcıotomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir. 

Komutun `docker run` daha fazla [örneği](luis-container-configuration.md#example-docker-run-commands) mevcuttur. 

> [!IMPORTANT]
> `Eula`Kapsayıcıyı `Billing`çalıştırmak `ApiKey` için , ve seçenekler belirtilmelidir; aksi takdirde, kapsayıcı başlamaz.  Daha fazla bilgi için [Faturalandırma'ya](#billing)bakın.
> ApiKey değeri, LUIS portalındaki **Azure Kaynakları** sayfasındaki **Anahtardır** ve `Cognitive Services` Azure kaynak anahtarları sayfasında da kullanılabilir.  

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

## <a name="endpoint-apis-supported-by-the-container"></a>Kapsayıcı tarafından desteklenen uç nokta API'leri

API'nin hem V2 hem de [V3](luis-migration-api-v3.md) sürümleri kapsayıcıda kullanılabilir. 

## <a name="query-the-containers-prediction-endpoint"></a>Kapsayıcının tahmin bitiş noktasını sorgula

Kapsayıcı REST tabanlı sorgu tahmin bitiş noktası API'leri sağlar. Yayımlanmış (evreleme veya üretim) uygulamaları için uç noktaları, sürümlenmiş uygulamalar için uç noktalardan _farklı_ bir rotaya sahiptir.

Kapsayıcı API'leri için ana bilgisayarı `http://localhost:5000`kullanın.

# <a name="v3-prediction-endpoint"></a>[V3 tahmin bitiş noktası](#tab/v3)

|Paket türü|HTTP fiili|Yol|Sorgu parametreleri|
|--|--|--|--|
|Yayımlanmış|GET, POST|`/luis/v3.0/apps/{appId}/slots/{slotName}/predict?`|`query={query}`<br>[`&verbose`]<br>[`&log`]<br>[`&show-all-intents`]|
|Sürümlü|GET, POST|`/luis/v3.0/apps/{appId}/versions/{versionId}/predict?`|`query={query}`<br>[`&verbose`]<br>[`&log`]<br>[`&show-all-intents`]|

Sorgu parametreleri, sorgu yanıtında nasıl ve ne döndürülür yapılandırır:

|Sorgu parametresi|Tür|Amaç|
|--|--|--|
|`query`|string|Kullanıcının söyleyiş.|
|`verbose`|boole|Tahmin edilen modeller için tüm meta verilerin döndürülüp döndürülmeyeceğini gösteren bir boolean değeri. Varsayılan değer false’tur.|
|`log`|boole|Daha sonra [etkin öğrenme](luis-how-to-review-endpoint-utterances.md)için kullanılabilecek sorguları günlüğe kaydeder. Varsayılan değer false’tur.|
|`show-all-intents`|boole|Tüm niyetleri veya sadece üst puanlama niyeti dönmek için olup olmadığını gösteren bir boolean değeri. Varsayılan değer false’tur.|

# <a name="v2-prediction-endpoint"></a>[V2 tahmin bitiş noktası](#tab/v2)

|Paket türü|HTTP fiili|Yol|Sorgu parametreleri|
|--|--|--|--|
|Yayımlanmış|[AL](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78), [POSTA](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)|`/luis/v2.0/apps/{appId}?`|`q={q}`<br>`&staging`<br>[`&timezoneOffset`]<br>[`&verbose`]<br>[`&log`]<br>|
|Sürümlü|GET, POST|`/luis/v2.0/apps/{appId}/versions/{versionId}?`|`q={q}`<br>[`&timezoneOffset`]<br>[`&verbose`]<br>[`&log`]|

Sorgu parametreleri, sorgu yanıtında nasıl ve ne döndürülür yapılandırır:

|Sorgu parametresi|Tür|Amaç|
|--|--|--|
|`q`|string|Kullanıcının söyleyiş.|
|`timezoneOffset`|number|TimezoneOffset, önceden oluşturulmuş varlık datetimeV2 tarafından kullanılan [saat dilimini değiştirmenize](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) olanak tanır.|
|`verbose`|boole|Doğru ayarlandığında tüm niyetleri ve puanlarını döndürür. Varsayılan, yalnızca üst niyeti döndüren yanlıştır.|
|`staging`|boole|Doğru ayarlanmışsa, evreleme ortamı sonuçlarından sorgu döndürür. |
|`log`|boole|Daha sonra [etkin öğrenme](luis-how-to-review-endpoint-utterances.md)için kullanılabilecek sorguları günlüğe kaydeder. Varsayılan değer doğru değeridir.|

***

### <a name="query-the-luis-app"></a>LUIS uygulamasını sorgula

Yayınlanan bir uygulama için kapsayıcıyı sorgulamak için örnek bir CURL komutu:

# <a name="v3-prediction-endpoint"></a>[V3 tahmin bitiş noktası](#tab/v3)

Bir modeli yuvada sorgulamak için aşağıdaki API'yi kullanın:

```bash
curl -G \
-d verbose=false \
-d log=true \
--data-urlencode "query=turn the lights on" \
"http://localhost:5000/luis/v3.0/apps/{APP_ID}/slots/production/predict"
```

**Evreleme** ortamına sorgu yapmak `production` için rotayı `staging`şu şekilde değiştirin:

`http://localhost:5000/luis/v3.0/apps/{APP_ID}/slots/staging/predict`

Sürümlenmiş bir modeli sorgulamak için aşağıdaki API'yi kullanın:

```bash
curl -G \
-d verbose=false \
-d log=false \
--data-urlencode "query=turn the lights on" \
"http://localhost:5000/luis/v3.0/apps/{APP_ID}/versions/{APP_VERSION}/predict"
```

# <a name="v2-prediction-endpoint"></a>[V2 tahmin bitiş noktası](#tab/v2)

Bir modeli yuvada sorgulamak için aşağıdaki API'yi kullanın:

```bash
curl -X GET \
"http://localhost:5000/luis/v2.0/apps/{APP_ID}?q=turn%20on%20the%20lights&staging=false&timezoneOffset=0&verbose=false&log=true" \
-H "accept: application/json"
```
**Hazırlama** ortamına sorgu yapmak **için, hazırlama** sorgusu dize parametre değerini true olarak değiştirin: 

`staging=true`

Sürümlenmiş bir modeli sorgulamak için aşağıdaki API'yi kullanın:

```bash
curl -X GET \
"http://localhost:5000/luis/v2.0/apps/{APP_ID}/versions/{APP_VERSION}?q=turn%20on%20the%20lights&timezoneOffset=0&verbose=false&log=true" \
-H "accept: application/json"
```
Sürüm adı en fazla 10 karakterden sahiptir ve yalnızca URL'de izin verilen karakterleri içerir.

***

## <a name="import-the-endpoint-logs-for-active-learning"></a>Etkin öğrenme için uç nokta günlüklerini alma

LUIS kapsayıcısı için bir çıktı yuvası belirtilirse, uygulama sorgu günlüğü `{INSTANCE_ID}` dosyaları çıkış dizinine kaydedilir ve konteyner kimliği burada dır. Uygulama sorgu günlüğü, LUIS kapsayıcısına gönderilen her tahmin sorgusu için sorgu, yanıt ve zaman damgalarını içerir. 

Aşağıdaki konum, kapsayıcının günlük dosyaları için iç içe olan dizin yapısını gösterir.
```
/output/luis/{INSTANCE_ID}/
```
 
LUIS portalından uygulamanızı seçin ve bu günlükleri yüklemek için **bitiş noktası günlüklerini içe aktar'ı** seçin. 

![Etkin öğrenme için kapsayıcının günlük dosyalarını alma](./media/luis-container-how-to/upload-endpoint-log-files.png)

Günlük yüklendikten sonra, LUIS portalındaki bitiş noktası açıklamalarını [gözden geçirin.](https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-review-endpoint-utterances)

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Kapsayıcıyı durdurma

Kapsayıcıyı kapatmak için, kapsayıcının çalıştığı komut satırı ortamında **Ctrl+C**tuşuna basın.

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcıyı çıktı [yuvası](luis-container-configuration.md#mount-settings) ve günlüğe kaydetme etkinken çalıştırıyorsanız, kapsayıcı kapsayıcıyı çalıştırırken veya çalıştırırken meydana gelen sorunları gidermeye yardımcı olan günlük dosyaları oluşturur.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Faturalandırma

LUIS kapsayıcısı, Azure hesabınızdaki _Bilişsel Hizmetler_ kaynağını kullanarak Azure'a fatura bilgilerini gönderir. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için [yapı kaplarını yapıya kullanabilirsiniz.](luis-container-configuration.md)

<!--blogs/samples/video courses -->
[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, Dil Anlama (LUIS) kapsayıcılarını indirmek, yüklemek ve çalıştırmak için kavramları ve iş akışını öğrendiniz. Özet:

* Dil Anlama (LUIS) Docker için söyleyiş uç nokta sorgu tahminleri sağlayan bir Linux konteyner sağlar.
* Kapsayıcı resimleri Microsoft Kapsayıcı Kayıt Defteri'nden (MCR) indirilir.
* Konteyner görüntüleri Docker'da çalışır.
* Kapsayıcının ana bilgisayarı URI'yi belirterek kapsayıcı uç noktalarını sorgulamak için REST API'yi kullanabilirsiniz.
* Bir kapsayıcıyı anında kullanırken fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel Hizmetler kapsayıcıları ölçüm için Azure'a bağlı olmadan çalışma lisansı na sahip değildir. Müşterilerin, konteynerlerin fatura bilgilerini her zaman ölçüm hizmetiyle iletmesini sağlaması gerekir. Bilişsel Hizmetler kapsayıcıları müşteri verilerini (örneğin, analiz edilen resim veya metin) Microsoft'a göndermez.

## <a name="next-steps"></a>Sonraki adımlar

* Yapılandırma ayarları için [kapsayıcıları yapılandırın.](luis-container-configuration.md)
* Bilinen yetenek kısıtlamaları için [LUIS konteyner sınırlamaları'na](luis-container-limitations.md) bakın.
* LUIS işleviyle ilgili sorunları gidermek için [Sorun Giderme](troubleshooting.md) konusuna bakın.
* Daha fazla [Bilişsel Hizmet Kapsayıcısı](../cognitive-services-container-support.md) Kullanın

<!-- Links - external -->
[download-published-package]: https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-packagepublishedapplicationasgzip
[download-versioned-package]: https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-packagetrainedapplicationasgzip

[unsupported-dependencies]: luis-container-limitations.md#unsupported-dependencies-for-latest-container
