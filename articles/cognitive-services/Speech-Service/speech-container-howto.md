---
title: Konuşma kapsayıcıları yükle - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma kapsayıcılarını yükleyin ve çalıştırın. Konuşmadan metne ses akışlarını, uygulamalarınızın, araçlarınızın veya aygıtlarınızın tüketebileceği veya görüntüleyebileceği gerçek zamanlı olarak metne çevirir. Metinden konuşmaya giriş metnini insan benzeri sentezlenmiş konuşmaya dönüştürür.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: dapine
ms.openlocfilehash: 2beee81bc365d00e59a62cacabacc5f5d6b62a42
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79474790"
---
# <a name="install-and-run-speech-service-containers-preview"></a>Konuşma hizmet kapsayıcılarını yükleme ve çalıştırma (Önizleme)

Kapsayıcılar, konuşma hizmeti API'lerinin bazılarını kendi ortamınızda çalıştırmanızı sağlar. Kapsayıcılar belirli güvenlik ve veri yönetimi gereksinimleri için harikadır. Bu makalede, konuşma kapsayıcısını nasıl indireceğinizi, yükleyeceğinizi ve çalıştıracağınız öğrenilir.

Konuşma kapsayıcıları, müşterilerin hem sağlam bulut yetenekleri hem de kenar yerelliği için en iyi duruma getirilmiş bir konuşma uygulaması mimarisi oluşturmasına olanak tanır. Dört farklı kapsayıcı mevcuttur. İki standart kapsayıcı **konuşma-to-metin** ve **metin-to-speech**vardır. İki özel kapsayıcı, **Özel Konuşma-metin** ve **Özel Metinden**konuşmaya'dır.

> [!IMPORTANT]
> Tüm konuşma kapsayıcıları şu anda [Genel "Gated" Önizleme'nin](../cognitive-services-container-support.md#public-gated-preview-container-registry-containerpreviewazurecrio)bir parçası olarak sunulmaktadır. Konuşma kapsayıcıları Genel Kullanılabilirliğe (GA) ilerlerken bir duyuru yapılacaktır.

| İşlev | Özellikler | En son |
|--|--|--|
| Konuşmayı metne dönüştürme | Sürekli gerçek zamanlı konuşma veya toplu ses kayıtlarını ara sonuçlarla metne dönüştürür. | 2.1.1 |
| Özel Konuşma-metin | [Özel Konuşma portalından](https://speech.microsoft.com/customspeech)özel bir model kullanarak, sürekli gerçek zamanlı konuşma veya toplu ses kayıtlarını ara sonuçlarla metne dönüştürür. | 2.1.1 |
| Metin okuma | Metni düz metin girişi veya Konuşma Sentezi Biçimlendirme Dili (SSML) ile doğal görünen konuşmaya dönüştürür. | 1.3.0 |
| Özel Metinden konuşmaya | [Özel Ses portalından](https://aka.ms/custom-voice-portal)özel bir model kullanarak, metni düz metin girişi veya Konuşma Sentezi Biçimlendirme Dili (SSML) ile doğal görünen konuşmaya dönüştürür. | 1.3.0 |

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Konuşma kapsayıcılarını kullanmadan önce aşağıdaki ön koşullar:

| Gerekli | Amaç |
|--|--|
| Docker altyapısı | Docker [Engine'in ana bilgisayara](#the-host-computer)yüklenmesi gerekir. Docker, [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms) üzerinde Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temel bilgileri ile ilgili giriş yapmak için [Docker’a genel bakış](https://docs.docker.com/engine/docker-overview/) bölümüne bakın.<br><br> Docker, kapsayıcıların Azure'a bağlanmasına ve fatura verilerini göndermesine izin verecek şekilde yapılandırılmalıdır. <br><br> **Windows'da**Docker, Linux kapsayıcılarını destekleyecek şekilde de yapılandırılmalıdır.<br><br> |
| Docker ile aşinalık | Docker kavramları hakkında, kayıt defterleri, depolar, konteynerler ve konteyner resimleri nin yanı sıra `docker` temel komutlar hakkında temel bilgilere sahip olmalısınız. |
| Konuşma kaynağı | Bu kapsayıcıları kullanabilmek için şunları yapmalısınız:<br><br>İlişkili API anahtarını ve bitiş noktası URI'yi almak için bir Azure _Konuşma_ kaynağı. Her iki değer de Azure portalının **Konuşmaya** Genel Bakış ve Keys sayfalarında mevcuttur. Her ikisi de kapsayıcı başlatmak için gereklidir.<br><br>**{API_KEY}**: **Keys** sayfasındaki mevcut iki kaynak anahtarından biri<br><br>**{ENDPOINT_URI}**: **Genel Bakış** sayfasında sağlanan bitiş noktası |

## <a name="request-access-to-the-container-registry"></a>Konteyner kayıt defterine erişim isteği

Kapsayıcıya erişim istemek için [Bilişsel Hizmetler Konuşma Kapları İstek formunu](https://aka.ms/speechcontainerspreview/) doldurun ve gönderin. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

[!INCLUDE [Gathering required parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="advanced-vector-extension-support"></a>Gelişmiş Vektör Uzantısı desteği

**Ana bilgisayar,** docker konteynerini çalıştıran bilgisayardır. Ana bilgisayar [Gelişmiş Vektör Uzantılarını](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#CPUs_with_AVX2) (AVX2) *desteklemelidir.* Aşağıdaki komutla Linux ana bilgisayarlarında AVX2 desteğini kontrol edebilirsiniz:

```console
grep -q avx2 /proc/cpuinfo && echo AVX2 supported || echo No AVX2 support detected
```
> [!WARNING]
> Ana bilgisayar AVX2 desteklemek için *gereklidir.* Konteyner AVX2 desteği olmadan doğru *çalışmaz.*

### <a name="container-requirements-and-recommendations"></a>Konteyner gereksinimleri ve öneriler

Aşağıdaki tabloda, her Konuşma kapsayıcısı için en az ve önerilen kaynak tahsisi açıklanmaktadır.

# <a name="speech-to-text"></a>[Konuşmayı metne dönüştürme](#tab/stt)

| Kapsayıcı | Minimum | Önerilen |
|-----------|---------|-------------|
| Konuşmayı metne dönüştürme | 2 çekirdekli, 2 GB bellek | 4 çekirdekli, 4 GB bellek |

# <a name="custom-speech-to-text"></a>[Özel Konuşma-metin](#tab/cstt)

| Kapsayıcı | Minimum | Önerilen |
|-----------|---------|-------------|
| Özel Konuşma-metin | 2 çekirdekli, 2 GB bellek | 4 çekirdekli, 4 GB bellek |

# <a name="text-to-speech"></a>[Metin okuma](#tab/tts)

| Kapsayıcı | Minimum | Önerilen |
|-----------|---------|-------------|
| Metin okuma | 1 çekirdekli, 2 GB bellek | 2 çekirdekli, 3 GB bellek |

# <a name="custom-text-to-speech"></a>[Özel Metinden konuşmaya](#tab/ctts)

| Kapsayıcı | Minimum | Önerilen |
|-----------|---------|-------------|
| Özel Metinden konuşmaya | 1 çekirdekli, 2 GB bellek | 2 çekirdekli, 3 GB bellek |

***

* Her çekirdek en az 2,6 gigahertz (GHz) veya daha hızlı olmalıdır.

Çekirdek ve bellek, `--cpus` `--memory` komutun bir parçası olarak `docker run` kullanılan ve ayarlara karşılık gelir.

> [!NOTE]
> Minimum ve önerilen docker sınırları *değil,* ana makine kaynakları dayanır. Örneğin, konuşmadan metne büyük bir dil modelinin bellek eşlemi bölümlerini kaplar ve tüm dosyanın ek bir 4-6 GB olan belleğe uyması *önerilir.* Ayrıca, modeller belleğe çağrıldığından, her iki kapsayıcının ilk çalışması daha uzun sürebilir.

## <a name="get-the-container-image-with-docker-pull"></a>Konteyner görüntüsünü`docker pull`

Konuşma için kapsayıcı görüntüleri aşağıdaki Konteyner Kayıt Defteri'nde mevcuttur.

# <a name="speech-to-text"></a>[Konuşmayı metne dönüştürme](#tab/stt)

| Kapsayıcı | Depo |
|-----------|------------|
| Konuşmayı metne dönüştürme | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest` |

# <a name="custom-speech-to-text"></a>[Özel Konuşma-metin](#tab/cstt)

| Kapsayıcı | Depo |
|-----------|------------|
| Özel Konuşma-metin | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text:latest` |

# <a name="text-to-speech"></a>[Metin okuma](#tab/tts)

| Kapsayıcı | Depo |
|-----------|------------|
| Metin okuma | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest` |

# <a name="custom-text-to-speech"></a>[Özel Metinden konuşmaya](#tab/ctts)

| Kapsayıcı | Depo |
|-----------|------------|
| Özel Metinden konuşmaya | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech:latest` |

***

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-speech-containers"></a>Konuşma kapları için Docker çekme

# <a name="speech-to-text"></a>[Konuşmayı metne dönüştürme](#tab/stt)

#### <a name="docker-pull-for-the-speech-to-text-container"></a>Konuşma-metin kapsayıcısı için Docker çekme

Konteyner Önizleme kayıt defterinden bir kapsayıcı görüntüsü indirmek için [docker çekme](https://docs.docker.com/engine/reference/commandline/pull/) komutunu kullanın.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest
```

> [!IMPORTANT]
> Etiket `latest` yerel alanı `en-US` çeker. Ek yerel ler için [metinden metine konuşma yerel lerini](#speech-to-text-locales)görün.

#### <a name="speech-to-text-locales"></a>Konuşmadan metne yerel

Bunlar dışında tüm `latest` etiketler aşağıdaki biçimdedir ve büyük/küçük harf duyarlıdır:

```
<major>.<minor>.<patch>-<platform>-<locale>-<prerelease>
```

Aşağıdaki etiket biçiminin bir örneğidir:

```
2.1.1-amd64-en-us-preview
```

**Konuşmadan metne** kapsayıcının desteklenen tüm yerel leri için lütfen [Konuşmadan Metne resim etiketlerine](../containers/container-image-tags.md#speech-to-text)bakın.

# <a name="custom-speech-to-text"></a>[Özel Konuşma-metin](#tab/cstt)

#### <a name="docker-pull-for-the-custom-speech-to-text-container"></a>Özel Konuşma-metin kapsayıcısı için Docker çekme

Konteyner Önizleme kayıt defterinden bir kapsayıcı görüntüsü indirmek için [docker çekme](https://docs.docker.com/engine/reference/commandline/pull/) komutunu kullanın.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text:latest
```

> [!NOTE]
> Ve `locale` `voice` özel Konuşma kapsayıcıları için konteyner tarafından yutulan özel model tarafından belirlenir.

# <a name="text-to-speech"></a>[Metin okuma](#tab/tts)

#### <a name="docker-pull-for-the-text-to-speech-container"></a>Metinden konuşmaya kapsayıcı için Docker çekme

Konteyner Önizleme kayıt defterinden bir kapsayıcı görüntüsü indirmek için [docker çekme](https://docs.docker.com/engine/reference/commandline/pull/) komutunu kullanın.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest
```

> [!IMPORTANT]
> Etiket `latest` yerel alanı `en-US` ve `jessarus` sesi çeker. Ek yerel olarak [Metinden konuşmaya yerel yerel tabanlıklara](#text-to-speech-locales)bakın.

#### <a name="text-to-speech-locales"></a>Metinden konuşmaya yerel

Bunlar dışında tüm `latest` etiketler aşağıdaki biçimdedir ve büyük/küçük harf duyarlıdır:

```
<major>.<minor>.<patch>-<platform>-<locale>-<voice>-<prerelease>
```

Aşağıdaki etiket biçiminin bir örneğidir:

```
1.3.0-amd64-en-us-jessarus-preview
```

Desteklenen tüm yerel yönetimler ve **metinden konuşmaya** kapsayıcının ilgili sesleri için lütfen [Metinden konuşmaya resim etiketlerine](../containers/container-image-tags.md#text-to-speech)bakın.

> [!IMPORTANT]
> *Standart metinden konuşmaya* HTTP POST'u yaparken, [Konuşma Sentezi Biçimlendirme Dili (SSML)](speech-synthesis-markup.md) iletisi özniteliği olan bir `voice` `name` öğe gerektirir. Değer, ["kısa ad"](language-support.md#standard-voices)olarak da bilinen karşılık gelen kapsayıcı yerel ve sestir. Örneğin, etiketin `latest` `en-US-JessaRUS`bir ses adı olurdu.

# <a name="custom-text-to-speech"></a>[Özel Metinden konuşmaya](#tab/ctts)

#### <a name="docker-pull-for-the-custom-text-to-speech-container"></a>Özel Metinden konuşmaya kapsayıcı için Docker çekme

Konteyner Önizleme kayıt defterinden bir kapsayıcı görüntüsü indirmek için [docker çekme](https://docs.docker.com/engine/reference/commandline/pull/) komutunu kullanın.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech:latest
```

> [!NOTE]
> Ve `locale` `voice` özel Konuşma kapsayıcıları için konteyner tarafından yutulan özel model tarafından belirlenir.

***

## <a name="how-to-use-the-container"></a>Kapsayıcı nasıl kullanılır?

Kapsayıcı [ana bilgisayara](#the-host-computer)yüklendikten sonra, kapsayıcıyla çalışmak için aşağıdaki işlemi kullanın.

1. Gerekli fatura ayarlarıyla [kapsayıcıyı çalıştırın.](#run-the-container-with-docker-run) Komutun `docker run` daha fazla [örneği](speech-container-configuration.md#example-docker-run-commands) mevcuttur.
1. [Kapsayıcının tahmin bitiş noktasını sorgula.](#query-the-containers-prediction-endpoint)

## <a name="run-the-container-with-docker-run"></a>Kapsayıcıyı çalıştırın`docker run`

Kapsayıcıyı çalıştırmak için [docker run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. Değerleri ve `{API_Key}` değerleri nasıl elde edineceklerine ilişkin ayrıntılar için [gerekli parametreleri](#gathering-required-parameters) `{Endpoint_URI}` toplamaya bakın. Komutun `docker run` ek [örnekleri](speech-container-configuration.md#example-docker-run-commands) de mevcuttur.

# <a name="speech-to-text"></a>[Konuşmayı metne dönüştürme](#tab/stt)

*Konuşma-metin* kapsayıcıçalıştırmak için aşağıdaki `docker run` komutu çalıştırın.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Şu komut:

* Kapsayıcı görüntüden *konuşma-metin* kapsayıcısı çalıştırın.
* 4 CPU çekirdeği ve 4 gigabayt (GB) bellek ayırır.
* TCP bağlantı noktası 5000'i açığa çıkarır ve kapsayıcı için sözde TTY ayırır.
* Çıktıktan sonra kapsayıcıotomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.

# <a name="custom-speech-to-text"></a>[Özel Konuşma-metin](#tab/cstt)

*Özel Konuşma-metin* kapsayıcısı özel bir konuşma modeline dayanır. Özel model [özel konuşma portalı](https://speech.microsoft.com/customspeech)kullanılarak [eğitilmiş](how-to-custom-speech-train-model.md) olması gerekir.

> [!IMPORTANT]
> Özel Konuşma modelinin aşağıdaki model sürümlerinden birinden eğitilmesi gerekir:
> * **20181201 (v3.3 Birleşik)**
> * **20190520 (v4.14 Birleşik)**
> * **20190701 (v4.17 Birleşik)**<br>
> ![Özel Konuşma tren konteyner modeli](media/custom-speech/custom-speech-train-model-container-scoped.png)

Kapsayıcıyı çalıştırmak için özel konuşma **Modeli Kimliği** gereklidir. Bu özel konuşma portalı **Eğitim** sayfasında bulunabilir. Özel konuşma portalından **Eğitim** sayfasına gidin ve modeli seçin.
<br>

![Özel konuşma eğitim sayfası](media/custom-speech/custom-speech-model-training.png)

Komutun **Model ID** `ModelId` parametresine bağımsız değişken olarak kullanmak üzere Model Kimliğini edinin. `docker run`
<br>

![Özel konuşma modeli ayrıntıları](media/custom-speech/custom-speech-model-details.png)

Aşağıdaki tablo çeşitli `docker run` parametreleri ve bunların karşılık gelen açıklamalarını temsil eder:

| Parametre | Açıklama |
|---------|---------|
| `{VOLUME_MOUNT}` | Ana bilgisayar [hacmi montaj](https://docs.docker.com/storage/volumes/), hangi docker özel modeli devam etmek için kullanır. Örneğin, *C sürücüsünün* ana makinede bulunduğu *C:\CustomSpeech.* |
| `{MODEL_ID}` | Özel konuşma portalının **Eğitim** sayfasından Özel Konuşma **Modeli Kimliği.** |
| `{ENDPOINT_URI}` | Ölçüm ve faturalandırma için bitiş noktası gereklidir. Daha fazla bilgi için [gerekli parametreleri toplamaya](#gathering-required-parameters)bakın. |
| `{API_KEY}` | API anahtarı gereklidir. Daha fazla bilgi için [gerekli parametreleri toplamaya](#gathering-required-parameters)bakın. |

*Özel Konuşma-metin* kapsayıcısını çalıştırmak için aşağıdaki `docker run` komutu uygulayın:

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

* Kapsayıcı görüntüden *özel konuşmadan metne* kapsayıcı çalıştırın.
* 4 CPU çekirdeği ve 4 gigabayt (GB) bellek ayırır.
* *C:\CustomSpeech*gibi birim giriş yuvasından *Özel Konuşma-Metin* modelini yükler.
* TCP bağlantı noktası 5000'i açığa çıkarır ve kapsayıcı için sözde TTY ayırır.
* Verilen modeli indirir `ModelId` (ses montaj üzerinde bulunmuyorsa).
* Özel model daha önce karşıdan `ModelId` yüklendiyse, göz ardı edilir.
* Çıktıktan sonra kapsayıcıotomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.

# <a name="text-to-speech"></a>[Metin okuma](#tab/tts)

*Metinden konuşmaya* kapsayıcısını çalıştırmak için aşağıdaki `docker run` komutu uygulayın.

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Şu komut:

* Kapsayıcı görüntüsünden *metinden konuşmaya* kapsayıcısı çalıştırın.
* 2 CPU çekirdeği ve bir gigabayt (GB) bellek ayırır.
* TCP bağlantı noktası 5000'i açığa çıkarır ve kapsayıcı için sözde TTY ayırır.
* Çıktıktan sonra kapsayıcıotomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.

# <a name="custom-text-to-speech"></a>[Özel Metinden konuşmaya](#tab/ctts)

*Özel Metinden konuşmaya* kapsayıcısı özel bir ses modeline dayanır. Özel model [özel ses portalı](https://aka.ms/custom-voice-portal)kullanılarak [eğitilmiş](how-to-custom-voice-create-voice.md) olması gerekir. Kapsayıcıyı çalıştırmak için özel ses **Modeli Kimliği** gereklidir. Bu özel ses portalı **Eğitim** sayfasında bulunabilir. Özel ses portalından **Eğitim** sayfasına gidin ve modeli seçin.
<br>

![Özel sesli eğitim sayfası](media/custom-voice/custom-voice-model-training.png)

Docker **Model ID** run komutunun `ModelId` parametresine bağımsız değişken olarak kullanmak üzere Model Kimliğini edinin.
<br>

![Özel ses modeli ayrıntıları](media/custom-voice/custom-voice-model-details.png)

Aşağıdaki tablo çeşitli `docker run` parametreleri ve bunların karşılık gelen açıklamalarını temsil eder:

| Parametre | Açıklama |
|---------|---------|
| `{VOLUME_MOUNT}` | Ana bilgisayar [hacmi montaj](https://docs.docker.com/storage/volumes/), hangi docker özel modeli devam etmek için kullanır. Örneğin, *C sürücüsünün* ana makinede bulunduğu *C:\CustomSpeech.* |
| `{MODEL_ID}` | Özel ses portalının **Eğitim** sayfasından Özel Konuşma **Modeli Kimliği.** |
| `{ENDPOINT_URI}` | Ölçüm ve faturalandırma için bitiş noktası gereklidir. Daha fazla bilgi için [gerekli parametreleri toplamaya](#gathering-required-parameters)bakın. |
| `{API_KEY}` | API anahtarı gereklidir. Daha fazla bilgi için [gerekli parametreleri toplamaya](#gathering-required-parameters)bakın. |

*Özel Metinden konuşmaya* kapsayıcısını çalıştırmak için `docker run` aşağıdaki komutu uygulayın:

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

* Kapsayıcı görüntüden *Özel Metinden konuşmaya* kapsayıcıçalıştırın.
* 2 CPU çekirdeği ve bir gigabayt (GB) bellek ayırır.
* *C:\CustomVoice*gibi birim giriş yuvasından *Özel Metinden konuşmaya* modeli yükler.
* TCP bağlantı noktası 5000'i açığa çıkarır ve kapsayıcı için sözde TTY ayırır.
* Verilen modeli indirir `ModelId` (ses montaj üzerinde bulunmuyorsa).
* Özel model daha önce karşıdan `ModelId` yüklendiyse, göz ardı edilir.
* Çıktıktan sonra kapsayıcıotomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.

***

> [!IMPORTANT]
> `Eula`Kapsayıcıyı `Billing`çalıştırmak `ApiKey` için , ve seçenekler belirtilmelidir; aksi takdirde, kapsayıcı başlamaz.  Daha fazla bilgi için [Faturalandırma'ya](#billing)bakın.

## <a name="query-the-containers-prediction-endpoint"></a>Kapsayıcının tahmin bitiş noktasını sorgula

| Kapsayıcılar | SDK Host URL | Protokol |
|--|--|--|
| Konuşmadan metne ve Özel Konuşmadan metne | `ws://localhost:5000` | WS |
| Metinden konuşmaya ve Özel Metinden konuşmaya | `http://localhost:5000` | HTTP |

WSS ve HTTPS protokollerini kullanma hakkında daha fazla bilgi için [kapsayıcı güvenliğine](../cognitive-services-container-support.md#azure-cognitive-services-container-security)bakın.

[!INCLUDE [Query Speech-to-text container endpoint](includes/speech-to-text-container-query-endpoint.md)]

### <a name="text-to-speech-or-custom-text-to-speech"></a>Metinden konuşmaya veya Özel Metinden konuşmaya

[!INCLUDE [Query Text-to-speech container endpoint](includes/text-to-speech-container-query-endpoint.md)]

### <a name="run-multiple-containers-on-the-same-host"></a>Aynı ana bilgisayarda birden çok kapsayıcı çalıştırma

Açıkta kalan bağlantı noktalarına sahip birden çok kapsayıcı çalıştırmayı planlıyorsanız, her kapsayıcıyı farklı bir açık bağlantı noktasına sahip çalıştırdığınızdan emin olun. Örneğin, 5000 portundaki ilk kapsayıcıyı ve 5001 no'daki ikinci kapsayıcıyı çalıştırın.

Bu kapsayıcıyı ve farklı bir Azure Bilişsel Hizmetler kapsayıcısını HOST'ta birlikte çalıştırabilirsiniz. Ayrıca, aynı Bilişsel Hizmetler kapsayıcısının birden çok kapsayıcısının çalışmasını da sağlayabilirsiniz.

[!INCLUDE [Validate container is running - Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Kapsayıcıyı durdurma

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcıyı çalıştırırken veya çalıştırırken sorunlarla karşılaşabilirsiniz. Çıktı [montajını](speech-container-configuration.md#mount-settings) kullanın ve günlüğe kaydetmeyi etkinleştirin. Bunu yapmak, sorun giderme sorunları olduğunda yardımcı olan günlük dosyaları oluşturmasına kapsayıcı sağlar.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Faturalandırma

Konuşma kapsayıcıları, Azure hesabınızdaki *konuşma* kaynağını kullanarak fatura bilgilerini Azure'a gönderir.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için [yapı kaplarını yapıya kullanabilirsiniz.](speech-container-configuration.md)

<!--blogs/samples/video courses -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, Konuşma kapsayıcılarını indirmek, yüklemek ve çalıştırmak için kavramları ve iş akışını öğrendiniz. Özet:

* Konuşma Docker için dört Linux kapsayıcısı sağlar, çeşitli yetenekleri kapsülleme:
  * *Konuşmayı metne dönüştürme*
  * *Özel Konuşma-metin*
  * *Metin okuma*
  * *Özel Metinden konuşmaya*
* Konteyner görüntüleri Azure'daki konteyner kayıt defterinden indirilir.
* Konteyner görüntüleri Docker'da çalışır.
* İster REST API'sını (yalnızca metinden konuşmaya) ister SDK'yı (konuşma dan metine veya metinden konuşmaya) kullanarak kabın ana bilgisayar URI'sini belirtirsiniz. 
* Bir kapsayıcıyı anında kullanırken fatura bilgilerini sağlamanız gerekir.

> [!IMPORTANT]
>  Bilişsel Hizmetler kapsayıcıları ölçüm için Azure'a bağlı olmadan çalışma lisansı na sahip değildir. Müşterilerin, konteynerlerin fatura bilgilerini her zaman ölçüm hizmetiyle iletmesini sağlaması gerekir. Bilişsel Hizmetler kapsayıcıları müşteri verilerini (örn. analiz edilen resim veya metin) Microsoft'a göndermez.

## <a name="next-steps"></a>Sonraki adımlar

* Yapılandırma ayarları için [yapı kaplarını gözden](speech-container-configuration.md) geçirme
* [Kubernetes ve Helm ile Konuşma hizmet kaplarını](speech-container-howto-on-premises.md) nasıl kullanacağınızı öğrenin
* Daha fazla [Bilişsel Hizmet kapsayıcısı](../cognitive-services-container-support.md) kullanın
