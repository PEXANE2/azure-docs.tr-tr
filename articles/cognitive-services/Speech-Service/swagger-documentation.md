---
title: Swagger belgeleri-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Swagger belgeleri, birkaç programlama dili için SDK 'Ları otomatik olarak oluşturmak üzere kullanılabilir. Hizmetimizdeki tüm işlemler Swagger tarafından destekleniyor
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: reference
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: dd9ac9654f916653af974e816485630423466ae5
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74815432"
---
# <a name="swagger-documentation"></a>Swagger belgeleri

Konuşma hizmeti, verileri içeri aktarmak, modeller oluşturmak, model doğruluğunu test etmek, Özel uç noktalar oluşturmak, toplu iş dökümlerini kuyruğa almak ve abonelikleri yönetmek için kullanılan el ile yapılan bir REST API 'si ile etkileşimde bulunmak için bir Swagger belirtimi sunar. Özel Konuşma Tanıma portalı aracılığıyla kullanılabilen çoğu işlem, bu API 'Ler kullanılarak programlı bir şekilde tamamlanabilir.

> [!NOTE]
> Hem konuşmadan metne hem de metinden konuşmaya işlemleri, ' de Swagger belirtiminde belgelenen REST API 'Leri olarak desteklenir.

## <a name="generating-code-from-the-swagger-specification"></a>Swagger belirtiminden kod oluşturuluyor

[Swagger belirtiminde](https://cris.ai/swagger/ui/index) , çeşitli yollar için hızlı test yapmanıza olanak sağlayan seçenekler vardır. Ancak, bazı durumlarda, gelecekteki çözümleri temel alan tek bir çağrı kitaplığı oluşturarak tüm yollar için kod oluşturulması istenebilir. Bir Python kitaplığı oluşturma işlemine göz atalım.

Swagger 'yi konuşma hizmeti aboneliğinizle aynı bölgeye ayarlamanız gerekir. Azure portal, konuşma hizmeti kaynağınızın altındaki bölgenizi doğrulayabilirsiniz. Desteklenen bölgelerin tüm listesi için bkz. [bölgeler](regions.md).

1. Şuraya gidin: https://editor.swagger.io
2. **Dosya**' ya ve ardından **içeri aktar** ' a tıklayın
3. Konuşma hizmeti aboneliğinizin bölgesini içeren Swagger URL 'sini girin `https://<your-region>.cris.ai/docs/v2.0/swagger`
4. **Istemci oluştur** ' a tıklayın ve Python ' ı seçin
5. İstemci kitaplığını Kaydet

[GitHub 'Daki konuşma hizmeti örnekleri](https://aka.ms/csspeech/samples)Ile oluşturduğunuz Python kitaplığını kullanabilirsiniz.

## <a name="reference-docs"></a>Başvuru belgeleri

* [REST (Swagger): toplu Iş dökümü ve özelleştirme](https://westus.cris.ai/swagger/ui/index)
* [REST API: konuşmayı metne dönüştürme](rest-speech-to-text.md)
* [REST API: metinden konuşmaya](rest-text-to-speech.md)

## <a name="next-steps"></a>Sonraki adımlar

* [GitHub 'Da konuşma hizmeti örnekleri](https://aka.ms/csspeech/samples).
* [Ücretsiz bir konuşma hizmeti abonelik anahtarı alın](get-started.md)
