---
title: Bir bilgi tabanını edin - QnA Maker
description: QnA Maker, kullanımı kolay bir düzenleme deneyimi sunarak bilgi tabanınızın içeriğini yönetmenize olanak tanır.
ms.topic: conceptual
ms.date: 03/19/2020
ms.openlocfilehash: 223ad3a607adc20bbe608598da9742d56788b2c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131646"
---
# <a name="edit-qna-sets-in-your-knowledge-base"></a>Bilgi tabanınızda QnA kümelerini edin

QnA Maker, kullanımı kolay bir düzenleme deneyimi sunarak bilgi tabanınızın içeriğini yönetmenize olanak tanır.

QnA kümeleri, dosya veya URL gibi bir veri kaynağından eklenir veya editoryal kaynak olarak eklenir. Bir editoryal kaynak QnA kümesinin QnA portalına el ile eklandığını gösterir. Tüm QnA setleri düzenleme için kullanılabilir.

## <a name="add-an-editorial-qna-set"></a>Editoryal QnA kümesi ekleme
1. [QnA portalında](https://www.qnamaker.ai/)oturum açın, ardından QnA kümesini eklemek için bilgi tabanını seçin.
1. Bilgi tabanının **EDIT** sayfasında, yeni bir QnA kümesi eklemek için **QnA kümesi** ekle'yi seçin.

1. Yeni QnA kümesi satırına, gerekli **Soru** ve **Yanıt** alanlarını ekleyin. Diğer alanlar isteğe bağlıdır. Tüm alanlar herhangi bir zamanda değiştirilebilir.

1. İsteğe bağlı olarak, **alternatif tümce**ekleyin. Alternatif tümce, özgün sorudan önemli ölçüde farklı olan ancak aynı yanıtı vermesi gereken herhangi bir soru biçimidir.

    Bilgi tabanınız yayınlandığında ve aktif öğrenimiaçık olduğunda, QnA Maker kabul etmeniz için alternatif tümce seçenekleri toplar. Bu seçimler, tahmin doğruluğunu artırmak için seçilir.

1. İsteğe bağlı olarak, **meta veri**ekleyin. Meta verileri görüntülemek için bağlam menüsünde **Seçenekleri Görüntüle'yi** seçin. Meta veriler, istemci uygulamasının sohbet botu gibi sağladığı yanıtlara filtreler sağlar.

1. İsteğe bağlı olarak, **izleme istemleri**ekleyin. İzleme istemleri, istemci uygulamasına kullanıcıya sunması için ek konuşma yolları sağlar.

1. Yeni QnA kümesi de dahil olmak üzere tahminleri görmek için **Kaydet'i seçin ve train'i** seçin.

## <a name="edit-a-qna-set"></a>QnA kümesini ayarlama

Herhangi bir QnA kümesindeki herhangi bir alan, orijinal veri kaynağından bağımsız olarak düzenlenebilir. Bağlam araç çubuğunda bulunan geçerli **Görünüm Seçenekleri** ayarlarınız nedeniyle bazı alanlar görünmeyebilir.

## <a name="delete-a-qna-set"></a>QnA kümesini silme

QnA'yı silmek için, QnA satırının en sağındaki **sil** simgesini tıklatın. Bu kalıcı bir operasyon. Geri alınamaz. Setleri silmeden önce KB'nizi **Yayımlama** sayfasından dışa aktarmayı düşünün.

![QnA kümesini silme](../media/qnamaker-how-to-edit-kb/delete-qnapair.png)

## <a name="find-the-qna-set-id"></a>QnA ayarlı kimliğini bulun

QnA ayarlı kimliğini bulmanız gerekiyorsa, iki yerde bulabilirsiniz:

* İlgilendiğiniz QnA kümesi satırındaki silme simgesine odaklanın. Gezinme metni QnA ayarlı kimliği içerir.
* Bilgi tabanını dışa aktarın. Bilgi tabanında ayarlanan her QnA, QnA kümesi kimliğini içerir.

## <a name="add-alternate-questions"></a>Alternatif sorular ekleme

Bir kullanıcı sorgusunda eşleşme olasılığını artırmak için varolan bir QnA kümesine alternatif sorular ekleyin.

![Alternatif Sorular Ekle](../media/qnamaker-how-to-edit-kb/add-alternate-question.png)

## <a name="linking-qna-sets"></a>QnA Kümelerini Bağlama

QnA kümelerini [bağlama, takip istemleri](multiturn-conversation.md)ile sağlanır. Bu, bilgi tabanı düzeyinde yönetilen QnA kümeleri arasındaki mantıksal bir bağlantıdır. QnA Maker portalında takip istemlerini atabilirsiniz.

Yanıtın meta verilerinde QnA kümelerini bağlayamadığınız.

## <a name="add-metadata"></a>Meta veri ekleme

Önce **Görünüm seçeneklerini**seçerek meta veri kümeleri ekleyin, ardından meta **verileri göster'i**seçin. Bu, meta veri sütununa görüntüler. Ardından, meta **+** veri kümesi eklemek için işareti seçin. Bu küme bir anahtar ve bir değerden oluşur.

## <a name="save-changes-to-the-qna-sets"></a>QnA kümelerine değişiklikleri kaydetme

Değişiklikleri kaybetmemek için düzenli olarak **Kaydet'i** seçin ve değişiklikler yaptıktan sonra eğitin.

![Meta veri ekleme](../media/qnamaker-how-to-edit-kb/add-metadata.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası üzerinde birlikte çalışma](./collaborate-knowledge-base.md)

* [QnA Maker tarafından kullanılan Azure kaynaklarını yönetme](set-up-qnamaker-service-azure.md)