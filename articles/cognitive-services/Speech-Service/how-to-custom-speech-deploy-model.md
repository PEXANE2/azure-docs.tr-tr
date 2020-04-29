---
title: Özel Konuşma Tanıma konuşma hizmeti için model dağıtma
titleSuffix: Azure Cognitive Services
description: Bu belgede, Özel Konuşma Tanıma portalını kullanarak bir uç nokta oluşturmayı ve dağıtmayı öğreneceksiniz.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: erhopf
ms.openlocfilehash: 9d48f09fe5d5c736f65b6a76211dd3fec85479ea
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81402190"
---
# <a name="deploy-a-custom-model"></a>Özel bir modeli dağıtma

Verileri karşıya yükledikten ve inceledikten, doğruluğu değerlendirdikten ve özel bir model eğitilene kadar, uygulamalarınız, araçlarınızla ve ürünlerinizle kullanmak üzere özel bir uç nokta dağıtabilirsiniz. Bu belgede, [özel konuşma tanıma portalını](https://speech.microsoft.com/customspeech)kullanarak bir uç nokta oluşturmayı ve dağıtmayı öğreneceksiniz.

## <a name="create-a-custom-endpoint"></a>Özel uç nokta oluşturma

Yeni bir özel uç nokta oluşturmak için [özel konuşma tanıma portalında](https://speech.microsoft.com/customspeech) oturum açın ve sayfanın üst kısmındaki özel konuşma tanıma menüsünden **dağıtım** ' yi seçin. İlk kez çalıştırıldıysanız tabloda listelenen bitiş noktaları olmadığını fark edeceksiniz. Bir uç nokta oluşturduktan sonra, bu sayfayı dağıtılan her bitiş noktasını izlemek için kullanacaksınız.

Sonra, **uç nokta Ekle** ' yi seçin ve özel uç noktanız Için bir **ad** ve **Açıklama** girin. Ardından, bu uç nokta ile ilişkilendirmek istediğiniz özel modeli seçin. Bu sayfadan günlüğe kaydetmeyi de etkinleştirebilirsiniz. Günlüğe kaydetme, uç nokta trafiğini izlemenizi sağlar. Devre dışı bırakılırsa trafik depolanmaz.

![Model dağıtma](./media/custom-speech/custom-speech-deploy-model.png)

> [!NOTE]
> Kullanım koşullarını ve fiyatlandırma ayrıntılarını kabul etmek zorunda kalmazsınız.

Sonra **Oluştur**' u seçin. Bu eylem sizi **dağıtım** sayfasına döndürür. Tablo artık özel uç noktanıza karşılık gelen bir giriş içerir. Uç noktanın durumu geçerli durumunu gösterir. Özel modellerinizi kullanarak yeni bir uç noktanın örneklendirilecek 30 dakikaya kadar zaman alabilir. Dağıtımın durumu **tamamlandığında**, uç nokta kullanıma kullanılır.

Uç noktanız dağıtıldıktan sonra, uç nokta adı bir bağlantı olarak görünür. Uç nokta anahtarı, uç nokta URL 'SI ve örnek kod gibi uç noktanıza özgü bilgileri göstermek için bağlantıya tıklayın.

## <a name="view-logging-data"></a>Günlük verilerini görüntüleme

Günlüğe kaydetme verileri, **uç nokta > ayrıntıları**altında indirilebilir.

## <a name="next-steps"></a>Sonraki adımlar

* Özel modelinizi [buradan](how-to-specify-source-language.md)nasıl kullanacağınızı öğrenin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Verilerinizi hazırlayın ve test edin](how-to-custom-speech-test-data.md)
* [Verilerinizi inceleyin](how-to-custom-speech-inspect-data.md)
* [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)
* [Modelinizi eğitme](how-to-custom-speech-train-model.md)
* [Modelinizi dağıtın](how-to-custom-speech-deploy-model.md)
