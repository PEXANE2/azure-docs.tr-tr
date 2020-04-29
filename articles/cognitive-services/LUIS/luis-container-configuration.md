---
title: Docker kapsayıcı ayarları-LUSıS
titleSuffix: Azure Cognitive Services
description: LUSıS kapsayıcı çalışma zamanı ortamı, `docker run` komut bağımsız değişkenleri kullanılarak yapılandırılır. LUO 'NUN birkaç isteğe bağlı ayarı ve birkaç isteğe bağlı ayarı vardır.
services: cognitive-services
author: aahill
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 48a9856c58a815eabcc0b105efcd548e66ddd552
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80874220"
---
# <a name="configure-language-understanding-docker-containers"></a>Language Understanding Docker kapsayıcılarını yapılandırma 

**Language Understanding** (lusıs) kapsayıcı çalışma zamanı ortamı, `docker run` komut bağımsız değişkenleri kullanılarak yapılandırılır. LUO 'NUN birkaç isteğe bağlı ayarı ve birkaç isteğe bağlı ayarı vardır. Birkaç komuta [örnek](#example-docker-run-commands) vardır. Kapsayıcıya özgü ayarlar giriş [bağlama ayarları](#mount-settings) ve faturalandırma ayarlardır. 

## <a name="configuration-settings"></a>Yapılandırma ayarları

Bu kapsayıcı aşağıdaki yapılandırma ayarlarına sahiptir:

|Gerekli|Ayar|Amaç|
|--|--|--|
|Yes|[ApiKey](#apikey-setting)|Faturalandırma bilgilerini izlemek için kullanılır.|
|Hayır|[ApplicationInsights](#applicationinsights-setting)|, Kapsayıcınıza [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) telemetri desteği eklemenize olanak tanır.|
|Yes|[Faturalama](#billing-setting)|Azure üzerindeki hizmet kaynağının uç nokta URI 'sini belirtir.|
|Yes|[Sözleşmesi](#eula-setting)| Kapsayıcının lisansını kabul ettiğinizi gösterir.|
|Hayır|[Fluentd](#fluentd-settings)|Günlük ve isteğe bağlı olarak ölçüm verilerini Floentd sunucusuna yazın.|
|Hayır|[Http proxy 'Si](#http-proxy-credentials-settings)|Giden istekler oluşturmak için bir HTTP proxy 'si yapılandırın.|
|Hayır|[Günlüğe Kaydetme](#logging-settings)|, Kapsayıcınız için ASP.NET Core günlük desteği sağlar. |
|Yes|[Kullan](#mount-settings)|Ana bilgisayardan kapsayıcıya ve kapsayıcıdan, ana bilgisayara geri veri okuma ve yazma.|

> [!IMPORTANT]
> [`ApiKey`](#apikey-setting), [`Billing`](#billing-setting)Ve [`Eula`](#eula-setting) ayarları birlikte kullanılır ve üçü için geçerli değerler sağlamalısınız; Aksi takdirde Kapsayıcınız başlatılmaz. Bir kapsayıcı oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalandırma](luis-container-howto.md#billing).

## <a name="apikey-setting"></a>ApiKey ayarı

Bu `ApiKey` ayar, kapsayıcının fatura bilgilerini izlemek Için kullanılan Azure Kaynak anahtarını belirtir. ApiKey için bir değer belirtmeniz gerekir ve değerin [`Billing`](#billing-setting) yapılandırma ayarı için belirtilen bilişsel _Hizmetler_ kaynağı için geçerli bir anahtar olması gerekir.

Bu ayar aşağıdaki konumlarda bulunabilir:

* Azure portal: bilişsel **Hizmetler** kaynak yönetimi, **anahtarlar** altında
* LUO Portalı: **anahtarlar ve uç nokta ayarları** sayfası. 

Başlangıç anahtarını veya yazma anahtarını kullanmayın. 

## <a name="applicationinsights-setting"></a>ApplicationInsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-setting"></a>Faturalandırma ayarı

`Billing` Ayar, Azure üzerinde bulunan bilişsel _Hizmetler_ kaynağının, kapsayıcının fatura bilgilerini ölçmek IÇIN kullanılan uç nokta URI 'sini belirtir. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve Azure 'daki bilişsel _Hizmetler_ kaynağı için değer geçerli bir uç nokta URI 'si olmalıdır. Kapsayıcı her 10 ila 15 dakikada bir kullanım raporu sağlar.

Bu ayar aşağıdaki konumlarda bulunabilir:

* Azure portal: bilişsel **Hizmetler** genel bakış, etiketli`Endpoint`
* LUO Portalı: uç nokta URI 'sinin bir parçası olarak **anahtarlar ve uç nokta ayarları** sayfası.

| Gerekli | Adı | Veri türü | Açıklama |
|----------|------|-----------|-------------|
| Yes      | `Billing` | string | Faturalama uç noktası URI 'SI. Faturalandırma URI 'sini alma hakkında daha fazla bilgi için bkz. [gerekli parametreleri toplama](luis-container-howto.md#gathering-required-parameters). Daha fazla bilgi ve bölgesel uç noktaların tamamen listesi için bkz. bilişsel [Hizmetler Için özel alt etki alanı adları](../cognitive-services-custom-subdomains.md). |

## <a name="eula-setting"></a>EULA ayarı

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Akışkan entd ayarları

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>HTTP proxy kimlik bilgileri ayarları

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Oturum açma ayarları
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]

## <a name="mount-settings"></a>Bağlama ayarları

Kapsayıcıya ve kapsayıcılardan veri okumak ve buradan veri yazmak için BIND bağlama kullanın. `--mount` [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunda seçeneğini belirterek bir giriş bağlama veya çıkış bağlama belirtebilirsiniz. 

LUSıS kapsayıcısı, eğitim veya hizmet verilerini depolamak için giriş veya çıkış takar. 

Konak bağlama konumunun tam sözdizimi, ana bilgisayar işletim sistemine bağlı olarak değişir. Ayrıca, Docker hizmeti hesabı ve konak bağlama konumu izinleri tarafından kullanılan izinler arasındaki bir çakışma nedeniyle [ana bilgisayarın](luis-container-howto.md#the-host-computer)bağlama konumu erişilebilir olmayabilir. 

Aşağıdaki tabloda desteklenen ayarlar açıklanmaktadır.

|Gerekli| Adı | Veri türü | Açıklama |
|-------|------|-----------|-------------|
|Yes| `Input` | Dize | Giriş bağlama hedefi. Varsayılan değer: `/input`. Bu, LUSıS paket dosyalarının konumudur. <br><br>Örnek:<br>`--mount type=bind,src=c:\input,target=/input`|
|Hayır| `Output` | Dize | Çıkış bağlama hedefi. Varsayılan değer: `/output`. Bu, günlüklerin konumudur. Bu, LUSıS sorgu günlüklerini ve kapsayıcı günlüklerini içerir. <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Örnek Docker Run komutları

Aşağıdaki örneklerde, komutlarının nasıl yazılacağını ve kullanılacağını `docker run` göstermek için yapılandırma ayarları kullanılır.  Çalışan bir kez, kapsayıcıyı [durduruncaya](luis-container-howto.md#stop-the-container) kadar çalışmaya devam eder.

* Bu örnekler, Windows 'ta herhangi bir `C:` izin çakışmasını önlemek için dizini sürücüden kullanır. Giriş dizini olarak belirli bir dizin kullanmanız gerekiyorsa, Docker hizmeti iznini vermeniz gerekebilir. 
* Docker Kapsayıcıları hakkında bilginiz yoksa bağımsız değişkenlerin sırasını değiştirmeyin.
* Farklı bir işletim sistemi kullanıyorsanız, sistem için doğru konsol/Terminal, bağlama için klasör söz dizimi ve satır devamlılık karakteri kullanın. Bu örneklerde bir Windows konsolunun satır devamlılık karakteriyle `^`birlikte varsayılmaktadır. Kapsayıcı bir Linux işletim sistemi olduğundan, hedef bağlama bir Linux stili klasör söz dizimini kullanır.

{_Argument_name_} değerini kendi değerlerinizle değiştirin:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
| **{API_KEY}** | Azure `LUIS` `LUIS` anahtarları sayfasında kaynağın uç nokta anahtarı. | `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` |
| **{ENDPOINT_URI}** | Faturalandırma uç noktası değeri, Azure `LUIS` Genel Bakış sayfasında bulunur.| Açık örnekler için [gerekli parametreleri toplama](luis-container-howto.md#gathering-required-parameters) konusuna bakın. |

[!INCLUDE [subdomains-note](../../../includes/cognitive-services-custom-subdomains-note.md)]

> [!IMPORTANT]
> Kapsayıcısını `Eula`çalıştırmak `Billing`için, `ApiKey` , ve seçenekleri belirtilmelidir; Aksi takdirde, kapsayıcı başlatılmaz. Daha fazla bilgi için bkz. [faturalandırma](luis-container-howto.md#billing).
> ApiKey **değeri,** Lua portalındaki anahtarlar ve uç noktalar sayfasından ve Azure `Cognitive Services` kaynak anahtarları sayfasında da kullanılabilir. 

### <a name="basic-example"></a>Temel örnek

Aşağıdaki örnek, kapsayıcıyı çalıştırmak için mümkün olan en az bağımsız değişkenlere sahiptir:

```console
docker run --rm -it -p 5000:5000 --memory 4g --cpus 2 ^
--mount type=bind,src=c:\input,target=/input ^
--mount type=bind,src=c:\output,target=/output ^
mcr.microsoft.com/azure-cognitive-services/luis:latest ^
Eula=accept ^
Billing={ENDPOINT_URL} ^
ApiKey={API_KEY}
```

### <a name="applicationinsights-example"></a>ApplicationInsights örneği

Aşağıdaki örnek, kapsayıcı çalışırken Application Insights telemetri göndermek için ApplicationInsights bağımsız değişkenini ayarlar:

```console
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 ^
--mount type=bind,src=c:\input,target=/input ^
--mount type=bind,src=c:\output,target=/output ^
mcr.microsoft.com/azure-cognitive-services/luis:latest ^
Eula=accept ^
Billing={ENDPOINT_URL} ^
ApiKey={API_KEY} ^
InstrumentationKey={INSTRUMENTATION_KEY}
```

### <a name="logging-example"></a>Günlüğe kaydetme örneği 

Aşağıdaki komut, günlük düzeyini olarak `Logging:Console:LogLevel` [`Information`](https://msdn.microsoft.com)yapılandırmak için günlüğe kaydetme düzeyini ayarlar. 

```console
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 ^
--mount type=bind,src=c:\input,target=/input ^
--mount type=bind,src=c:\output,target=/output ^
mcr.microsoft.com/azure-cognitive-services/luis:latest ^
Eula=accept ^
Billing={ENDPOINT_URL} ^
ApiKey={API_KEY} ^
Logging:Console:LogLevel:Default=Information
```

## <a name="next-steps"></a>Sonraki adımlar

* [Kapsayıcıları yüklemeyi ve çalıştırmayı](luis-container-howto.md) inceleyin
* LUSıS işlevleriyle ilgili sorunları çözmek için [sorun giderme](troubleshooting.md) bölümüne bakın.
* Daha fazla bilişsel [Hizmetler kapsayıcısı](../cognitive-services-container-support.md) kullanın
