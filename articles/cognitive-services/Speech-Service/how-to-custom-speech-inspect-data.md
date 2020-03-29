---
title: Özel Konuşma için veri kalitesini inceleyin - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Özel Konuşma, ses verilerini karşılık gelen tanıma sonucuyla karşılaştırarak bir modelin tanıma kalitesini görsel olarak incelemenize olanak tanıyan araçlar sağlar. Yüklenen sesi geri çalabilir ve sağlanan tanıma sonucunun doğru olup olmadığını belirleyebilirsiniz.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: erhopf
ms.openlocfilehash: 60b415f69800885f37ee53cc4f090098f5ebf1f1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74806088"
---
# <a name="inspect-custom-speech-data"></a>Özel Konuşma verilerini inceleyin

> [!NOTE]
> Bu sayfa, [Özel Konuşma için test verilerini hazırla'yı](how-to-custom-speech-test-data.md) okuduğunuz ve denetim için bir veri kümesi yüklediğinizi varsayar.

Özel Konuşma, ses verilerini karşılık gelen tanıma sonucuyla karşılaştırarak bir modelin tanıma kalitesini görsel olarak incelemenize olanak tanıyan araçlar sağlar. Özel [Konuşma portalından](https://speech.microsoft.com/customspeech)yüklenen sesi oynatabilir ve sağlanan tanıma sonucunun doğru olup olmadığını belirleyebilirsiniz. Bu araç, herhangi bir ses verisini aktarmaya gerek kalmadan Microsoft'un temel konuşma-metin modelinin veya eğitimli özel modelin kalitesini hızlı bir şekilde incelemenize olanak tanır.

Bu belgede, daha önce yüklediğiniz eğitim verilerini kullanarak bir modelin kalitesini görsel olarak nasıl inceleyeceğinizi öğreneceksiniz.

Bu sayfada, Microsoft'un temel konuşma-metin modelinin ve/veya eğittiğiniz özel bir modelin kalitesini görsel olarak nasıl inceleyeceğinizi öğreneceksiniz. Test etmek için **Veri** sekmesine yüklediğiniz verileri kullanırsınız.

## <a name="create-a-test"></a>Test oluşturma

Bir test oluşturmak için aşağıdaki yönergeleri izleyin:

1. Özel Konuşma [portalında](https://speech.microsoft.com/customspeech)oturum açın.
2. Özel **Konuşma > Testi> metinden metne**gidin.
3. **Test Ekle'yi**tıklatın.
4. **Kaliteyi Denetle 'yi seçin (yalnızca ses verileri)**. Teste bir ad, açıklama verin ve ses veri setinizi seçin.
5. Test etmek istediğiniz en fazla iki model seçin.
6. **Oluştur'u**tıklatın.

Bir test başarıyla oluşturulduktan sonra, modelleri yan yana karşılaştırabilirsiniz.

[!INCLUDE [service-pricing-advisory](includes/service-pricing-advisory.md)]

## <a name="side-by-side-model-comparisons"></a>Yan yana model karşılaştırmaları

Test durumu _Başarılı_olduğunda, testin ayrıntılarını görmek için test öğesi adını tıklatın. Bu ayrıntı sayfası, gönderilen veri kümesinden transkripsiyonla birlikte iki modelin tanıma sonuçlarını gösteren veri setinizdeki tüm deyişlenleri listeler.

Yan yana karşılaştırmayı denetlemeye yardımcı olmak için ekleme, silme ve değiştirme gibi çeşitli hata türlerini geçişyapabilirsiniz. Sesi dinleyerek ve her sütundaki tanıma sonuçlarını karşılaştırarak (insan etiketli transkripsiyon ve iki konuşma-metin modelinin sonuçlarını göstererek), hangi modelin ihtiyaçlarınızı karşıladığına ve iyileştirmelerin nerede gerekli olduğuna karar verebilirsiniz.

Bir konuşma tanıma bitiş noktasının kalitesinin bir uygulama için yeterli olup olmadığını doğrulamak için kalite testini incelemek yararlıdır. Yazıyla yazılmış ses gerektiren objektif bir doğruluk ölçüsü için, [Doğruluğu Değerlendir'de](how-to-custom-speech-evaluate-data.md)bulunan yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)
- [Modelinizi eğitme](how-to-custom-speech-train-model.md)
- [Modelinizi dağıtın](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Ek kaynaklar

- [Özel Konuşma için test verilerini hazırlama](how-to-custom-speech-test-data.md)
