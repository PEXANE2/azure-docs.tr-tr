---
title: Docker konteyner ayarları - LUIS
titleSuffix: Azure Cognitive Services
description: LUIS kapsayıcı çalışma zamanı ortamı komut `docker run` bağımsız değişkenleri kullanılarak yapılandırılır. LUIS birkaç isteğe bağlı ayarları ile birlikte birkaç gerekli ayarları vardır.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/07/2019
ms.author: dapine
ms.openlocfilehash: a30fcd0ec7e53c78876596baf787639e81c638db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73795018"
---
# <a name="configure-language-understanding-docker-containers"></a>Dil Anlama Docker kapsayıcıları yapılandırma 

**Dil Anlayışı** (LUIS) kapsayıcı çalışma zamanı ortamı `docker run` komut bağımsız değişkenleri kullanılarak yapılandırılır. LUIS birkaç isteğe bağlı ayarları ile birlikte birkaç gerekli ayarları vardır. Komutun birkaç [örneği](#example-docker-run-commands) mevcuttur. Kapsayıcıya özgü ayarlar, giriş [montaj ayarları](#mount-settings) ve faturalandırma ayarlarıdır. 

## <a name="configuration-settings"></a>Yapılandırma ayarları

Bu kapsayıcı aşağıdaki yapılandırma ayarlarına sahiptir:

|Gerekli|Ayar|Amaç|
|--|--|--|
|Evet|[ApiKey](#apikey-setting)|Fatura bilgilerini izlemek için kullanılır.|
|Hayır|[ApplicationInsights](#applicationinsights-setting)|Kapsayıcınıza [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) telemetri desteği eklemenize olanak tanır.|
|Evet|[Faturalama](#billing-setting)|Azure'daki hizmet kaynağının bitiş noktası URI'yi belirtir.|
|Evet|[Eula](#eula-setting)| Kapsayıcının lisansını kabul ettiğinizi gösterir.|
|Hayır|[Akıcı](#fluentd-settings)|Log ve isteğe bağlı olarak metrik verileri Akıcı bir sunucuya yazın.|
|Hayır|[Http Proxy](#http-proxy-credentials-settings)|Giden isteklerde bulunmak için bir HTTP proxy'si yapılandırın.|
|Hayır|[Günlüğe Kaydetme](#logging-settings)|Kapsayıcınız için ASP.NET Çekirdek günlük desteği sağlar. |
|Evet|[Bağlar](#mount-settings)|Ana bilgisayardan kapsayıcıya ve kapsayıcıdan ana bilgisayara kadar verileri okuma ve yazma.|

> [!IMPORTANT]
> [`ApiKey`](#apikey-setting), [`Billing`](#billing-setting)ve [`Eula`](#eula-setting) ayarlar birlikte kullanılır ve her üçü için de geçerli değerler sağlamanız gerekir; aksi takdirde konteyneriniz çalışmayabaşlamaz. Bir kapsayıcıyı anında kullanmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için [Faturalandırma'ya](luis-container-howto.md#billing)bakın.

## <a name="apikey-setting"></a>ApiKey ayarı

Ayar, `ApiKey` kapsayıcının fatura bilgilerini izlemek için kullanılan Azure kaynak anahtarını belirtir. ApiKey için bir değer belirtmeniz gerekir ve değer [`Billing`](#billing-setting) yapılandırma ayarı için belirtilen Bilişsel _Hizmetler_ kaynağı için geçerli bir anahtar olmalıdır.

Bu ayar aşağıdaki yerlerde bulunabilir:

* Azure portalı: **Bilişsel Hizmetler** Kaynak Yönetimi, **Anahtarlar** altında
* LUIS portalı: **Tuşlar ve Bitiş Noktası ayarları** sayfası. 

Başlangıç anahtarını veya yazma anahtarını kullanmayın. 

## <a name="applicationinsights-setting"></a>ApplicationInsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-setting"></a>Faturalandırma ayarı

Ayar, `Billing` kapsayıcının fatura bilgilerini ölçmede kullanılan _Azure'daki Bilişsel Hizmetler_ kaynağının bitiş noktası URI'yi belirtir. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve değer Azure'daki _Bilişsel Hizmetler_ kaynağı için geçerli bir bitiş noktası URI olmalıdır. Kapsayıcı her 10 ila 15 dakikada bir kullanımı bildirir.

Bu ayar aşağıdaki yerlerde bulunabilir:

* Azure portalı: **Bilişsel Hizmetlere** Genel Bakış, etiketli`Endpoint`
* LUIS portalı: Bitiş noktası URI'nin bir parçası olarak **tuşlar ve Bitiş Noktası ayarları** sayfası.

| Gerekli | Adı | Veri türü | Açıklama |
|----------|------|-----------|-------------|
| Evet      | `Billing` | string | Faturalandırma uç noktası URI. FaturaLAMA URI'si edinme hakkında daha fazla bilgi için [gerekli parametreleri toplamaya](luis-container-howto.md#gathering-required-parameters)bakın. Daha fazla bilgi ve bölgesel uç noktaların tam listesi [için, Bilişsel Hizmetler için Özel alt alan adları bölümüne](../cognitive-services-custom-subdomains.md)bakın. |

## <a name="eula-setting"></a>Eula ayarı

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Akıcı ayarlar

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>HTTP proxy kimlik bilgileri ayarları

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Oturum açma ayarları
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]

## <a name="mount-settings"></a>Montaj ayarları

Kapsayıcıya ve kapsayıcıdan veri okumak ve yazmak için bağlama bağlarını kullanın. Docker `--mount` [run](https://docs.docker.com/engine/reference/commandline/run/) komutundaki seçeneği belirterek bir giriş yuvası veya çıktı montajı belirtebilirsiniz. 

LUIS kapsayıcısı, eğitim veya hizmet verilerini depolamak için giriş veya çıktı montajları kullanmaz. 

Ana bilgisayar montaj konumunun tam sözdizimi ana bilgisayar işletim sistemine bağlı olarak değişir. Ayrıca, [ana bilgisayar'ın](luis-container-howto.md#the-host-computer)montaj konumu, docker servis hesabı tarafından kullanılan izinler ile ana bilgisayar montaj konum izinleri arasındaki bir çakışma nedeniyle erişilemeyebilir. 

Aşağıdaki tabloda desteklenen ayarlar açıklanmaktadır.

|Gerekli| Adı | Veri türü | Açıklama |
|-------|------|-----------|-------------|
|Evet| `Input` | Dize | Giriş yuvasının hedefi. Varsayılan değer: `/input`. Burası LUIS paket dosyalarının yeri. <br><br>Örnek:<br>`--mount type=bind,src=c:\input,target=/input`|
|Hayır| `Output` | Dize | Çıkış montaj hedefi. Varsayılan değer: `/output`. Bu günlüklerin yeridir. Buna LUIS sorgu günlükleri ve kapsayıcı günlükleri dahildir. <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Örnek docker çalıştır komutları

Aşağıdaki örnekler, komutların nasıl yazılabildiğini `docker run` ve kullanılacağını göstermek için yapılandırma ayarlarını kullanır.  Bir kez çalışırken, kapsayıcı [onu durdurunkadar](luis-container-howto.md#stop-the-container) çalışmaya devam ediyor.

* Bu örnekler, Windows'ta `C:` herhangi bir izin çakışmasını önlemek için sürücü üzerindeki dizin kullanılır. Giriş dizini olarak belirli bir dizini kullanmanız gerekiyorsa, docker hizmeti ne izin verebilirsiniz. 
* Docker konteynerleri hakkında çok bilginiz olmadıkça bağımsız değişkenlerin sırasını değiştirmeyin.
* Farklı bir işletim sistemi kullanıyorsanız, sisteminiz için doğru konsol/terminal, bağlar için klasör sözdizimini ve satır devam karakterini kullanın. Bu örnekler, satır devam karakteri `^`olan bir Windows konsolu varsayalım. Kapsayıcı bir Linux işletim sistemi olduğundan, hedef montaj Linux tarzı klasör sözdizimini kullanır.

{_argument_name_} 'yi kendi değerlerinizle değiştirin:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
| **{API_KEY}** | Azure `LUIS` Tuşları sayfasındaki `LUIS` kaynağın bitiş noktası anahtarı. | `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` |
| **{ENDPOINT_URI}** | Faturalandırma bitiş noktası değeri Azure `LUIS` Genel Bakış sayfasında kullanılabilir.| Açık örnekler için [gerekli parametreleri toplamaya](luis-container-howto.md#gathering-required-parameters) bakın. |

[!INCLUDE [subdomains-note](../../../includes/cognitive-services-custom-subdomains-note.md)]

> [!IMPORTANT]
> `Eula`Kapsayıcıyı `Billing`çalıştırmak `ApiKey` için , ve seçenekler belirtilmelidir; aksi takdirde, kapsayıcı başlamaz. Daha fazla bilgi için [Faturalandırma'ya](luis-container-howto.md#billing)bakın.
> ApiKey değeri, LUIS portalındaki Anahtar ve Uç Noktalar sayfasından gelen **Anahtardır** ve Azure `Cognitive Services` kaynak anahtarları sayfasında da kullanılabilir. 

### <a name="basic-example"></a>Temel örnek

Aşağıdaki örnek, kapsayıcıyı çalıştırmak için mümkün olan en az bağımsız değişkene sahiptir:

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

Aşağıdaki örnek, kapsayıcı çalışırken Uygulama Öngörüleri'ne telemetri göndermek için ApplicationInsights bağımsız değişkenini ayarlar:

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

### <a name="logging-example"></a>Günlük örneği 

Aşağıdaki komut, `Logging:Console:LogLevel`günlük düzeyini ' de yapılandırmak [`Information`](https://msdn.microsoft.com)için günlük düzeyini ayarlar. 

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

* [Kapsayıcıların nasıl yüklenir ve çalıştırılabildiğini](luis-container-howto.md) gözden geçirin
* LUIS işleviyle ilgili sorunları gidermek için [Sorun Giderme](troubleshooting.md) konusuna bakın.
* Daha fazla [Bilişsel Hizmet Kapsayıcısı](../cognitive-services-container-support.md) Kullanın
