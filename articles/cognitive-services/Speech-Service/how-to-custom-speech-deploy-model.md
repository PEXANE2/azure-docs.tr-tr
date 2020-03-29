---
title: Özel Konuşma için bir model dağıtma - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Bu belgede, Özel Konuşma portalını kullanarak bir uç nokta oluşturmayı ve dağıtmayı öğreneceksiniz.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: erhopf
ms.openlocfilehash: e41653e4644b09cc357510223e5ee86c3806ae2c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74806122"
---
# <a name="deploy-a-custom-model"></a>Özel bir modeli dağıtma

Verileri yükledikten ve inceledikten, doğruluğu değerlendirdikten ve özel bir model eğittikten sonra, uygulamalarınız, araçlarınız ve ürünleriniz için kullanmak üzere özel bir bitiş noktası dağıtabilirsiniz. Bu belgede, [Özel Konuşma portalını](https://speech.microsoft.com/customspeech)kullanarak bir uç nokta oluşturmayı ve dağıtmayı öğreneceksiniz.

## <a name="create-a-custom-endpoint"></a>Özel bir bitiş noktası oluşturma

Yeni bir özel bitiş noktası oluşturmak için [Özel Konuşma portalında](https://speech.microsoft.com/customspeech) oturum açın ve sayfanın üst kısmındaki Özel Konuşma menüsünden **Dağıtım'ı** seçin. Bu ilk çalışmanızsa, tabloda listelenen uç nokta olmadığını fark esiniz. Bir bitiş noktası oluşturduktan sonra, dağıtılan her bitiş noktasını izlemek için bu sayfayı kullanırsınız.

Ardından, **bitiş noktası ekle'yi** seçin ve özel bitiş noktanız için bir **Ad** ve **Açıklama** girin. Ardından, bu bitiş noktasıyla ilişkilendirmek istediğiniz özel modeli seçin. Bu sayfadan, günlüğe kaydetmeyi de etkinleştirebilirsiniz. Günlüğe kaydetme, uç nokta trafiğini izlemenize olanak tanır. Devre dışı bırakılırsa, trafik depolanmayacak.

![Bir model nasıl dağıtılır?](./media/custom-speech/custom-speech-deploy-model.png)

> [!NOTE]
> Kullanım koşullarını ve fiyatlandırma ayrıntılarını kabul etmeyi unutmayın.

Ardından **Oluştur'u**seçin. Bu eylem sizi **Dağıtım** sayfasına döndürür. Tablo artık özel bitiş noktanıza karşılık gelen bir giriş içerir. Bitiş noktasının durumu geçerli durumunu gösterir. Özel modellerinizi kullanarak yeni bir bitiş noktasının anlık olarak ayarlanınması 30 dakika kadar sürebilir. Dağıtım Durumu Tamamla'da **Complete**değiştiğinde, bitiş noktası kullanıma hazır olur.

Bitiş noktanız dağıtıldıktan sonra, bitiş noktası adı bağlantı olarak görünür. Uç nokta tuşu, uç nokta URL'si ve örnek kod gibi uç noktanıza özgü bilgileri görüntülemek için bağlantıyı tıklatın.

## <a name="view-logging-data"></a>Günlük verilerini görüntüleme

Günlüğe kaydetme verileri **Endpoint > Ayrıntılar**altında indirilebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma SDK](speech-sdk.md) ile özel bitiş noktanızı kullanın

## <a name="additional-resources"></a>Ek kaynaklar

* [Verilerinizi hazırlayın ve test edin](how-to-custom-speech-test-data.md)
* [Verilerinizi inceleyin](how-to-custom-speech-inspect-data.md)
* [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)
* [Modelinizi eğitme](how-to-custom-speech-train-model.md)
* [Modelinizi dağıtın](how-to-custom-speech-deploy-model.md)
