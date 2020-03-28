---
title: 'Quickstart: LUIS tuşu oluşturma'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, luis uygulaması oluşturmayı ve bir anahtar almayı öğreneceksiniz.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: tutorial
ms.date: 02/10/2020
ms.author: dapine
ms.openlocfilehash: 5b1a5ac4867379457d161d07f4f4f2fc2d8ee6c3
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77119587"
---
# <a name="quickstart-getting-a-luis-endpoint-key"></a>Quickstart: LUIS uç noktası anahtarı alma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce aşağıdaki öğelere sahip olduğundan emin olun:

* LUIS hesabı. [LUIS portalından](https://www.luis.ai/home) ücretsiz bir hesap alabilirsiniz.

## <a name="luis-and-speech"></a>LUIS ve konuşma

LUIS, konuşmadaki amaçları tanımak için Konuşma hizmetiyle tümleştirilir. Konuşma hizmeti aboneliğine ihtiyacınız yoktur; LUIS yeterlidir.

LUIS üç tür anahtar kullanır:

|Anahtar türü|Amaç|
|--------|-------|
|Yazma|LUIS uygulamalarını programlı olarak oluşturmanıza ve değiştirmenize olanak tanır|
|Başlangıç|LUIS uygulamanızı yalnızca metin kullanarak test etmenizi sağlar|
|Uç Nokta |Belirli bir LUIS uygulamasına erişimi yetkilendirme|

Bu öğretici için uç nokta anahtar türüne ihtiyacınız var. Öğretici, [önceden oluşturulmuş Ev otomasyonu uygulamasını](https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app) hızlı başlat'ı kullanarak oluşturabileceğiniz örnek Ev Otomasyonu LUIS uygulamasını kullanır. Kendi LUIS uygulamanızı oluşturduysanız, bunun yerine kullanabilirsiniz.

Bir LUIS uygulaması oluşturduğunuzda, LUIS otomatik olarak bir başlangıç anahtarı oluşturur, böylece uygulamayı metin sorgularını kullanarak test edebilirsiniz. Bu anahtar Konuşma hizmeti tümleştirmesini etkinleştirmez ve bu öğreticiyle çalışmaz. Azure panosunda bir LUIS kaynağı oluşturun ve LUIS uygulamasına atayın. Bu öğretici için ücretsiz abonelik katmanını kullanabilirsiniz.

Azure panosunda LUIS kaynağını oluşturduktan [sonra, LUIS portalına](https://www.luis.ai/home)giriş yapın, **Uygulamalarım** sayfasında uygulamanızı seçin ve ardından uygulamanın **Yönet** sayfasına geçin. Son olarak, kenar çubuğundaki **Anahtarlar ve Uç Noktaları'nı** seçin.

![LUIS portalı anahtarları ve uç nokta ayarları](~/articles/cognitive-services/Speech-Service/media/sdk/luis-keys-endpoints-page.png)

**Anahtarlar ve Bitiş Noktası ayarları** sayfasında:

1. Kaynaklar ve **Anahtarlar** bölümüne gidin ve **kaynak ata'yı**seçin.
1. Uygulama iletişim **kutunuza bir anahtar atarken** aşağıdaki değişiklikleri yapın:

   * **Kiracı**altında, **Microsoft'u**seçin.
   * **Abonelik Adı**altında, kullanmak istediğiniz LUIS kaynağını içeren Azure aboneliğini seçin.
   * **Key'in**altında, uygulamayla birlikte kullanmak istediğiniz LUIS kaynağını seçin.

   Kısa süre içinde yeni abonelik sayfanın altındaki tabloda görüntülenir.

1. Panoya kopyalamak için anahtarın yanındaki simgeyi seçin. (İstediğiniz anahtarı kullanabilirsiniz.)

![LUIS app abonelik anahtarları](~/articles/cognitive-services/Speech-Service/media/sdk/luis-keys-assigned.png)


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Niyetleri Tanıyın](~/articles/cognitive-services/Speech-Service/quickstarts/intent-recognition.md)
