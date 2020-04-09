---
title: Kapsayıcıları yükleme ve çalıştırma - Metin Analizi
titleSuffix: Azure Cognitive Services
description: Bu walkthrough öğreticisinde Text Analytics için kapsayıcıları indirme, yükleme ve çalıştırma.
services: cognitive-services
author: aahill
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 2d44df1bb828140e662b06ffbe5fb14f207f68e0
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80877080"
---
# <a name="install-and-run-text-analytics-containers"></a>Metin Analizi kapsayıcılarını yükleme ve çalıştırma

Kapsayıcılar, Metin Analizi API'lerini kendi ortamınızda çalıştırmanızı sağlar ve özel güvenlik ve veri yönetimi gereksinimleriniz için harikadır. Text Analytics kapsayıcıları ham metin üzerinde gelişmiş doğal dil işleme sağlar ve üç ana işlevi içerir: duygu analizi, anahtar cümle çıkarma ve dil algılama. Varlık bağlantısı şu anda bir kapsayıcıda desteklenmez.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

> [!IMPORTANT]
> Ücretsiz hesap ayda 5.000 işlemle sınırlıdır ve **Standard** <a href="https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics" target="_blank">konteynerler <span class="docon docon-navigate-external x-hidden-focus"></span> </a> için yalnızca **Ücretsiz** ve Standart fiyatlandırma katmanları geçerlidir. Hareket isteği oranları hakkında daha fazla bilgi için [Veri Sınırları'na](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview#data-limits)bakın.

## <a name="prerequisites"></a>Ön koşullar

Text Analytics kapsayıcılarından herhangi birini çalıştırmak için ana bilgisayar ve kapsayıcı ortamlarına sahip olmalısınız.

## <a name="preparation"></a>Hazırlık

Text Analytics kapsayıcılarını kullanmadan önce aşağıdaki ön koşulları karşılamanız gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Docker [Engine'in ana bilgisayara](#the-host-computer)yüklenmesi gerekir. Docker, [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms) üzerinde Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temel bilgileri ile ilgili giriş yapmak için [Docker’a genel bakış](https://docs.docker.com/engine/docker-overview/) bölümüne bakın.<br><br> Docker, kapsayıcıların Azure'a bağlanmasına ve fatura verilerini göndermesine izin verecek şekilde yapılandırılmalıdır. <br><br> **Windows'da**Docker, Linux kapsayıcılarını destekleyecek şekilde de yapılandırılmalıdır.<br><br>|
|Docker ile aşinalık | Docker kavramları hakkında, kayıt defterleri, depolar, konteynerler ve konteyner resimleri nin yanı sıra `docker` temel komutlar hakkında temel bilgilere sahip olmalısınız.| 
|Metin Analizi kaynağı |Kapsayıcıyı kullanabilmek için aşağıdakilere sahip olmalısınız:<br><br>İlişkili API anahtarını ve bitiş noktası URI'yi almak için bir Azure [Metin Analizi kaynağı.](../../cognitive-services-apis-create-account.md) Her iki değer de Azure portalının Metin Analizi Genel Bakış ve Keys sayfalarında mevcuttur ve kapsayıcıyı başlatmak için gereklidir.<br><br>**{API_KEY}**: **Keys** sayfasındaki mevcut iki kaynak anahtarından biri<br><br>**{ENDPOINT_URI}**: **Genel Bakış** sayfasında sağlanan bitiş noktası|

[!INCLUDE [Gathering required parameters](../../containers/includes/container-gathering-required-parameters.md)]

## <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Konteyner gereksinimleri ve öneriler

Aşağıdaki tabloda, her Text Analytics kapsayıcısı için tahsis etmek üzere gigabaytlarda (GB) en az 2,6 gigahertz (GHz) veya daha hızlı ve bellek olan minimum ve önerilen CPU çekirdekleri açıklanmaktadır.

# <a name="key-phrase-extraction"></a>[Anahtar İfade Ayıklama](#tab/keyphrase)

[!INCLUDE [key-phrase-extraction-container-requirements](../includes/key-phrase-extraction-container-requirements.md)]

# <a name="language-detection"></a>[Dil Algılama](#tab/language)

[!INCLUDE [language-detection-container-requirements](../includes/language-detection-container-requirements.md)]

# <a name="sentiment-analysis"></a>[Duygusallık Analizi](#tab/sentiment)

[!INCLUDE [sentiment-analysis-container-requirements](../includes/sentiment-analysis-container-requirements.md)]

***

* Her çekirdek en az 2,6 gigahertz (GHz) veya daha hızlı olmalıdır.
* TPS - saniyedeki işlemler

Çekirdek ve bellek, `--cpus` `--memory` komutun bir parçası olarak `docker run` kullanılan ve ayarlara karşılık gelir.

## <a name="get-the-container-image-with-docker-pull"></a>Konteyner görüntüsünü`docker pull`

Metin Analizi için kapsayıcı resimleri Microsoft Konteyner Kayıt Defteri'nde mevcuttur.

# <a name="key-phrase-extraction"></a>[Anahtar İfade Ayıklama](#tab/keyphrase)

[!INCLUDE [key-phrase-extraction-container-repository](../includes/key-phrase-extraction-container-repository.md)]

# <a name="language-detection"></a>[Dil Algılama](#tab/language)

[!INCLUDE [language-detection-container-repository](../includes/language-detection-container-repository.md)]

# <a name="sentiment-analysis"></a>[Duygusallık Analizi](#tab/sentiment)

[!INCLUDE [sentiment-analysis-container-repository](../includes/sentiment-analysis-container-repository.md)]

***

[!INCLUDE [Tip for using docker list](../../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-text-analytics-containers"></a>Metin Analizi kapsayıcıları için Docker çekme

# <a name="key-phrase-extraction"></a>[Anahtar İfade Ayıklama](#tab/keyphrase)

[!INCLUDE [docker-pull-key-phrase-extraction-container](../includes/docker-pull-key-phrase-extraction-container.md)]

# <a name="language-detection"></a>[Dil Algılama](#tab/language)

[!INCLUDE [docker-pull-language-detection-container](../includes/docker-pull-language-detection-container.md)]

# <a name="sentiment-analysis"></a>[Duygusallık Analizi](#tab/sentiment)

[!INCLUDE [docker-pull-sentiment-analysis-container](../includes/docker-pull-sentiment-analysis-container.md)]

***

## <a name="how-to-use-the-container"></a>Kapsayıcı nasıl kullanılır?

Kapsayıcı [ana bilgisayara](#the-host-computer)yüklendikten sonra, kapsayıcıyla çalışmak için aşağıdaki işlemi kullanın.

1. Gerekli fatura ayarlarıyla [kapsayıcıyı çalıştırın.](#run-the-container-with-docker-run) Komutun `docker run` daha fazla [örneği](../text-analytics-resource-container-config.md#example-docker-run-commands) mevcuttur.
1. [Kapsayıcının tahmin bitiş noktasını sorgula.](#query-the-containers-prediction-endpoint)

## <a name="run-the-container-with-docker-run"></a>Kapsayıcıyı çalıştırın`docker run`

Üç kapsayıcıdan herhangi birini çalıştırmak için [docker run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. Değerleri ve `{API_KEY}` değerleri nasıl elde edineceklerine ilişkin ayrıntılar için [gerekli parametreleri](#gathering-required-parameters) `{ENDPOINT_URI}` toplamaya bakın.

Komut [örnekleri](../text-analytics-resource-container-config.md#example-docker-run-commands) `docker run` mevcuttur.

# <a name="key-phrase-extraction"></a>[Anahtar İfade Ayıklama](#tab/keyphrase)

[!INCLUDE [docker-run-key-phrase-extraction-container](../includes/docker-run-key-phrase-extraction-container.md)]

# <a name="language-detection"></a>[Dil Algılama](#tab/language)

[!INCLUDE [docker-run-language-detection-container](../includes/docker-run-language-detection-container.md)]

# <a name="sentiment-analysis"></a>[Duygusallık Analizi](#tab/sentiment)

[!INCLUDE [docker-run-sentiment-analysis-container](../includes/docker-run-sentiment-analysis-container.md)]

***

> [!IMPORTANT]
> `Eula`Kapsayıcıyı `Billing`çalıştırmak `ApiKey` için , ve seçenekler belirtilmelidir; aksi takdirde, kapsayıcı başlamaz.  Daha fazla bilgi için [Faturalandırma'ya](#billing)bakın.

[!INCLUDE [Running multiple containers on the same host](../../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

## <a name="query-the-containers-prediction-endpoint"></a>Kapsayıcının tahmin bitiş noktasını sorgula

Kapsayıcı REST tabanlı sorgu tahmin bitiş noktası API'leri sağlar.

Kapsayıcı API'leri için ana bilgisayarı `http://localhost:5000`kullanın.

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Kapsayıcıyı durdurma

[!INCLUDE [How to stop the container](../../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcıyı çıktı [yuvası](../text-analytics-resource-container-config.md#mount-settings) ve günlüğe kaydetme etkinken çalıştırıyorsanız, kapsayıcı kapsayıcıyı çalıştırırken veya çalıştırırken meydana gelen sorunları gidermeye yardımcı olan günlük dosyaları oluşturur.

[!INCLUDE [Cognitive Services FAQ note](../../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Faturalandırma

Metin Analizi kapsayıcıları, Azure hesabınızdaki _Metin Analizi_ kaynağını kullanarak fatura bilgilerini Azure'a gönderir. 

[!INCLUDE [Container's Billing Settings](../../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için [yapı kaplarını yapıya kullanabilirsiniz.](../text-analytics-resource-container-config.md)

<!--blogs/samples/video course -->

[!INCLUDE [Discoverability of more container information](../../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, Metin Analizi kapsayıcılarını indirmek, yüklemek ve çalıştırmak için kavramları ve iş akışını öğrendiniz. Özet:

* Text Analytics, Docker için çeşitli yetenekleri kapsayan üç Linux kapsayıcısı sağlar:
   * *Anahtar İfade Ayıklama*
   * *Dil Algılama*
   * *Duygusallık Analizi*
* Kapsayıcı resimleri Azure'daki Microsoft Kapsayıcı Kayıt Defteri'nden (MCR) indirilir.
* Konteyner görüntüleri Docker'da çalışır.
* Kapsayıcının ana bilgisayar URI'sini belirterek Text Analytics kaplarında ki işlemleri aramak için REST API veya SDK'yı kullanabilirsiniz.
* Bir kapsayıcıyı anında kullanırken fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel Hizmetler kapsayıcıları ölçüm için Azure'a bağlı olmadan çalışma lisansı na sahip değildir. Müşterilerin, konteynerlerin fatura bilgilerini her zaman ölçüm hizmetiyle iletmesini sağlaması gerekir. Bilişsel Hizmetler kapsayıcıları müşteri verilerini (örn. analiz edilen resim veya metin) Microsoft'a göndermez.

## <a name="next-steps"></a>Sonraki adımlar

* Yapılandırma ayarları için [kapsayıcıları yapılandırmayı](../text-analytics-resource-container-config.md) gözden geçirin
* İşlevsellikle ilgili sorunları çözmek için [sık sorulan sorulara (SSS)](../text-analytics-resource-faq.md) bakın.
