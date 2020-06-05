---
title: dosya dahil etme
description: dosya dahil etme
services: cognitive-services
author: roy-har
manager: diberry
ms.service: cognitive-services
ms.date: 06/03/2020
ms.subservice: language-understanding
ms.topic: include
ms.custom: include file
ms.author: roy-har
ms.openlocfilehash: 8e67a6d0c98a3839922a79e9b452465087da1b69
ms.sourcegitcommit: 8e5b4e2207daee21a60e6581528401a96bfd3184
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84418076"
---
1. Dosya için GitHub sayfasını açmak için [pizza-App-for-Luu-V6. JSON](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/apps/pizza-app-for-luis-v6.json) ' ı seçin `pizza-app-for-luis.json` .
1. **Ham** düğmesine sağ tıklayın veya uzun dokunduktan sonra **Bağlantıyı farklı kaydet** ' i seçerek bilgisayarınıza kaydedin `pizza-app-for-luis.json` .
1. [Lui portalında](https://www.luis.ai)oturum açın.
1. [Uygulamalarım](https://www.luis.ai/applications)' ı seçin.
1. **Uygulamalarım** sayfasında, **konuşma için + yeni uygulama**' yı seçin.
1. **JSON olarak Içeri aktar**' ı seçin.
1. **Yeni uygulama al** Iletişim kutusunda **Dosya Seç** düğmesini seçin.
1. `pizza-app-for-luis.json`İndirdiğiniz dosyayı seçin ve **Aç**' ı seçin.
1. **Yeni uygulama Içeri aktarma** Iletişim kutusu **adı** alanına, pizza uygulamanız için bir ad girin ve ardından **bitti** düğmesini seçin.

Uygulama içeri aktarılacak.

**Etkin BIR Luo uygulaması oluşturma**iletişim kutusunu görürseniz, iletişim kutusunu kapatın.

## <a name="train-and-publish-the-pizza-app"></a>Pizza uygulamasını eğitme ve yayımlama

Pizza uygulamasındaki amaçları içeren bir liste ile **amaçlar** sayfasını görmeniz gerekir.

[!INCLUDE [How to train](howto-train.md)]

[!INCLUDE [How to publish](howto-publish.md)]

Pizza uygulamanız artık kullanıma hazırdır.

## <a name="record-the-app-id-prediction-key-and-prediction-endpoint-of-your-pizza-app"></a>Pizza uygulamanızın uygulama KIMLIĞI, tahmin anahtarı ve tahmin uç noktasını kaydetme

Yeni pizza uygulamanızı kullanmak için, pizza uygulamanızın uygulama KIMLIĞI, tahmin anahtarı ve tahmin uç noktasına ihtiyacınız olacaktır.

Bu değerleri bulmak için:

1. **Amaçlar** sayfasından **Yönet**' i seçin.
1. **Uygulama ayarları** sayfasında, **uygulama kimliğini**kaydedin.
1. **Azure kaynakları**' nı seçin.
1. **Azure kaynakları** sayfasından **birincil anahtarı**kaydedin. Bu değer, tahmin anahtarınıza göre yapılır.
1. **Uç nokta URL 'sini**kaydedin. Bu değer, tahmin uç noktanıza ait.
