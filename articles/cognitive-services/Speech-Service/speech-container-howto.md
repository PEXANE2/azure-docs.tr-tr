---
title: Konuşma kapsayıcıları yüklemesi-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma kapsayıcılarını yükleyip çalıştırın. Konuşmadan metne dönüştürme, uygulamalarınızın, araçlarınızın veya cihazlarınızın tüketebileceği veya görüntüleyeceği gerçek zamanlı olarak metin halinde ses akışları. Metin okuma, giriş metnini insan benzeri sentezleştirilmiş konuşmaya dönüştürür.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/05/2020
ms.author: aahi
ms.openlocfilehash: 84000016af4b16bd0716c15e7c27b4ab56614532
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83702173"
---
# <a name="install-and-run-speech-service-containers-preview"></a>Konuşma hizmeti kapsayıcılarını (Önizleme) yükleyip çalıştırın

Kapsayıcılar, konuşma hizmeti API 'Lerinden bazılarını kendi ortamınızda çalıştırmanızı sağlar. Kapsayıcılar, belirli güvenlik ve veri idare gereksinimleri için harika. Bu makalede, bir konuşma kapsayıcısının nasıl indirileceği, yükleneceği ve çalıştırılacağı hakkında bilgi edineceksiniz.

Konuşma kapsayıcıları, müşterilerin hem güçlü bulut özellikleri hem de kenar konumu için iyileştirilmiş bir konuşma uygulaması mimarisi oluşturmasını sağlar. Dört farklı kapsayıcı mevcuttur. İki standart kapsayıcı, **konuşmadan metne** ve **metinden konuşmaya**. İki özel kapsayıcı **özel konuşma tanıma metin** ve **özel metinden konuşmaya**. Konuşma kapsayıcıları, bulut tabanlı Azure konuşma hizmetleriyle aynı [fiyatlandırmaya](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) sahiptir.

> [!IMPORTANT]
> Tüm konuşma kapsayıcıları Şu anda [Genel "geçitli" önizlemenin](../cognitive-services-container-support.md#public-gated-preview-container-registry-containerpreviewazurecrio)bir parçası olarak sunulmaktadır. Konuşma kapsayıcıları genel kullanıma (GA) İlerlemede bir duyuru yapılır.

| İşlev | Özellikler | En son |
|--|--|--|
| Konuşmayı metne dönüştürme | , Yaklaşım ve sürekli gerçek zamanlı konuşma veya toplu ses kayıtlarını ara sonuçlarla analiz eder.  | 2.2.0 |
| Özel Konuşma Tanıma metin | [Özel konuşma tanıma portalından](https://speech.microsoft.com/customspeech)özel bir model kullanarak, sürekli gerçek zamanlı konuşmayı veya toplu ses kayıtlarını, ara sonuçlarla birlikte metne ekleyin. | 2.2.0 |
| Metin okuma | Düz metin girişi veya konuşma birleştirme biçimlendirme dili (SSML) ile metni doğal-sounkonuşmaya dönüştürür. | 1.3.0 |
| Özel metin okuma | [Özel ses portalından](https://aka.ms/custom-voice-portal)özel bir model kullanarak, düz metin girişi veya konuşma birleştirme biçimlendirme DILI (SSML) ile metni doğal-sounkonuşmaya dönüştürür. | 1.3.0 |

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Konuşma kapsayıcılarını kullanmadan önce aşağıdaki Önkoşullar:

| Gerekli | Amaç |
|--|--|
| Docker altyapısı | Bir [ana bilgisayarda](#the-host-computer)Docker altyapısının yüklü olması gerekir. Docker, [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms) üzerinde Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temel bilgileri ile ilgili giriş yapmak için [Docker’a genel bakış](https://docs.docker.com/engine/docker-overview/) bölümüne bakın.<br><br> Kapsayıcıların Azure 'a bağlanıp faturalandırma verilerini göndermesini sağlamak için Docker yapılandırılmalıdır. <br><br> **Windows 'da**Docker 'ın de Linux kapsayıcılarını destekleyecek şekilde yapılandırılması gerekir.<br><br> |
| Docker ile benzerlik | Kayıt defterleri, depolar, kapsayıcılar ve kapsayıcı görüntüleri gibi Docker kavramlarının yanı sıra temel komutlar hakkında bilgi sahibi olmanız gerekir `docker` . |
| Konuşma kaynağı | Bu kapsayıcıları kullanabilmeniz için, şunları yapmanız gerekir:<br><br>İlişkili API anahtarını ve uç nokta URI 'sini almak için bir Azure _konuşma_ kaynağı. Her iki değer de Azure portal **konuşmaya** genel bakış ve anahtarlar sayfalarında kullanılabilir. Kapsayıcının başlatılması için her ikisi de gereklidir.<br><br>**{API_KEY}**: **anahtarlar** sayfasında kullanılabilir iki kaynak anahtardan biri<br><br>**{ENDPOINT_URI}**: **genel bakış** sayfasında belirtilen bitiş noktası |

## <a name="request-access-to-the-container-registry"></a>Kapsayıcı kayıt defterine erişim isteme

Kapsayıcıya erişim istemek için [istek formunu](https://aka.ms/speechcontainerspreview/) doldurun ve iletin. 


[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

[!INCLUDE [Gathering required parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="advanced-vector-extension-support"></a>Gelişmiş vektör uzantısı desteği

**Konak** , Docker kapsayıcısını çalıştıran bilgisayardır. Ana bilgisayar *must support* [Gelişmiş vektör uzantılarını](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#CPUs_with_AVX2) desteklemelidir (AVX2). Aşağıdaki komutla Linux konaklarda AVX2 desteğini denetleyebilirsiniz:

```console
grep -q avx2 /proc/cpuinfo && echo AVX2 supported || echo No AVX2 support detected
```
> [!WARNING]
> Ana bilgisayar, AVX2 desteklemek için *gereklidir* . Kapsayıcı, AVX2 desteği olmadan *düzgün çalışmaz.*

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

Aşağıdaki tabloda, her bir konuşma kapsayıcısı için kaynakların en düşük ve önerilen ayırması açıklanmaktadır.

# <a name="speech-to-text"></a>[Konuşmayı metne dönüştürme](#tab/stt)

| Kapsayıcı | Minimum | Önerilen |
|-----------|---------|-------------|
| Konuşmayı metne dönüştürme | 2 çekirdek, 2 GB bellek | 4 çekirdek, 4 GB bellek |

# <a name="custom-speech-to-text"></a>[Özel Konuşma Tanıma metin](#tab/cstt)

| Kapsayıcı | Minimum | Önerilen |
|-----------|---------|-------------|
| Özel Konuşma Tanıma metin | 2 çekirdek, 2 GB bellek | 4 çekirdek, 4 GB bellek |

# <a name="text-to-speech"></a>[Metin okuma](#tab/tts)

| Kapsayıcı | Minimum | Önerilen |
|-----------|---------|-------------|
| Metin okuma | 1 çekirdek, 2 GB bellek | 2 çekirdek, 3 GB bellek |

# <a name="custom-text-to-speech"></a>[Özel metin okuma](#tab/ctts)

| Kapsayıcı | Minimum | Önerilen |
|-----------|---------|-------------|
| Özel metin okuma | 1 çekirdek, 2 GB bellek | 2 çekirdek, 3 GB bellek |

***

* Her çekirdek en az 2,6 gigahertz (GHz) veya daha hızlı olmalıdır.

Çekirdek ve bellek, `--cpus` `--memory` komutunun bir parçası olarak kullanılan ve ayarlarına karşılık gelir `docker run` .

> [!NOTE]
> En düşük ve önerilen, ana makine kaynaklarından *değil* , Docker sınırlarına dayanır. Örneğin, konuşmadan metne kapsayıcı bellek eşleme, büyük bir dil modelinin bölümlerine eşlenir ve tüm dosyanın belleğe sığması *önerilir* , bu da ek 4-6 GB 'tır. Ayrıca, modellerin bellekte sayfalandığından kapsayıcının ilk çalışması daha uzun sürebilir.

## <a name="get-the-container-image-with-docker-pull"></a>Kapsayıcı görüntüsünü al`docker pull`

Konuşma için kapsayıcı görüntüleri aşağıdaki Container Registry kullanılabilir.

# <a name="speech-to-text"></a>[Konuşmayı metne dönüştürme](#tab/stt)

| Kapsayıcı | Depo |
|-----------|------------|
| Konuşmayı metne dönüştürme | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest` |

# <a name="custom-speech-to-text"></a>[Özel Konuşma Tanıma metin](#tab/cstt)

| Kapsayıcı | Depo |
|-----------|------------|
| Özel Konuşma Tanıma metin | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text:latest` |

# <a name="text-to-speech"></a>[Metin okuma](#tab/tts)

| Kapsayıcı | Depo |
|-----------|------------|
| Metin okuma | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest` |

# <a name="custom-text-to-speech"></a>[Özel metin okuma](#tab/ctts)

| Kapsayıcı | Depo |
|-----------|------------|
| Özel metin okuma | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech:latest` |

***

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-speech-containers"></a>Konuşma kapsayıcıları için Docker çekme

# <a name="speech-to-text"></a>[Konuşmayı metne dönüştürme](#tab/stt)

#### <a name="docker-pull-for-the-speech-to-text-container"></a>Konuşmadan metne kapsayıcı için Docker çekme

Kapsayıcı önizlemesi kayıt defterinden bir kapsayıcı görüntüsünü indirmek için [Docker Pull](https://docs.docker.com/engine/reference/commandline/pull/) komutunu kullanın.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest
```

> [!IMPORTANT]
> `latest`Etiket, `en-US` yerel ayarı çeker. Daha fazla yerel ayar için bkz. [konuşmayı metne göre yerel ayarlar](#speech-to-text-locales).

#### <a name="speech-to-text-locales"></a>Konuşmayı metne göre yerel ayarlar

Dışındaki tüm Etiketler `latest` aşağıdaki biçimdedir ve büyük/küçük harfe duyarlıdır:

```
<major>.<minor>.<patch>-<platform>-<locale>-<prerelease>
```

Aşağıdaki etiket, biçiminin bir örneğidir:

```
2.2.0-amd64-en-us-preview
```

**Konuşmadan metne** kapsayıcının desteklenen tüm yerel ayarları Için lütfen [konuşmadan metne görüntü etiketleri](../containers/container-image-tags.md#speech-to-text)bölümüne bakın.

# <a name="custom-speech-to-text"></a>[Özel Konuşma Tanıma metin](#tab/cstt)

#### <a name="docker-pull-for-the-custom-speech-to-text-container"></a>Özel Konuşma Tanıma metin kapsayıcısı için Docker Pull

Kapsayıcı önizlemesi kayıt defterinden bir kapsayıcı görüntüsünü indirmek için [Docker Pull](https://docs.docker.com/engine/reference/commandline/pull/) komutunu kullanın.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text:latest
```

> [!NOTE]
> `locale` `voice` Özel konuşma kapsayıcıları için ve, kapsayıcı tarafından alınan özel model tarafından belirlenir.

# <a name="text-to-speech"></a>[Metin okuma](#tab/tts)

#### <a name="docker-pull-for-the-text-to-speech-container"></a>Metin okuma kapsayıcısı için Docker çekme

Kapsayıcı önizlemesi kayıt defterinden bir kapsayıcı görüntüsünü indirmek için [Docker Pull](https://docs.docker.com/engine/reference/commandline/pull/) komutunu kullanın.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest
```

> [!IMPORTANT]
> `latest`Etiket, `en-US` yerel ayarı ve `jessarus` sesi çeker. Daha fazla yerel ayar için bkz. [metin okuma yerel ayarları](#text-to-speech-locales).

#### <a name="text-to-speech-locales"></a>Metinden konuşmaya yerel ayarlar

Dışındaki tüm Etiketler `latest` aşağıdaki biçimdedir ve büyük/küçük harfe duyarlıdır:

```
<major>.<minor>.<patch>-<platform>-<locale>-<voice>-<prerelease>
```

Aşağıdaki etiket, biçiminin bir örneğidir:

```
1.3.0-amd64-en-us-jessarus-preview
```

Desteklenen tüm yerel ayarlar ve **metin okuma** kapsayıcısının karşılık gelen sesleri için bkz. [metin okuma görüntü etiketleri](../containers/container-image-tags.md#text-to-speech).

> [!IMPORTANT]
> *Standart bir metinden konuşmaya* http gönderisi oluştururken, [konuşma sen, biçimlendirme dili (SSML)](speech-synthesis-markup.md) iletisi için bir `voice` özniteliği olan bir öğesi gerekir `name` . Değer, ["kısa ad"](language-support.md#standard-voices)olarak da bilinen karşılık gelen kapsayıcı yerel ayarı ve sestir. Örneğin, `latest` etiketinin bir ses adı olacaktır `en-US-JessaRUS` .

# <a name="custom-text-to-speech"></a>[Özel metin okuma](#tab/ctts)

#### <a name="docker-pull-for-the-custom-text-to-speech-container"></a>Özel metin okuma kapsayıcısı için Docker Pull

Kapsayıcı önizlemesi kayıt defterinden bir kapsayıcı görüntüsünü indirmek için [Docker Pull](https://docs.docker.com/engine/reference/commandline/pull/) komutunu kullanın.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech:latest
```

> [!NOTE]
> `locale` `voice` Özel konuşma kapsayıcıları için ve, kapsayıcı tarafından alınan özel model tarafından belirlenir.

***

## <a name="how-to-use-the-container"></a>Kapsayıcıyı kullanma

Kapsayıcı [ana bilgisayardan](#the-host-computer)olduktan sonra, kapsayıcında çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı](#run-the-container-with-docker-run)gerekli faturalandırma ayarlarıyla çalıştırın. Komuta [examples](speech-container-configuration.md#example-docker-run-commands) daha fazla örnek `docker run` kullanılabilir.
1. [Kapsayıcının tahmin uç noktasını sorgulayın](#query-the-containers-prediction-endpoint).

## <a name="run-the-container-with-docker-run"></a>Kapsayıcıyı ile çalıştırma`docker run`

Kapsayıcıyı çalıştırmak için [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. Ve değerlerini alma hakkında ayrıntılar için [gerekli parametreleri toplama](#gathering-required-parameters) bölümüne bakın `{Endpoint_URI}` `{API_Key}` . Komuta ek [örnekler](speech-container-configuration.md#example-docker-run-commands) `docker run` de mevcuttur.

# <a name="speech-to-text"></a>[Konuşmayı metne dönüştürme](#tab/stt)

*Konuşmaya metin* kapsayıcısını çalıştırmak için aşağıdaki `docker run` komutu yürütün.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Şu komut:

* Kapsayıcı görüntüsünden *konuşmaya metin* kapsayıcısı çalıştırır.
* 4 CPU çekirdeği ve 4 gigabayt (GB) bellek ayırır.
* TCP bağlantı noktası 5000 ' i gösterir ve kapsayıcı için bir sözde TTY ayırır.
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.


#### <a name="analyze-sentiment-on-the-speech-to-text-output"></a>Konuşmayı metne dönüştürme sırasında yaklaşımı çözümleme 

Konuşmayı metin kapsayıcısının 2.2.0 ' den başlayarak, çıkışta yaklaşım [Analizi v3 API](../text-analytics/how-tos/text-analytics-how-to-sentiment-analysis.md) 'sini çağırabilirsiniz. Yaklaşım analizini çağırmak için Metin Analizi API'si kaynak uç noktasına ihtiyacınız olacaktır. Örnek: 
* `https://westus2.api.cognitive.microsoft.com/text/analytics/v3.0-preview.1/sentiment`
* `https://localhost:5000/text/analytics/v3.0-preview.1/sentiment`

Buluttaki bir metin analizi uç noktasına erişiyorsanız, bir anahtara ihtiyacınız olacaktır. Metin Analizi yerel olarak çalıştırıyorsanız, bunu sağlamanız gerekebilir.

Anahtar ve uç nokta, aşağıdaki örnekte olduğu gibi, konuşma kapsayıcısına bağımsız değişkenler olarak geçirilir.

```bash
docker run -it --rm -p 5000:5000 \
containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
CloudAI:SentimentAnalysisSettings:TextAnalyticsHost={TEXT_ANALYTICS_HOST} \
CloudAI:SentimentAnalysisSettings:SentimentAnalysisApiKey={SENTIMENT_APIKEY}
```

Şu komut:

* Yukarıdaki komutla aynı adımları gerçekleştirir.
* , Yaklaşım Analizi isteklerini göndermek için bir Metin Analizi API'si uç noktası ve anahtarı depolar. 


# <a name="custom-speech-to-text"></a>[Özel Konuşma Tanıma metin](#tab/cstt)

*Özel konuşma tanıma metin* kapsayıcısı özel bir konuşma modeline bağlıdır. Özel bir model [özel konuşma portalı](https://speech.microsoft.com/customspeech)kullanılarak [eğitilmiş](how-to-custom-speech-train-model.md) olmalıdır.

> [!IMPORTANT]
> Özel Konuşma Tanıma modelinin aşağıdaki model sürümlerinden birinden eğitilecek olması gerekir:
> * **20181201 (v 3.3 Birleşik)**
> * **20190520 (v 4.14 Birleşik)**
> * **20190701 (v 4.17 Birleşik)**<br>
> ![Özel Konuşma Tanıma kapsayıcı modeline eğitme](media/custom-speech/custom-speech-train-model-container-scoped.png)

Kapsayıcıyı çalıştırmak için özel konuşma **MODELI kimliği** gereklidir. Özel konuşma portalının **eğitim** sayfasında bulunabilir. Özel konuşma portalından **eğitim** sayfasına gidin ve modeli seçin.
<br>

![Özel konuşma eğitimi sayfası](media/custom-speech/custom-speech-model-training.png)

Komutun parametresine bağımsız değişken olarak kullanılacak **model kimliğini** edinin `ModelId` `docker run` .
<br>

![Özel konuşma modeli ayrıntıları](media/custom-speech/custom-speech-model-details.png)

Aşağıdaki tablo çeşitli `docker run` parametreleri ve bunlara karşılık gelen açıklamalarını temsil eder:

| Parametre | Açıklama |
|---------|---------|
| `{VOLUME_MOUNT}` | Docker 'ın özel modeli kalıcı hale getirmek için kullandığı Konak bilgisayar [birimi bağlama](https://docs.docker.com/storage/volumes/). Örneğin, *c sürücüsünün* konak makinede bulunduğu *c:\customspeech* . |
| `{MODEL_ID}` | Özel konuşma portalının **eğitim** sayfasından özel konuşma tanıma **model kimliği** . |
| `{ENDPOINT_URI}` | Uç nokta, ölçüm ve faturalandırma için gereklidir. Daha fazla bilgi için bkz. [gerekli parametreleri toplama](#gathering-required-parameters). |
| `{API_KEY}` | API anahtarı gereklidir. Daha fazla bilgi için bkz. [gerekli parametreleri toplama](#gathering-required-parameters). |

*Özel konuşma tanıma metin* kapsayıcısını çalıştırmak için aşağıdaki `docker run` komutu yürütün:

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Şu komut:

* Kapsayıcı görüntüsünden *özel konuşma tanıma metin* kapsayıcısı çalıştırır.
* 4 CPU çekirdeği ve 4 gigabayt (GB) bellek ayırır.
* *Özel konuşma tanıma metin* modelini birim girişi bağlamalarından yükler, örneğin *c:\customspeech*.
* TCP bağlantı noktası 5000 ' i gösterir ve kapsayıcı için bir sözde TTY ayırır.
* Verilen modeli indirir `ModelId` (birim bağlaması üzerinde bulunmazsa).
* Özel model daha önce indirildiyse, yok `ModelId` sayılır.
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.

# <a name="text-to-speech"></a>[Metin okuma](#tab/tts)

*Metin okuma* kapsayıcısını çalıştırmak için aşağıdaki `docker run` komutu yürütün.

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Şu komut:

* Kapsayıcı görüntüsünden *metinden konuşmaya* kapsayıcısı çalıştırır.
* 2 CPU çekirdeği ve bir gigabayt (GB) bellek ayırır.
* TCP bağlantı noktası 5000 ' i gösterir ve kapsayıcı için bir sözde TTY ayırır.
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.

# <a name="custom-text-to-speech"></a>[Özel metin okuma](#tab/ctts)

*Özel metin okuma* kapsayıcısı özel bir ses modeline dayanır. Özel bir modelin [özel ses portalı](https://aka.ms/custom-voice-portal)kullanılarak [eğitilli](how-to-custom-voice-create-voice.md) olması gerekir. Kapsayıcıyı çalıştırmak için özel ses **MODELI kimliği** gereklidir. Özel ses portalının **eğitim** sayfasında bulunabilir. Özel ses portalından **eğitim** sayfasına gidin ve modeli seçin.
<br>

![Özel ses eğitimi sayfası](media/custom-voice/custom-voice-model-training.png)

Docker Run komutunun parametresine bağımsız değişken olarak kullanılacak **model kimliğini** edinin `ModelId` .
<br>

![Özel ses modeli ayrıntıları](media/custom-voice/custom-voice-model-details.png)

Aşağıdaki tablo çeşitli `docker run` parametreleri ve bunlara karşılık gelen açıklamalarını temsil eder:

| Parametre | Açıklama |
|---------|---------|
| `{VOLUME_MOUNT}` | Docker 'ın özel modeli kalıcı hale getirmek için kullandığı Konak bilgisayar [birimi bağlama](https://docs.docker.com/storage/volumes/). Örneğin, *c sürücüsünün* konak makinede bulunduğu *c:\customspeech* . |
| `{MODEL_ID}` | Özel ses portalının **eğitim** sayfasından özel konuşma tanıma **model kimliği** . |
| `{ENDPOINT_URI}` | Uç nokta, ölçüm ve faturalandırma için gereklidir. Daha fazla bilgi için bkz. [gerekli parametreleri toplama](#gathering-required-parameters). |
| `{API_KEY}` | API anahtarı gereklidir. Daha fazla bilgi için bkz. [gerekli parametreleri toplama](#gathering-required-parameters). |

*Özel metin okuma* kapsayıcısını çalıştırmak için aşağıdaki `docker run` komutu yürütün:

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Şu komut:

* Kapsayıcı görüntüsünden *özel bir metin okuma* kapsayıcısı çalıştırır.
* 2 CPU çekirdeği ve bir gigabayt (GB) bellek ayırır.
* Toplu giriş bağlamalarından *özel metin okuma* modelini yükler, örneğin *c:\customvoice*.
* TCP bağlantı noktası 5000 ' i gösterir ve kapsayıcı için bir sözde TTY ayırır.
* Verilen modeli indirir `ModelId` (birim bağlaması üzerinde bulunmazsa).
* Özel model daha önce indirildiyse, yok `ModelId` sayılır.
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.

***

> [!IMPORTANT]
> `Eula` `Billing` `ApiKey` Kapsayıcıyı çalıştırmak için, ve seçenekleri belirtilmelidir; Aksi takdirde kapsayıcı başlatılmaz.  Daha fazla bilgi için bkz. [faturalandırma](#billing).

## <a name="query-the-containers-prediction-endpoint"></a>Kapsayıcının tahmin uç noktasını sorgulama

> [!NOTE]
> Birden çok kapsayıcı çalıştırıyorsanız benzersiz bir bağlantı noktası numarası kullanın.

| Kapsayıcılar | SDK ana bilgisayar URL 'SI | Protokol |
|--|--|--|
| Konuşmadan metne ve Özel Konuşma Tanıma metne dönüştürme | `ws://localhost:5000` | WS |
| Metin okuma ve özel metin okuma | `http://localhost:5000` | HTTP |

WSS ve HTTPS protokollerini kullanma hakkında daha fazla bilgi için bkz. [kapsayıcı güvenliği](../cognitive-services-container-support.md#azure-cognitive-services-container-security).

[!INCLUDE [Query Speech-to-text container endpoint](includes/speech-to-text-container-query-endpoint.md)]

#### <a name="analyze-sentiment"></a>Yaklaşımı analiz etme

Metin Analizi API'si kimlik bilgilerinizi [kapsayıcıda](#analyze-sentiment-on-the-speech-to-text-output)sağladıysanız konuşma tanıma isteklerini yaklaşım analiziyle göndermek için konuşma SDK 'sını kullanabilirsiniz. API yanıtlarını *basit* veya *ayrıntılı* bir biçim kullanacak şekilde yapılandırabilirsiniz.

# <a name="simple-format"></a>[Basit biçim](#tab/simple-format)

Konuşma istemcisini basit bir biçim kullanacak şekilde yapılandırmak için, `"Sentiment"` için bir değer olarak ekleyin `Simple.Extensions` . Belirli bir Metin Analizi modeli sürümü seçmek istiyorsanız, `'latest'` `speechcontext-phraseDetection.sentimentAnalysis.modelversion` özellik yapılandırmasında değiştirin.

```python
speech_config.set_service_property(
    name='speechcontext-PhraseOutput.Simple.Extensions',
    value='["Sentiment"]',
    channel=speechsdk.ServicePropertyChannel.UriQueryParameter
)
speech_config.set_service_property(
    name='speechcontext-phraseDetection.sentimentAnalysis.modelversion',
    value='latest',
    channel=speechsdk.ServicePropertyChannel.UriQueryParameter
)
```

`Simple.Extensions`yanıtın kök katmanında yaklaşım sonucunu döndürür.

```json
{
   "DisplayText":"What's the weather like?",
   "Duration":13000000,
   "Id":"6098574b79434bd4849fee7e0a50f22e",
   "Offset":4700000,
   "RecognitionStatus":"Success",
   "Sentiment":{
      "Negative":0.03,
      "Neutral":0.79,
      "Positive":0.18
   }
}
```

# <a name="detailed-format"></a>[Ayrıntılı biçim](#tab/detailed-format)

Konuşma istemcisini, ayrıntılı bir biçim kullanacak şekilde yapılandırmak için, `"Sentiment"` `Detailed.Extensions` veya her ikisi için bir değer olarak ekleyin `Detailed.Options` . Belirli bir Metin Analizi modeli sürümü seçmek istiyorsanız, `'latest'` `speechcontext-phraseDetection.sentimentAnalysis.modelversion` özellik yapılandırmasında değiştirin.

```python
speech_config.set_service_property(
    name='speechcontext-PhraseOutput.Detailed.Options',
    value='["Sentiment"]',
    channel=speechsdk.ServicePropertyChannel.UriQueryParameter
)
speech_config.set_service_property(
    name='speechcontext-PhraseOutput.Detailed.Extensions',
    value='["Sentiment"]',
    channel=speechsdk.ServicePropertyChannel.UriQueryParameter
)
speech_config.set_service_property(
    name='speechcontext-phraseDetection.sentimentAnalysis.modelversion',
    value='latest',
    channel=speechsdk.ServicePropertyChannel.UriQueryParameter
)
```

`Detailed.Extensions`yanıtın kök katmanında yaklaşım sonucunu sağlar. `Detailed.Options`yanıtın katmanında sonucu sağlar `NBest` . Bunlar ayrı olarak veya birlikte kullanılabilir.

```json
{
   "DisplayText":"What's the weather like?",
   "Duration":13000000,
   "Id":"6a2aac009b9743d8a47794f3e81f7963",
   "NBest":[
      {
         "Confidence":0.973695,
         "Display":"What's the weather like?",
         "ITN":"what's the weather like",
         "Lexical":"what's the weather like",
         "MaskedITN":"What's the weather like",
         "Sentiment":{
            "Negative":0.03,
            "Neutral":0.79,
            "Positive":0.18
         }
      },
      {
         "Confidence":0.9164971,
         "Display":"What is the weather like?",
         "ITN":"what is the weather like",
         "Lexical":"what is the weather like",
         "MaskedITN":"What is the weather like",
         "Sentiment":{
            "Negative":0.02,
            "Neutral":0.88,
            "Positive":0.1
         }
      }
   ],
   "Offset":4700000,
   "RecognitionStatus":"Success",
   "Sentiment":{
      "Negative":0.03,
      "Neutral":0.79,
      "Positive":0.18
   }
}
```

---

Yaklaşım analizini tamamen devre dışı bırakmak istiyorsanız, `false` öğesine bir değer ekleyin `sentimentanalysis.enabled` .

```python
speech_config.set_service_property(
    name='speechcontext-phraseDetection.sentimentanalysis.enabled',
    value='false',
    channel=speechsdk.ServicePropertyChannel.UriQueryParameter
)
```

### <a name="text-to-speech-or-custom-text-to-speech"></a>Metin okuma veya özel metin okuma

[!INCLUDE [Query Text-to-speech container endpoint](includes/text-to-speech-container-query-endpoint.md)]

### <a name="run-multiple-containers-on-the-same-host"></a>Birden çok kapsayıcıyı aynı konakta Çalıştır

Açığa çıkarılan bağlantı noktalarıyla birden çok kapsayıcı çalıştırmak istiyorsanız, her kapsayıcıyı farklı bir açığa çıkarılan bağlantı noktasıyla çalıştırdığınızdan emin olun. Örneğin, bağlantı noktası 5000 ' deki ilk kapsayıcıyı ve bağlantı noktası 5001 üzerindeki ikinci kapsayıcıyı çalıştırın.

Bu kapsayıcınızı ve KONAKTA çalışan farklı bir Azure bilişsel Hizmetler kapsayıcısını birlikte kullanabilirsiniz. Aynı bilişsel Hizmetler kapsayıcısının çalışan birden fazla kapsayıcınızı da kullanabilirsiniz.

[!INCLUDE [Validate container is running - Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Kapsayıcıyı durdurma

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcıyı başlatırken veya çalıştırırken sorunlarla karşılaşabilirsiniz. Çıkış [bağlaması](speech-container-configuration.md#mount-settings) kullanın ve günlüğe kaydetmeyi etkinleştirin. Bunun yapılması, kapsayıcının sorunları giderirken faydalı olan günlük dosyaları oluşturmasına izin verir.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Faturalandırma

Konuşma kapsayıcıları, Azure hesabınızdaki bir *konuşma* kaynağını kullanarak faturalama bilgilerini Azure 'a gönderir.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](speech-container-configuration.md).

<!--blogs/samples/video courses -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, konuşma kapsayıcılarını indirmek, yüklemek ve çalıştırmak için kavramları ve iş akışını öğrendiniz. Özet:

* Konuşma, çeşitli özellikleri kapsülleyerek Docker için dört Linux kapsayıcısı sağlar:
  * *Konuşmayı metne dönüştürme*
  * *Özel Konuşma Tanıma metin*
  * *Metin okuma*
  * *Özel metin okuma*
* Kapsayıcı görüntüleri Azure 'daki kapsayıcı kayıt defterinden indirilir.
* Kapsayıcı görüntüleri Docker 'da çalışır.
* REST API (yalnızca metinden konuşmaya) veya SDK (konuşmadan metne veya metinden konuşmaya) kullanarak kapsayıcının ana bilgisayar URI 'sini belirtirsiniz. 
* Bir kapsayıcıyı örnekledikten sonra faturalandırma bilgilerini sağlamanız gerekir.

> [!IMPORTANT]
>  Bilişsel hizmetler kapsayıcıları, ölçüm için Azure 'a bağlı kalmadan çalıştırılmak üzere lisanslanmaz. Müşterilerin, ödeme bilgilerini her zaman ölçüm hizmetiyle iletişimine olanak tanımak için kapsayıcıların etkinleştirilmesi gerekir. Bilişsel hizmetler kapsayıcıları, müşteri verilerini (ör., çözümlenmekte olan resim veya metin) Microsoft 'a göndermez.

## <a name="next-steps"></a>Sonraki adımlar

* Yapılandırma ayarları için [kapsayıcıları](speech-container-configuration.md) yapılandırmayı gözden geçir
* [Kubernetes ve Held Ile konuşma hizmeti kapsayıcılarını nasıl kullanacağınızı](speech-container-howto-on-premises.md) öğrenin
* Daha fazla bilişsel [Hizmetler kapsayıcısı](../cognitive-services-container-support.md) kullanın
