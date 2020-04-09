---
title: Kapsayıcıları yapılandırma - Bilgisayar Lı Vizyon
titleSuffix: Azure Cognitive Services
description: Bu makalede, Bilgisayar Görüşü'nde Metin kapsayıcıları tanın için hem gerekli hem de isteğe bağlı ayarları nasıl yapılandırabileceğiniz gösterilmektedir.
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
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80879325"
---
# <a name="configure-computer-vision-docker-containers"></a>Bilgisayarlı Vision Docker kaplarını yapılandırın

Komut bağımsız değişkenlerini kullanarak Computer Vision kapsayıcısının `docker run` çalışma zamanı ortamını yapılandırabilirsiniz. Bu kapsayıcı birkaç isteğe bağlı ayarları ile birlikte birkaç gerekli ayarları vardır. Komutun birkaç [örneği](#example-docker-run-commands) mevcuttur. Kapsayıcıya özgü ayarlar fatura ayarlarıdır. 

## <a name="configuration-settings"></a>Yapılandırma ayarları

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting)ve [`Eula`](#eula-setting) ayarlar birlikte kullanılır ve her üçü için de geçerli değerler sağlamanız gerekir; aksi takdirde konteyneriniz çalışmayabaşlamaz. Bir kapsayıcıyı anında kullanmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için [Faturalandırma'ya](computer-vision-how-to-install-containers.md)bakın.

## <a name="apikey-configuration-setting"></a>ApiKey yapılandırma ayarı

Ayar, `ApiKey` kapsayıcının fatura `Cognitive Services` bilgilerini izlemek için kullanılan Azure kaynak anahtarını belirtir. ApiKey için bir değer belirtmeniz gerekir ve değer [`Billing`](#billing-configuration-setting) yapılandırma ayarı için belirtilen Bilişsel _Hizmetler_ kaynağı için geçerli bir anahtar olmalıdır.

Bu ayar aşağıdaki yerde bulunabilir:

* Azure portalı: **Bilişsel Hizmetler** Kaynak Yönetimi, **Anahtarlar** altında

## <a name="applicationinsights-setting"></a>ApplicationInsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Faturalandırma yapılandırma ayarı

Ayar, `Billing` kapsayıcının fatura bilgilerini ölçmede kullanılan _Azure'daki Bilişsel Hizmetler_ kaynağının bitiş noktası URI'yi belirtir. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve değer Azure'daki _Bilişsel Hizmetler_ kaynağı için geçerli bir bitiş noktası URI olmalıdır. Kapsayıcı her 10 ila 15 dakikada bir kullanımı bildirir.

Bu ayar aşağıdaki yerde bulunabilir:

* Azure portalı: **Bilişsel Hizmetlere** Genel Bakış, etiketli`Endpoint`

Aşağıdaki tabloda `vision/v1.0` gösterildiği gibi bitiş noktası URI'ye yönlendirmeeklemeyi unutmayın. 

|Gerekli| Adı | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Evet| `Billing` | Dize | Faturalandırma bitiş noktası URI<br><br>Örnek:<br>`Billing=https://westcentralus.api.cognitive.microsoft.com/vision/v1.0` |

## <a name="eula-setting"></a>Eula ayarı

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Akıcı ayarlar

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>HTTP proxy kimlik bilgileri ayarları

[!INCLUDE [Container shared configuration HTTP proxy settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Oturum açma ayarları
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]

## <a name="mount-settings"></a>Montaj ayarları

Kapsayıcıya ve kapsayıcıdan veri okumak ve yazmak için bağlama bağlarını kullanın. Docker `--mount` [run](https://docs.docker.com/engine/reference/commandline/run/) komutundaki seçeneği belirterek bir giriş yuvası veya çıktı montajı belirtebilirsiniz.

Computer Vision kapsayıcıları, eğitim veya hizmet verilerini depolamak için giriş veya çıktı montajları kullanmaz. 

Ana bilgisayar montaj konumunun tam sözdizimi ana bilgisayar işletim sistemine bağlı olarak değişir. Ayrıca, docker hizmet hesabı tarafından kullanılan izinler ile ana bilgisayar montaj konum izinleri arasındaki bir çakışma nedeniyle [ana bilgisayarın](computer-vision-how-to-install-containers.md#the-host-computer)montaj konumuna erişilemeyebilir. 

|İsteğe bağlı| Adı | Veri türü | Açıklama |
|-------|------|-----------|-------------|
|İzin verilmiyor| `Input` | Dize | Computer Vision kapsayıcıları bunu kullanmaz.|
|İsteğe bağlı| `Output` | Dize | Çıkış montaj hedefi. Varsayılan değer: `/output`. Bu günlüklerin yeridir. Buna kapsayıcı günlükleri de dahildir. <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Örnek docker çalıştır komutları

Aşağıdaki örnekler, komutların nasıl yazılabildiğini `docker run` ve kullanılacağını göstermek için yapılandırma ayarlarını kullanır.  Bir kez çalışırken, kapsayıcı [onu durdurunkadar](computer-vision-how-to-install-containers.md#stop-the-container) çalışmaya devam ediyor.

* **Satır devamı karakteri**: Aşağıdaki bölümlerdeki Docker komutları, `\`çizgi devamı karakteri olarak arka çizgiyi kullanır. Ana bilgisayar işletim sisteminizin gereksinimlerine göre bunu değiştirin veya kaldırın. 
* **Bağımsız değişken sırası**: Docker kapsayıcılarını çok iyi bilmediğiniz sürece bağımsız değişkenlerin sırasını değiştirmeyin.

{_argument_name_} 'yi kendi değerlerinizle değiştirin:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
| **{API_KEY}** | Azure `Computer Vision` Tuşları sayfasındaki `Computer Vision` kaynağın bitiş noktası anahtarı. | `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` |
| **{ENDPOINT_URI}** | Faturalandırma bitiş noktası değeri Azure `Computer Vision` Genel Bakış sayfasında kullanılabilir.| Açık örnekler için [gerekli parametreleri toplamaya](computer-vision-how-to-install-containers.md#gathering-required-parameters) bakın. |

[!INCLUDE [subdomains-note](../../../includes/cognitive-services-custom-subdomains-note.md)]

> [!IMPORTANT]
> `Eula`Kapsayıcıyı `Billing`çalıştırmak `ApiKey` için , ve seçenekler belirtilmelidir; aksi takdirde, kapsayıcı başlamaz.  Daha fazla bilgi için [Faturalandırma'ya](computer-vision-how-to-install-containers.md#billing)bakın.
> ApiKey değeri, Azure `Cognitive Services` Kaynak anahtarları sayfasındaki **Anahtardır.**

## <a name="container-docker-examples"></a>Konteyner Docker örnekleri

Aşağıdaki Docker örnekleri Read kapsayıcısı içindir.

### <a name="basic-example"></a>Temel örnek

  ```docker
  docker run --rm -it -p 5000:5000 --memory 16g --cpus 8 \
  containerpreview.azurecr.io/microsoft/cognitive-services-read \
  Eula=accept \
  Billing={ENDPOINT_URI} \
  ApiKey={API_KEY} 
  ```

### <a name="logging-example"></a>Günlük örneği 

  ```docker
  docker run --rm -it -p 5000:5000 --memory 16g --cpus 8 \
  containerpreview.azurecr.io/microsoft/cognitive-services-read \
  Eula=accept \
  Billing={ENDPOINT_URI} \
  ApiKey={API_KEY} \
  Logging:Console:LogLevel:Default=Information
  ```

## <a name="next-steps"></a>Sonraki adımlar

* [Kapsayıcıların nasıl yüklenir ve çalıştırılabildiğini gözden geçirin.](computer-vision-how-to-install-containers.md)
