---
title: Kapsayıcıları yükleyip çalıştırma-Metin Analizi
titleSuffix: Azure Cognitive Services
description: Bu adım adım öğreticide Metin Analizi için kapsayıcıları indirme, yükleme ve çalıştırma.
services: cognitive-services
author: aahill
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 05/08/2020
ms.author: aahi
ms.openlocfilehash: efe76323b4159af01f1eaf470d9c1833edd0a186
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83702138"
---
# <a name="install-and-run-text-analytics-containers"></a>Metin Analizi kapsayıcılarını yükleme ve çalıştırma

> [!NOTE]
> * V3 Yaklaşım Analizi kapsayıcısı genel kullanıma sunulmuştur. Anahtar tümceciği ayıklama ve dil algılama kapsayıcıları, geçitli genel önizleme olarak kullanılabilir.
> * Varlık bağlama ve NER Şu anda bir kapsayıcı olarak kullanılamıyor.

Kapsayıcılar, kendi ortamınızda metin analitik API 'Leri çalıştırmanızı sağlar ve belirli güvenlik ve veri idare gereksinimleriniz için harika bir yöntemdir. Metin Analizi kapsayıcıları ham metin üzerinde gelişmiş doğal dil işleme sağlar ve üç ana işlev içerir: yaklaşım analizi, anahtar ifade ayıklama ve dil algılama. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!IMPORTANT]
> Ücretsiz hesap, ayda 5.000 işlem ile sınırlıdır ve kapsayıcılar için yalnızca **ücretsiz** ve **Standart** <a href="https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics" target="_blank">fiyatlandırma katmanları <span class="docon docon-navigate-external x-hidden-focus"></span> </a> geçerlidir. İşlem isteği ücretleri hakkında daha fazla bilgi için bkz. [veri limitleri](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview#data-limits).

## <a name="prerequisites"></a>Önkoşullar

Metin Analizi kapsayıcılarından herhangi birini çalıştırmak için konak bilgisayar ve kapsayıcı ortamları olmalıdır.

## <a name="preparation"></a>Hazırlık

Metin Analizi kapsayıcıları kullanmadan önce aşağıdaki önkoşulları karşılamanız gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Bir [ana bilgisayarda](#the-host-computer)Docker altyapısının yüklü olması gerekir. Docker, [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms) üzerinde Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temel bilgileri ile ilgili giriş yapmak için [Docker’a genel bakış](https://docs.docker.com/engine/docker-overview/) bölümüne bakın.<br><br> Kapsayıcıların Azure 'a bağlanıp faturalandırma verilerini göndermesini sağlamak için Docker yapılandırılmalıdır. <br><br> **Windows 'da**Docker 'ın de Linux kapsayıcılarını destekleyecek şekilde yapılandırılması gerekir.<br><br>|
|Docker ile benzerlik | Kayıt defterleri, depolar, kapsayıcılar ve kapsayıcı görüntüleri gibi Docker kavramlarının yanı sıra temel komutlar hakkında bilgi sahibi olmanız gerekir `docker` .| 
|Metin Analizi kaynağı |Kapsayıcısını kullanabilmeniz için şunları yapmanız gerekir:<br><br>İlişkili API anahtarını ve uç nokta URI 'sini almak için bir Azure [metin analizi kaynağı](../../cognitive-services-apis-create-account.md) . Her iki değer de Azure portal Metin Analizi genel bakış ve anahtarlar sayfalarında bulunur ve kapsayıcıyı başlatmak için gereklidir.<br><br>**{API_KEY}**: **anahtarlar** sayfasında kullanılabilir iki kaynak anahtardan biri<br><br>**{ENDPOINT_URI}**: **genel bakış** sayfasında belirtilen bitiş noktası|

[!INCLUDE [Gathering required parameters](../../containers/includes/container-gathering-required-parameters.md)]

## <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

Aşağıdaki tabloda Metin Analizi kapsayıcıları için en düşük ve önerilen özellikler açıklanmaktadır. En az 2 gigabayt (GB) bellek gereklidir ve her CPU çekirdeği en az 2,6 gigahertz (GHz) veya daha hızlı olmalıdır. Bölüm başına izin verilen Işlem (TPS) de listelenir.

|  | En düşük ana bilgisayar özellikleri | Önerilen konak özellikleri | En az TPS | Maksimum TPS|
|---|---------|-------------|--|--|
| **Dil algılama, anahtar tümceciği ayıklama**   | 1 çekirdek, 2GB bellek | 1 çekirdek, 4GB bellek |15 | 30|
| **Yaklaşım Analizi v3**   | 1 çekirdek, 2GB bellek | 4 çekirdek, 8GB bellek |15 | 30|

CPU çekirdeği ve belleği, `--cpus` `--memory` komutunun bir parçası olarak kullanılan ve ayarlarına karşılık gelir `docker run` .

## <a name="get-the-container-image-with-docker-pull"></a>Kapsayıcı görüntüsünü al`docker pull`

[!INCLUDE [Tip for using docker list](../../../../includes/cognitive-services-containers-docker-list-tip.md)]

Metin Analizi için kapsayıcı görüntüleri Microsoft Container Registry kullanılabilir.

# <a name="sentiment-analysis-v3"></a>[Yaklaşım Analizi v3](#tab/sentiment)

[!INCLUDE [docker-pull-sentiment-analysis-container](../includes/docker-pull-sentiment-analysis-container.md)]

# <a name="key-phrase-extraction-preview"></a>[Anahtar İfade Ayıklama (Önizleme)](#tab/keyphrase)

[!INCLUDE [docker-pull-key-phrase-extraction-container](../includes/docker-pull-key-phrase-extraction-container.md)]

# <a name="language-detection-preview"></a>[Dil Algılama (Önizleme)](#tab/language)

[!INCLUDE [docker-pull-language-detection-container](../includes/docker-pull-language-detection-container.md)]

***

## <a name="how-to-use-the-container"></a>Kapsayıcıyı kullanma

Kapsayıcı [ana bilgisayardan](#the-host-computer)olduktan sonra, kapsayıcında çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı](#run-the-container-with-docker-run)gerekli faturalandırma ayarlarıyla çalıştırın.
1. [Kapsayıcının tahmin uç noktasını sorgulayın](#query-the-containers-prediction-endpoint).

## <a name="run-the-container-with-docker-run"></a>Kapsayıcıyı ile çalıştırma`docker run`

Kapsayıcıları çalıştırmak için [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. Kapsayıcı siz durduruncaya kadar çalışmaya devam edecektir.

Aşağıdaki yer tutucuları kendi değerlerinizle değiştirin:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
| **{API_KEY}** | Metin Analizi kaynağınız için anahtar. Bunu, kaynağın **anahtar ve uç nokta** sayfasında, Azure Portal bulabilirsiniz. |`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`|
| **{ENDPOINT_URI}** | Metin Analizi API'si erişmek için uç nokta. Bunu, kaynağın **anahtar ve uç nokta** sayfasında, Azure Portal bulabilirsiniz. | `https://<your-custom-subdomain>.cognitiveservices.azure.com` |

> [!IMPORTANT]
> * Aşağıdaki bölümlerdeki Docker komutları, `\` satır devamlılık karakteri olarak ters eğik çizgi kullanır. Bunu, ana bilgisayar işletim sisteminizin gereksinimlerine göre değiştirin veya kaldırın. 
> * `Eula` `Billing` `ApiKey` Kapsayıcıyı çalıştırmak için, ve seçenekleri belirtilmelidir; Aksi takdirde kapsayıcı başlatılmaz.  Daha fazla bilgi için bkz. [faturalandırma](#billing).
> * Yaklaşım Analizi v3 kapsayıcısı, yanıttaki yaklaşım [etiketleri](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) döndüren genel kullanıma sunulmuştur. Anahtar ifade ayıklama ve dil algılama kapsayıcıları API 'nin v2 kullanır ve önizlemededir.

# <a name="sentiment-analysis-v3"></a>[Yaklaşım Analizi v3](#tab/sentiment)

[!INCLUDE [docker-run-sentiment-analysis-container](../includes/docker-run-sentiment-analysis-container.md)]

# <a name="key-phrase-extraction-preview"></a>[Anahtar İfade Ayıklama (Önizleme)](#tab/keyphrase)

[!INCLUDE [docker-run-key-phrase-extraction-container](../includes/docker-run-key-phrase-extraction-container.md)]

# <a name="language-detection-preview"></a>[Dil Algılama (Önizleme)](#tab/language)

[!INCLUDE [docker-run-language-detection-container](../includes/docker-run-language-detection-container.md)]

***

[!INCLUDE [Running multiple containers on the same host](../../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

## <a name="query-the-containers-prediction-endpoint"></a>Kapsayıcının tahmin uç noktasını sorgulama

Kapsayıcı, REST tabanlı sorgu tahmin uç noktası API 'Leri sağlar.

`http://localhost:5000`Kapsayıcı API 'leri için Konağı kullanın.

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Kapsayıcıyı durdurma

[!INCLUDE [How to stop the container](../../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcıyı bir çıkış [bağlaması](../text-analytics-resource-container-config.md#mount-settings) ve günlüğü etkin olarak çalıştırırsanız kapsayıcı, kapsayıcıyı başlatırken veya çalıştırırken oluşan sorunları gidermek için yararlı olan günlük dosyaları oluşturur.

[!INCLUDE [Cognitive Services FAQ note](../../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Faturalandırma

Metin Analizi kapsayıcıları Azure hesabınızdaki bir _metin analizi_ kaynak kullanarak faturalandırma bilgilerini Azure 'a gönderir. 

[!INCLUDE [Container's Billing Settings](../../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](../text-analytics-resource-container-config.md).

<!--blogs/samples/video course -->

[!INCLUDE [Discoverability of more container information](../../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, Metin Analizi kapsayıcıları indirmek, yüklemek ve çalıştırmak için kavramları ve iş akışını öğrendiniz. Özet:

* Metin Analizi, çeşitli özellikleri kapsüllemek üzere Docker için üç Linux kapsayıcısı sağlar:
   * *Yaklaşım Analizi*
   * *Anahtar İfade Ayıklama (Önizleme)* 
   * *Dil Algılama (Önizleme)*
   
* Kapsayıcı görüntüleri Azure 'daki Microsoft Container Registry (MCR) ' den indirilir.
* Kapsayıcı görüntüleri Docker 'da çalışır.
* Kapsayıcının ana bilgisayar URI 'sini belirterek Metin Analizi kapsayıcılarındaki işlemleri çağırmak için REST API veya SDK kullanabilirsiniz.
* Bir kapsayıcıyı örnekledikten sonra faturalandırma bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel hizmetler kapsayıcıları, ölçüm için Azure 'a bağlı kalmadan çalıştırılmak üzere lisanslanmaz. Müşterilerin, ödeme bilgilerini her zaman ölçüm hizmetiyle iletişimine olanak tanımak için kapsayıcıların etkinleştirilmesi gerekir. Bilişsel hizmetler kapsayıcıları, müşteri verilerini (örn. çözümlenmekte olan metin) Microsoft 'a göndermez.

## <a name="next-steps"></a>Sonraki adımlar

* Yapılandırma ayarları için [kapsayıcıları](../text-analytics-resource-container-config.md) yapılandırmayı gözden geçir
* İşlevlerle ilgili sorunları çözmek için [sık sorulan sorular (SSS)](../text-analytics-resource-faq.md) bölümüne bakın.
