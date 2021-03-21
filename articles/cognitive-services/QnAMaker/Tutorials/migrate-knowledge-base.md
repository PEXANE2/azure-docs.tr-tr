---
title: Bilgi temellerini geçirme-Soru-Cevap Oluşturma
description: Bilgi bankasını geçirmek, bir bilgi tabanından dışarı aktarma ve sonra başka bir içeri aktarma gerektirir.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: how-to
ms.date: 11/09/2020
ms.openlocfilehash: c89ab375cb02824a08ff57e6b5278dd9299126ff
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96350934"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>Dışarı aktarma kullanarak Bilgi Bankası geçirme

Geçiş, mevcut bir bilgi bankasından yeni bir Bilgi Bankası oluşturma işlemidir. Bu, birkaç nedenden dolayı bunu yapabilirsiniz:

* Yedekleme ve geri yükleme işlemi
* CI/CD işlem hattı
* bölgeleri taşı

Bilgi bankasını geçirmek, mevcut bir bilgi bankasından dışa aktarma ve sonra başka bir içeri aktarma gerektirir.

> [!NOTE]
> Mevcut bilgi bankaınızı yeni bir Soru-Cevap Oluşturma yönetilen (Önizleme) ile geçirmek için aşağıdaki yönergeleri izleyin.

## <a name="prerequisites"></a>Önkoşullar

* Başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/cognitive-services/) oluşturun.
* Yeni bir [soru-cevap oluşturma hizmeti](../How-To/set-up-qnamaker-service-azure.md) ayarlama

## <a name="migrate-a-knowledge-base-from-qna-maker"></a>Bilgi bankasını Soru-Cevap Oluşturma geçirme
1. [Soru-cevap oluşturma portalında](https://qnamaker.ai)oturum açın.
1. Geçirmek istediğiniz kaynak Bilgi Bankası ' nı seçin.

1. **Ayarlar** sayfasında, kaynak Bilgi Bankası 'nizin içeriğini içeren bir. tsv dosyasını indirmek için **Bilgi Bankası 'nı dışarı aktar** ' ı seçin-sorular, yanıtlar, meta veriler, izleme istemleri ve ayıklanan veri kaynağı adları. Sorular ve yanıtlarla birlikte verilen QnA kimlikleri, [güncelleştirme API 'si](/rest/api/cognitiveservices/qnamaker/knowledgebase/update)kullanılarak belirli bir QNA çiftini güncelleştirmek için kullanılabilir. Belirli bir QnA çiftinin QnA KIMLIĞI, birden çok dışa aktarma işlemi arasında değişmeden kalır.

1. Üstteki menüden **Bilgi Bankası oluştur** ' u seçin ve _boş_ bir Bilgi Bankası oluşturun. Bu, siz oluşturduğunuzda boş bir URL veya dosya ekleyemeyeceklerdir. Bunlar, oluşturulduktan sonra içeri aktarma adımı sırasında eklenir.

    Bilgi Bankası 'nı yapılandırın. Yalnızca yeni Bilgi Bankası adı ' nı ayarlayın. Yinelenen adlar desteklenir ve özel karakterler de desteklenir.

    Dosyayı içeri aktardığınızda değerlerin üzerine yazılacağı için 4. adımdan herhangi bir şey seçmeyin.

1. 5. adımda **Oluştur**' u seçin.

1. Bu yeni Bilgi Bankası 'nda **Ayarlar** sekmesini açın ve **Bilgi Bankası 'nı içeri aktar**' ı seçin. Bu, soruları, yanıtları, meta verileri, izleme istemlerini içeri aktarır ve ayıklanan veri kaynağı adlarını korur. **Yeni Bilgi Bankası 'nda oluşturulan QNA çiftleri, içe aktarılmış dosyada bulunan aynı QNA kimliğine sahip olacaktır**. Bu, Bilgi Bankası 'nın tam bir çoğaltmasını oluşturmanıza yardımcı olur.

   > [!div class="mx-imgBorder"]
   > [![Bilgi bankasını içeri aktar](../media/qnamaker-how-to-migrate-kb/Import.png)](../media/qnamaker-how-to-migrate-kb/Import.png#lightbox)

1. Test panelini kullanarak yeni bilgi bankasını **Test** edin. [Bilgi Bankalarınızı test](../How-To/test-knowledge-base.md)etme hakkında bilgi edinin.

1. Bilgi Bankası 'nı **yayımlayın** ve bir sohbet bot oluşturun. [Bilgi tabanınızı yayımlamayı](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base)öğrenin.

## <a name="programmatically-migrate-a-knowledge-base-from-qna-maker"></a>Bilgi bankasını Soru-Cevap Oluşturma aracılığıyla program aracılığıyla geçirme

Geçiş işlemi, aşağıdaki REST API 'Leri kullanılarak programlı olarak kullanılabilir:

**Dışarı Aktarma**

* [Bilgi Bankası API 'sini indirin](/rest/api/cognitiveservices/qnamaker4.0/knowledgebase/download)

**İçeri Aktar**

* [API değiştirme (aynı Bilgi Bankası KIMLIĞIYLE yeniden yükle)](/rest/api/cognitiveservices/qnamaker4.0/knowledgebase/replace)
* [API oluştur (yeni Bilgi Bankası KIMLIĞIYLE yükle)](/rest/api/cognitiveservices/qnamaker4.0/knowledgebase/create)


## <a name="chat-logs-and-alterations"></a>Sohbet günlükleri ve değişiklikleri
Büyük/küçük harf duyarsız değişiklikleri (eş anlamlılar) otomatik olarak içeri aktarılmaz. Yeni Bilgi Bankası 'ndaki değişiklikleri taşımak için [v4 API 'lerini](/rest/api/cognitiveservices/qnamaker4.0/knowledgebase) kullanın.

Yeni Bilgi Bankası, sohbet günlüklerini depolamak için Application Insights kullandığından, sohbet günlüklerini geçirme yolu yoktur.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası düzenleme](../How-To/edit-knowledge-base.md)