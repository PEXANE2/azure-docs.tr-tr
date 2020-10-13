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
ms.openlocfilehash: 6bb50e427fa85a170c5ad23a63d67c01e898a17d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91665693"
---
# <a name="swagger-documentation"></a>Swagger belgeleri

Konuşma hizmeti, verileri içeri aktarmak, modeller oluşturmak, model doğruluğunu test etmek, Özel uç noktalar oluşturmak, toplu iş dökümlerini kuyruğa almak ve abonelikleri yönetmek için kullanılan el ile yapılan bir REST API 'si ile etkileşimde bulunmak için bir Swagger belirtimi sunar. Özel Konuşma Tanıma portalı aracılığıyla kullanılabilen çoğu işlem, bu API 'Ler kullanılarak programlı bir şekilde tamamlanabilir.

> [!NOTE]
> Hem konuşmadan metne hem de metinden konuşmaya işlemleri, ' de Swagger belirtiminde belgelenen REST API 'Leri olarak desteklenir.

## <a name="generating-code-from-the-swagger-specification"></a>Swagger belirtiminden kod oluşturuluyor

[Swagger belirtiminde](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0) , çeşitli yollar için hızlı test yapmanıza olanak sağlayan seçenekler vardır. Ancak, bazı durumlarda, gelecekteki çözümleri temel alan tek bir çağrı kitaplığı oluşturarak tüm yollar için kod oluşturulması istenebilir. Bir Python kitaplığı oluşturma işlemine göz atalım.

Swagger 'yi konuşma hizmeti aboneliğinizle aynı bölgeye ayarlamanız gerekir. Azure portal, konuşma hizmeti kaynağınızın altındaki bölgenizi doğrulayabilirsiniz. Desteklenen bölgelerin tüm listesi için bkz. [bölgeler](regions.md).

1. Bir tarayıcıda, bölgeniz için Swagger belirtimine gidin:  
       `https://<your-region>.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0`
1. Bu sayfada, **API tanımı**' na tıklayın ve **Swagger**' ye tıklayın. Görüntülenen sayfanın URL 'sini kopyalayın.
1. Yeni bir tarayıcıda şuraya gidin https://editor.swagger.io
1. **Dosya**, URL 'Yi **içeri aktar**' a tıklayın, URL 'yi yapıştırın ve **Tamam**' a tıklayın.
1. **Istemci oluştur** ' a tıklayın ve **Python**' ı seçin. İstemci kitaplığı bir dosyadaki bilgisayarınıza indirilir `.zip` .
1. İndirmenin her şeyi ayıklayın. `tar -xf`Her şeyi ayıklamak için kullanabilirsiniz.
1. Ayıklanan modülün Python ortamınıza yüklenmesi:  
       `pip install path/to/package/python-client`
1. Yüklü paketin adı `swagger_client` . Yüklemenin işe yaradığını denetleyin:  
       `python -c "import swagger_client"`

[GitHub 'Daki konuşma hizmeti örnekleri](https://aka.ms/csspeech/samples)Ile oluşturduğunuz Python kitaplığını kullanabilirsiniz.

## <a name="reference-docs"></a>Başvuru belgeleri

* [REST (Swagger): toplu Iş dökümü ve özelleştirme](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0)
* [REST API: konuşmayı metne dönüştürme](rest-speech-to-text.md)
* [REST API: metinden konuşmaya](rest-text-to-speech.md)

## <a name="next-steps"></a>Sonraki adımlar

* [GitHub 'Da konuşma hizmeti örnekleri](https://aka.ms/csspeech/samples).
* [Ücretsiz bir konuşma hizmeti abonelik anahtarı alın](overview.md#try-the-speech-service-for-free)
