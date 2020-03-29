---
title: Anomaly Detector API kullanmak için konteynerler nasıl yüklenir ve çalıştırUlur?
titleSuffix: Azure Cognitive Services
description: Zaman serisi verilerinizdeki anormallikleri belirlemek için Anomali Dedektörü API'sını kullanın.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: dapine
ms.openlocfilehash: 45abd904ea95cf8e68583ba5630a485af59479ec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220352"
---
# <a name="install-and-run-anomaly-detector-containers-preview"></a>Anomali Dedektörü konteynerleri yükleyin ve çalıştırın (Önizleme)

Anomali Dedektörü aşağıdaki konteyner özelliği işlevine sahiptir:

| İşlev | Özellikler |
|--|--|
| Anomali dedektörü | <li> Anomalileri gerçek zamanlı olarak algılar. <li> Veri setinizdeki anormallikleri toplu olarak algılar. <li> Verilerinizin beklenen normal aralığını çıkar. <li> Verilerinizi daha iyi sığdırmak için anomali algılama hassasiyeti ayarını destekler. |

API'ler hakkında ayrıntılı bilgi için lütfen bkz:
* [Anomaly Detector API hizmeti hakkında daha fazla bilgi edinin](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Anomaly Detector konteynerleri kullanmadan önce aşağıdaki ön koşulları karşılamanız gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Docker [Engine'in ana bilgisayara](#the-host-computer)yüklenmesi gerekir. Docker, [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms) üzerinde Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temel bilgileri ile ilgili giriş yapmak için [Docker’a genel bakış](https://docs.docker.com/engine/docker-overview/) bölümüne bakın.<br><br> Docker, kapsayıcıların Azure'a bağlanmasına ve fatura verilerini göndermesine izin verecek şekilde yapılandırılmalıdır. <br><br> **Windows'da**Docker, Linux kapsayıcılarını destekleyecek şekilde de yapılandırılmalıdır.<br><br>|
|Docker ile aşinalık | Docker kavramları hakkında, kayıt defterleri, depolar, konteynerler ve konteyner resimleri nin yanı sıra `docker` temel komutlar hakkında temel bilgilere sahip olmalısınız.| 
|Anomali Dedektörü kaynağı |Bu kapsayıcıları kullanabilmek için şunları yapmalısınız:<br><br>İlişkili API anahtarını ve bitiş noktası URI'yi almak için bir Azure _Anomali Dedektörü_ kaynağı. Her iki değer de Azure portalının **Anomali Dedektörü** Genel Bakışı ve Anahtarları sayfalarında mevcuttur ve kapsayıcıyı başlatmak için gereklidir.<br><br>**{API_KEY}**: **Keys** sayfasındaki mevcut iki kaynak anahtarından biri<br><br>**{ENDPOINT_URI}**: **Genel Bakış** sayfasında sağlanan bitiş noktası|

[!INCLUDE [Gathering required container parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="request-access-to-the-container-registry"></a>Konteyner kayıt defterine erişim isteği

Konteynere erişim talebinde bulunmak için öncelikle [Anomali Dedektörü Konteyner İstek formunu](https://aka.ms/adcontainer) doldurmanız ve göndermeniz gerekmektedir.

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

<!--* [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/). For instructions of deploying Anomaly Detector module in IoT Edge, see [How to deploy Anomaly Detector module in IoT Edge](how-to-deploy-anomaly-detector-module-in-iot-edge.md).-->

### <a name="container-requirements-and-recommendations"></a>Konteyner gereksinimleri ve öneriler

Aşağıdaki tabloda Anomaly Detector konteyner için ayırmak için minimum ve önerilen CPU çekirdekleri ve bellek açıklanır.

| QPS(Sorgular/saniye) | Minimum | Önerilen |
|-----------|---------|-------------|
| 10 QPS | 4 çekirdekli, 1 GB bellek | 8 çekirdekli 2 GB bellek |
| 20 QPS | 8 çekirdekli, 2 GB bellek | 16 çekirdekli 4 GB bellek |

Her çekirdek en az 2,6 gigahertz (GHz) veya daha hızlı olmalıdır.

Çekirdek ve bellek, `--cpus` `--memory` komutun bir parçası olarak `docker run` kullanılan ve ayarlara karşılık gelir.

## <a name="get-the-container-image-with-docker-pull"></a>Konteyner görüntüsünü`docker pull`

Kapsayıcı [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) görüntüsünü indirmek için komutu kullanın.

| Kapsayıcı | Depo |
|-----------|------------|
| bilişsel hizmetler-anomali-dedektör | `containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector:latest` |

<!--
For a full description of available tags, such as `latest` used in the preceding command, see [anomaly-detector](https://go.microsoft.com/fwlink/?linkid=2083827&clcid=0x409) on Docker Hub.
-->
[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-anomaly-detector-container"></a>Anomali Dedektörü konteyneri için Docker çekme

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector:latest
```

## <a name="how-to-use-the-container"></a>Kapsayıcı nasıl kullanılır?

Kapsayıcı [ana bilgisayara](#the-host-computer)yüklendikten sonra, kapsayıcıyla çalışmak için aşağıdaki işlemi kullanın.

1. Gerekli fatura ayarlarıyla [kapsayıcıyı çalıştırın.](#run-the-container-with-docker-run) Komutun `docker run` daha fazla [örneği](anomaly-detector-container-configuration.md#example-docker-run-commands) mevcuttur.
1. [Kapsayıcının tahmin bitiş noktasını sorgula.](#query-the-containers-prediction-endpoint)

## <a name="run-the-container-with-docker-run"></a>Kapsayıcıyı çalıştırın`docker run`

Kapsayıcıyı çalıştırmak için [docker run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. Değerleri ve `{API_KEY}` değerleri nasıl elde edineceklerine ilişkin ayrıntılar için [gerekli parametreleri](#gathering-required-parameters) `{ENDPOINT_URI}` toplamaya bakın.

Komut [örnekleri](anomaly-detector-container-configuration.md#example-docker-run-commands) `docker run` mevcuttur.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector:latest \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Şu komut:

* Konteyner görüntüsünden bir Anomali Dedektörü konteyneri çalıştırın
* Bir CPU çekirdeği ve 4 gigabayt (GB) bellek ayırır
* TCP bağlantı noktası 5000'i açığa çıkarır ve kapsayıcı için sözde TTY ayırır
* Çıktıktan sonra kapsayıcıotomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir. 

> [!IMPORTANT]
> `Eula`Kapsayıcıyı `Billing`çalıştırmak `ApiKey` için , ve seçenekler belirtilmelidir; aksi takdirde, kapsayıcı başlamaz.  Daha fazla bilgi için [Faturalandırma'ya](#billing)bakın.

### <a name="running-multiple-containers-on-the-same-host"></a>Aynı ana bilgisayarda birden çok kapsayıcıyı çalıştırma

Açıkta kalan bağlantı noktalarına sahip birden çok kapsayıcı çalıştırmayı planlıyorsanız, her kapsayıcıyı farklı bir bağlantı noktasına sahip çalıştırdığınızdan emin olun. Örneğin, 5000 portundaki ilk kapsayıcıyı ve 5001 no'daki ikinci kapsayıcıyı çalıştırın.

`<container-registry>` Ve kullandığınız `<container-name>` kapsayıcıların değerleriyle değiştirin. Bunlar aynı kapsayıcı olmak zorunda değildir. Anomali Dedektörü konteyneri ve LUIS konteynerinin HOST üzerinde birlikte çalışmasını sağlayabilir veya birden fazla Anomali Dedektörü konteynırı çalıştırabilirsiniz. 

5000 portundaki ilk konteynırı çalıştırın. 

```bash 
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

5001 portundaki ikinci konteynırı çalıştırın.


```bash 
docker run --rm -it -p 5000:5001 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Sonraki her kapsayıcı farklı bir bağlantı noktasında olmalıdır. 

## <a name="query-the-containers-prediction-endpoint"></a>Kapsayıcının tahmin bitiş noktasını sorgula

Kapsayıcı REST tabanlı sorgu tahmin bitiş noktası API'leri sağlar. 

Kapsayıcı API'leri için ana bilgisayarı http://localhost:5000kullanın.

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Kapsayıcıyı durdurma

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcıyı çıktı [yuvası](anomaly-detector-container-configuration.md#mount-settings) ve günlüğe kaydetme etkinken çalıştırıyorsanız, kapsayıcı kapsayıcıyı çalıştırırken veya çalıştırırken meydana gelen sorunları gidermeye yardımcı olan günlük dosyaları oluşturur.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Faturalandırma

Anomali Dedektörü kapsayıcıları, Azure hesabınızdaki _Bir Anomali Dedektörü_ kaynağını kullanarak fatura bilgilerini Azure'a gönderir. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için [yapı kaplarını yapıya kullanabilirsiniz.](anomaly-detector-container-configuration.md)

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, Anomaly Detector kaplarını indirmek, yüklemek ve çalıştırmak için kavramları ve iş akışını öğrendiniz. Özet:

* Anomali Dedektörü Docker için bir Linux konteyner sağlar, toplu vs akış ile anomali algılama kapsülleme, beklenen aralık çıkarımı, ve duyarlılık aparat.
* Kapsayıcı görüntüleri, kapsayıcıönizlemesi için özel bir Azure Kapsayıcı Kayıt Defteri'nden indirilir.
* Konteyner görüntüleri Docker'da çalışır.
* Rest API veya SDK'yı kullanarak Anomali Dedektörü kaplarında ki işlemleri kabın ana bilgisayarı URI'yi belirterek arayabilirsiniz.
* Bir kapsayıcıyı anında kullanırken fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel Hizmetler kapsayıcıları ölçüm için Azure'a bağlı olmadan çalışma lisansı na sahip değildir. Müşterilerin, konteynerlerin fatura bilgilerini her zaman ölçüm hizmetiyle iletmesini sağlaması gerekir. Bilişsel Hizmetler kapsayıcıları müşteri verilerini (örn. analiz edilen zaman serisi verileri) Microsoft'a göndermez.

## <a name="next-steps"></a>Sonraki adımlar

* Yapılandırma ayarları için [kapsayıcıları yapılandırmayı](anomaly-detector-container-configuration.md) gözden geçirin
* [Azure Kapsayıcı Örneklerine Bir Anomali Dedektörü kapsayıcısı dağıtma](how-to/deploy-anomaly-detection-on-container-instances.md)
* [Anomaly Detector API hizmeti hakkında daha fazla bilgi edinin](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)
