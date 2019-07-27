---
title: Bilgi Bankası - soru-cevap Oluşturucu test etme
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu bankanızı test etme, verilen yanıtları doğruluğunu artırmak için bir süreçtir önemli bir parçasıdır. Bilgi Bankası düzenlemeler yapmak da sağlayan gelişmiş sohbet arabiriminden test edebilirsiniz.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 05/08/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 6a512098d5dfda47b7755e24b286aabf83aa7e69
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563074"
---
# <a name="test-your-knowledge-base-interactively-in-qna-maker"></a>Bilgi bankanızı etkileşimli soru-cevap Oluşturucu test edin

Soru-cevap Oluşturucu bankanızı test etme, verilen yanıtları doğruluğunu artırmak için bir süreçtir önemli bir parçasıdır. Bilgi Bankası düzenlemeler yapmak da sağlayan gelişmiş sohbet arabiriminden test edebilirsiniz.

## <a name="test-answer-matching"></a>Test yanıt eşleştirme

1. Bilgi **tabanlarım** sayfasında adını seçerek bilgi tabanınızı erişin.
1. Test slayt paneli paneline erişmek için uygulamanızın üst panelinde **Test** ' i seçin.
1. Metin kutusuna bir sorgu girin ve Enter tuşuna basın.
1. Bilgi Bankası'nden en iyi eşleşen yanıtlama yanıt döndürülür.

## <a name="clear-test-panel"></a>Açık test paneli

Tüm girilen test sorgularını ve sonuçlarını test konsolundan temizlemek için, test bölmesinin sol üst köşesinden **başla** ' yı seçin.

## <a name="close-test-panel"></a>Kapat test paneli

Test panelini kapatmak için, **Test** düğmesini yeniden seçin. Bilgi Bankası içerikleri, Test panel açıkken düzenleyemezsiniz.

## <a name="inspect-score"></a>Puan inceleyin

Inceleme panelinde test sonucunun ayrıntılarını inceleyebilirsiniz.

1.  Test slayt paneli açık olduğunda, bu yanıt hakkında daha fazla ayrıntı için **İnceleme** ' yi seçin.

    ![Yanıtları](../media/qnamaker-how-to-test-kb/inspect.png)

2.  Inceleme paneli görüntülenir. Panelin amacını ve bunun yanı sıra tanımlanan herhangi bir varlık Puanlama üst içerir. Paneli, seçilen utterance sonucunu gösterir.

## <a name="correct-the-top-scoring-answer"></a>Yanıt Puanlama üst düzeltin

Yanıt Puanlama üst yanlışsa, doğru yanıtı listesi ve select seçin **kaydedin ve eğitme**.

![Yanıt Puanlama üst düzeltin](../media/qnamaker-how-to-test-kb/choose-answer.png)

## <a name="add-alternate-questions"></a>Diğer sorular ekleyin

Belirli bir yanıt soru diğer formlara ekleyebilirsiniz. Diğer tıklayın ve metin kutusu içinde yanıt türü bunları eklemek için enter. Seçin **kaydedin ve eğitme** güncelleştirmeleri depolamak için.

![Diğer sorular ekleyin](../media/qnamaker-how-to-test-kb/add-alternate-question.png)

## <a name="add-a-new-answer"></a>Yeni bir yanıt Ekle

Yanıt Bilgi Bankası (KB iyi eşleşme bulundu) mevcut değil veya eşleştirilmiş olan mevcut yanıtlar yanlış olan, yeni bir yanıt ekleyebilirsiniz. 

Yanıtlar listesinin en altında, yeni bir yanıt girmek için metin kutusunu kullanın ve eklemek için ENTER tuşuna basın. 

Seçin **kaydedin ve eğitme** bu yanıt kalıcı hale getirmek için. Yeni bir soru-cevap çifti şimdi Bilgi Bankası'na eklendi. 

> [!NOTE]
> Tuşuna bastığınızda tüm düzenlemeleri bilgi bankanızı yalnızca kaydedileceği **kaydedin ve eğitme** düğmesi.

## <a name="test-the-published-knowledge-base"></a>Yayınlanan Bilgi Bankası 'nı test etme

Bilgi Bankası 'nın yayımlanmış sürümünü test bölmesinde test edebilirsiniz. KB 'yi yayımladıktan sonra yayımlanmış **KB kutusunu seçin** ve yayımlanan KB 'den sonuçları almak için bir sorgu gönderin.

![Yayımlanan bir KB 'ye karşı test](../media/qnamaker-how-to-test-kb/test-against-published-kb.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası yayımlama](./publish-knowledge-base.md)
