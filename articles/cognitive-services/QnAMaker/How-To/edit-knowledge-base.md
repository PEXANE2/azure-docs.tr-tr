---
title: Bir bilgi tabanını edin - QnA Maker
description: QnA Maker, kullanımı kolay bir düzenleme deneyimi sunarak bilgi tabanınızın içeriğini yönetmenize olanak tanır.
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: 3cf5dcedd4d2cfa82fda002b71abca1e5bea4b18
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80756731"
---
# <a name="edit-qna-pairs-in-your-knowledge-base"></a>Bilgi tabanınızda QnA çiftlerini edin

QnA Maker, kullanımı kolay bir düzenleme deneyimi sunarak bilgi tabanınızın içeriğini yönetmenize olanak tanır.

QnA çiftleri, dosya veya URL gibi bir veri kaynağından eklenir veya editoryal kaynak olarak eklenir. Bir editoryal kaynak QnA çiftinin QnA portalına el ile eklenmiş olduğunu gösterir. Tüm QnA çiftleri düzenleme için kullanılabilir.

## <a name="add-an-editorial-qna-pair"></a>Editoryal QnA çifti ekleme
1. [QnA portalında](https://www.qnamaker.ai/)oturum açın, ardından QnA çiftini eklemek için bilgi tabanını seçin.
1. Bilgi tabanının **EDIT** sayfasında, yeni bir QnA çifti eklemek için **QnA çifti ekle'yi** seçin.

1. Yeni QnA çifti satırına, gerekli **Soru** ve **Yanıt** alanlarını ekleyin. Diğer alanlar isteğe bağlıdır. Tüm alanlar herhangi bir zamanda değiştirilebilir.

1. İsteğe bağlı olarak, **alternatif tümce**ekleyin. Alternatif tümce, özgün sorudan önemli ölçüde farklı olan ancak aynı yanıtı vermesi gereken herhangi bir soru biçimidir.

    Bilgi tabanınız yayınlandığında ve [aktif öğrenimiaçık](use-active-learning.md) olduğunda, QnA Maker kabul etmeniz için alternatif tümce seçenekleri toplar. Bu seçimler, tahmin doğruluğunu artırmak için seçilir.

1. İsteğe bağlı olarak, **meta veri**ekleyin. Meta verileri görüntülemek için bağlam menüsünde **Seçenekleri Görüntüle'yi** seçin. Meta veriler, istemci uygulamasının sohbet botu gibi sağladığı yanıtlara filtreler sağlar.

1. İsteğe bağlı olarak, **izleme istemleri**ekleyin. İzleme istemleri, istemci uygulamasına kullanıcıya sunması için ek konuşma yolları sağlar.

1. Yeni QnA çifti de dahil olmak üzere tahminleri görmek için **Kaydet'i** seçin ve train'i seçin.

## <a name="edit-a-qna-pair"></a>QnA çiftini edin

Herhangi bir QnA çiftindeki herhangi bir alan, orijinal veri kaynağından bağımsız olarak düzenlenebilir. Bağlam araç çubuğunda bulunan geçerli **Görünüm Seçenekleri** ayarlarınız nedeniyle bazı alanlar görünmeyebilir.

## <a name="delete-a-qna-pair"></a>QnA çiftini silme

QnA'yı silmek için, QnA satırının en sağındaki **sil** simgesini tıklatın. Bu kalıcı bir operasyon. Geri alınamaz. Setleri silmeden önce KB'nizi **Yayımlama** sayfasından dışa aktarmayı düşünün.

![QnA çiftini silme](../media/qnamaker-how-to-edit-kb/delete-qnapair.png)

## <a name="find-the-qna-pair-id"></a>QnA çift kimliğini bulun

QnA çift kimliğini bulmanız gerekiyorsa, iki yerde bulabilirsiniz:

* İlgilendiğiniz QnA çifti satırındaki silme simgesine odaklanın. Gezinme metni QnA çift kimliği içerir.
* Bilgi tabanını dışa aktarın. Bilgi tabanındaki her QnA çifti QnA çift kimliğini içerir.

## <a name="add-alternate-questions"></a>Alternatif sorular ekleme

Bir kullanıcı sorgusunda eşleşme olasılığını artırmak için varolan bir QnA çiftine alternatif sorular ekleyin.

![Alternatif Sorular Ekle](../media/qnamaker-how-to-edit-kb/add-alternate-question.png)

## <a name="linking-qna-pairs"></a>QnA çiftlerini bağlama

QnA çiftlerini [bağlama, takip istemleri](multiturn-conversation.md)ile sağlanır. Bu, bilgi tabanı düzeyinde yönetilen QnA çiftleri arasındaki mantıksal bir bağlantıdır. QnA Maker portalında takip istemlerini atabilirsiniz.

Yanıtın meta verilerinde QnA çiftlerini bağlayamadığınız.

## <a name="add-metadata"></a>Meta veri ekleme

Önce **Görünüm seçeneklerini**seçerek meta veri kümeleri ekleyin, ardından meta **verileri göster'i**seçin. Bu, meta veri sütununa görüntüler. Ardından, meta **+** veri kümesi eklemek için işareti seçin. Bu küme bir anahtar ve bir değerden oluşur.

## <a name="save-changes-to-the-qna-pairs"></a>QnA çiftlerinde değişiklikleri kaydetme

Değişiklikleri kaybetmemek için düzenli olarak **Kaydet'i** seçin ve değişiklikler yaptıktan sonra eğitin.

![Meta veri ekleme](../media/qnamaker-how-to-edit-kb/add-metadata.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası üzerinde birlikte çalışma](./collaborate-knowledge-base.md)

* [QnA Maker tarafından kullanılan Azure kaynaklarını yönetme](set-up-qnamaker-service-azure.md)