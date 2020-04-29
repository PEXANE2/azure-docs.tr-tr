---
title: Bilgi Bankası Soru-Cevap Oluşturma Düzenleme-
description: Soru-Cevap Oluşturma, kullanımı kolay bir düzen deneyimi sağlayarak bilgi Bankalarınızın içeriğini yönetmenizi sağlar.
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: 3cf5dcedd4d2cfa82fda002b71abca1e5bea4b18
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80756731"
---
# <a name="edit-qna-pairs-in-your-knowledge-base"></a>Bilgi bankasındaki QnA çiftlerini düzenleme

Soru-Cevap Oluşturma, kullanımı kolay bir düzen deneyimi sağlayarak bilgi Bankalarınızın içeriğini yönetmenizi sağlar.

Bir dosya veya URL gibi veya bir düzenleme kaynağı olarak eklenen bir veri kaynağından QnA çiftleri eklenir. Bir düzenleme kaynağı QNA çiftinin QnA portalında el ile eklendiğini gösterir. Tüm QnA çiftleri düzenlenmek üzere kullanılabilir.

## <a name="add-an-editorial-qna-pair"></a>Bir düzenleme QnA çifti ekleyin
1. Soru- [cevap portalı](https://www.qnamaker.ai/)' nda oturum açın ve ardından QNA çiftini eklemek için Bilgi Bankası ' nı seçin.
1. Bilgi Bankası 'nın **düzenleme** sayfasında, yeni bir QNA çifti eklemek Için **QNA Pair Ekle** ' yi seçin.

1. Yeni QnA Pair satırına, gerekli **soru** ve **Yanıt** alanlarını ekleyin. Diğer alanlar isteğe bağlıdır. Tüm alanlar herhangi bir zamanda değiştirilebilir.

1. İsteğe bağlı olarak **Alternatif**bir ifade ekleyin. Alternatif ifade, özgün sorudan önemli ölçüde farklı olan ancak aynı yanıtı sağlamaları gereken herhangi bir soru biçimidir.

    Bilgi tabanınız yayımlandığında ve [etkin öğrenme](use-active-learning.md) özelliği açıksa, soru-cevap oluşturma kabul etmeniz için alternatif ifade seçeneklerini toplar. Tahmin doğruluğunu artırmak için bu seçimler seçilidir.

1. İsteğe bağlı olarak **meta veriler**ekleyin. Meta verileri görüntülemek için bağlam menüsünde **seçenekleri görüntüle** ' yi seçin. Meta veriler, bir sohbet bot gibi istemci uygulamasının sağladığı yanıtlara filtreler sağlar.

1. İsteğe bağlı olarak, **izleme istemleri**ekleyin. İzleme istemleri, istemci uygulamasına kullanıcıya sunmak için ek konuşma yolları sağlar.

1. Yeni QnA çiftiyle birlikte tahminleri görmek için **Kaydet ve eğitme '** yi seçin.

## <a name="edit-a-qna-pair"></a>Bir QnA çiftini düzenleme

Özgün veri kaynağından bağımsız olarak, herhangi bir QnA çiftiyle herhangi bir alan düzenlenebilir. Bazı alanlar, içerik araç çubuğunda bulunan geçerli **Görünüm seçenekleri** ayarlarınız nedeniyle görünür olmayabilir.

## <a name="delete-a-qna-pair"></a>Bir QnA çiftini silme

Bir QnA 'yi silmek için QnA satırının en sağındaki **Sil** simgesine tıklayın. Bu kalıcı bir işlemdir. Geri alınamaz. Kümeleri silmeden önce **Yayımla** sayfasından KB 'nizi dışarı aktarmayı düşünün.

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

Önce **Görünüm seçeneklerini**belirleyip **meta verileri göster**' i seçerek meta veri kümeleri ekleyin. Bu, meta veri sütununu görüntüler. Sonra, meta veri **+** kümesi eklemek için imzala ' yı seçin. Bu küme bir anahtar ve bir değer içerir.

## <a name="save-changes-to-the-qna-pairs"></a>Değişiklikleri QnA çiftlerine Kaydet

Değişiklikleri kaybetmemek için düzenleme yaptıktan sonra düzenli olarak **Kaydet ve eğit '** i seçin.

![Meta veri Ekle](../media/qnamaker-how-to-edit-kb/add-metadata.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası üzerinde birlikte çalışma](./collaborate-knowledge-base.md)

* [Soru-Cevap Oluşturma tarafından kullanılan Azure kaynaklarını yönetme](set-up-qnamaker-service-azure.md)