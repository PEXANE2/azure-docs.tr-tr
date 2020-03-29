---
title: Bilgi tabanlarını geçir - QnA Maker
description: Bir bilgi tabanını geçirmek, bir bilgi tabanından dışa aktarmayı, sonra da başka bir bilgi tabanına aktarmayı gerektirir.
ms.topic: article
ms.date: 03/25/2020
ms.openlocfilehash: 13e5e79bf4eaf6ec59e41b3e12aa1bb23f2c1578
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80258099"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>Dışa aktarma-alma kullanarak bilgi tabanını geçirme

Geçiş, varolan bir bilgi tabanından yeni bir bilgi tabanı oluşturma sürecidir. Bunu çeşitli nedenlerden dolayı yapabilirsiniz:

* yedekleme ve geri yükleme işlemi
* CI/CD boru hattı
* bölgeleri taşıma

Bir bilgi tabanını geçirmek, varolan bir bilgi tabanından dışa aktarmayı, sonra da başka bir bilgi tabanına aktarmayı gerektirir.

## <a name="prerequisites"></a>Ön koşullar

* Başlamadan önce ücretsiz bir [hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* Yeni bir [QnA Maker hizmeti](../How-To/set-up-qnamaker-service-azure.md) ayarlama

## <a name="migrate-a-knowledge-base-from-qna-maker"></a>QnA Maker'dan bir bilgi tabanını geçirin
1. [QnA Maker portalında](https://qnamaker.ai)oturum açın.
1. Geçirmek istediğiniz kaynak bilgi tabanını seçin.

1. **Ayarlar** sayfasında, başlangıç bilgi tabanınızın içeriğini içeren bir .tsv dosyasını indirmek için **Bilgi Verme tabanını** seçin - sorular, yanıtlar, meta veriler, izleme istemleri ve bunların ayıklandığı veri kaynağı adları.

1. Üst menüden **bir bilgi tabanı oluştur'u** seçin ve ardından _boş_ bir bilgi tabanı oluşturun. Boş, çünkü oluşturduğunuzda, herhangi bir URL veya dosya eklemeyin. Bunlar, oluşturulduktan sonra, alma adımı sırasında eklenir.

    Bilgi tabanını yapılandırın. Yalnızca yeni bilgi bankası adını ayarlayın. Yinelenen adlar desteklenir ve özel karakterler de desteklenir.

    Dosyayı içe aktardığınızda bu değerler üzerine yazıldığından, Adım 4'ten hiçbir şey seçmeyin.

1. Adım 5'te **Oluştur'u**seçin.

1. Bu yeni bilgi tabanında Ayarlar **sekmesini** açın ve **Bilgi Tabanını Aktar'ı**seçin. Bu, soruları, yanıtları, meta verileri, izleme istemlerini içeri aktarıyor ve çıkarıldıkları veri kaynağı adlarını korur.

   > [!div class="mx-imgBorder"]
   > [![İthalat bilgi tabanı](../media/qnamaker-how-to-migrate-kb/Import.png)](../media/qnamaker-how-to-migrate-kb/Import.png#lightbox)

1. Test panelini kullanarak yeni bilgi tabanını **test** edin. Bilgi tabanınızı nasıl [test edebilirsiniz](../How-To/test-knowledge-base.md)öğrenin.

1. Bilgi tabanını **yayınlayın** ve bir sohbet botu oluşturun. Bilgi tabanınızı nasıl [yayınlayacağınızı](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base)öğrenin.

## <a name="programmatically-migrate-a-knowledge-base-from-qna-maker"></a>Programlı QnA Maker bir bilgi tabanı göç

Geçiş işlemi aşağıdaki REST API'leri kullanılarak programlanabilir şekilde kullanılabilir:

**Dışarı aktar**

* [Bilgi bankası API'sini indirin](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/download)

**İçeri Aktarma**

* [API'yi değiştirin (aynı bilgi temel kimliğiyle yeniden yükleme)](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/replace)
* [API oluşturma (yeni bilgi bankası kimliğiyle yüklenir)](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create)


## <a name="chat-logs-and-alterations"></a>Sohbet günlükleri ve değişiklikler
Büyük/küçük harf duyarlı değişiklikler (eşanlamlılar) otomatik olarak alınmaz. Yeni bilgi tabanındaki değişiklikleri taşımak için [V4 API'lerini](https://go.microsoft.com/fwlink/?linkid=2092179) kullanın.

Yeni bilgi tabanı sohbet günlüklerini depolamak için Uygulama Bilgileri kullandığından, sohbet günlüklerini geçirmenin bir yolu yoktur.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası düzenleme](../How-To/edit-knowledge-base.md)
