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
ms.openlocfilehash: 942932c229ace82a0bf66da7a5421f936b028088
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78302568"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>İçeri dışarı aktarma kullanarak Bilgi Bankası geçirme

Bilgi bankasını geçirmek, bir bilgi tabanından dışarı aktarma ve sonra başka bir içeri aktarma gerektirir.

## <a name="prerequisites"></a>Önkoşullar

* Başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
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

1. Test panelini kullanarak yeni bilgi bankasını **Test** edin. [Bilgi Bankalarınızı test](../How-To/test-knowledge-base.md)etme hakkında bilgi edinin.

1. Bilgi Bankası 'nı **yayımlayın** ve bir sohbet bot oluşturun. [Bilgi tabanınızı yayımlamayı](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base)öğrenin.

## <a name="chat-logs-and-alterations"></a>Sohbet günlükleri ve değişiklikleri
Büyük/küçük harf duyarsız değişiklikleri (eş anlamlılar) otomatik olarak içeri aktarılmaz. Yeni Bilgi Bankası 'ndaki değişiklikleri taşımak için [v4 API 'lerini](https://go.microsoft.com/fwlink/?linkid=2092179) kullanın.

Yeni Bilgi Bankası, sohbet günlüklerini depolamak için Application Insights kullandığından, sohbet günlüklerini geçirme yolu yoktur.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası düzenleme](../How-To/edit-knowledge-base.md)
