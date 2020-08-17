---
title: Anomali algılayıcısı ortam değişkenleri
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/30/2020
ms.author: aahi
ms.openlocfilehash: 4a44df3f2108421c73c161d8f2df37330b9c2d14
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85980233"
---
### <a name="create-an-environment-variable"></a>Ortam değişkeni oluşturma

>[!NOTE]
> 1 Temmuz 2019 ' den sonra oluşturulan deneme olmayan kaynaklar için uç noktalar aşağıda gösterilen özel alt etki alanı biçimini kullanır. Daha fazla bilgi ve bölgesel uç noktaların tamamen listesi için bkz. bilişsel [Hizmetler Için özel alt etki alanı adları](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains). 

Oluşturduğunuz kaynaktan anahtarınızı ve uç noktayı kullanarak, kimlik doğrulama için iki ortam değişkeni oluşturun:

* `ANOMALY_DETECTOR_KEY` -İsteklerinizin kimliğini doğrulamak için kaynak anahtarı.
* `ANOMALY_DETECTOR_ENDPOINT` -API istekleri göndermek için kaynak uç noktası. Şu şekilde görünür: 
  * `https://<your-custom-subdomain>.api.cognitive.microsoft.com` 

İşletim sisteminiz için yönergeleri kullanın.

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx ANOMALY_DETECTOR_KEY <replace-with-your-anomaly-detector-key>
setx ANOMALY_DETECTOR_ENDPOINT <replace-with-your-anomaly-detector-endpoint>
```

Ortam değişkenini ekledikten sonra konsol penceresini yeniden başlatın.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export ANOMALY_DETECTOR_KEY=<replace-with-your-anomaly-detector-key>
export ANOMALY_DETECTOR_ENDPOINT=<replace-with-your-anomaly-detector-endpoint>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.bashrc` çalıştırın.

#### <a name="macos"></a>[macOS](#tab/unix)

Hesabınızı düzenleyin `.bash_profile` ve ortam değişkenini ekleyin:

```bash
export ANOMALY_DETECTOR_KEY=<replace-with-your-anomaly-detector-key>
export ANOMALY_DETECTOR_ENDPOINT=<replace-with-your-anomaly-detector-endpoint>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source .bash_profile` çalıştırın.

***