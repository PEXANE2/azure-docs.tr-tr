---
title: Kapsayıcıları yapılandırma-Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Bu makalede, Görüntü İşleme Metin Tanıma kapsayıcıları için hem gerekli hem de isteğe bağlı ayarların nasıl yapılandırılacağı gösterilir.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.custom: seodec18
ms.openlocfilehash: 3be302019c712c13bd29d7ed3781151a1648e847
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80879325"
---
# <a name="configure-computer-vision-docker-containers"></a>Görüntü İşleme Docker kapsayıcılarını yapılandırma

`docker run` Komut bağımsız değişkenlerini kullanarak görüntü işleme kapsayıcısının çalışma zamanı ortamını yapılandırırsınız. Bu kapsayıcıda bazı gerekli ayarlar ve bazı isteğe bağlı ayarlar vardır. Birkaç komuta [örnek](#example-docker-run-commands) vardır. Kapsayıcıya özgü ayarlar faturalandırma ayarlardır. 

## <a name="configuration-settings"></a>Yapılandırma ayarları

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting)Ve [`Eula`](#eula-setting) ayarları birlikte kullanılır ve üçü için geçerli değerler sağlamalısınız; Aksi takdirde Kapsayıcınız başlatılmaz. Bir kapsayıcı oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalandırma](computer-vision-how-to-install-containers.md).

## <a name="apikey-configuration-setting"></a>ApiKey yapılandırma ayarı

Bu `ApiKey` ayar, kapsayıcının fatura `Cognitive Services` bilgilerini izlemek için kullanılan Azure Kaynak anahtarını belirtir. ApiKey için bir değer belirtmeniz gerekir ve değerin [`Billing`](#billing-configuration-setting) yapılandırma ayarı için belirtilen bilişsel _Hizmetler_ kaynağı için geçerli bir anahtar olması gerekir.

Bu ayar aşağıdaki yerde bulunabilir:

* Azure portal: bilişsel **Hizmetler** kaynak yönetimi, **anahtarlar** altında

## <a name="applicationinsights-setting"></a>ApplicationInsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Faturalandırma yapılandırma ayarı

`Billing` Ayar, Azure üzerinde bulunan bilişsel _Hizmetler_ kaynağının, kapsayıcının fatura bilgilerini ölçmek IÇIN kullanılan uç nokta URI 'sini belirtir. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve Azure 'daki bilişsel _Hizmetler_ kaynağı için değer geçerli bir uç nokta URI 'si olmalıdır. Kapsayıcı her 10 ila 15 dakikada bir kullanım raporu sağlar.

Bu ayar aşağıdaki yerde bulunabilir:

* Azure portal: bilişsel **Hizmetler** genel bakış, etiketli`Endpoint`

Aşağıdaki tabloda gösterildiği gibi `vision/v1.0` , yönlendirmeyi, uç nokta URI 'sine eklemeyi unutmayın. 

|Gerekli| Adı | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Yes| `Billing` | Dize | Faturalama uç noktası URI 'SI<br><br>Örnek:<br>`Billing=https://westcentralus.api.cognitive.microsoft.com/vision/v1.0` |

## <a name="eula-setting"></a>EULA ayarı

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Akışkan entd ayarları

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>HTTP proxy kimlik bilgileri ayarları

[!INCLUDE [Container shared configuration HTTP proxy settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Oturum açma ayarları
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]

## <a name="mount-settings"></a>Bağlama ayarları

Kapsayıcıya ve kapsayıcılardan veri okumak ve buradan veri yazmak için BIND bağlama kullanın. `--mount` [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunda seçeneğini belirterek bir giriş bağlama veya çıkış bağlama belirtebilirsiniz.

Görüntü İşleme kapsayıcıları, eğitim veya hizmet verilerini depolamak için giriş veya çıkış taklarını kullanmaz. 

Konak bağlama konumunun tam sözdizimi, ana bilgisayar işletim sistemine bağlı olarak değişir. Ayrıca, Docker hizmeti hesabı ve konak bağlama konumu izinleri tarafından kullanılan izinler arasındaki bir çakışma nedeniyle [ana bilgisayarın](computer-vision-how-to-install-containers.md#the-host-computer)bağlama konumu erişilebilir olmayabilir. 

|İsteğe Bağlı| Adı | Veri türü | Açıklama |
|-------|------|-----------|-------------|
|İzin verilmiyor| `Input` | Dize | Görüntü İşleme kapsayıcılar bunu kullanmaz.|
|İsteğe Bağlı| `Output` | Dize | Çıkış bağlama hedefi. Varsayılan değer: `/output`. Bu, günlüklerin konumudur. Bu, kapsayıcı günlüklerini içerir. <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Örnek Docker Run komutları

Aşağıdaki örneklerde, komutlarının nasıl yazılacağını ve kullanılacağını `docker run` göstermek için yapılandırma ayarları kullanılır.  Çalışan bir kez, kapsayıcıyı [durduruncaya](computer-vision-how-to-install-containers.md#stop-the-container) kadar çalışmaya devam eder.

* **Satır devamlılık karakteri**: aşağıdaki bölümlerdeki Docker komutları, satır devamlılık karakteri olarak ters eğik `\`çizgi kullanır. Bunu, ana bilgisayar işletim sisteminizin gereksinimlerine göre değiştirin veya kaldırın. 
* **Bağımsız değişken sırası**: Docker Kapsayıcıları hakkında bilginiz yoksa bağımsız değişkenlerin sırasını değiştirmeyin.

{_Argument_name_} değerini kendi değerlerinizle değiştirin:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
| **{API_KEY}** | Azure `Computer Vision` `Computer Vision` anahtarları sayfasında kaynağın uç nokta anahtarı. | `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` |
| **{ENDPOINT_URI}** | Faturalandırma uç noktası değeri, Azure `Computer Vision` Genel Bakış sayfasında bulunur.| Açık örnekler için [gerekli parametreleri toplama](computer-vision-how-to-install-containers.md#gathering-required-parameters) konusuna bakın. |

[!INCLUDE [subdomains-note](../../../includes/cognitive-services-custom-subdomains-note.md)]

> [!IMPORTANT]
> Kapsayıcısını `Eula`çalıştırmak `Billing`için, `ApiKey` , ve seçenekleri belirtilmelidir; Aksi takdirde, kapsayıcı başlatılmaz.  Daha fazla bilgi için bkz. [faturalandırma](computer-vision-how-to-install-containers.md#billing).
> ApiKey değeri, Azure `Cognitive Services` kaynak anahtarları sayfasından alınan **anahtardır** .

## <a name="container-docker-examples"></a>Kapsayıcı Docker örnekleri

Aşağıdaki Docker örnekleri okuma kapsayıcısı içindir.

### <a name="basic-example"></a>Temel örnek

  ```docker
  docker run --rm -it -p 5000:5000 --memory 16g --cpus 8 \
  containerpreview.azurecr.io/microsoft/cognitive-services-read \
  Eula=accept \
  Billing={ENDPOINT_URI} \
  ApiKey={API_KEY} 
  ```

### <a name="logging-example"></a>Günlüğe kaydetme örneği 

  ```docker
  docker run --rm -it -p 5000:5000 --memory 16g --cpus 8 \
  containerpreview.azurecr.io/microsoft/cognitive-services-read \
  Eula=accept \
  Billing={ENDPOINT_URI} \
  ApiKey={API_KEY} \
  Logging:Console:LogLevel:Default=Information
  ```

## <a name="next-steps"></a>Sonraki adımlar

* [Kapsayıcıları yüklemeyi ve çalıştırmayı](computer-vision-how-to-install-containers.md)gözden geçirin.
