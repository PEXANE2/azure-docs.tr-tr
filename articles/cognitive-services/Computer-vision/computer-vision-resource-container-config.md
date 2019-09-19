---
title: Kapsayıcıları yapılandırma-Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Görüntü İşleme Metin Tanıma kapsayıcıları için çeşitli ayarları yapılandırın.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 09/18/2019
ms.author: dapine
ms.custom: seodec18
ms.openlocfilehash: aba846ade9e2b5e19304df87ea3e29713aacf4ba
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71129957"
---
# <a name="configure-computer-vision-docker-containers"></a>Görüntü İşleme Docker kapsayıcılarını yapılandırma

`docker run` Komut bağımsız değişkenlerini kullanarak görüntü işleme kapsayıcısının çalışma zamanı ortamını yapılandırırsınız. Bu kapsayıcıda bazı gerekli ayarlar ve bazı isteğe bağlı ayarlar vardır. Birkaç [örnekler](#example-docker-run-commands) komutu kullanılabilir. Kapsayıcıya özgü ayarlar faturalandırma ayarlardır. 

## <a name="configuration-settings"></a>Yapılandırma ayarları

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), Ve [ `Eula` ](#eula-setting) ayarları birlikte kullanılır ve bunları; Aksi takdirde, tüm üç için geçerli değerler sağlamanız gerekir kapsayıcınızı başlatılamıyor. Bir kapsayıcı örneği oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalama](computer-vision-how-to-install-containers.md).

## <a name="apikey-configuration-setting"></a>ApiKey yapılandırma ayarı

Bu ayar, kapsayıcının fatura `Cognitive Services` bilgilerini izlemek için kullanılan Azure Kaynak anahtarını belirtir. `ApiKey` Apikey için bir değer belirtmeniz gerekir ve değerin [`Billing`](#billing-configuration-setting) yapılandırma ayarı için belirtilen bilişsel _Hizmetler_ kaynağı için geçerli bir anahtar olması gerekir.

Bu ayar aşağıdaki yerde bulunabilir:

* Azure portal: Bilişsel **Hizmetler** Kaynak yönetimi, **anahtarlar** altında

## <a name="applicationinsights-setting"></a>Applicationınsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Yapılandırma ayarı faturalama

Ayar, Azure üzerinde bulunan bilişsel hizmetler kaynağının, kapsayıcının fatura bilgilerini ölçmek için kullanılan uç nokta URI 'sini belirtir. `Billing` Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve Azure 'daki bilişsel _Hizmetler_ kaynağı için değer geçerli bir uç nokta URI 'si olmalıdır. Kapsayıcı her 10 ila 15 dakikada bir kullanım raporu sağlar.

Bu ayar aşağıdaki yerde bulunabilir:

* Azure portal: Bilişsel **Hizmetler** Genel bakış, etiketli`Endpoint`

Aşağıdaki tabloda gösterildiği gibi `vision/v1.0` , yönlendirmeyi, uç nokta URI 'sine eklemeyi unutmayın. 

|Gerekli| Ad | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Evet| `Billing` | Dize | Faturalandırma uç noktası URI'si<br><br>Örnek:<br>`Billing=https://westcentralus.api.cognitive.microsoft.com/vision/v1.0` |

## <a name="eula-setting"></a>EULA'yı ayarlama

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Fluentd ayarları

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>HTTP proxy kimlik bilgileri ayarları

[!INCLUDE [Container shared configuration HTTP proxy settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Günlük ayarları
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]

## <a name="mount-settings"></a>Bağlama ayarları

Kullanım bağlama okumak ve kapsayıcı gelen ve giden veri yazmak için bağlar. Bir giriş bağlama belirtin veya çıkış bağlama belirterek `--mount` seçeneğini [docker run](https://docs.docker.com/engine/reference/commandline/run/) komutu.

Görüntü İşleme kapsayıcıları, eğitim veya hizmet verilerini depolamak için giriş veya çıkış taklarını kullanmaz. 

Konak bağlama konumu söz dizimi konak işletim sistemine göre değişir. Ayrıca, Docker hizmeti hesabı ve konak bağlama konumu izinleri tarafından kullanılan izinler arasındaki bir çakışma nedeniyle [ana bilgisayarın](computer-vision-how-to-install-containers.md#the-host-computer)bağlama konumu erişilebilir olmayabilir. 

|İsteğe Bağlı| Name | Veri türü | Açıklama |
|-------|------|-----------|-------------|
|İzin verilmedi| `Input` | Dize | Görüntü İşleme kapsayıcılar bunu kullanmaz.|
|İsteğe Bağlı| `Output` | Dize | Çıkış bağlama hedefi. Varsayılan değer `/output` şeklindedir. Bu günlükler konumdur. Bu, kapsayıcı günlüklerini içerir. <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Örnek docker komutlarını çalıştırın

Aşağıdaki örnekler, yazma ve kullanma göstermek için yapılandırma ayarlarını kullanır. `docker run` komutları.  Kapsayıcıyı çalıştıran sonra dek çalıştırmaya devam [Durdur](computer-vision-how-to-install-containers.md#stop-the-container) bu.

* **Satır devamlılık karakteri**: Aşağıdaki bölümlerdeki Docker komutları, satır devamlılık karakteri olarak ters eğik `\`çizgi kullanır. Bu konak işletim sisteminin gereksinimlerine göre kaldırın veya değiştirin. 
* **Bağımsız değişken sırası**: Docker Kapsayıcıları hakkında bilginiz yoksa bağımsız değişkenlerin sırasını değiştirmeyin.

Yerine {_argument_name_} kendi değerlerinizle:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
| **{API_KEY}** | `Computer Vision` Azure`Computer Vision` anahtarları sayfasında kaynağın uç nokta anahtarı. | `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` |
| **{ENDPOINT_URI}** | Faturalandırma uç noktası değeri, Azure `Computer Vision` Genel Bakış sayfasında bulunur.| Açık örnekler için [gerekli parametreleri toplama](computer-vision-how-to-install-containers.md#gathering-required-parameters) konusuna bakın. |

[!INCLUDE [subdomains-note](../../../includes/cognitive-services-custom-subdomains-note.md)]

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](computer-vision-how-to-install-containers.md#billing).
> Apikey değeri, Azure `Cognitive Services` kaynak anahtarları sayfasından alınan **anahtardır** .

## <a name="container-docker-examples"></a>Kapsayıcı Docker örnekleri

#### <a name="readtabread"></a>[Okuma](#tab/read)

Aşağıdaki Docker örnekleri okuma kapsayıcısı içindir.

### <a name="basic-example"></a>Temel örnek

  ```
  docker run --rm -it -p 5000:5000 --memory 16g --cpus 8 \
  containerpreview.azurecr.io/microsoft/cognitive-services-read \
  Eula=accept \
  Billing={ENDPOINT_URI} \
  ApiKey={API_KEY} 
  ```

### <a name="logging-example"></a>Günlüğe kaydetme örneği 

  ```
  docker run --rm -it -p 5000:5000 --memory 16g --cpus 8 \
  containerpreview.azurecr.io/microsoft/cognitive-services-read \
  Eula=accept \
  Billing={ENDPOINT_URI} \
  ApiKey={API_KEY} \
  Logging:Console:LogLevel:Default=Information
  ```

#### <a name="recognize-texttabrecognize-text"></a>[Metin tanıma](#tab/recognize-text)

Aşağıdaki Docker örnekleri Metin Tanıma kapsayıcısı içindir.

### <a name="basic-example"></a>Temel örnek

  ```
  docker run --rm -it -p 5000:5000 --memory 16g --cpus 8 \
  containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text \
  Eula=accept \
  Billing={ENDPOINT_URI} \
  ApiKey={API_KEY} 
  ```

### <a name="logging-example"></a>Günlüğe kaydetme örneği

  ```
  docker run --rm -it -p 5000:5000 --memory 16g --cpus 8 \
  containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text \
  Eula=accept \
  Billing={ENDPOINT_URI} \
  ApiKey={API_KEY} \
  Logging:Console:LogLevel:Default=Information
  ```

***

## <a name="next-steps"></a>Sonraki adımlar

* [Kapsayıcıları yüklemeyi ve çalıştırmayı](computer-vision-how-to-install-containers.md)gözden geçirin.
