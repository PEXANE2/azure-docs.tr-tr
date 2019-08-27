---
title: Kapsayıcıları yüklemek ve çalıştırmak-Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: İndirme, yükleme ve bu izlenecek yol öğreticide görüntü işleme için kapsayıcıları çalıştırmak nasıl.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: dapine
ms.custom: seodec18
ms.openlocfilehash: cbf199c391b49518bb595d7d1a0ed47147903a85
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70034489"
---
# <a name="install-and-run-recognize-text-containers"></a>Metin Tanıma kapsayıcıları yükleyip çalıştırma

Görüntü işleme metni tanı kısmı, bir Docker kapsayıcısı da sunulur. Algılayıp farklı yüzey ve arka planlar, giriş ve posterler kartvizitler gibi çeşitli nesne görüntülerdeki yazılı metni Ayıkla verir.

> [!IMPORTANT]
> Metni Tanı kapsayıcı şu anda yalnızca İngilizce ile çalışır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Metin Tanıma kapsayıcıları kullanmadan önce aşağıdaki önkoşulları karşılamanız gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Bir [ana bilgisayarda](#the-host-computer)Docker altyapısının yüklü olması gerekir. Docker, [MacOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/)ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms)'ta Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).<br><br> Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır. <br><br> **Windows 'da**Docker 'ın de Linux kapsayıcılarını destekleyecek şekilde yapılandırılması gerekir.<br><br>|
|Docker ile benzerlik | Kayıt defterleri, depolar, kapsayıcılar ve kapsayıcı görüntüleri gibi Docker kavramlarının yanı sıra temel `docker` komutlar hakkında bilgi sahibi olmanız gerekir.| 
|Görüntü İşleme kaynağı |Kapsayıcısını kullanabilmeniz için şunları yapmanız gerekir:<br><br>Uç nokta URI 'SI olan bir Azure **görüntü işleme** kaynağı ve ilişkili API anahtarı. Her iki değer de kaynak için genel bakış ve anahtarlar sayfalarında bulunur ve kapsayıcıyı başlatmak için gereklidir.<br><br>**{API_KEY}** : **Anahtarlar** sayfasındaki kullanılabilir iki kaynak anahtardan biri<br><br>**{ENDPOINT_URI}** : **Genel bakış** sayfasında belirtilen bitiş noktası|

## <a name="request-access-to-the-private-container-registry"></a>Özel kapsayıcı kayıt defterine erişim isteği

[!INCLUDE [Request access to public preview](../../../includes/cognitive-services-containers-request-access.md)]

### <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

[!INCLUDE [Container requirements and recommendations](includes/container-requirements-and-recommendations.md)]

## <a name="get-the-container-image-with-docker-pull"></a>Kapsayıcı görüntüsünü al`docker pull`

Metin Tanıma için kapsayıcı görüntüleri kullanılabilir. 

| Kapsayıcı | Havuz |
|-----------|------------|
|Metin Tanıma | `containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest` |

Bir kapsayıcı görüntüsünü indirmek için [komutunukullanın.`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/)


### <a name="docker-pull-for-the-recognize-text-container"></a>Metin Tanıma kapsayıcısı için Docker Pull

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest
```

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

## <a name="how-to-use-the-container"></a>Kapsayıcıyı kullanma

Kapsayıcı [ana bilgisayardan](#the-host-computer)olduktan sonra, kapsayıcında çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı](#run-the-container-with-docker-run)gerekli faturalandırma ayarlarıyla çalıştırın. `docker run` Komuta daha fazla [örnek](computer-vision-resource-container-config.md) kullanılabilir. 
1. [Kapsayıcının tahmin uç noktasını sorgulayın](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Kapsayıcıyı ile çalıştırma`docker run`

Kapsayıcıyı çalıştırmak için [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. Komut aşağıdaki parametreleri kullanır:

| Yer tutucu | Value |
|-------------|-------|
|{API_KEY} | Bu anahtar, kapsayıcıyı başlatmak için kullanılır ve Azure `Cognitive Services` anahtarları sayfasında kullanılabilir.  |
|{ENDPOINT_URI} | Faturalandırma uç noktası URI değeri. Örnek:`https://westus.api.cognitive.microsoft.com/vision/v2.0`|

Aşağıdaki BILLING_ENDPOINT_URI örneğinde gösterildiği gibi `vision/v2.0` , yönlendirmeyi uç nokta URI 'sine eklemeniz gerekir.

Aşağıdaki örnek `docker run` komutta bu parametreleri kendi değerlerinizle değiştirin.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Bu komut:

* Kapsayıcı görüntüsünden bir tanıma kapsayıcısı çalıştırır
* Bir CPU çekirdeği ve 4 gigabayt (GB) bellek ayırır
* 5000 numaralı TCP bağlantı noktasını kullanıma sunar ve sahte TTY için kapsayıcı ayırır.
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir. 

`docker run` Komuta daha fazla [örnek](./computer-vision-resource-container-config.md#example-docker-run-commands) kullanılabilir. 

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](#billing).

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]


## <a name="query-the-containers-prediction-endpoint"></a>Kapsayıcının tahmin uç noktasını sorgulama

Kapsayıcı, REST tabanlı sorgu tahmin uç noktası API 'Leri sağlar. 

Kapsayıcı API 'leri için `http://localhost:5000`Konağı kullanın.

### <a name="asynchronous-text-recognition"></a>Zaman uyumsuz metin tanıma

Kullanabileceğiniz `POST /vision/v2.0/recognizeText` ve `GET /vision/v2.0/textOperations/*{id}*` işlemlerini zaman uyumsuz olarak yazdırılan metin, görüntü işleme hizmeti bu karşılık gelen REST işlemlerini nasıl kullandığı için benzer resim tanımak için uyumlu bir şekilde. Metni Tanı kapsayıcı yazdırılan metin, el yazısı olmayan metin şu anda yalnızca tanır. böylece `mode` normalde görüntü işleme hizmeti işlemi metni tanı kapsayıcı tarafından göz ardı edilir için belirtilen parametre.

### <a name="synchronous-text-recognition"></a>Zaman uyumlu metin tanıma

Kullanabileceğiniz `POST /vision/v2.0/recognizeTextDirect` görüntüdeki basılı metin eş zamanlı olarak tanıyacak şekilde işlemi. Bu işlem zaman uyumlu olduğundan, bu işlem için istek gövdesi `POST /vision/v2.0/recognizeText` işlemle aynıdır, ancak bu işlemin yanıt gövdesi `GET /vision/v2.0/textOperations/*{id}*` işlem tarafından döndürülen ile aynıdır.

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]


## <a name="stop-the-container"></a>Kapsayıcıyı durdur

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcıyı bir çıkış [bağlaması](./computer-vision-resource-container-config.md#mount-settings) ve günlüğü etkin olarak çalıştırırsanız kapsayıcı, kapsayıcıyı başlatırken veya çalıştırırken oluşan sorunları gidermek için yararlı olan günlük dosyaları oluşturur. 


## <a name="billing"></a>Faturalandırma

Metin Tanıma kapsayıcıları Azure hesabınızdaki bir _metin tanıma_ kaynak kullanarak faturalandırma bilgilerini Azure 'a gönderir. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](./computer-vision-resource-container-config.md).

<!--blogs/samples/video course -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, Metin Tanıma kapsayıcıları indirmek, yüklemek ve çalıştırmak için kavramları ve iş akışını öğrendiniz. Özet:

* Metin Tanıma, Docker için bir Linux kapsayıcısı sağlar ve Kapsüllenen metni tanır.
* Kapsayıcı görüntülerini azure'da Microsoft kapsayıcı kayıt defteri (MCR) alanından indirilir.
* Docker kapsayıcı görüntüleri çalıştırın.
* Kapsayıcının ana bilgisayar URI 'sini belirterek Metin Tanıma kapsayıcılarındaki işlemleri çağırmak için REST API veya SDK kullanabilirsiniz.
* Bir kapsayıcı örneği oluşturulurken, fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcıları, müşteri verilerini (örneğin, çözümlenmekte olan resim veya metin) Microsoft 'a göndermez.

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [kapsayıcıları yapılandırma](computer-vision-resource-container-config.md) yapılandırma ayarları
* Gözden geçirme [görüntü işleme genel bakış](Home.md) yazdırılan ve el yazısı metinleri tanıma hakkında daha fazla bilgi edinmek için  
* Başvurmak [görüntü işleme API'si](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) kapsayıcı tarafından desteklenen yöntemleri hakkında ayrıntılar için.
* Başvurmak [sık sorulan sorular (SSS)](FAQ.md) görüntü işleme işlevselliği ile ilgili sorunları gidermek için.
* Daha fazla bilişsel [Hizmetler kapsayıcısı](../cognitive-services-container-support.md) kullanın
