---
title: Konteynerleri yükleme ve çalıştırma - Yüz
titleSuffix: Azure Cognitive Services
description: Bu makalede, bu gözden geçirme öğreticisinde Face için kapsayıcıları nasıl indirdiğinizi, yükleyip çalıştırabileceğinizi gösterir.
services: cognitive-services
author: aahill
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 74465bddb57c14af4d02c1d3bfdc46f3ac25bef3
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878553"
---
# <a name="install-and-run-face-containers-preview"></a>Yüz kaplarını yükleme ve çalıştırma (Önizleme)

Azure Bilişsel Hizmetler Yüzü, Docker için görüntülerdeki insan yüzlerini algılayan standart laştırılmış bir Linux kapsayıcısı sağlar. Ayrıca, burun ve gözler, cinsiyet, yaş ve makine tarafından öngörülen diğer yüz özellikleri gibi yüz işaretleri içeren özellikleri tanımlar. Algılamaya ek olarak, Face aynı görüntüdeki iki yüzün veya farklı görüntülerin güven puanı kullanarak aynı olup olmadığını kontrol edebilir. Yüz ayrıca, benzer görünümlü veya aynı yüzün zaten var olup olmadığını görmek için yüzleri veritabanıyla karşılaştırabilir. Ayrıca paylaşılan görsel özellikleri kullanarak benzer yüzleri gruplar halinde düzenleyebilir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Face servis kaplarını kullanmadan önce aşağıdaki ön koşulları karşılamanız gerekir.

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Docker Engine [bir ana bilgisayara](#the-host-computer)yüklenmelidir. Docker, [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms) üzerinde Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temel bilgileri ile ilgili giriş yapmak için [Docker’a genel bakış](https://docs.docker.com/engine/docker-overview/) bölümüne bakın.<br><br> Docker, kapsayıcıların Azure'a bağlanmasına ve fatura verilerini göndermesine izin verecek şekilde yapılandırılmalıdır. <br><br> Windows'da Docker, Linux kapsayıcılarını destekleyecek şekilde de yapılandırılmalıdır.<br><br>|
|Docker ile aşinalık | Kayıt defterleri, depolar, kapsayıcılar ve kapsayıcı görüntüleri gibi Docker kavramları hakkında temel bir anlayışa ihtiyacınız vardır. Ayrıca temel `docker` komutları bilgi gerekir.| 
|Yüz kaynağı |Kapsayıcıyı kullanmak için şunları yapmış olmalısınız:<br><br>Azure **Yüz** kaynağı ve ilişkili API anahtarı ve bitiş noktası URI. Her iki değer de kaynak için **Genel Bakış** ve **Anahtarlar** sayfalarında kullanılabilir. Konteynırı çalıştırmaları gerekiyor.<br><br>**{API_KEY}**: **Keys** sayfasındaki mevcut iki kaynak anahtarından biri<br><br>**{ENDPOINT_URI}**: **Genel Bakış** sayfasında sağlanan bitiş noktası

[!INCLUDE [Gathering required container parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="request-access-to-the-private-container-registry"></a>Özel konteyner kayıt defterine erişim isteği

[!INCLUDE [Request access to private container registry](../../../includes/cognitive-services-containers-request-access.md)]

### <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Konteyner gereksinimleri ve öneriler

Aşağıdaki tabloda, her Yüz servis kapsayıcısı için ayrılacak minimum ve önerilen CPU çekirdekleri ve bellek açıklanmaktadır.

| Kapsayıcı | Minimum | Önerilen | Saniyedeki işlemler<br>(Minimum, maksimum)|
|-----------|---------|-------------|--|
|Yüz | 1 çekirdekli, 2 GB bellek | 1 çekirdekli, 4 GB bellek |10, 20|

* Her çekirdek en az 2,6 GHz veya daha hızlı olmalıdır.
* Saniyedeki hareketler (TPS).

Çekirdek ve bellek, `--cpus` `--memory` komutun bir parçası olarak `docker run` kullanılan ve ayarlara karşılık gelir.

## <a name="get-the-container-image-with-docker-pull"></a>Docker çekme ile konteyner görüntüsünü alın

Face hizmeti için konteyner görüntüleri mevcuttur. 

| Kapsayıcı | Depo |
|-----------|------------|
| Yüz | `containerpreview.azurecr.io/microsoft/cognitive-services-face:latest` |

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-face-container"></a>Yüz konteyneri için Docker çekme

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-face:latest
```

## <a name="use-the-container"></a>Kapsayıcıyı kullanma

Kapsayıcı [ana bilgisayarda](#the-host-computer)sonra, kapsayıcı ile çalışmak için aşağıdaki işlemi kullanın.

1. Gerekli fatura ayarlarıyla [kapsayıcıyı çalıştırın.](#run-the-container-with-docker-run) Komutun `docker run` daha fazla [örneği](./face-resource-container-config.md#example-docker-run-commands) mevcuttur. 
1. [Kapsayıcının tahmin bitiş noktasını sorgula.](#query-the-containers-prediction-endpoint) 

## <a name="run-the-container-with-docker-run"></a>Docker çalıştırın ile konteyner çalıştırın

Kapsayıcıyı çalıştırmak için [docker run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. Değerleri ve `{API_KEY}` değerleri nasıl elde edineceklerine ilişkin ayrıntılar için [gerekli parametreleri](#gathering-required-parameters) `{ENDPOINT_URI}` toplamaya bakın.

Komut [örnekleri](face-resource-container-config.md#example-docker-run-commands) `docker run` mevcuttur.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-face \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Şu komut:

* Konteyner görüntüsünden bir yüz kapsayıcısı çalıştırın.
* Bir CPU çekirdeği ve 4 GB bellek ayırır.
* TCP bağlantı noktası 5000'i açığa çıkarır ve kapsayıcı için sahte bir TTY ayırır.
* Çıktıktan sonra kapsayıcıotomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir. 

Komutun `docker run` daha fazla [örneği](./face-resource-container-config.md#example-docker-run-commands) mevcuttur. 

> [!IMPORTANT]
> `Eula`Kapsayıcıyı `Billing`çalıştırmak `ApiKey` için , , ve seçenekler belirtilmelidir, yoksa kapsayıcı başlamaz. Daha fazla bilgi için [Faturalandırma'ya](#billing)bakın.

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]


## <a name="query-the-containers-prediction-endpoint"></a>Kapsayıcının tahmin bitiş noktasını sorgula

Kapsayıcı REST tabanlı sorgu tahmin bitiş noktası API'leri sağlar. 

Kapsayıcı API'leri için ana bilgisayarı `http://localhost:5000`kullanın.


<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Kapsayıcıyı durdurma

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcıyı bir çıktı [yuvasıyla](./face-resource-container-config.md#mount-settings) çalıştırıyorsanız ve günlüğe kaydetme etkinse, kapsayıcı, kapsayıcıyı çalıştırırken veya çalıştırırken meydana gelen sorunları gidermek için yardımcı olan günlük dosyaları oluşturur.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Faturalandırma

Face hizmet kapsayıcıları, Azure hesabınızda bir Yüz kaynağı kullanarak fatura bilgilerini Azure'a gönderir. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için [yapı kaplarını yapıya kullanabilirsiniz.](./face-resource-container-config.md)

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, Face hizmet kapsayıcılarını nasıl karşıdan yükleyin, yükleyin ve çalıştırabileceğinize yönelik kavramları ve iş akışını öğrendiniz. Özet:

* Kapsayıcı resimleri Azure Kapsayıcı Kayıt Defteri'nden indirilir.
* Konteyner görüntüleri Docker'da çalışır.
* Kabın ana bilgisayarı URI'yi belirterek Face servis kaplarında işlemleri aramak için REST API veya SDK'yı kullanabilirsiniz.
* Bir kapsayıcıyı anında yaptığınızda fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel Hizmetler kapsayıcıları ölçüm için Azure'a bağlı olmadan çalışma lisansına sahip değildir. Müşteriler, kapsayıcıların fatura bilgilerini her zaman ölçüm hizmetiyle iletmesini sağlamalıdır. Bilişsel Hizmetler kapsayıcıları, analiz edilen resim veya metin gibi müşteri verilerini Microsoft'a göndermez.

## <a name="next-steps"></a>Sonraki adımlar

* Yapılandırma ayarları için yapı [kaplarını yapıya kullanabilirsiniz.](face-resource-container-config.md)
* Yüzleri nasıl algılayıp tanımlayıp tanımlayabildiğini öğrenmek için [Bkz. Yüz genel bakışı.](Overview.md)
* Kapsayıcı tarafından desteklenen yöntemler hakkında bilgi için [Yüz API'sına](//westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)bakın.
* Daha fazla Bilişsel Hizmetler kapsayıcıları kullanmak için, [Bilişsel Hizmetler kapsayıcılarına](../cognitive-services-container-support.md)bakın.
