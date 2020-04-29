---
title: Bilgi Bankası Soru-Cevap Oluşturma test etme-
description: Soru-Cevap Oluşturma bilgi tabanınızı test etmek, döndürülen yanıtların doğruluğunu artırmak için yinelemeli bir işlemin önemli bir parçasıdır. Bilgi bankasını, düzenleme yapmanıza olanak tanıyan gelişmiş bir sohbet arabiriminden test edebilirsiniz.
ms.topic: conceptual
ms.date: 03/05/2020
ms.openlocfilehash: da4988ced0b077952ce64e6227d16e58d40ae329
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "78927268"
---
# <a name="test-your-knowledge-base-in-qna-maker"></a>Bilgi tabanınızı Soru-Cevap Oluşturma test edin

Soru-Cevap Oluşturma bilgi tabanınızı test etmek, döndürülen yanıtların doğruluğunu artırmak için yinelemeli bir işlemin önemli bir parçasıdır. Bilgi bankasını, düzenleme yapmanıza olanak tanıyan gelişmiş bir sohbet arabiriminden test edebilirsiniz.

## <a name="interactively-test-in-qna-maker-portal"></a>Soru-Cevap Oluşturma portalında etkileşimli test

1. Bilgi **tabanlarım** sayfasında adını seçerek bilgi tabanınızı erişin.
1. Test slayt paneli paneline erişmek için uygulamanızın üst panelinde **Test** ' i seçin.
1. Metin kutusuna bir sorgu girin ve ENTER ' u seçin.
1. Bilgi bankasından en iyi eşleşen yanıt yanıt olarak döndürülür.

### <a name="clear-test-panel"></a>Test panelini temizle

Tüm girilen test sorgularını ve sonuçlarını test konsolundan temizlemek için, test bölmesinin sol üst köşesinden **başla** ' yı seçin.

### <a name="close-test-panel"></a>Test panelini kapat

Test panelini kapatmak için, **Test** düğmesini yeniden seçin. Test paneli açıkken Bilgi Bankası içeriğini düzenleyemezsiniz.

### <a name="inspect-score"></a>Puanı İncele

Inceleme panelinde test sonucunun ayrıntılarını inceleyebilirsiniz.

1.  Test slayt paneli açık olduğunda, bu yanıt hakkında daha fazla ayrıntı için **İnceleme** ' yi seçin.

    ![Yanıtları İncele](../media/qnamaker-how-to-test-kb/inspect.png)

2.  Inceleme paneli görüntülenir. Panel, en iyi Puanlama amacını ve tanımlanan tüm varlıkları içerir. Panel, seçili utterlik 'in sonucunu gösterir.

### <a name="correct-the-top-scoring-answer"></a>En üst Puanlama yanıtını düzeltin

En üst Puanlama yanıtı yanlışsa, listeden doğru yanıtı seçin ve **Kaydet ve eğit**' i seçin.

![En üst Puanlama yanıtını düzeltin](../media/qnamaker-how-to-test-kb/choose-answer.png)

### <a name="add-alternate-questions"></a>Alternatif sorular ekleyin

Belirli bir yanıta bir sorunun alternatif formlarını ekleyebilirsiniz. Diğer yanıtları metin kutusuna yazın ve ENTER ' a tıklayarak ekleyin. Güncelleştirmeleri depolamak için **Kaydet ve eğitme '** yi seçin.

![Alternatif sorular ekleyin](../media/qnamaker-how-to-test-kb/add-alternate-question.png)

### <a name="add-a-new-answer"></a>Yeni bir yanıt ekleyin

Eşlenen mevcut yanıtlardan herhangi biri hatalıysa veya Yanıt Bilgi Bankası 'nda yoksa (KB 'de iyi eşleşme bulunamadıysanız) yeni bir yanıt ekleyebilirsiniz.

Yanıtlar listesinin en altında, yeni bir yanıt girmek için metin kutusunu kullanın ve eklemek için ENTER tuşuna basın.

Bu yanıtı kalıcı hale getirmek için **Kaydet ve eğitme** seçeneğini belirleyin. Bilgi tabanıza yeni bir soru-cevap çifti eklenmiştir.

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

Toplu işlem testi, Batch test aracı ile sağlanır. Bu araç, karşıdan yüklenmek üzere veya [C# kaynak kodu](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/batchtesting)olarak bir [ZIP yürütülebilir dosyası](https://aka.ms/qnamakerbatchtestingtool) olarak kullanılabilir.

[Araçla Ilgili başvuru belgeleri](../reference-tsv-format-batch-testing.md) şunları içerir:

* aracın komut satırı örneği
* TSV giriş ve çıkışdosyası dosyaları için biçim

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası yayımlama](./publish-knowledge-base.md)
