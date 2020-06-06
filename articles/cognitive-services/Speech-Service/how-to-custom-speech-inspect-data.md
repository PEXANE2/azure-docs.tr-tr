---
title: Özel Konuşma Tanıma konuşma hizmeti için veri kalitesini İnceleme
titleSuffix: Azure Cognitive Services
description: Özel Konuşma Tanıma, ses verilerini karşılık gelen tanıma sonucuyla karşılaştırarak bir modelin tanınma kalitesini görsel olarak incelemenize olanak sağlayan araçlar sağlar. Karşıya yüklenen sesi kayıttan yürütebilir ve belirtilen tanıma sonucunun doğru olup olmadığını belirleyebilirsiniz.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: erhopf
ms.openlocfilehash: 7a8fec876556d943d29756a38ffc27ae8095e3c4
ms.sourcegitcommit: 813f7126ed140a0dff7658553a80b266249d302f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84466247"
---
# <a name="inspect-custom-speech-data"></a>Özel Konuşma verilerini inceleme

> [!NOTE]
> Bu sayfa, [özel konuşma tanıma için test verilerini hazırlama](how-to-custom-speech-test-data.md) okuduğunuzu ve İnceleme için bir veri kümesi yüklediğinizi varsayar.

Özel Konuşma Tanıma, ses verilerini karşılık gelen tanıma sonucuyla karşılaştırarak bir modelin tanınma kalitesini görsel olarak incelemenize olanak sağlayan araçlar sağlar. [Özel konuşma tanıma portalından](https://speech.microsoft.com/customspeech)karşıya yüklenen sesi kayıttan yürütebilir ve belirtilen tanınma sonucunun doğru olup olmadığını belirleyebilirsiniz. Bu araç, Microsoft 'un temel konuşma konuşmadan metin modelinin kalitesini incelemenize, eğitilen özel bir modeli incelemenize veya iki model ile döküm karşılaştırmanıza yardımcı olur.

Bu belgede, daha önce karşıya yüklediğiniz eğitim verilerini kullanarak bir modelin kalitesini görsel olarak incelemeyi öğreneceksiniz.

Bu sayfada, Microsoft 'un temel konuşma konuşmadan metin modeli ve/veya eğitilen özel bir modelin kalitesini görsel olarak incelemeyi öğreneceksiniz. Test için **veri** sekmesine yüklediğiniz verileri kullanacaksınız.

## <a name="create-a-test"></a>Test oluşturma

Bir test oluşturmak için aşağıdaki yönergeleri izleyin:

1. [Özel konuşma tanıma portalında](https://speech.microsoft.com/customspeech)oturum açın.
2. **> konuşmadan metne git Özel Konuşma Tanıma > testi**yapın.
3. **Test Ekle**' ye tıklayın.
4. **İnceleme kalitesi (yalnızca ses verileri)** seçeneğini belirleyin. Teste bir ad, açıklama verin ve ses veri kümenizi seçin.
5. Test etmek istediğiniz en fazla iki model seçin.
6. **Oluştur**' a tıklayın.

Bir test başarıyla oluşturulduktan sonra, bir modelin belirttiğiniz ses veri kümesini nasıl oluşturduğunu veya iki modelden gelen sonuçları yan yana nasıl karşılaştırabileceğiniz hakkında bilgi alabilirsiniz.

[!INCLUDE [service-pricing-advisory](includes/service-pricing-advisory.md)]

## <a name="side-by-side-model-comparisons"></a>Yan yana model karşılaştırmaları

Test durumu _başarılı_olduğunda testin ayrıntılarını görmek için test öğesi adına tıklayın. Bu ayrıntı sayfasında, veri kümenizdeki tüm bildirimler listelenir ve karşılaştırdığınız iki modelin tanınma sonuçları gösterilir.

Yan yana karşılaştırmayı incelemeye yardımcı olması için ekleme, silme ve değiştirme gibi çeşitli hata türlerini değiştirebilirsiniz. Her sütunda ses dinlemek ve tanıma sonuçlarını karşılaştırmak yoluyla (insan tarafından etiketlenmiş bir konum ve iki konuşmadan metne ait sonuçları gösterir), hangi modelin ihtiyaçlarınıza uygun olduğunu ve iyileştirmelerin gerekli olduğunu belirtebilirsiniz.

Yan yana model testi, bir uygulama için en uygun konuşma tanıma modelini doğrulamak için kullanışlıdır. Bir doğruluğun bir amaç ölçüsü için, daha fazla ses kullanılmasını gerektiren [doğruluğu değerlendir](how-to-custom-speech-evaluate-data.md)bölümünde bulunan yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)
- [Modelinizi eğitme](how-to-custom-speech-train-model.md)
- [Modelinizi geliştirme](how-to-custom-speech-improve-accuracy.md)
- [Modelinizi dağıtın](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Ek kaynaklar

- [Özel Konuşma Tanıma için test verilerini hazırlama](how-to-custom-speech-test-data.md)
