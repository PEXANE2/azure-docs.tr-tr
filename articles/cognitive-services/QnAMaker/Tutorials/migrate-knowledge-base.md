---
title: Bilgi temellerini geçirme-Soru-Cevap Oluşturma
titleSuffix: Azure Cognitive Services
description: Bilgi bankasını geçirmek, bir bilgi tabanından dışarı aktarma ve sonra başka bir içeri aktarma gerektirir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/28/2020
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b441eb1e6531030a998fe628ae833b29a5d9fe5a
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76902034"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>İçeri dışarı aktarma kullanarak Bilgi Bankası geçirme

Bilgi bankasını geçirmek, bir bilgi tabanından dışarı aktarma ve sonra başka bir içeri aktarma gerektirir.

## <a name="prerequisites"></a>Ön koşullar

* Oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) başlamadan önce.
* Yeni bir [soru-cevap oluşturma hizmeti](../How-To/set-up-qnamaker-service-azure.md) ayarlama

## <a name="migrate-a-knowledge-base-from-qna-maker"></a>Bilgi bankasını Soru-Cevap Oluşturma geçirme
1. [Soru-cevap oluşturma portalında](https://qnamaker.ai)oturum açın.
1. Geçirmek istediğiniz kaynak Bilgi Bankası ' nı seçin.

1. **Ayarlar** sayfasında, kaynak Bilgi Bankası 'nizin içeriğini içeren bir. tsv dosyasını indirmek için **Bilgi Bankası 'nı dışarı aktar** ' ı seçin-sorular, yanıtlar, meta veriler, izleme istemleri ve ayıklanan veri kaynağı adları.

1. Üstteki menüden **Bilgi Bankası oluştur** ' u seçin ve _boş_ bir Bilgi Bankası oluşturun. Bu, siz oluşturduğunuzda boş bir URL veya dosya ekleyemeyeceklerdir. Bunlar, oluşturulduktan sonra içeri aktarma adımı sırasında eklenir.

    Bilgi Bankası 'nı yapılandırın. Yalnızca yeni Bilgi Bankası adı ' nı ayarlayın. Yinelenen adları desteklenir ve özel karakterler de desteklenir.

    Dosyayı içeri aktardığınızda değerlerin üzerine yazılacağı için 4. adımdan herhangi bir şey seçmeyin.

1. 5\. adımda **Oluştur**' u seçin.

1. Bu yeni Bilgi Bankası 'nda **Ayarlar** sekmesini açın ve **Bilgi Bankası 'nı içeri aktar**' ı seçin. Bu, soruları, yanıtları, meta verileri, izleme istemlerini içeri aktarır ve ayıklanan veri kaynağı adlarını korur.

   > [!div class="mx-imgBorder"]
   > [Bilgi Bankası ![Içeri aktar](../media/qnamaker-how-to-migrate-kb/Import.png)](../media/qnamaker-how-to-migrate-kb/Import.png#lightbox)

1. **Test** Test Masası'nı kullanarak yeni Bilgi Bankası. Bilgi edinmek için nasıl [bilgi bankanızı test](../How-To/test-knowledge-base.md).
1. **Yayımlama** Bilgi Bankası. Bilgi edinmek için nasıl [, Bilgi Bankası yayımlama](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base).
1. Uygulamanızda veya bot kodunuzda uç noktayı kullanın. Burada gördüğünüz nasıl [soru-cevap Robotu oluşturun](../Tutorials/create-qna-bot.md).

    ![Soru-cevap Oluşturucu değerleri](../media/qnamaker-how-to-migrate-kb/qnamaker-settings-kbid-key.png)

    Bu noktada, tüm Bilgi Bankası içerikleri - soruları yanıtlar ve kaynak dosyaları ve URL'leri adlarını yanı sıra meta veri yeni Bilgi Bankası'na aktarılır.

## <a name="chat-logs-and-alterations"></a>Sohbet günlükleri ve değişiklikleri
Büyük/küçük harf duyarsız değişiklikleri (eş anlamlılar) otomatik olarak içeri aktarılmaz. Yeni Bilgi Bankası 'ndaki değişiklikleri taşımak için [v4 API 'lerini](https://go.microsoft.com/fwlink/?linkid=2092179) kullanın.

Yeni Bilgi Bankası, sohbet günlüklerini depolamak için Application Insights kullandığından, sohbet günlüklerini geçirme yolu yoktur.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası Düzenle](../How-To/edit-knowledge-base.md)
