---
title: Kapsayıcıları yapılandırma-Translator Metin Çevirisi
titleSuffix: Azure Cognitive Services
description: Translator Metin Çevirisi, kapsayıcılarınızın depolama, günlüğe kaydetme ve telemetri ve güvenlik ayarlarını kolayca yapılandırıp yönetebilmeniz için ortak bir yapılandırma çerçevesiyle kapsayıcılar sağlar.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: dapine
ms.openlocfilehash: 3dbedcb5e5212091dc8906defa2b1cf5c7868d60
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73507676"
---
# <a name="configure-translator-text-docker-containers"></a>Translator Metin Çevirisi Docker kapsayıcılarını yapılandırma

Translator Metin Çevirisi, her kapsayıcıyı ortak bir yapılandırma çerçevesiyle sağlar; böylece, kapsayıcılarınız için depolama, günlüğe kaydetme ve telemetri ve güvenlik ayarlarını kolayca yapılandırabilir ve yönetebilirsiniz.

## <a name="configuration-settings"></a>Yapılandırma ayarları

Kapsayıcı aşağıdaki yapılandırma ayarlarına sahiptir:

|Gerekli|Ayar|Amaç|
|--|--|--|
|Hayır|[ApplicationInsights](#applicationinsights-setting)|, Kapsayıcınıza [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) telemetri desteğinin eklenmesini sağlar.|
|Evet|[Sözleşmesi](#eula-setting)| Kapsayıcının lisansını kabul ettiğinizi gösterir.|
|Hayır|[Fluentd](#fluentd-settings)|Günlük ve isteğe bağlı olarak ölçüm verilerini Floentd sunucusuna yazar.|
|Hayır|HTTP proxy 'si|Giden istekler oluşturmak için bir HTTP proxy yapılandırır.|
|Hayır|[Açmak](#logging-settings)|, Kapsayıcınız için ASP.NET Core günlük desteği sağlar. |
|Hayır|[Kullan](#mount-settings)|Ana bilgisayardan kapsayıcıya ve kapsayıcıdan ana bilgisayara geri veri okur ve yazar.|

> [!IMPORTANT]
> [`Eula`](#eula-setting) ayarı `accept`değeri ile sağlanmalıdır; Aksi takdirde, Kapsayıcınız başlatılmaz.

## <a name="applicationinsights-setting"></a>ApplicationInsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="eula-setting"></a>EULA ayarı

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Akışkan entd ayarları

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>HTTP proxy kimlik bilgileri ayarları

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Günlük kaydı ayarları
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]

## <a name="mount-settings"></a>Bağlama ayarları

Kapsayıcıya ve kapsayıcılardan veri okumak ve buradan veri yazmak için BIND bağlama kullanın. [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunda `--mount` seçeneğini belirterek bir giriş bağlama veya çıkış bağlama belirtebilirsiniz.

Translator Metin Çevirisi kapsayıcıları, eğitim veya hizmet verilerini depolamak için giriş veya çıkış taklarını kullanmaz. 

Konak bağlama konumunun tam sözdizimi, ana bilgisayar işletim sistemine bağlı olarak farklılık gösterir. Ayrıca, Docker hizmeti hesabı ve konak bağlama konumu izinleri tarafından kullanılan izinler arasındaki bir çakışma nedeniyle [ana bilgisayarın](how-to-install-containers.md#the-host-computer)bağlama konumu erişilebilir olmayabilir. 

|İsteğe bağlı| Ad | Veri türü | Açıklama |
|-------|------|-----------|-------------|
|İzin verilmiyor| `Input` | Dize | Translator Metin Çevirisi kapsayıcılar bunu kullanmaz.|
|İsteğe bağlı| `Output` | Dize | Çıkış bağlama hedefi. Varsayılan değer `/output` ' dır. Bu, günlüklerin konumudur. Bu, kapsayıcı günlüklerini içerir. <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Örnek Docker Run komutları 

Aşağıdaki örnekler `docker run` komutlarının nasıl yazılacağını ve kullanılacağını göstermek için yapılandırma ayarlarını kullanır.  Çalışan bir kez, kapsayıcıyı [durduruncaya](how-to-install-containers.md#stop-the-container) kadar çalışmaya devam eder.

* **Satır devamlılık karakteri**: aşağıdaki bölümlerdeki Docker komutları, satır devamlılık karakteri olarak `\`ters eğik çizgi kullanır. Bunu, ana bilgisayar işletim sisteminizin gereksinimlerine göre değiştirin veya kaldırın. 
* **Bağımsız değişken sırası**: Docker Kapsayıcıları hakkında bilginiz yoksa bağımsız değişkenlerin sırasını değiştirmeyin.

## <a name="translator-text-container-docker-examples"></a>Translator Metin Çevirisi kapsayıcı Docker örnekleri

Aşağıdaki Docker örnekleri Translator Metin Çevirisi kapsayıcısı içindir.

### <a name="basic-example"></a>Temel örnek

```
docker run --rm -it -p 5000:80 --memory 4g --cpus 4 \
containerpreview.azurecr.io/microsoft/cognitive-services-translator-text \
Eula=accept
```

### <a name="logging-example"></a>Günlüğe kaydetme örneği

```
docker run --rm -it -p 5000:80 --memory 4g --cpus 4 \
containerpreview.azurecr.io/microsoft/cognitive-services-translator-text \
Eula=accept \
Logging:Console:LogLevel:Default=Information
```

## <a name="next-steps"></a>Sonraki adımlar

* [Kapsayıcıları yüklemeyi ve çalıştırmayı](how-to-install-containers.md) inceleyin
* Daha fazla bilişsel [Hizmetler kapsayıcısı](../cognitive-services-container-support.md) kullanın
