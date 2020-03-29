---
title: Kapsayıcıları yapılandırma - Metin Analizi
titleSuffix: Azure Cognitive Services
description: Text Analytics, kapsayıcılarınızın depolama, günlük ve telemetri ve güvenlik ayarlarını kolayca yapılandırabilmeniz ve yönetebilmeniz için her kapsayıcıya ortak bir yapılandırma çerçevesi sağlar.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 11/07/2019
ms.author: dapine
ms.openlocfilehash: 8a39327275dca43ddb6ce0e46a3e3bb51ec4555b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73795304"
---
# <a name="configure-text-analytics-docker-containers"></a>Metin Analizi docker konteynerlerini yapılandırma

Text Analytics, kapsayıcılarınızın depolama, günlük ve telemetri ve güvenlik ayarlarını kolayca yapılandırabilmeniz ve yönetebilmeniz için her kapsayıcıya ortak bir yapılandırma çerçevesi sağlar.

## <a name="configuration-settings"></a>Yapılandırma ayarları

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting)ve [`Eula`](#eula-setting) ayarlar birlikte kullanılır ve her üçü için de geçerli değerler sağlamanız gerekir; aksi takdirde konteyneriniz çalışmayabaşlamaz. Bir kapsayıcıyı anında kullanmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için [Faturalandırma'ya](how-tos/text-analytics-how-to-install-containers.md#billing)bakın.

## <a name="apikey-configuration-setting"></a>ApiKey yapılandırma ayarı

Ayar, `ApiKey` kapsayıcının fatura bilgilerini izlemek için kullanılan Azure kaynak anahtarını belirtir. ApiKey için bir değer belirtmeniz gerekir ve değer [`Billing`](#billing-configuration-setting) yapılandırma ayarı için belirtilen Metin _Analizi_ kaynağı için geçerli bir anahtar olmalıdır.

Bu ayar aşağıdaki yerde bulunabilir:

* Azure portalı: **Text Analytics** kaynak yönetimi, **Keys** altında

## <a name="applicationinsights-setting"></a>ApplicationInsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Faturalandırma yapılandırma ayarı

Ayar, `Billing` kapsayıcının fatura bilgilerini ölçmede kullanılan _Azure'daki Metin Analizi_ kaynağının bitiş noktası URI'yi belirtir. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve değer Azure'daki __Text Analytics_ kaynağı için geçerli bir bitiş noktası URI olmalıdır. Kapsayıcı her 10 ila 15 dakikada bir kullanımı bildirir.

Bu ayar aşağıdaki yerde bulunabilir:

* Azure portalı: **Metin Analizine** Genel Bakış, etiketli`Endpoint`

|Gerekli| Adı | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Evet| `Billing` | Dize | Faturalandırma uç noktası URI. FaturaLAMA URI'si edinme hakkında daha fazla bilgi için [gerekli parametreleri toplamaya](how-tos/text-analytics-how-to-install-containers.md#gathering-required-parameters)bakın. Daha fazla bilgi ve bölgesel uç noktaların tam listesi [için, Bilişsel Hizmetler için Özel alt alan adları bölümüne](../cognitive-services-custom-subdomains.md)bakın. |

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

Metin Analizi kapsayıcıları, eğitim veya hizmet verilerini depolamak için giriş veya çıktı montajları kullanmaz. 

Ana bilgisayar montaj konumunun tam sözdizimi ana bilgisayar işletim sistemine bağlı olarak değişir. Ayrıca, [ana bilgisayar'ın](how-tos/text-analytics-how-to-install-containers.md#the-host-computer)montaj konumu, docker servis hesabı tarafından kullanılan izinler ile ana bilgisayar montaj konum izinleri arasındaki bir çakışma nedeniyle erişilemeyebilir. 

|İsteğe bağlı| Adı | Veri türü | Açıklama |
|-------|------|-----------|-------------|
|İzin verilmiyor| `Input` | Dize | Metin Analizi kapsayıcıları bunu kullanmaz.|
|İsteğe bağlı| `Output` | Dize | Çıkış montaj hedefi. Varsayılan değer: `/output`. Bu günlüklerin yeridir. Buna kapsayıcı günlükleri de dahildir. <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Örnek docker çalıştır komutları 

Aşağıdaki örnekler, komutların nasıl yazılabildiğini `docker run` ve kullanılacağını göstermek için yapılandırma ayarlarını kullanır.  Bir kez çalışırken, kapsayıcı [onu durdurunkadar](how-tos/text-analytics-how-to-install-containers.md#stop-the-container) çalışmaya devam ediyor.

* **Satır devam karakteri**: Aşağıdaki bölümlerdeki docker komutları, `\`çizgi devamı karakteri olarak arka çizgiyi kullanır. Ana bilgisayar işletim sisteminizin gereksinimlerine göre bunu değiştirin veya kaldırın. 
* **Bağımsız değişken sırası**: Docker kapları hakkında çok bilginiz olmadıkça bağımsız değişkenlerin sırasını değiştirmeyin.

{_argument_name_} 'yi kendi değerlerinizle değiştirin:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
| **{API_KEY}** | Azure `Text Analytics` Tuşları sayfasında `Text Analytics` bulunan kaynağın bitiş noktası anahtarı. |`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`|
| **{ENDPOINT_URI}** | Faturalandırma bitiş noktası değeri Azure `Text Analytics` Genel Bakış sayfasında kullanılabilir.| Açık örnekler için [gerekli parametreleri toplamaya](how-tos/text-analytics-how-to-install-containers.md#gathering-required-parameters) bakın. |

> [!IMPORTANT]
> `Eula`Kapsayıcıyı `Billing`çalıştırmak `ApiKey` için , ve seçenekler belirtilmelidir; aksi takdirde, kapsayıcı başlamaz.  Daha fazla bilgi için [Faturalandırma'ya](how-tos/text-analytics-how-to-install-containers.md#billing)bakın.
> ApiKey değeri, Azure `Text Analytics` Kaynak anahtarları sayfasındaki **Anahtardır.** 

#### <a name="key-phrase-extraction"></a>[Anahtar İfade Ayıklama](#tab/keyphrase)

[!INCLUDE [key-phrase-extraction-docker-examples](includes/key-phrase-extraction-docker-examples.md)]

#### <a name="language-detection"></a>[Dil Algılama](#tab/language)

[!INCLUDE [language-detection-docker-examples](includes/language-detection-docker-examples.md)]

#### <a name="sentiment-analysis"></a>[Duygusallık Analizi](#tab/sentiment)

[!INCLUDE [sentiment-analysis-docker-examples](includes/sentiment-analysis-docker-examples.md)]

***

## <a name="next-steps"></a>Sonraki adımlar

* [Kapsayıcıların nasıl yüklenir ve çalıştırılabildiğini](how-tos/text-analytics-how-to-install-containers.md) gözden geçirin
* Daha fazla [Bilişsel Hizmet Kapsayıcısı](../cognitive-services-container-support.md) Kullanın
