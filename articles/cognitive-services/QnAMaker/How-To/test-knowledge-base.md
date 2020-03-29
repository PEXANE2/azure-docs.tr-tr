---
title: Nasıl bir bilgi tabanı test etmek - QnA Maker
description: QnA Maker bilgi tabanınızı test etmek, döndürülen yanıtların doğruluğunu artırmak için yinelemeli bir işlemin önemli bir parçasıdır. Ayrıca, değişiklikler yapmanızı sağlayan gelişmiş bir sohbet arabirimi aracılığıyla bilgi tabanını test edebilirsiniz.
ms.topic: conceptual
ms.date: 03/05/2020
ms.openlocfilehash: da4988ced0b077952ce64e6227d16e58d40ae329
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78927268"
---
# <a name="test-your-knowledge-base-in-qna-maker"></a>QnA Maker'da bilgi tabanınızı test edin

QnA Maker bilgi tabanınızı test etmek, döndürülen yanıtların doğruluğunu artırmak için yinelemeli bir işlemin önemli bir parçasıdır. Ayrıca, değişiklikler yapmanızı sağlayan gelişmiş bir sohbet arabirimi aracılığıyla bilgi tabanını test edebilirsiniz.

## <a name="interactively-test-in-qna-maker-portal"></a>QnA Maker portalında etkileşimli test

1. **Bilgi tabanım** sayfasında adını seçerek bilgi tabanınıza erişin.
1. Test slayt-out paneline erişmek için, uygulamanızın üst panelinde **Test'i** seçin.
1. Metin kutusuna bir sorgu girin ve Enter'u seçin.
1. Bilgi tabanından en iyi eşleşen yanıt yanıt olarak döndürülür.

### <a name="clear-test-panel"></a>Test panelini temizle

Girilen tüm test sorgularını ve sonuçlarını test konsolundan temizlemek için Test panelinin sol üst köşesinden **Baştan Başla'yı** seçin.

### <a name="close-test-panel"></a>Test panelini kapat

Test panelini kapatmak için **Test** düğmesini yeniden seçin. Test paneli açıkken, Bilgi Bankası içeriğini kaldıramazsınız.

### <a name="inspect-score"></a>Skoru inceleyin

Test sonucunun ayrıntılarını İncele panelinde incelersiniz.

1.  Test slayt-out paneli açıkken, bu yanıt la ilgili daha fazla ayrıntı için **İncele'yi** seçin.

    ![Yanıtları inceleyin](../media/qnamaker-how-to-test-kb/inspect.png)

2.  Teftiş paneli görüntülenir. Panelde en yüksek puanlama niyetinin yanı sıra tanımlanmış tüm varlıklar da yer alıyor. Panel, seçilen söyleyiş sonucunu gösterir.

### <a name="correct-the-top-scoring-answer"></a>En yüksek puanlama yanıtını düzeltme

En yüksek puanlama yanıtı yanlışsa, listeden doğru yanıtı seçin ve **Kaydet ve Eğit'i**seçin.

![En yüksek puanlama yanıtını düzeltme](../media/qnamaker-how-to-test-kb/choose-answer.png)

### <a name="add-alternate-questions"></a>Alternatif sorular ekleme

Belirli bir yanıta alternatif soru biçimleri ekleyebilirsiniz. Metin kutusuna alternatif yanıtları yazın ve eklemek için enter'u tıklatın. Güncelleştirmeleri depolamak için **Kaydet ve Tren'i** seçin.

![Alternatif sorular ekleme](../media/qnamaker-how-to-test-kb/add-alternate-question.png)

### <a name="add-a-new-answer"></a>Yeni bir yanıt ekleme

Eşleşen mevcut yanıtlardan herhangi biri yanlışsa veya yanıt bilgi tabanında yoksa yeni bir yanıt ekleyebilirsiniz (KB'de iyi eşleşme yok).

Yanıt listesinin en altında, yeni bir yanıt girmek için metin kutusunu kullanın ve eklemek için enter tuşuna basın.

Bu yanıtı devam etmek için **Kaydet ve Eğit'i** seçin. Bilgi tabanınıza yeni bir soru-cevap çifti eklendi.

> [!NOTE]
> Bilgi tabanınızdaki tüm yapılan tüzünler yalnızca **Kaydet ve Tren** düğmesine bastığınızda kaydedilir.

### <a name="test-the-published-knowledge-base"></a>Yayınlanan bilgi tabanını test edin

Bilgi tabanının yayımlanmış sürümünü test bölmesinde test edebilirsiniz. KB'yi yayımladıktan **sonra, Yayınlanan KB** kutusunu seçin ve yayınlanan KB'den sonuç almak için bir sorgu gönderin.

![Yayınlanan bir KB'ye karşı test](../media/qnamaker-how-to-test-kb/test-against-published-kb.png)

## <a name="batch-test-with-tool"></a>Araçla toplu test

İstediğince toplu iş test aracını kullanın:

* bir dizi soru için en yüksek yanıtı ve puanı belirleme
* soru kümesi için beklenen yanıtı doğrulamak

Adım adım talimatlar için toplu iş testi [öğreticisini](../Quickstarts/batch-testing.md) okuyun.

Toplu iş testi, toplu test aracıyla sağlanır. Bu araç, karşıdan [yüklenebilir sıkıştırılabilir](https://aka.ms/qnamakerbatchtestingtool) veya [C# kaynak kodu](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/batchtesting)olarak kullanılabilir.

[Araçla ilgili başvuru belgeleri](../reference-tsv-format-batch-testing.md) şunları içerir:

* aracın komut satırı örneği
* TSV giriş ve çıkış dosyaları için biçim

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası yayımlama](./publish-knowledge-base.md)
