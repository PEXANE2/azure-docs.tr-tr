---
title: Swagger belgeleri - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Swagger belgeleri, bir dizi programlama dili için Otomatik SDK oluşturmak için kullanılabilir. Hizmetimizdeki tüm operasyonlar Swagger tarafından desteklenir
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: reference
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: fcc43caf895dadfaf832a47c3254f9b828bcb71a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77430814"
---
# <a name="swagger-documentation"></a>Swagger belgeleri

Konuşma hizmeti, veri almak, modeller oluşturmak, model doğruluğunu test etmek, özel uç noktalar oluşturmak, toplu transkripsiyonları sıraya almak ve abonelikleri yönetmek için kullanılan bir avuç REST API'si ile etkileşim kurmak için bir Swagger belirtimi sunar. Özel Konuşma portalı aracılığıyla kullanılabilen işlemlerin çoğu bu API'ler kullanılarak programlanabilir.

> [!NOTE]
> Hem Konuşma-Metin hem de Metinden Konuşmaya işlemleri, Swagger belirtiminde belgelenen REST API'leri olarak desteklenir.

## <a name="generating-code-from-the-swagger-specification"></a>Swagger belirtiminden kod oluşturma

[Swagger belirtimi,](https://cris.ai/swagger/ui/index) çeşitli yollar için hızlı bir şekilde test etmenize olanak tanıyan seçeneklere sahiptir. Ancak, bazen tüm yollar için kod oluşturmak, gelecekteki çözümleri temel alabileceğiniz tek bir çağrı kitaplığı oluşturmak istenir. Python kitaplığı oluşturmak için işleme bir göz atalım.

Swagger'ı Konuşma hizmeti aboneliğinizle aynı bölgeye ayarlamanız gerekir. Konuşma hizmeti kaynağınız altındaki Azure portalında bölgenizi onaylayabilirsiniz. Desteklenen bölgelerin tam listesi için [bkz.](regions.md)

1. Şuraya gidin: https://editor.swagger.io
2. **Dosya'yı**tıklatın, sonra **İçe Aktar'ı** tıklatın
3. Konuşma hizmeti aboneliğiniz için bölge de dahil olmak üzere havalı URL'yi girin`https://<your-region>.cris.ai/docs/v2.0/swagger`
4. **İstemciyi Oluştur'u** tıklatın ve Python'u seçin
5. İstemci kitaplığını kaydetme

[GitHub'daki Konuşma hizmeti örnekleriyle](https://aka.ms/csspeech/samples)oluşturduğunuz Python kitaplığını kullanabilirsiniz.

## <a name="reference-docs"></a>Referans dokümanları

* [REST (Swagger): Toplu transkripsiyon ve özelleştirme](https://westus.cris.ai/swagger/ui/index)
* [REST API: Konuşma-metin](rest-speech-to-text.md)
* [REST API: Metinden konuşmaya](rest-text-to-speech.md)

## <a name="next-steps"></a>Sonraki adımlar

* [GitHub'da konuşma hizmeti örnekleri.](https://aka.ms/csspeech/samples)
* [Konuşma hizmeti abonelik anahtarını ücretsiz alın](get-started.md)
