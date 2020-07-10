---
title: Ortam değişkenleri
description: ortam değişkenlerini ayarlama
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 05/06/2020
ms.author: pafarley
ms.openlocfilehash: d63e902a59411a549235c955a39d7dbc4be068ba
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86156626"
---
Oluşturduğunuz kaynaktan anahtarınızı ve uç noktayı kullanarak, kimlik doğrulama için iki ortam değişkeni oluşturun:

* `FORM_RECOGNIZER_KEY`-İsteklerinizin kimliğini doğrulamak için kaynak anahtarı.
* `FORM_RECOGNIZER_ENDPOINT`-API istekleri göndermek için kaynak uç noktası. Şöyle görünür: 
  * `https://<your-custom-subdomain>.cognitiveservices.azure.com`

>[!NOTE]
> 1 Temmuz 2019 ' den sonra oluşturulan kaynaklar için uç noktalar aşağıda gösterilen özel alt etki alanı biçimini kullanır. Daha fazla bilgi ve bölgesel uç noktaların tamamen listesi için bkz. bilişsel [Hizmetler Için özel alt etki alanı adları](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains). 

İşletim Sisteminizdeki ortam değişkenlerini ayarlamak için aşağıdaki yönergeleri kullanın.

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx FORM_RECOGNIZER_KEY <replace-with-your-form-recognizer-key>
setx FORM_RECOGNIZER_ENDPOINT <replace-with-your-form-recognizer-endpoint>
```

Ortam değişkenlerini ekledikten sonra konsol penceresini kapatın ve yeniden açın.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export FORM_RECOGNIZER_KEY=<replace-with-your-product-name-key>
export FORM_RECOGNIZER_ENDPOINT=<replace-with-your-product-name-endpoint>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.bashrc` çalıştırın.

#### <a name="macos"></a>[macOS](#tab/unix)

Hesabınızı düzenleyin `.bash_profile` ve ortam değişkenini ekleyin:

```bash
export FORM_RECOGNIZER_KEY=<replace-with-your-product-name-key>
export FORM_RECOGNIZER_ENDPOINT=<replace-with-your-product-name-endpoint>
```

Ortam değişkenlerini ekledikten sonra, `source .bash_profile` değişikliklerin etkili olması için konsol penceresinizden çalıştırın.
***
