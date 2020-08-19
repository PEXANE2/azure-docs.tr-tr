---
title: Bilgi temellerini geçirme-Soru-Cevap Oluşturma
description: Bilgi bankasını geçirmek, bir bilgi tabanından dışarı aktarma ve sonra başka bir içeri aktarma gerektirir.
ms.topic: how-to
ms.date: 03/25/2020
ms.openlocfilehash: 4148431ebb4de288ac517c7c006a925e36c5f460
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88542930"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>Dışarı aktarma kullanarak Bilgi Bankası geçirme

Geçiş, mevcut bir bilgi bankasından yeni bir Bilgi Bankası oluşturma işlemidir. Bu, birkaç nedenden dolayı bunu yapabilirsiniz:

* Yedekleme ve geri yükleme işlemi
* CI/CD işlem hattı
* bölgeleri taşı

Bilgi bankasını geçirmek, mevcut bir bilgi bankasından dışa aktarma ve sonra başka bir içeri aktarma gerektirir.

## <a name="prerequisites"></a>Ön koşullar

* Başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/cognitive-services/) oluşturun.
* Yeni bir [soru-cevap oluşturma hizmeti](../How-To/set-up-qnamaker-service-azure.md) ayarlama

## <a name="migrate-a-knowledge-base-from-qna-maker"></a>Bilgi bankasını Soru-Cevap Oluşturma geçirme
1. [Soru-cevap oluşturma portalında](https://qnamaker.ai)oturum açın.
1. Geçirmek istediğiniz kaynak Bilgi Bankası ' nı seçin.

1. **Ayarlar** sayfasında, kaynak Bilgi Bankası 'nizin içeriğini içeren bir. tsv dosyasını indirmek için **Bilgi Bankası 'nı dışarı aktar** ' ı seçin-sorular, yanıtlar, meta veriler, izleme istemleri ve ayıklanan veri kaynağı adları.

1. Üstteki menüden **Bilgi Bankası oluştur** ' u seçin ve _boş_ bir Bilgi Bankası oluşturun. Bu, siz oluşturduğunuzda boş bir URL veya dosya ekleyemeyeceklerdir. Bunlar, oluşturulduktan sonra içeri aktarma adımı sırasında eklenir.

    Bilgi Bankası 'nı yapılandırın. Yalnızca yeni Bilgi Bankası adı ' nı ayarlayın. Yinelenen adlar desteklenir ve özel karakterler de desteklenir.

    Dosyayı içeri aktardığınızda değerlerin üzerine yazılacağı için 4. adımdan herhangi bir şey seçmeyin.

1. 5. adımda **Oluştur**' u seçin.

1. Bu yeni Bilgi Bankası 'nda **Ayarlar** sekmesini açın ve **Bilgi Bankası 'nı içeri aktar**' ı seçin. Bu, soruları, yanıtları, meta verileri, izleme istemlerini içeri aktarır ve ayıklanan veri kaynağı adlarını korur.

   > [!div class="mx-imgBorder"]
   > [![Bilgi bankasını içeri aktar](../media/qnamaker-how-to-migrate-kb/Import.png)](../media/qnamaker-how-to-migrate-kb/Import.png#lightbox)

1. Test panelini kullanarak yeni bilgi bankasını **Test** edin. [Bilgi Bankalarınızı test](../How-To/test-knowledge-base.md)etme hakkında bilgi edinin.

1. Bilgi Bankası 'nı **yayımlayın** ve bir sohbet bot oluşturun. [Bilgi tabanınızı yayımlamayı](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base)öğrenin.

## <a name="programmatically-migrate-a-knowledge-base-from-qna-maker"></a>Bilgi bankasını Soru-Cevap Oluşturma aracılığıyla program aracılığıyla geçirme

Geçiş işlemi, aşağıdaki REST API 'Leri kullanılarak programlı olarak kullanılabilir:

**Dışarı Aktarma**

* [Bilgi Bankası API 'sini indirin](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/download)

**İçeri Aktar**

* [API değiştirme (aynı Bilgi Bankası KIMLIĞIYLE yeniden yükle)](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/replace)
* [API oluştur (yeni Bilgi Bankası KIMLIĞIYLE yükle)](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create)


## <a name="chat-logs-and-alterations"></a>Sohbet günlükleri ve değişiklikleri
Büyük/küçük harf duyarsız değişiklikleri (eş anlamlılar) otomatik olarak içeri aktarılmaz. Yeni Bilgi Bankası 'ndaki değişiklikleri taşımak için [v4 API 'lerini](https://go.microsoft.com/fwlink/?linkid=2092179) kullanın.

Yeni Bilgi Bankası, sohbet günlüklerini depolamak için Application Insights kullandığından, sohbet günlüklerini geçirme yolu yoktur.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası düzenleme](../How-To/edit-knowledge-base.md)
