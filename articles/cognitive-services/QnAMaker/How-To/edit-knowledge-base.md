---
title: Bilgi Bankası Soru-Cevap Oluşturma Düzenleme-
description: Soru-Cevap Oluşturma, kullanımı kolay bir düzen deneyimi sağlayarak bilgi Bankalarınızın içeriğini yönetmenizi sağlar.
ms.topic: conceptual
ms.date: 05/19/2020
ms.openlocfilehash: 66adf9e1fe227f11514276147de32e8691db7fab
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83993584"
---
# <a name="edit-qna-pairs-in-your-knowledge-base"></a>Bilgi bankasındaki QnA çiftlerini düzenleme

Soru-Cevap Oluşturma, kullanımı kolay bir düzen deneyimi sağlayarak bilgi Bankalarınızın içeriğini yönetmenizi sağlar.

Bir dosya veya URL gibi veya bir düzenleme kaynağı olarak eklenen bir veri kaynağından QnA çiftleri eklenir. Bir düzenleme kaynağı QNA çiftinin QnA portalında el ile eklendiğini gösterir. Tüm QnA çiftleri düzenlenmek üzere kullanılabilir.

<a name="add-an-editorial-qna-set"></a>

## <a name="add-an-editorial-qna-pair"></a>Bir düzenleme QnA çifti ekleyin

1. Soru- [cevap portalı](https://www.qnamaker.ai/)' nda oturum açın ve ardından QNA çiftini eklemek için Bilgi Bankası ' nı seçin.
1. Bilgi Bankası 'nın **düzenleme** sayfasında, yeni bir QNA çifti eklemek Için **QNA Pair Ekle** ' yi seçin.

    > [!div class="mx-imgBorder"]
    > ![QnA çifti Ekle](../media/qnamaker-how-to-edit-kb/add-qnapair.png)

1. Yeni QnA Pair satırına, gerekli soru ve yanıt alanlarını ekleyin. Diğer alanlar isteğe bağlıdır. Tüm alanlar herhangi bir zamanda değiştirilebilir.

1. İsteğe bağlı olarak **[Alternatif](../Quickstarts/add-question-metadata-portal.md#add-additional-alternatively-phrased-questions)** bir ifade ekleyin. Alternatif ifade, özgün sorudan önemli ölçüde farklı olan ancak aynı yanıtı sağlamaları gereken herhangi bir soru biçimidir.

    Bilgi tabanınız yayımlandığında ve [etkin öğrenme](use-active-learning.md) özelliği açıksa, soru-cevap oluşturma kabul etmeniz için alternatif ifade seçeneklerini toplar. Tahmin doğruluğunu artırmak için bu seçimler seçilidir.

1. İsteğe bağlı olarak **[meta veriler](../Quickstarts/add-question-metadata-portal.md#add-metadata-to-filter-the-answers)** ekleyin. Meta verileri görüntülemek için bağlam menüsünde **seçenekleri görüntüle** ' yi seçin. Meta veriler, bir sohbet bot gibi istemci uygulamasının sağladığı yanıtlara filtreler sağlar.

1. İsteğe bağlı olarak, **[izleme istemleri](multiturn-conversation.md)** ekleyin. İzleme istemleri, istemci uygulamasına kullanıcıya sunmak için ek konuşma yolları sağlar.

1. Yeni QnA çiftiyle birlikte tahminleri görmek için **Kaydet ve eğitme '** yi seçin.

## <a name="rich-text-editing-for-answer"></a>Yanıt için zengin metin düzenlemesi

Yanıt metninizi zengin metin düzenlemesi, basit bir araç çubuğundan Markin stili sağlar.

1. Yanıt için metin alanını seçin, zengin metin düzenleyici araç çubuğu QnA çifti satırında görüntülenir.

    > [!div class="mx-imgBorder"]
    > ![Bir QnA çifti satırının sorusu ve yanıtı ile zengin metin düzenleyicisinin ekran görüntüsü.](../media/qnamaker-how-to-edit-kb/rich-text-control-qna-pair-row.png)

    Kullanıcı onu bir bot 'tan görebildiğinden, yanıtdaki tüm metinler doğru şekilde görüntülenir.

1. Metni düzenleyin. Zengin metin düzenlemesi araç çubuğundan biçimlendirme özellikleri ' ni seçin veya markı sözdizimine geçiş yapmak için geçiş özelliğini kullanın.

    > [!div class="mx-imgBorder"]
    > ![Metin yazmak ve biçimlendirmek ve markaşağı olarak kaydetmek için zengin metin düzenleyici 'yi kullanın.](../media/qnamaker-how-to-edit-kb/rich-text-display-image.png)

    |Zengin metin düzenleyici özellikleri|Klavye kısayolu|
    |--|--|
    |Zengin metin düzenleyici ve markaşağı arasında geçiş yapın. `</>`|CTRL+M|
    |Kalın. **B**|MRK + LB|
    |İtalik, italik ı ile gösterilir ** _I_**|CTRL + ı|
    |Sırasız liste||
    |Sıralı liste||
    |Paragraf stili||
    |Görüntü-genel bir URL 'den kullanılabilen bir resim ekleyin.|CTRL + G|
    |Genel olarak kullanılabilir URL 'ye bağlantı ekleyin.|CTRL + K|
    |İfade-bir ifade seçiminden ekleme.|CTRL + E|
    |Gelişmiş menü-geri al|CTRL+Z|
    |Gelişmiş Menü-Yinele|CTRL + Y|

1. Zengin metin araç çubuğundaki görüntü simgesini kullanarak yanıta bir resim ekleyin. Yerinde düzenleyicide, genel olarak erişilebilen görüntü URL 'SI ve görüntü için alternatif metin gerekir.


    > [!div class="mx-imgBorder"]
    > ![Zengin metin düzenleyici 'yi kullanarak herkese açık bir şekilde erişilebilir bir görüntü ve onun ALTERNATIF metnini ekleyin.](../media/qnamaker-how-to-edit-kb/add-image-url-alternate-text.png)

1. Yanıt içindeki metni seçip araç çubuğunda bağlantı simgesini seçerek veya araç çubuğundaki bağlantı simgesini seçerek yeni metin ve URL 'yi girerek URL 'ye bir bağlantı ekleyin.

    > [!div class="mx-imgBorder"]
    > ![Zengin metin düzenleyici 'yi kullanarak herkese açık bir şekilde erişilebilir bir görüntü ve onun ALTERNATIF metnini ekleyin.](../media/qnamaker-how-to-edit-kb/add-link-to-answer-rich-text-editor.png)

## <a name="edit-a-qna-pair"></a>Bir QnA çiftini düzenleme

Özgün veri kaynağından bağımsız olarak, herhangi bir QnA çiftiyle herhangi bir alan düzenlenebilir. Bazı alanlar, içerik araç çubuğunda bulunan geçerli **Görünüm seçenekleri** ayarlarınız nedeniyle görünür olmayabilir.

## <a name="delete-a-qna-pair"></a>Bir QnA çiftini silme

Bir QnA 'yi silmek için QnA satırının en sağındaki **Sil** simgesine tıklayın. Bu kalıcı bir işlemdir. Geri alınamaz. Çiftleri silinmeden önce **Yayımla** sayfasından KB 'nizi dışarı aktarmayı düşünün.

![QnA çiftini Sil](../media/qnamaker-how-to-edit-kb/delete-qnapair.png)

## <a name="find-the-qna-pair-id"></a>QnA çiftinin KIMLIĞINI bulun

QnA çiftinin KIMLIĞINI bulmanız gerekiyorsa, bunu iki yerde bulabilirsiniz:

* İlgilendiğiniz QnA çifti satırındaki silme simgesine gelin. Vurgulu metin, QnA çiftinin KIMLIĞINI içerir.
* Bilgi Bankası 'nı dışarı aktarın. Bilgi Bankası 'ndaki her QnA çifti QnA çiftinin KIMLIĞINI içerir.

## <a name="add-alternate-questions"></a>Alternatif sorular ekleyin

Bir kullanıcı sorgusuyla eşleşme olasılığını artırmak için mevcut bir QnA çiftine alternatif sorular ekleyin.

![Alternatif sorular ekleyin](../media/qnamaker-how-to-edit-kb/add-alternate-question.png)

## <a name="linking-qna-pairs"></a>QnA çiftlerini bağlama

Soru-cevap çiftlerini bağlama, [izleme istemleriyle](multiturn-conversation.md)birlikte sağlanır. Bu, Bilgi Bankası düzeyinde yönetilen QnA çiftleri arasındaki mantıksal bir bağlantıdır. Soru-Cevap Oluşturma portalında izleme istemlerini düzenleyebilirsiniz.

Yanıtın meta verilerinde QnA çiftlerini bağlayamazsınız.

## <a name="add-metadata"></a>Meta veri Ekle

Önce **Görünüm seçeneklerini**belirleyip **meta verileri göster**' i seçerek meta veri çiftleri ekleyin. Bu, meta veri sütununu görüntüler. Sonra, **+** bir meta veri çifti eklemek için imzala ' yı seçin. Bu çift bir anahtar ve bir değer içerir.

## <a name="save-changes-to-the-qna-pairs"></a>Değişiklikleri QnA çiftlerine Kaydet

Değişiklikleri kaybetmemek için düzenleme yaptıktan sonra düzenli olarak **Kaydet ve eğit '** i seçin.

![Meta veri Ekle](../media/qnamaker-how-to-edit-kb/add-metadata.png)

## <a name="when-to-use-rich-text-editing-versus-markdown"></a>Zengin metin düzenlemesi ve markaşağı için ne zaman kullanılacağı

Yanıtları [zengin metin düzenlemesi](#add-an-editorial-qna-set) , yazar olarak, metni hızlıca seçip biçimlendirmek için bir biçimlendirme araç çubuğu kullanmanıza olanak sağlar.

Bir CI/CD [işlem](../Quickstarts/batch-testing.md)hattının parçası olarak içeri aktarılmak üzere bilgi tabanları oluşturmak için içerik oluşturmanız gerektiğinde [markaşağı](../reference-markdown-format.md) , daha iyi bir araçtır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası üzerinde birlikte çalışma](./collaborate-knowledge-base.md)

* [Soru-Cevap Oluşturma tarafından kullanılan Azure kaynaklarını yönetme](set-up-qnamaker-service-azure.md)
