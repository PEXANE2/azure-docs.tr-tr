---
title: Konuşma kapsayıcılarını yükler
titleSuffix: Azure Cognitive Services
description: Konuşma kapsayıcılarını yükleyip çalıştırın. Konuşmadan metne dönüştürme, uygulamalarınızın, araçlarınızın veya cihazlarınızın tüketebileceği veya görüntüleyeceği gerçek zamanlı olarak metin halinde ses akışları. Metin okuma, giriş metnini insan benzeri sentezleştirilmiş konuşmaya dönüştürür.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: dapine
ms.openlocfilehash: 089b9030debc7489e123d49b5c78052f597ca469
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68348402"
---
# <a name="install-and-run-speech-service-containers"></a>Konuşma hizmeti kapsayıcılarını yükleyip çalıştırma

Konuşma kapsayıcıları, müşterilerin hem güçlü bulut özellikleri hem de kenar yerinin avantajlarından yararlanmak için optimize edilmiş bir konuşma uygulaması mimarisi oluşturmasına imkan tanır. 

İki konuşma kapsayıcısı **konuşmadan metne** ve **metinden konuşmaya**. 

|İşlev|Özellikler|sürümü|
|-|-|--|
|Konuşmayı Metne Dönüştürme| <li>Sürekli gerçek zamanlı konuşmayı veya toplu ses kayıtlarını, ara sonuçlarla metin içine ekleyin.|1.1.3|
|Metin Okuma| <li>Metni, doğal sesli konuşmaya dönüştürür. düz metin girişi veya konuşma sen, biçimlendirme dili (SSML) ile. |1.1.0|

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Konuşma kapsayıcılarını kullanmadan önce aşağıdaki önkoşulları karşılamanız gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Bir [ana bilgisayarda](#the-host-computer)Docker altyapısının yüklü olması gerekir. Docker, [MacOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/)ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms)'ta Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).<br><br> Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır. <br><br> **Windows 'da**Docker 'ın de Linux kapsayıcılarını destekleyecek şekilde yapılandırılması gerekir.<br><br>|
|Docker ile benzerlik | Kayıt defterleri, depolar, kapsayıcılar ve kapsayıcı görüntüleri gibi Docker kavramlarının yanı sıra temel `docker` komutlar hakkında bilgi sahibi olmanız gerekir.| 
|Konuşma kaynağı |Bu kapsayıcıları kullanabilmeniz için, şunları yapmanız gerekir:<br><br>İlişkili API anahtarını ve uç nokta URI 'sini almak için bir Azure _konuşma_ kaynağı. Her iki değer de Azure portal **konuşmaya** genel bakış ve anahtarlar sayfalarında kullanılabilir. Kapsayıcının başlatılması için her ikisi de gereklidir.<br><br>**{API_KEY}** : **Anahtarlar** sayfasındaki kullanılabilir iki kaynak anahtardan biri<br><br>**{ENDPOINT_URI}** : **Genel bakış** sayfasında belirtilen bitiş noktası|

## <a name="request-access-to-the-container-registry"></a>Kapsayıcı kayıt defterine erişim isteme

Kapsayıcıya erişim istemek için önce bilişsel [Hizmetler konuşma kapsayıcıları istek formunu](https://aka.ms/speechcontainerspreview/) doldurmanız ve göndermeniz gerekir. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="advanced-vector-extension-support"></a>Gelişmiş vektör uzantısı desteği

**Konak** , Docker kapsayıcısını çalıştıran bilgisayardır. Ana bilgisayar [Gelişmiş vektör uzantılarını](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#CPUs_with_AVX2) desteklemelidir (AVX2). Bu desteği Linux konaklarında aşağıdaki komutla kontrol edebilirsiniz: 

```console
grep -q avx2 /proc/cpuinfo && echo AVX2 supported || echo No AVX2 support detected
```

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

Aşağıdaki tabloda, her bir konuşma kapsayıcısı için ayrılacak minimum ve önerilen CPU çekirdekleri ve bellek açıklanmaktadır.

| Kapsayıcı | Minimum | Önerilen |
|-----------|---------|-------------|
|bilişsel hizmetler-konuşmayı metne dönüştürme | 2 çekirdek<br>2 GB bellek  | 4 çekirdek<br>4 GB bellek  |
|bilişsel hizmetler-metinden konuşmaya | 1 çekirdek, 0,5 GB bellek| 2 çekirdek, 1 GB bellek |

* Her çekirdek en az 2,6 gigahertz (GHz) veya daha hızlı olmalıdır.

Çekirdek ve bellek, `--cpus` `docker run` komutunun bir parçası `--memory` olarak kullanılan ve ayarlarına karşılık gelir.

**Note**; En düşük ve önerilen, ana makine kaynaklarından *değil* , Docker sınırlarına dayanır. Örneğin, konuşmadan metne kapsayıcı bellek eşleme, büyük bir dil modelinin bölümlerine eşlenir ve tüm dosyanın belleğe sığması _önerilir_ , bu da ek 4-6 GB 'tır. Ayrıca, modellerin bellekte sayfalandığından kapsayıcının ilk çalışması daha uzun sürebilir.

## <a name="get-the-container-image-with-docker-pull"></a>Kapsayıcı görüntüsünü al`docker pull`

Konuşma için kapsayıcı görüntüleri kullanılabilir.

| Kapsayıcı | Havuz |
|-----------|------------|
| bilişsel hizmetler-konuşmayı metne dönüştürme | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest` |
| bilişsel hizmetler-metinden konuşmaya | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest` |

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="language-locale-is-in-container-tag"></a>Dil yerel ayarı kapsayıcı etiketinde

Etiket, `en-us` yerel ayarı ve `jessarus` sesi çeker. `latest`

#### <a name="speech-to-text-locales"></a>Konuşmayı metin yerel ayarlarına dönüştürme

Dışındaki `latest` tüm Etiketler, yerel ayar kapsayıcısını `<culture>` gösterdiği şekilde aşağıdaki biçimdedir:

```
<major>.<minor>.<patch>-<platform>-<culture>-<prerelease>
```

Aşağıdaki etiket, biçiminin bir örneğidir:

```
1.1.3-amd64-en-us-preview
```

Aşağıdaki tabloda, kapsayıcının 1.1.3 sürümünde **konuşmayı metne** yönelik desteklenen yerel ayarlar listelenmiştir:

|Dil yerel ayarı|Tags|
|--|--|
|Çince|`zh-cn`|
|Türkçe |`en-us`<br>`en-gb`<br>`en-au`<br>`en-in`|
|Fransızca |`fr-ca`<br>`fr-fr`|
|Almanca|`de-de`|
|İtalyanca|`it-it`|
|Japonca|`ja-jp`|
|Korece|`ko-kr`|
|Portekizce|`pt-br`|
|İspanyolca|`es-es`<br>`es-mx`|

#### <a name="text-to-speech-locales"></a>Metinden konuşmaya yerel ayarları

Dışındaki `latest` tüm Etiketler aşağıdaki biçimdedir; `<culture>` burada, yerel ayarı `<voice>` gösterir ve kapsayıcının sesini gösterir:

```
<major>.<minor>.<patch>-<platform>-<culture>-<voice>-<prerelease>
```

Aşağıdaki etiket, biçiminin bir örneğidir:

```
1.1.0-amd64-en-us-jessarus-preview
```

Aşağıdaki tabloda, kapsayıcının 1.1.0 sürümünde **metinden konuşmaya** için desteklenen yerel ayarlar listelenmektedir:

|Dil yerel ayarı|Tags|Desteklenen sesler|
|--|--|--|
|Çince|`zh-cn`|kuihuırus<br>kangkang-Apollo<br>yaoyao-Apollo|
|Türkçe |`en-au`|Catherine<br>hayleyrus|
|Türkçe |`en-gb`|George-Apollo<br>hazelrus<br>çiğdem-Apollo|
|Türkçe |`en-in`|Heera-Apollo<br>Primus<br>rampadili-Apollo<br>|
|Türkçe |`en-us`|jessarus<br>benjaminrus<br>jessa24krus<br>zirarus<br>guy24krus|
|Fransızca|`fr-ca`|Caroline dili<br>HARMİ Erus|
|Fransızca|`fr-fr`|hortenserus<br>Julie-Apollo<br>Paul-Apollo|
|Almanca|`de-de`|Hedda<br>heddarus<br>Stefan-Apollo|
|İtalyanca|`it-it`|cosımo-Apollo<br>luciarus|
|Japonca|`ja-jp`|Ayumi-Apollo<br>harukarus<br>Ichiro-Apollo|
|Korece|`ko-kr`|heamirus|
|Portekizce|`pt-br`|Daniel-Apollo<br>heloisarus|
|İspanyolca|`es-es`|elenarus<br>gamze-Apollo<br>Pablo-Apollo<br>|
|İspanyolca|`es-mx`|tepdarus<br>vaul-Apollo|

### <a name="docker-pull-for-the-speech-containers"></a>Konuşma kapsayıcıları için Docker çekme

#### <a name="speech-to-text"></a>Konuşmayı Metne Dönüştürme

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest
```

#### <a name="text-to-speech"></a>Metin okuma

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest
```

## <a name="how-to-use-the-container"></a>Kapsayıcıyı kullanma

Kapsayıcı [ana bilgisayardan](#the-host-computer)olduktan sonra, kapsayıcında çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı](#run-the-container-with-docker-run)gereken ancak kullanılmayan faturalandırma ayarlarıyla çalıştırın. `docker run` Komuta daha fazla [örnek](speech-container-configuration.md#example-docker-run-commands) kullanılabilir.
1. [Kapsayıcının tahmin uç noktasını sorgulayın](#query-the-containers-prediction-endpoint).

## <a name="run-the-container-with-docker-run"></a>Kapsayıcıyı ile çalıştırma`docker run`

Üç kapsayıcının herhangi birini çalıştırmak için [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. Komut aşağıdaki parametreleri kullanır:

**Önizleme sırasında**, kapsayıcıyı başlatmak için faturalandırma ayarlarının geçerli olması gerekir, ancak kullanım için faturalandırılmaz.

| Yer tutucu | Value |
|-------------|-------|
|{API_KEY} | Bu anahtar, kapsayıcıyı başlatmak için kullanılır ve Azure portal konuşma anahtarları sayfasında kullanılabilir.  |
|{ENDPOINT_URI} | Faturalandırma uç noktası URI değeri Azure portal konuşmaya Genel Bakış sayfasında kullanılabilir.|

Aşağıdaki örnek `docker run` komutta bu parametreleri kendi değerlerinizle değiştirin.

### <a name="text-to-speech"></a>Metin okuma

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

### <a name="speech-to-text"></a>Konuşmayı Metne Dönüştürme

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 2 \
containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Bu komut:

* Kapsayıcı görüntüsünden bir konuşma kapsayıcısı çalıştırır
* 2 CPU çekirdeği ve 2 gigabayt (GB) bellek ayırır
* 5000 numaralı TCP bağlantı noktasını kullanıma sunar ve sahte TTY için kapsayıcı ayırır.
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](#billing).

## <a name="query-the-containers-prediction-endpoint"></a>Kapsayıcının tahmin uç noktasını sorgulama

|Kapsayıcı|Uç Nokta|
|--|--|
|Konuşmayı Metne Dönüştürme|ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1|
|Metin okuma|http://localhost:5000/speech/synthesize/cognitiveservices/v1|

### <a name="speech-to-text"></a>Konuşmayı Metne Dönüştürme

Kapsayıcı, [konuşma SDK 'sı](index.yml)aracılığıyla erişilen WebSocket tabanlı sorgu uç noktası API 'leri sağlar.

Varsayılan olarak, konuşma SDK 'Sı çevrimiçi konuşma hizmetlerini kullanır. Kapsayıcıyı kullanmak için başlatma yöntemini değiştirmeniz gerekir. Aşağıdaki örneklere bakın.

#### <a name="for-c"></a>BekleniyorC#

Bu Azure-Cloud başlatma çağrısını kullanarak değiştirin:

```csharp
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

kapsayıcı uç noktasını kullanarak bu çağrıya:

```csharp
var config = SpeechConfig.FromEndpoint(
    new Uri("ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1"),
    "YourSubscriptionKey");
```

#### <a name="for-python"></a>Python için

Bu Azure-Cloud başlatma çağrısını kullanarak Değiştir

```python
speech_config = speechsdk.SpeechConfig(
    subscription=speech_key, region=service_region)
```

kapsayıcı uç noktasını kullanarak bu çağrıya:

```python
speech_config = speechsdk.SpeechConfig(
    subscription=speech_key, endpoint="ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1")
```

### <a name="text-to-speech"></a>Metin okuma

Kapsayıcı, [burada](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-text-to-speech) bulunan REST uç nokta API 'lerini sağlar ve örnekleri [burada](https://azure.microsoft.com/resources/samples/cognitive-speech-tts/)bulabilirsiniz.

[!INCLUDE [Validate container is running - Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Kapsayıcıyı durdur

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcıyı çalıştırdığınızda kapsayıcı, kapsayıcıyı başlatırken veya çalıştırırken oluşan sorunları gidermeye yardımcı olan bilgileri almak için **stdout** ve **stderr** kullanır.

## <a name="billing"></a>Faturalandırma

Konuşma kapsayıcıları, Azure hesabınızdaki bir _konuşma_ kaynağını kullanarak faturalama bilgilerini Azure 'a gönderir.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](speech-container-configuration.md).

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, konuşma kapsayıcılarını indirmek, yüklemek ve çalıştırmak için kavramları ve iş akışını öğrendiniz. Özet:

* Konuşma, Docker için konuşmayı metin ve metinden konuşmaya bir şekilde kapsüllemek için iki Linux kapsayıcısı sağlar.
* Kapsayıcı görüntüleri Azure 'daki özel kapsayıcı kayıt defterinden indirilir.
* Docker kapsayıcı görüntüleri çalıştırın.
* Kapsayıcının ana bilgisayar URI 'sini belirterek, konuşma kapsayıcılarındaki işlemleri çağırmak için REST API veya SDK kullanabilirsiniz.
* Bir kapsayıcıyı örnekledikten sonra faturalandırma bilgilerini sağlamanız gerekir.

> [!IMPORTANT]
>  Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcılar, Microsoft müşteri verilerini (örneğin, görüntü veya metin analiz edilen) göndermeyin.

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [kapsayıcıları yapılandırma](speech-container-configuration.md) yapılandırma ayarları
* Daha fazla bilişsel [Hizmetler kapsayıcısı](../cognitive-services-container-support.md) kullanın
