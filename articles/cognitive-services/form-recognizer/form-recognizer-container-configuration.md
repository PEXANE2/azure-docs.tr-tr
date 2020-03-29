---
title: Form Tanıyıcısı için bir kapsayıcı nasıl yapılandırılır?
titleSuffix: Azure Cognitive Services
description: Form Ve tablo verilerini ayrıştıracak şekilde Form Tanıyılayıcı kapsayıcısını nasıl yapılandırıştırmayı öğrenin.
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 11/07/2019
ms.author: dapine
ms.openlocfilehash: 5439ec0c0aab5b8c127b651147e4b25d27c58390
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75379632"
---
# <a name="configure-form-recognizer-containers"></a>Form Tanıyıcı kapsayıcılarını yapılandırın

Azure Form Tanıyıcı kapsayıcılarını kullanarak, hem sağlam bulut özelliklerinden hem de kenar yerelliğinden yararlanmak için en iyi duruma getirilmiş bir uygulama mimarisi oluşturabilirsiniz.

Komut bağımsız değişkenlerini kullanarak Form Recognizer kapsayıcı `docker run` çalışma zamanı ortamını yapılandırırsınız. Bu kapsayıcı birkaç gerekli ayarları ve birkaç isteğe bağlı ayarları vardır. Birkaç örnek için ["Örnek docker run komutları"](#example-docker-run-commands) bölümüne bakın. Kapsayıcıya özgü ayarlar fatura ayarlarıdır.

> [!IMPORTANT]
> Form Tanıyıcı kapsayıcıları şu anda Form Tanıyıcı API sürümü 1.0 kullanın. Bunun yerine yönetilen hizmeti kullanarak API'nin en son sürümüne erişebilirsiniz.

## <a name="configuration-settings"></a>Yapılandırma ayarları

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting)ve [`Eula`](#eula-setting) ayarlar birlikte kullanılır. Her üç ayar için de geçerli değerler sağlamanız gerekir; aksi takdirde, kapsayıcınız başlamaz. Bir kapsayıcıyı anında kullanmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için [Faturalandırma'ya](form-recognizer-container-howto.md#billing)bakın.

## <a name="apikey-configuration-setting"></a>ApiKey yapılandırma ayarı

Ayar, `ApiKey` kapsayıcının fatura bilgilerini izlemek için kullanılan Azure kaynak anahtarını belirtir. ApiKey değeri, "Faturalandırma yapılandırma ayarı" bölümünde belirtilen Form `Billing` _Tanıyıcı_ kaynağı için geçerli bir anahtar olmalıdır.

Bu ayarı Azure portalında, **Form Tanıyıcı Kaynak Yönetimi'nde** **Keys**altında bulabilirsiniz.

## <a name="applicationinsights-setting"></a>ApplicationInsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Faturalandırma yapılandırma ayarı

Ayar, `Billing` Azure'daki _Form Tanıyıcı_ kaynağının kapsayıcının fatura bilgilerini ölçmeye alışkın bitiş noktası URI'sini belirtir. Bu yapılandırma ayarı için değer, Azure'daki form _tanıma kaynağı_ için geçerli bir bitiş noktası URI olmalıdır. Kapsayıcı her 10 ila 15 dakikada bir kullanımı bildirir.

Bu ayarı Azure portalında, **Form Tanıma Genel Bakışı'nda**, **Endpoint**altında bulabilirsiniz.

|Gerekli| Adı | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Evet| `Billing` | Dize | Faturalandırma uç noktası URI. FaturaLAMA URI'si edinme hakkında daha fazla bilgi için [gerekli parametreleri toplamaya](form-recognizer-container-howto.md#gathering-required-parameters)bakın. Daha fazla bilgi ve bölgesel uç noktaların tam listesi [için, Bilişsel Hizmetler için Özel alt alan adları bölümüne](../cognitive-services-custom-subdomains.md)bakın. |

## <a name="eula-setting"></a>Eula ayarı

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Akıcı ayarlar

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>HTTP proxy kimlik bilgileri ayarları

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Oturum açma ayarları

[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]


## <a name="mount-settings"></a>Montaj ayarları

Kapsayıcıya ve kapsayıcıdan veri okumak ve yazmak için bağlama bağlarını kullanın. Komuttaki `--mount` seçeneği belirterek bir giriş yuvası veya çıktı montajı belirtebilirsiniz. [ `docker run` ](https://docs.docker.com/engine/reference/commandline/run/)

Form Recognizer konteyneri bir giriş yuvası ve çıkış montaj gerektirir. Giriş yuvası salt okunur ve eğitim ve puanlama için kullanılan verilere erişmek için gereklidir. Çıktı montaj ıstabiri yazılabilir olmalı ve modelleri ve geçici verileri depolamak için kullanırsınız.

Ana bilgisayar montaj konumunun tam sözdizimi ana bilgisayar işletim sistemine bağlı olarak değişir. Ayrıca, Docker hizmet hesabı izinleri ile ana bilgisayar montaj konum izinleri arasındaki bir çakışma nedeniyle [ana bilgisayarın](form-recognizer-container-howto.md#the-host-computer) montaj konumuna erişilemeyebilir.

|İsteğe bağlı| Adı | Veri türü | Açıklama |
|-------|------|-----------|-------------|
|Gerekli| `Input` | Dize | Giriş yuvasının hedefi. Varsayılan değer: `/input`.    <br><br>Örnek:<br>`--mount type=bind,src=c:\input,target=/input`|
|Gerekli| `Output` | Dize | Çıkış montaj hedefi. Varsayılan değer: `/output`.  <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Örnek docker çalıştır komutları

Aşağıdaki örnekler, komutların nasıl yazılabildiğini `docker run` ve kullanılacağını göstermek için yapılandırma ayarlarını kullanır. Çalışırken, siz durduruncaya kadar kapsayıcı [stop it](form-recognizer-container-howto.md#stop-the-container)çalışmaya devam eder.

* **Satır devam karakteri**: Aşağıdaki bölümlerdeki Docker komutları\\çizgi devamı karakteri olarak bir arka çizgi ( ) kullanır. Ana bilgisayar işletim sisteminizin gereksinimlerine bağlı olarak bu karakteri değiştirin veya kaldırın.
* **Bağımsız değişken sırası**: Docker kapsayıcılarını bilmiyorsanız bağımsız değişkenlerin sırasını değiştirmeyin.

{_argument_name_} 'yi aşağıdaki tabloda kendi değerlerinizle değiştirin:

| Yer tutucu | Değer |
|-------------|-------|
| **{FORM_RECOGNIZER_API_KEY}** | Konteynırı başlatmak için kullanılan anahtar. Azure portalı Form Recognizer Keys sayfasında mevcuttur. |
| **{FORM_RECOGNIZER_ENDPOINT_URI}** | Faturalandırma bitiş noktası URI değeri, Azure portalı Form Recognizer Overview sayfasında kullanılabilir.|
| **{COMPUTER_VISION_API_KEY}** | Anahtar, Azure portalı Computer Vision API Keys sayfasında mevcuttur.|
| **{COMPUTER_VISION_ENDPOINT_URI}** | Faturalandırma bitiş noktası. Bulut tabanlı bir Computer Vision kaynağı kullanıyorsanız, URI değeri Azure portalı Computer Vision API Genel Bakış sayfasında kullanılabilir. *Bilişsel hizmetler-tanıma-metin* kapsayıcısı kullanıyorsanız, komuttaki kapsayıcıya geçirilen faturalandırma bitiş noktası URL'sini `docker run` kullanın. |

Bu değerlerin nasıl elde edileceklerine ilişkin ayrıntılar için [gerekli parametreleri toplamaya](form-recognizer-container-howto.md#gathering-required-parameters) bakın.

[!INCLUDE [cognitive-services-custom-subdomains-note](../../../includes/cognitive-services-custom-subdomains-note.md)]

> [!IMPORTANT]
> Kapsayıcıyı çalıştırmak için `Eula`, `Billing`ve `ApiKey` seçenekleri belirtin; aksi takdirde, kapsayıcı başlamaz. Daha fazla bilgi için [Faturalandırma'ya](#billing-configuration-setting)bakın.

## <a name="form-recognizer-container-docker-examples"></a>Form Recognizer konteyner Docker örnekleri

Aşağıdaki Docker örnekleri Form Tanıyıcısı kapsayıcısı içindir.

### <a name="basic-example-for-form-recognizer"></a>Form Tanıyıcısı için temel örnek

```Docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 2 \
--mount type=bind,source=c:\input,target=/input  \
--mount type=bind,source=c:\output,target=/output \
containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer \
Eula=accept \
Billing={FORM_RECOGNIZER_ENDPOINT_URI} \
ApiKey={FORM_RECOGNIZER_API_KEY} \
FormRecognizer:ComputerVisionApiKey={COMPUTER_VISION_API_KEY} \
FormRecognizer:ComputerVisionEndpointUri={COMPUTER_VISION_ENDPOINT_URI}
```

### <a name="logging-example-for-form-recognizer"></a>Form Tanıyıcı için günlük örneği

```Docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 2 \
--mount type=bind,source=c:\input,target=/input  \
--mount type=bind,source=c:\output,target=/output \
containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer \
Eula=accept \
Billing={FORM_RECOGNIZER_ENDPOINT_URI} \
ApiKey={FORM_RECOGNIZER_API_KEY} \
FormRecognizer:ComputerVisionApiKey={COMPUTER_VISION_API_KEY} \
FormRecognizer:ComputerVisionEndpointUri={COMPUTER_VISION_ENDPOINT_URI}
Logging:Console:LogLevel:Default=Information
```

## <a name="next-steps"></a>Sonraki adımlar

* [Derleme Kapsayıcıları Yükleyin ve çalıştırın.](form-recognizer-container-howto.md)
