---
title: Anomali Dedektörü API'si için bir kapsayıcı nasıl yapılandırılır?
titleSuffix: Azure Cognitive Services
description: Anomaly Detector API konteyner çalışma zamanı ortamı `docker run` komut bağımsız değişkenleri kullanılarak yapılandırılır. Bu kapsayıcı birkaç isteğe bağlı ayarları ile birlikte birkaç gerekli ayarları vardır.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 11/07/2019
ms.author: dapine
ms.openlocfilehash: f7e04a16fa35d492b8e5e6c53a05220e8b96a38a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73795844"
---
# <a name="configure-anomaly-detector-containers"></a>Anomali Algılayıcısı kapsayıcılarını yapılandırma

**Anomaly Detector** kapsayıcı çalışma zamanı ortamı `docker run` komut bağımsız değişkenleri kullanılarak yapılandırılır. Bu kapsayıcı birkaç isteğe bağlı ayarları ile birlikte birkaç gerekli ayarları vardır. Komutun birkaç [örneği](#example-docker-run-commands) mevcuttur. Kapsayıcıya özgü ayarlar fatura ayarlarıdır. 

## <a name="configuration-settings"></a>Yapılandırma ayarları

Bu kapsayıcı aşağıdaki yapılandırma ayarlarına sahiptir:

|Gerekli|Ayar|Amaç|
|--|--|--|
|Evet|[ApiKey](#apikey-configuration-setting)|Fatura bilgilerini izlemek için kullanılır.|
|Hayır|[ApplicationInsights](#applicationinsights-setting)|Kapsayıcınıza [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) telemetri desteği eklemenize olanak tanır.|
|Evet|[Faturalama](#billing-configuration-setting)|Azure'daki hizmet kaynağının bitiş noktası URI'yi belirtir.|
|Evet|[Eula](#eula-setting)| Kapsayıcının lisansını kabul ettiğinizi gösterir.|
|Hayır|[Akıcı](#fluentd-settings)|Log ve isteğe bağlı olarak metrik verileri Akıcı bir sunucuya yazın.|
|Hayır|[Http Proxy](#http-proxy-credentials-settings)|Giden isteklerde bulunmak için bir HTTP proxy'si yapılandırın.|
|Hayır|[Günlüğe Kaydetme](#logging-settings)|Kapsayıcınız için ASP.NET Çekirdek günlük desteği sağlar. |
|Hayır|[Bağlar](#mount-settings)|Ana bilgisayardan kapsayıcıya ve kapsayıcıdan ana bilgisayara kadar verileri okuma ve yazma.|

> [!IMPORTANT]
> [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting)ve [`Eula`](#eula-setting) ayarlar birlikte kullanılır ve her üçü için de geçerli değerler sağlamanız gerekir; aksi takdirde konteyneriniz çalışmayabaşlamaz. Bir kapsayıcıyı anında kullanmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için [Faturalandırma'ya](anomaly-detector-container-howto.md#billing)bakın.

## <a name="apikey-configuration-setting"></a>ApiKey yapılandırma ayarı

Ayar, `ApiKey` kapsayıcının fatura bilgilerini izlemek için kullanılan Azure kaynak anahtarını belirtir. ApiKey için bir değer belirtmeniz gerekir ve değer [`Billing`](#billing-configuration-setting) yapılandırma ayarı için belirtilen _Anomaly Detector_ kaynağı için geçerli bir anahtar olmalıdır.

Bu ayar aşağıdaki yerde bulunabilir:

* Azure portalı: **Anomali Dedektörü** Kaynak Yönetimi, **Anahtarlar** altında

## <a name="applicationinsights-setting"></a>ApplicationInsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Faturalandırma yapılandırma ayarı

Ayar, `Billing` kapsayıcının fatura bilgilerini ölçmede kullanılan _Azure'daki Anomali Dedektörü_ kaynağının bitiş noktası URI'yi belirtir. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve değer Azure'daki bir _Anomali Dedektörü_ kaynağı için geçerli bir bitiş noktası URI olmalıdır.

Bu ayar aşağıdaki yerde bulunabilir:

* Azure portalı: **Anomali Dedektörü'ne** Genel Bakış, etiketli`Endpoint`

|Gerekli| Adı | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Evet| `Billing` | Dize | Faturalandırma uç noktası URI. FaturaLAMA URI'si edinme hakkında daha fazla bilgi için [gerekli parametreleri toplamaya](anomaly-detector-container-howto.md#gathering-required-parameters)bakın. Daha fazla bilgi ve bölgesel uç noktaların tam listesi [için, Bilişsel Hizmetler için Özel alt alan adları bölümüne](../cognitive-services-custom-subdomains.md)bakın. |

## <a name="eula-setting"></a>Eula ayarı

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Akıcı ayarlar

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>Http proxy kimlik bilgileri ayarları

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Oturum açma ayarları
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]


## <a name="mount-settings"></a>Montaj ayarları

Kapsayıcıya ve kapsayıcıdan veri okumak ve yazmak için bağlama bağlarını kullanın. Docker `--mount` [run](https://docs.docker.com/engine/reference/commandline/run/) komutundaki seçeneği belirterek bir giriş yuvası veya çıktı montajı belirtebilirsiniz.

Anomali Dedektörü kapları, eğitim veya hizmet verilerini depolamak için giriş veya çıkış montajları kullanmaz. 

Ana bilgisayar montaj konumunun tam sözdizimi ana bilgisayar işletim sistemine bağlı olarak değişir. Ayrıca, docker hizmet hesabı tarafından kullanılan izinler ile ana bilgisayar montaj konum izinleri arasındaki bir çakışma nedeniyle [ana bilgisayarın](anomaly-detector-container-howto.md#the-host-computer)montaj konumuna erişilemeyebilir. 

|İsteğe bağlı| Adı | Veri türü | Açıklama |
|-------|------|-----------|-------------|
|İzin verilmiyor| `Input` | Dize | Anomali Dedektörü kapları bunu kullanmaz.|
|İsteğe bağlı| `Output` | Dize | Çıkış montaj hedefi. Varsayılan değer: `/output`. Bu günlüklerin yeridir. Buna kapsayıcı günlükleri de dahildir. <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Örnek docker çalıştır komutları 

Aşağıdaki örnekler, komutların nasıl yazılabildiğini `docker run` ve kullanılacağını göstermek için yapılandırma ayarlarını kullanır.  Bir kez çalışırken, kapsayıcı [onu durdurunkadar](anomaly-detector-container-howto.md#stop-the-container) çalışmaya devam ediyor.

* **Satır devamı karakteri**: Aşağıdaki bölümlerdeki Docker komutları, `\`bash kabuğu için çizgi devamı karakteri olarak arka çizgiyi kullanır. Ana bilgisayar işletim sisteminizin gereksinimlerine göre bunu değiştirin veya kaldırın. Örneğin, pencereler için satır devamı karakteri bir `^`caret, . Arka çizgiyi caret ile değiştirin. 
* **Bağımsız değişken sırası**: Docker kapsayıcılarını çok iyi bilmediğiniz sürece bağımsız değişkenlerin sırasını değiştirmeyin.

Değeri parantez içinde `{}`kendi değerlerinizle değiştirin:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
| **{API_KEY}** | Azure `Anomaly Detector` Tuşları sayfasındaki `Anomaly Detector` kaynağın bitiş noktası anahtarı. | `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` |
| **{ENDPOINT_URI}** | Faturalandırma bitiş noktası değeri Azure `Anomaly Detector` Genel Bakış sayfasında kullanılabilir.| Açık örnekler için [gerekli parametreleri toplamaya](anomaly-detector-container-howto.md#gathering-required-parameters) bakın. |

[!INCLUDE [subdomains-note](../../../includes/cognitive-services-custom-subdomains-note.md)]

> [!IMPORTANT]
> `Eula`Kapsayıcıyı `Billing`çalıştırmak `ApiKey` için , ve seçenekler belirtilmelidir; aksi takdirde, kapsayıcı başlamaz.  Daha fazla bilgi için [Faturalandırma'ya](anomaly-detector-container-howto.md#billing)bakın.
> ApiKey değeri, Azure Anomali Dedektörü Kaynak anahtarları sayfasındaki **Anahtardır.** 

## <a name="anomaly-detector-container-docker-examples"></a>Anomali Dedektörkonteyner Docker örnekleri

Aşağıdaki Docker örnekleri Anomali Dedektörü konteyner içindir. 

### <a name="basic-example"></a>Temel örnek 

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector \
  Eula=accept \
  Billing={ENDPOINT_URI} \
  ApiKey={API_KEY} 
  ```

### <a name="logging-example-with-command-line-arguments"></a>Komut satırı bağımsız değişkenleriyle günlük örneği

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector \
  Eula=accept \
  Billing={ENDPOINT_URI} ApiKey={API_KEY} \
  Logging:Console:LogLevel:Default=Information
  ```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Kapsayıcı Örneklerine Bir Anomali Dedektörü kapsayıcısı dağıtma](how-to/deploy-anomaly-detection-on-container-instances.md)
* [Anomaly Detector API hizmeti hakkında daha fazla bilgi edinin](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)
