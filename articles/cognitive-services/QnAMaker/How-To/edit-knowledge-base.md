---
title: Bilgi Bankası - soru-cevap Oluşturucu Düzenle
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu, kullanımı kolay bir düzenleme deneyimi sunarak bilgi bankanızı içeriğini yönetmenizi sağlar.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b5ee7f60eab0349378767473c9c80f035a65c9a5
ms.sourcegitcommit: 05b36f7e0e4ba1a821bacce53a1e3df7e510c53a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78668773"
---
# <a name="edit-a-knowledge-base-in-qna-maker"></a>Soru-cevap Oluşturucu, Bilgi Bankası Düzenle

Soru-cevap Oluşturucu, kullanımı kolay bir düzenleme deneyimi sunarak bilgi bankanızı içeriğini yönetmenizi sağlar.

<a name="add-datasource"></a>

## <a name="edit-your-knowledge-base-content"></a>Bilgi Bankası içeriğinizi düzenleyin

1.  Üst gezinti çubuğunda **bilgi tabanlarımı** seçin. 

    Oluşturduğunuz veya paylaştığınız tüm hizmetleri, **son değiştirilme** tarihinin azalan sırasına göre görebilirsiniz.

    ![My bilgi Bankalarından](../media/qnamaker-how-to-edit-kb/my-kbs.png)

1. Düzenlemeler yapmak için belirli bir Bilgi Bankası'nı seçin.
 
1. **Ayarlar**' ı seçin. Burada zorunlu alan hizmet adını düzenleyebilirsiniz.
  
    |Hedef|Eylem|
    |--|--|
    |URL ekle|Bilgi bankasını **Yönet-> ' + URL Ekle '** bağlantısına tıklayarak Bilgi Bankası 'na yenı bir SSS içeriği eklemek Için yeni URL 'ler ekleyebilirsiniz.|
    |URL Sil|Var olan URL'ler Sil simgesini seçerek silebilirsiniz, çöp kutusu.|
    |İçeriği Yenile|Bilgi Bankalarınızın mevcut URL 'lerin en son içeriğini gezinmesi istiyorsanız **Yenile** onay kutusunu seçin. Bu işlem, Bilgi Bankası 'nı en son URL içeriğiyle bir kez güncelleştirir. Bu, güncelleştirmelerin düzenli bir zamanlamasını ayarlamadır.|
    |Dosya Ekle|Bilgi bankasını **Yönet**' i seçip, ardından **+ Dosya Ekle** ' yi seçerek bir bilgi tabanının parçası olmak üzere desteklenen bir dosya belgesi ekleyebilirsiniz|
    |İçeri Aktarma|Ayrıca, **bilgi tabanı al** düğmesini seçerek mevcut bilgi bankasını içeri aktarabilirsiniz. |
    |Güncelleştir|Bilgi bankasını güncelleştirme, Bilgi Bankası ile ilişkili Soru-Cevap Oluşturma Hizmeti oluştururken kullanılan **Yönetim fiyatlandırma katmanına** bağlıdır. Gerekli olursa yönetim katmanı Azure Portal'dan da güncelleştirebilirsiniz.

1. Bilgi bankasında değişiklik yapmayı tamamladıktan sonra, değişiklikleri kalıcı hale getirmek için sayfanın sağ üst köşesindeki **Kaydet ve eğit** ' i seçin.    

    ![Kaydet ve eğitme](../media/qnamaker-how-to-edit-kb/save-and-train.png)

    >[!CAUTION]
    >**Kaydet ve eğitme**seçmeden önce sayfadan ayrılırsanız tüm değişiklikler kaybedilir.

## <a name="add-a-qna-pair"></a>Soru-Cevap çifti ekleme

Bilgi Bankası tablosuna yeni bir satır eklemek için **Düzenle** sayfasında **QNA Pair Ekle** ' yi seçin.

![Soru-cevap çifti Ekle](../media/qnamaker-how-to-edit-kb/add-qnapair.png)

## <a name="delete-a-qna-pair"></a>Soru-cevap çifti Sil

Bir QnA 'yi silmek için QnA satırının en sağındaki **Sil** simgesine tıklayın. Bu kalıcı bir işlemdir. Geri alınamaz. Çiftleri silinmeden önce **Yayımla** sayfasından KB 'nizi dışarı aktarmayı düşünün. 

![Soru-cevap çifti Sil](../media/qnamaker-how-to-edit-kb/delete-qnapair.png)

## <a name="add-alternate-questions"></a>Diğer sorular ekleyin

Bir kullanıcı sorgu için bir eşleşme olasılığını artırmak için var olan bir soru-cevap çifti diğer sorular ekleyin.

![Diğer sorular ekleyin](../media/qnamaker-how-to-edit-kb/add-alternate-question.png)

## <a name="add-metadata"></a>meta veri ekleme

Önce **Görünüm seçeneklerini**belirleyip **meta verileri göster**' i seçerek meta veri çiftleri ekleyin. Bu, meta veri sütununu görüntüler. Sonra, bir meta veri çifti eklemek için **+** işaretini seçin. Bu çift bir anahtar ve bir değer içerir.

![Meta veri ekleme](../media/qnamaker-how-to-edit-kb/add-metadata.png)

> [!TIP]
> Düzenli olarak kaydedin ve değişiklikleri kaybetmek istemiyorsanız düzenlemeler yaptıktan sonra Bilgi Bankası eğitme emin olun.

## <a name="manage-large-knowledge-bases"></a>Büyük bilgi bankalarından yönetme

* **Veri kaynağı grupları**: qnas, ayıklandıkları veri kaynağına göre gruplandırılır. Genişlet veya daralt veri kaynağı.

    ![Veri kaynağı sorularını ve yanıtlarını daraltmak ve genişletmek için Soru-Cevap Oluşturma veri kaynağı çubuğunu kullanın](../media/qnamaker-how-to-edit-kb/data-source-grouping.png)

* **Bilgi Bankası ara**: Bilgi Bankası tablosunun en üstündeki metin kutusuna yazarak bilgi bankasında arama yapabilirsiniz. Soru, yanıt veya meta veri içeriğini aramak için ENTER'a tıklayın. Arama filtresi kaldırmak için X simgesine tıklayın.

    ![Görünümü yalnızca filtre eşleme öğelerine düşürmek için soruların ve yanıtların üzerindeki Soru-Cevap Oluşturma arama kutusunu kullanın](../media/qnamaker-how-to-edit-kb/search-paginate-group.png)

* **Sayfalandırma**: büyük bilgi bankaslarını yönetmek için veri kaynaklarından hızlıca geçiş yapın

    ![Soruların ve yanıtların sayfalarında gezinmek için soruların ve yanıtların üzerinde Soru-Cevap Oluşturma sayfalandırma özelliklerini kullanın](../media/qnamaker-how-to-edit-kb/pagination.png)

## <a name="delete-knowledge-bases"></a>Bilgi bankaları Sil

Bilgi Bankası (KB) silinmesi kalıcı bir işlemdir. Geri alınamaz. Bilgi Bankası 'nı silmeden önce, Bilgi Bankası 'nı Soru-Cevap Oluşturma portalının **Ayarlar** sayfasından dışarı aktarmanız gerekir. 

KB 'nizi [ortak çalışanlarla](collaborate-knowledge-base.md) paylaşıyorsanız, herkes KB 'ye erişimi kaybeder. 

## <a name="delete-azure-resources"></a>Azure kaynaklarını silme 

Soru-cevap Oluşturucu, bilgi bankaları için kullanılan Azure kaynakları silerseniz, bilgi bankalarından artık çalışmaz. Herhangi bir kaynağı silmeden önce, bilgi temellerinizi **Ayarlar** sayfasından dışarı aktardığınızdan emin olun. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası 'nda işbirliği yapma](./collaborate-knowledge-base.md)
