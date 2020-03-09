---
title: Bilgi Bankası - soru-cevap Oluşturucu test etme
description: Soru-cevap Oluşturucu bankanızı test etme, verilen yanıtları doğruluğunu artırmak için bir süreçtir önemli bir parçasıdır. Bilgi Bankası düzenlemeler yapmak da sağlayan gelişmiş sohbet arabiriminden test edebilirsiniz.
ms.topic: conceptual
ms.date: 03/05/2020
ms.openlocfilehash: da4988ced0b077952ce64e6227d16e58d40ae329
ms.sourcegitcommit: 9cbd5b790299f080a64bab332bb031543c2de160
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2020
ms.locfileid: "78927268"
---
# <a name="test-your-knowledge-base-in-qna-maker"></a>Bilgi tabanınızı Soru-Cevap Oluşturma test edin

Soru-cevap Oluşturucu bankanızı test etme, verilen yanıtları doğruluğunu artırmak için bir süreçtir önemli bir parçasıdır. Bilgi Bankası düzenlemeler yapmak da sağlayan gelişmiş sohbet arabiriminden test edebilirsiniz.

## <a name="interactively-test-in-qna-maker-portal"></a>Soru-Cevap Oluşturma portalında etkileşimli test

1. Bilgi **tabanlarım** sayfasında adını seçerek bilgi tabanınızı erişin.
1. Test slayt paneli paneline erişmek için uygulamanızın üst panelinde **Test** ' i seçin.
1. Metin kutusuna bir sorgu girin ve Enter tuşuna basın.
1. Bilgi Bankası'nden en iyi eşleşen yanıtlama yanıt döndürülür.

### <a name="clear-test-panel"></a>Açık test paneli

Tüm girilen test sorgularını ve sonuçlarını test konsolundan temizlemek için, test bölmesinin sol üst köşesinden **başla** ' yı seçin.

### <a name="close-test-panel"></a>Kapat test paneli

Test panelini kapatmak için, **Test** düğmesini yeniden seçin. Bilgi Bankası içerikleri, Test panel açıkken düzenleyemezsiniz.

### <a name="inspect-score"></a>Puan inceleyin

Inceleme panelinde test sonucunun ayrıntılarını inceleyebilirsiniz.

1.  Test slayt paneli açık olduğunda, bu yanıt hakkında daha fazla ayrıntı için **İnceleme** ' yi seçin.

    ![Yanıtları](../media/qnamaker-how-to-test-kb/inspect.png)

2.  Inceleme paneli görüntülenir. Panelin amacını ve bunun yanı sıra tanımlanan herhangi bir varlık Puanlama üst içerir. Paneli, seçilen utterance sonucunu gösterir.

### <a name="correct-the-top-scoring-answer"></a>Yanıt Puanlama üst düzeltin

En üst Puanlama yanıtı yanlışsa, listeden doğru yanıtı seçin ve **Kaydet ve eğit**' i seçin.

![Yanıt Puanlama üst düzeltin](../media/qnamaker-how-to-test-kb/choose-answer.png)

### <a name="add-alternate-questions"></a>Diğer sorular ekleyin

Belirli bir yanıt soru diğer formlara ekleyebilirsiniz. Diğer tıklayın ve metin kutusu içinde yanıt türü bunları eklemek için enter. Güncelleştirmeleri depolamak için **Kaydet ve eğitme '** yi seçin.

![Diğer sorular ekleyin](../media/qnamaker-how-to-test-kb/add-alternate-question.png)

### <a name="add-a-new-answer"></a>Yeni bir yanıt Ekle

Yanıt Bilgi Bankası (KB iyi eşleşme bulundu) mevcut değil veya eşleştirilmiş olan mevcut yanıtlar yanlış olan, yeni bir yanıt ekleyebilirsiniz.

Yanıtlar listesinin en altında, yeni bir yanıt girmek için metin kutusunu kullanın ve eklemek için ENTER tuşuna basın.

Bu yanıtı kalıcı hale getirmek için **Kaydet ve eğitme** seçeneğini belirleyin. Yeni bir soru-cevap çifti şimdi Bilgi Bankası'na eklendi.

> [!NOTE]
> Bilgi bankasındaki tüm düzenlemeler yalnızca **Kaydet ve eğitme** düğmesine bastığınızda kaydedilir.

### <a name="test-the-published-knowledge-base"></a>Yayınlanan Bilgi Bankası 'nı test etme

Bilgi Bankası 'nın yayımlanmış sürümünü test bölmesinde test edebilirsiniz. KB 'yi yayımladıktan **sonra yayımlanmış KB kutusunu seçin** ve yayımlanan KB 'den sonuçları almak için bir sorgu gönderin.

![Yayımlanan bir KB 'ye karşı test](../media/qnamaker-how-to-test-kb/test-against-published-kb.png)

## <a name="batch-test-with-tool"></a>Araçla Batch testi

Şunları yapmak istediğinizde Batch test aracını kullanın:

* bir soru kümesinin en iyi yanıtını ve Puanını belirleme
* soru kümesi için beklenen yanıtı doğrulama

Adım adım yönergeler için Batch test [öğreticisini](../Quickstarts/batch-testing.md) okuyun.

Toplu işlem testi, Batch test aracı ile sağlanır. Bu araç, karşıdan yüklenmek üzere veya [ C# kaynak kodu](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/batchtesting)olarak [Sıkıştırılmış bir yürütülebilir dosya](https://aka.ms/qnamakerbatchtestingtool) olarak kullanılabilir.

[Araçla Ilgili başvuru belgeleri](../reference-tsv-format-batch-testing.md) şunları içerir:

* aracın komut satırı örneği
* TSV giriş ve çıkışdosyası dosyaları için biçim

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası yayımlama](./publish-knowledge-base.md)
