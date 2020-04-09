---
title: Konuşma kapsayıcılarını yapılandırma
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti, kapsayıcılarınız için depolama, günlük ve telemetri ve güvenlik ayarlarını kolayca yapılandırabilmeniz ve yönetebilmeniz için her kapsayıcıya ortak bir yapılandırma çerçevesi sağlar.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 5c74aa48b18661236eb55278d1e5a05215b2432c
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80877584"
---
# <a name="configure-speech-service-containers"></a>Konuşma hizmet kapsayıcılarını yapılandırma

Konuşma kapsayıcıları, müşterilerin hem sağlam bulut özelliklerinden hem de kenar yerelliğinden yararlanmak için optimize edilmiş tek bir konuşma uygulaması mimarisi oluşturmasına olanak tanır. Şu anda desteklediğimiz dört konuşma kapsayıcısı, **konuşmadan metne**, **özel konuşmadan metne,** **metinden konuşmaya**ve **özel metinden konuşmaya.**

**Konuşma** kapsayıcısı çalışma zamanı ortamı `docker run` komut bağımsız değişkenleri kullanılarak yapılandırılır. Bu kapsayıcı birkaç isteğe bağlı ayarları ile birlikte birkaç gerekli ayarları vardır. Komutun birkaç [örneği](#example-docker-run-commands) mevcuttur. Kapsayıcıya özgü ayarlar fatura ayarlarıdır.

## <a name="configuration-settings"></a>Yapılandırma ayarları

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting)ve [`Eula`](#eula-setting) ayarlar birlikte kullanılır ve her üçü için de geçerli değerler sağlamanız gerekir; aksi takdirde konteyneriniz çalışmayabaşlamaz. Bir kapsayıcıyı anında kullanmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için [Faturalandırma'ya](speech-container-howto.md#billing)bakın.

## <a name="apikey-configuration-setting"></a>ApiKey yapılandırma ayarı

Ayar, `ApiKey` kapsayıcının fatura bilgilerini izlemek için kullanılan Azure kaynak anahtarını belirtir. ApiKey için bir değer belirtmeniz gerekir ve değer [`Billing`](#billing-configuration-setting) yapılandırma ayarı için belirtilen _Konuşma_ kaynağı için geçerli bir anahtar olmalıdır.

Bu ayar aşağıdaki yerde bulunabilir:

- Azure portalı: **Keys** altında **Konuşmanın** Kaynak Yönetimi

## <a name="applicationinsights-setting"></a>ApplicationInsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Faturalandırma yapılandırma ayarı

Ayar, `Billing` kapsayıcının fatura bilgilerini ölçmede kullanılan _Azure'daki Konuşma_ kaynağının bitiş noktası URI'yi belirtir. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve değer Azure'daki bir _Konuşma_ kaynağı için geçerli bir bitiş noktası URI olmalıdır. Kapsayıcı her 10 ila 15 dakikada bir kullanımı bildirir.

Bu ayar aşağıdaki yerde bulunabilir:

- Azure portalı: **Konuşmanın** Genel Bakışı, etiketli`Endpoint`

| Gerekli | Adı | Veri türü | Açıklama |
| -------- | ---- | --------- | ----------- |
| Evet | `Billing` | Dize | Faturalandırma uç noktası URI. FaturaLAMA URI'si edinme hakkında daha fazla bilgi için [gerekli parametreleri toplamaya](speech-container-howto.md#gathering-required-parameters)bakın. Daha fazla bilgi ve bölgesel uç noktaların tam listesi [için, Bilişsel Hizmetler için Özel alt alan adları bölümüne](../cognitive-services-custom-subdomains.md)bakın. |

## <a name="eula-setting"></a>Eula ayarı

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Akıcı ayarlar

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>HTTP proxy kimlik bilgileri ayarları

[!INCLUDE [Container shared HTTP proxy settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Oturum açma ayarları

[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]

## <a name="mount-settings"></a>Montaj ayarları

Kapsayıcıya ve kapsayıcıdan veri okumak ve yazmak için bağlama bağlarını kullanın. Docker `--mount` [run](https://docs.docker.com/engine/reference/commandline/run/) komutundaki seçeneği belirterek bir giriş yuvası veya çıktı montajı belirtebilirsiniz.

Standart Konuşma kapsayıcıları, eğitim veya hizmet verilerini depolamak için giriş veya çıktı montajları kullanmaz. Ancak, özel konuşma kapsayıcıları ses montajlarına dayanır.

Ana bilgisayar montaj konumunun tam sözdizimi ana bilgisayar işletim sistemine bağlı olarak değişir. Ayrıca, [ana bilgisayar'ın](speech-container-howto.md#the-host-computer)montaj konumu, docker servis hesabı tarafından kullanılan izinler ile ana bilgisayar montaj konum izinleri arasındaki bir çakışma nedeniyle erişilemeyebilir.

| İsteğe bağlı | Adı | Veri türü | Açıklama |
| -------- | ---- | --------- | ----------- |
| İzin verilmiyor | `Input` | Dize | Standart Konuşma kapsayıcıları bunu kullanmaz. Özel konuşma [kapsayıcıları ses bağlar](#volume-mount-settings)kullanın.                                                                                    |
| İsteğe bağlı | `Output` | Dize | Çıkış montaj hedefi. Varsayılan değer: `/output`. Bu günlüklerin yeridir. Buna kapsayıcı günlükleri de dahildir. <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output` |

## <a name="volume-mount-settings"></a>Ses montaj ayarları

Özel konuşma [kapsayıcıları,](https://docs.docker.com/storage/volumes/) özel modelleri sürdürmek için birim bağlar kullanır. [Docker çalıştır](https://docs.docker.com/engine/reference/commandline/run/) komutuna `-v` (veya) `--volume`seçeneğini ekleyerek bir birim montaj belirtebilirsiniz.

Özel modeller, özel konuşma konteyner docker çalıştır komutunun bir parçası olarak yeni bir modelin yutulur ilk kez indirilir. Özel bir konuşma `ModelId` kapsayıcısı için aynı sıralı çalışır, daha önce indirilen modeli kullanır. Ses montaj sağlanmaz, özel modeller kalıcı olamaz.

Birim montaj ayarı üç `:` renk ayrılmış alandan oluşur:

1. İlk alan, ana makinedeki birimin adıdır, örneğin _C:\input._
2. İkinci alan, örneğin _/usr/local/models_gibi kapsayıcıdaki dizindir.
3. Üçüncü alan (isteğe bağlı) daha fazla bilgi için [kullanım hacimleri](https://docs.docker.com/storage/volumes/)bakın, seçeneklerin virgülle ayrılmış bir listesidir.

### <a name="volume-mount-example"></a>Hacim montaj örneği

```bash
-v C:\input:/usr/local/models
```

Bu komut, kaplar _/usr/local/models_ dizinine ana makine _C:\giriş_ dizinini bağlar.

> [!IMPORTANT]
> Ses montaj ayarları yalnızca **Özel Konuşmadan metne** ve **Özel Metinden Konuşmaya** kapsayıcılar için geçerlidir. Standart **Konuşma-metin** ve **Metin-to-speech** kapsayıcıları ses montajları kullanmaz.

## <a name="example-docker-run-commands"></a>Örnek docker çalıştır komutları

Aşağıdaki örnekler, komutların nasıl yazılabildiğini `docker run` ve kullanılacağını göstermek için yapılandırma ayarlarını kullanır. Bir kez çalışırken, kapsayıcı [onu durdurunkadar](speech-container-howto.md#stop-the-container) çalışmaya devam ediyor.

- **Satır devamı karakteri**: Aşağıdaki bölümlerdeki Docker komutları, `\`çizgi devamı karakteri olarak arka çizgiyi kullanır. Ana bilgisayar işletim sisteminizin gereksinimlerine göre bunu değiştirin veya kaldırın.
- **Bağımsız değişken sırası**: Docker kapsayıcılarını bilmiyorsanız bağımsız değişkenlerin sırasını değiştirmeyin.

{_argument_name_} 'yi kendi değerlerinizle değiştirin:

| Yer tutucu | Değer | Biçim veya örnek |
| ----------- | ----- | ----------------- |
| **{API_KEY}** | Azure `Speech` Tuşları sayfasındaki `Speech` kaynağın bitiş noktası anahtarı.   | `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`                                                                                  |
| **{ENDPOINT_URI}** | Faturalandırma bitiş noktası değeri Azure `Speech` Genel Bakış sayfasında kullanılabilir. | Açık örnekler için [gerekli parametreleri toplamaya](speech-container-howto.md#gathering-required-parameters) bakın. |

[!INCLUDE [subdomains-note](../../../includes/cognitive-services-custom-subdomains-note.md)]

> [!IMPORTANT]
> `Eula`Kapsayıcıyı `Billing`çalıştırmak `ApiKey` için , ve seçenekler belirtilmelidir; aksi takdirde, kapsayıcı başlamaz. Daha fazla bilgi için [Faturalandırma'ya](#billing-configuration-setting)bakın.
> ApiKey değeri, Azure Konuşma Kaynağı tuşları sayfasındaki **Anahtardır.**

## <a name="speech-container-docker-examples"></a>Konuşma konteynerDocker örnekleri

Aşağıdaki Docker örnekleri Konuşma kapsayıcısı içindir.

## <a name="speech-to-text"></a>[Konuşmayı metne dönüştürme](#tab/stt)

### <a name="basic-example-for-speech-to-text"></a>Konuşmadan metne temel örnek

```Docker
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

### <a name="logging-example-for-speech-to-text"></a>Konuşmadan metne için günlük örneği

```Docker
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
Logging:Console:LogLevel:Default=Information
```

## <a name="custom-speech-to-text"></a>[Özel Konuşma-metin](#tab/cstt)

### <a name="basic-example-for-custom-speech-to-text"></a>Özel Konuşma-metin için temel örnek

```Docker
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

### <a name="logging-example-for-custom-speech-to-text"></a>Özel Konuşma-metin için günlük örneği

```Docker
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
Logging:Console:LogLevel:Default=Information
```

## <a name="text-to-speech"></a>[Metin okuma](#tab/tss)

### <a name="basic-example-for-text-to-speech"></a>Metinden konuşmaya temel örnek

```Docker
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

### <a name="logging-example-for-text-to-speech"></a>Metinden konuşmaya için günlük örneği

```Docker
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
Logging:Console:LogLevel:Default=Information
```

## <a name="custom-text-to-speech"></a>[Özel Metinden konuşmaya](#tab/ctts)

### <a name="basic-example-for-custom-text-to-speech"></a>Özel Metinden konuşmaya temel örnek

```Docker
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

### <a name="logging-example-for-custom-text-to-speech"></a>Özel Metinden konuşmaya için günlük örneği

```Docker
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
Logging:Console:LogLevel:Default=Information
```

---

## <a name="next-steps"></a>Sonraki adımlar

- [Kapsayıcıların nasıl yüklenir ve çalıştırılabildiğini](speech-container-howto.md) gözden geçirin
