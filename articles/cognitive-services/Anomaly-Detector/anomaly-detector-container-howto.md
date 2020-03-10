---
title: Anomali algılayıcı API 'sini kullanarak kapsayıcıları yüklemek ve çalıştırmak
titleSuffix: Azure Cognitive Services
description: Zaman serisi verilerinizde bozukluklar belirlemek için anomali algılayıcı API 'sinin gelişmiş algoritmalarını kullanın.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: dapine
ms.openlocfilehash: 45abd904ea95cf8e68583ba5630a485af59479ec
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78390721"
---
# <a name="install-and-run-anomaly-detector-containers-preview"></a>Anomali algılayıcı kapsayıcılarını (Önizleme) yükleyip çalıştırın

Anomali algılayıcısı aşağıdaki kapsayıcı özelliği işlevselliğine sahiptir:

| İşlev | Özellikler |
|--|--|
| Anomali algılayıcısı | <li> Gerçek zamanlı olarak gerçekleştikleri gibi bozukluklar algılar. <li> Veri kümesinin tamamında bir toplu iş olarak oluşan bozukluklar algılar. <li> Verilerinizin beklenen normal aralığını haller. <li> Verilerinize daha iyi uyum sağlamak için anomali algılama duyarlılığı ayarlamayı destekler. |

API 'Ler hakkında ayrıntılı bilgi için lütfen bkz:
* [Anomali algılayıcı API hizmeti hakkında daha fazla bilgi edinin](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Anomali algılayıcı kapsayıcılarını kullanmadan önce aşağıdaki önkoşulları karşılamanız gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Bir [ana bilgisayarda](#the-host-computer)Docker altyapısının yüklü olması gerekir. Docker, [MacOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/)ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms)'ta Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temelleri hakkında bilgi için bkz. [Docker genel bakış](https://docs.docker.com/engine/docker-overview/).<br><br> Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır. <br><br> **Windows 'da**Docker 'ın de Linux kapsayıcılarını destekleyecek şekilde yapılandırılması gerekir.<br><br>|
|Docker ile benzerlik | Kayıt defterleri, depolar, kapsayıcılar ve kapsayıcı görüntüleri gibi Docker kavramlarından ve temel `docker` komutlarının bilgisine sahip olmanız gerekir.| 
|Anomali algılayıcı kaynağı |Bu kapsayıcıları kullanabilmeniz için, şunları yapmanız gerekir:<br><br>İlişkili API anahtarını ve uç nokta URI 'sini almak için bir Azure _anomali algılayıcısı_ kaynağı. Her iki değer de Azure portal **anomali algılayıcısının** genel bakış ve anahtarlar sayfalarında bulunur ve kapsayıcıyı başlatmak için gereklidir.<br><br>**{API_KEY}** : **anahtarlar** sayfasında kullanılabilir iki kaynak anahtardan biri<br><br>**{ENDPOINT_URI}** : **genel bakış** sayfasında belirtilen bitiş noktası|

[!INCLUDE [Gathering required container parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="request-access-to-the-container-registry"></a>Kapsayıcı kayıt defterine erişim isteme

Kapsayıcıya erişim istemek için öncelikle [anomali algılayıcı kapsayıcı isteği formunu](https://aka.ms/adcontainer) tamamlayıp göndermeniz gerekir.

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

<!--* [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/). For instructions of deploying Anomaly Detector module in IoT Edge, see [How to deploy Anomaly Detector module in IoT Edge](how-to-deploy-anomaly-detector-module-in-iot-edge.md).-->

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

Aşağıdaki tabloda, anomali algılayıcı kapsayıcısı için ayrılacak minimum ve önerilen CPU çekirdekleri ve bellek açıklanmaktadır.

| QPS (saniye başına sorgu) | Minimum | Önerilen |
|-----------|---------|-------------|
| 10 QPS | 4 çekirdek, 1 GB bellek | 8 çekirdekli 2 GB bellek |
| 20 QPS | 8 çekirdek, 2 GB bellek | 16 çekirdek 4 GB bellek |

Her çekirdek en az 2,6 gigahertz (GHz) veya daha hızlı olmalıdır.

Çekirdek ve bellek, `docker run` komutunun bir parçası olarak kullanılan `--cpus` ve `--memory` ayarlarına karşılık gelir.

## <a name="get-the-container-image-with-docker-pull"></a>`docker pull` kapsayıcı görüntüsünü al

Bir kapsayıcı görüntüsünü indirmek için [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) komutunu kullanın.

| Kapsayıcı | Depo |
|-----------|------------|
| bilişsel hizmetler-anomali-algılayıcı | `containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector:latest` |

<!--
For a full description of available tags, such as `latest` used in the preceding command, see [anomaly-detector](https://go.microsoft.com/fwlink/?linkid=2083827&clcid=0x409) on Docker Hub.
-->
[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-anomaly-detector-container"></a>Anomali algılayıcı kapsayıcısı için Docker Pull

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector:latest
```

## <a name="how-to-use-the-container"></a>Kapsayıcıyı kullanma

Kapsayıcı [ana bilgisayardan](#the-host-computer)olduktan sonra, kapsayıcında çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı](#run-the-container-with-docker-run)gerekli faturalandırma ayarlarıyla çalıştırın. `docker run` komutuna daha fazla [örnek](anomaly-detector-container-configuration.md#example-docker-run-commands) kullanılabilir.
1. [Kapsayıcının tahmin uç noktasını sorgulayın](#query-the-containers-prediction-endpoint).

## <a name="run-the-container-with-docker-run"></a>Kapsayıcıyı `docker run` ile çalıştırma

Kapsayıcıyı çalıştırmak için [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. `{ENDPOINT_URI}` ve `{API_KEY}` değerlerini alma hakkında ayrıntılar için [gerekli parametreleri toplama](#gathering-required-parameters) bölümüne bakın.

`docker run` komut [örnekleri](anomaly-detector-container-configuration.md#example-docker-run-commands) mevcuttur.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector:latest \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Bu komut:

* Kapsayıcı görüntüsünden bir anomali algılayıcı kapsayıcısı çalıştırır
* Bir CPU çekirdeği ve 4 gigabayt (GB) bellek ayırır
* 5000 numaralı TCP bağlantı noktasını kullanıma sunar ve sahte TTY için kapsayıcı ayırır.
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir. 

> [!IMPORTANT]
> Kapsayıcıyı çalıştırmak için `Eula`, `Billing`ve `ApiKey` seçenekleri belirtilmelidir; Aksi takdirde, kapsayıcı başlatılmaz.  Daha fazla bilgi için bkz. [faturalandırma](#billing).

### <a name="running-multiple-containers-on-the-same-host"></a>Aynı konakta birden çok kapsayıcı çalıştırma

Birden çok kapsayıcıyı açığa çıkarılan bağlantı noktalarıyla çalıştırmayı düşünüyorsanız, her kapsayıcıyı farklı bir bağlantı noktasıyla çalıştırdığınızdan emin olun. Örneğin, bağlantı noktası 5000 ' deki ilk kapsayıcıyı ve bağlantı noktası 5001 üzerindeki ikinci kapsayıcıyı çalıştırın.

`<container-registry>` ve `<container-name>`, kullandığınız kapsayıcıların değerleriyle değiştirin. Bunların aynı kapsayıcı olması gerekmez. Anomali algılayıcı kapsayıcısının ve LUSıS kapsayıcısının KONAKTA birlikte çalışmasını sağlayabilir veya çalışan birden fazla anomali algılayıcı kapsayıcınıza sahip olabilirsiniz. 

İlk kapsayıcıyı 5000 numaralı bağlantı noktasında çalıştırın. 

```bash 
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

İkinci kapsayıcıyı 5001 numaralı bağlantı noktasında çalıştırın.


```bash 
docker run --rm -it -p 5000:5001 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Sonraki her kapsayıcı farklı bir bağlantı noktasında olmalıdır. 

## <a name="query-the-containers-prediction-endpoint"></a>Kapsayıcının tahmin uç noktasını sorgulama

Kapsayıcı, REST tabanlı sorgu tahmin uç noktası API 'Leri sağlar. 

Kapsayıcı API 'Leri için http://localhost:5000konak kullanın.

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Kapsayıcıyı durdur

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcıyı bir çıkış [bağlaması](anomaly-detector-container-configuration.md#mount-settings) ve günlüğü etkin olarak çalıştırırsanız kapsayıcı, kapsayıcıyı başlatırken veya çalıştırırken oluşan sorunları gidermek için yararlı olan günlük dosyaları oluşturur.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Faturalama

Anomali algılayıcı kapsayıcıları, Azure hesabınızda bir _anomali algılayıcı_ kaynağı kullanarak faturalandırma bilgilerini Azure 'a gönderir. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](anomaly-detector-container-configuration.md).

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, anomali algılayıcı kapsayıcılarını indirmek, yüklemek ve çalıştırmak için kavramlar ve iş akışı öğrendiniz. Özet:

* Anomali algılayıcısı, Docker için bir Linux kapsayıcısı, toplu iş vs akışı, beklenen Aralık çıkarımı ve duyarlık ayarlama ile Kapsülleyici algılama sağlar.
* Kapsayıcı görüntüleri, kapsayıcılar önizlemesi için adanmış bir özel Azure Container Registry indirilir.
* Docker kapsayıcı görüntüleri çalıştırın.
* Kapsayıcının ana bilgisayar URI 'sini belirterek anomali algılayıcı kapsayıcılarındaki işlemleri çağırmak için REST API veya SDK kullanabilirsiniz.
* Bir kapsayıcı örneği oluşturulurken, fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcıları müşteri verilerini (ör. çözümlenmekte olan zaman serisi verileri) Microsoft 'a göndermez.

## <a name="next-steps"></a>Sonraki adımlar

* Yapılandırma ayarları için [kapsayıcıları](anomaly-detector-container-configuration.md) yapılandırmayı gözden geçir
* [Azure Container Instances için bir anomali algılayıcı kapsayıcısı dağıtın](how-to/deploy-anomaly-detection-on-container-instances.md)
* [Anomali algılayıcı API hizmeti hakkında daha fazla bilgi edinin](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)
