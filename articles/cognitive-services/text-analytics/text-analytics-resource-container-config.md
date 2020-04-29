---
title: Kapsayıcıları yapılandırma-Metin Analizi
titleSuffix: Azure Cognitive Services
description: Metin Analizi, her kapsayıcıyı ortak bir yapılandırma çerçevesiyle sağlar; böylece, kapsayıcılarınız için depolama, günlüğe kaydetme ve telemetri ve güvenlik ayarlarını kolayca yapılandırabilir ve yönetebilirsiniz.
services: cognitive-services
author: aahill
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: f062fb2f3a653bc1b2845b92e373fdb67ba583d8
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80878706"
---
# <a name="configure-text-analytics-docker-containers"></a>Metin Analizi Docker kapsayıcılarını yapılandırma

Metin Analizi, her kapsayıcıyı ortak bir yapılandırma çerçevesiyle sağlar; böylece, kapsayıcılarınız için depolama, günlüğe kaydetme ve telemetri ve güvenlik ayarlarını kolayca yapılandırabilir ve yönetebilirsiniz.

## <a name="configuration-settings"></a>Yapılandırma ayarları

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting)Ve [`Eula`](#eula-setting) ayarları birlikte kullanılır ve üçü için geçerli değerler sağlamalısınız; Aksi takdirde Kapsayıcınız başlatılmaz. Bir kapsayıcı oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalandırma](how-tos/text-analytics-how-to-install-containers.md#billing).

## <a name="apikey-configuration-setting"></a>ApiKey yapılandırma ayarı

Bu `ApiKey` ayar, kapsayıcının fatura bilgilerini izlemek Için kullanılan Azure Kaynak anahtarını belirtir. ApiKey için bir değer belirtmeniz gerekir ve değerin [`Billing`](#billing-configuration-setting) yapılandırma ayarı için belirtilen _metin analizi_ kaynağı için geçerli bir anahtar olması gerekir.

Bu ayar aşağıdaki yerde bulunabilir:

* Azure portal: **metin analizi** kaynak yönetimi, **anahtarlar** altında

## <a name="applicationinsights-setting"></a>ApplicationInsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Faturalandırma yapılandırma ayarı

Bu `Billing` ayar, Azure 'da kapsayıcının fatura bilgilerini ölçmek için kullanılan _metin analizi_ kaynağının uç nokta URI 'sini belirtir. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve Azure 'da bir __metin analizi_ kaynağı için değer geçerli bir uç nokta URI 'si olmalıdır. Kapsayıcı her 10 ila 15 dakikada bir kullanım raporu sağlar.

Bu ayar aşağıdaki yerde bulunabilir:

* Azure portal: **metin analizi** genel bakış, etiketli`Endpoint`

|Gerekli| Adı | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Yes| `Billing` | Dize | Faturalama uç noktası URI 'SI. Faturalandırma URI 'sini alma hakkında daha fazla bilgi için bkz. [gerekli parametreleri toplama](how-tos/text-analytics-how-to-install-containers.md#gathering-required-parameters). Daha fazla bilgi ve bölgesel uç noktaların tamamen listesi için bkz. bilişsel [Hizmetler Için özel alt etki alanı adları](../cognitive-services-custom-subdomains.md). |

## <a name="eula-setting"></a>EULA ayarı

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Akışkan entd ayarları

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>Http proxy kimlik bilgileri ayarları

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Oturum açma ayarları
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]

## <a name="mount-settings"></a>Bağlama ayarları

Kapsayıcıya ve kapsayıcılardan veri okumak ve buradan veri yazmak için BIND bağlama kullanın. `--mount` [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunda seçeneğini belirterek bir giriş bağlama veya çıkış bağlama belirtebilirsiniz.

Metin Analizi kapsayıcıları, eğitim veya hizmet verilerini depolamak için giriş veya çıkış taklarını kullanmaz. 

Konak bağlama konumunun tam sözdizimi, ana bilgisayar işletim sistemine bağlı olarak değişir. Ayrıca, Docker hizmeti hesabı ve konak bağlama konumu izinleri tarafından kullanılan izinler arasındaki bir çakışma nedeniyle [ana bilgisayarın](how-tos/text-analytics-how-to-install-containers.md#the-host-computer)bağlama konumu erişilebilir olmayabilir. 

|İsteğe Bağlı| Adı | Veri türü | Açıklama |
|-------|------|-----------|-------------|
|İzin verilmiyor| `Input` | Dize | Metin Analizi kapsayıcılar bunu kullanmaz.|
|İsteğe Bağlı| `Output` | Dize | Çıkış bağlama hedefi. Varsayılan değer: `/output`. Bu, günlüklerin konumudur. Bu, kapsayıcı günlüklerini içerir. <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Örnek Docker Run komutları 

Aşağıdaki örneklerde, komutlarının nasıl yazılacağını ve kullanılacağını `docker run` göstermek için yapılandırma ayarları kullanılır.  Çalışan bir kez, kapsayıcıyı [durduruncaya](how-tos/text-analytics-how-to-install-containers.md#stop-the-container) kadar çalışmaya devam eder.

* **Satır devamlılık karakteri**: aşağıdaki bölümlerdeki Docker komutları, satır devamlılık karakteri olarak ters eğik `\`çizgi kullanır. Bunu, ana bilgisayar işletim sisteminizin gereksinimlerine göre değiştirin veya kaldırın. 
* **Bağımsız değişken sırası**: Docker Kapsayıcıları hakkında bilginiz yoksa bağımsız değişkenlerin sırasını değiştirmeyin.

{_Argument_name_} değerini kendi değerlerinizle değiştirin:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
| **{API_KEY}** | Azure `Text Analytics` `Text Analytics` anahtarları sayfasında kullanılabilir kaynağın uç nokta anahtarı. |`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`|
| **{ENDPOINT_URI}** | Faturalandırma uç noktası değeri, Azure `Text Analytics` Genel Bakış sayfasında bulunur.| Açık örnekler için [gerekli parametreleri toplama](how-tos/text-analytics-how-to-install-containers.md#gathering-required-parameters) konusuna bakın. |

> [!IMPORTANT]
> Kapsayıcısını `Eula`çalıştırmak `Billing`için, `ApiKey` , ve seçenekleri belirtilmelidir; Aksi takdirde, kapsayıcı başlatılmaz.  Daha fazla bilgi için bkz. [faturalandırma](how-tos/text-analytics-how-to-install-containers.md#billing).
> ApiKey değeri, Azure `Text Analytics` kaynak anahtarları sayfasından alınan **anahtardır** . 

#### <a name="key-phrase-extraction"></a>[Anahtar İfade Ayıklama](#tab/keyphrase)

[!INCLUDE [key-phrase-extraction-docker-examples](includes/key-phrase-extraction-docker-examples.md)]

#### <a name="language-detection"></a>[Dil Algılama](#tab/language)

[!INCLUDE [language-detection-docker-examples](includes/language-detection-docker-examples.md)]

#### <a name="sentiment-analysis"></a>[Yaklaşım Analizi](#tab/sentiment)

[!INCLUDE [sentiment-analysis-docker-examples](includes/sentiment-analysis-docker-examples.md)]

***

## <a name="next-steps"></a>Sonraki adımlar

* [Kapsayıcıları yüklemeyi ve çalıştırmayı](how-tos/text-analytics-how-to-install-containers.md) inceleyin
* Daha fazla bilişsel [Hizmetler kapsayıcısı](../cognitive-services-container-support.md) kullanın
