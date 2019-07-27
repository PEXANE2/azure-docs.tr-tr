---
title: Sürümleri yönetme-LUSıS
titleSuffix: Azure Cognitive Services
description: Sürümleri, farklı modelleri yayımladığınız olanak tanır. Modele bir değişiklik yapmadan önce uygulamanın farklı bir sürümünü geçerli etkin modele kopyalama iyi bir uygulamadır.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/16/2019
ms.author: diberry
ms.openlocfilehash: 1169af85c35f04a302816f564caeb49eaf455d0e
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563658"
---
# <a name="use-versions-to-edit-and-test-without-impacting-staging-or-production-apps"></a>Hazırlama veya üretim uygulamalarını etkilemeden düzenlemek ve test etmek için sürümleri kullanın

Sürümleri, farklı modelleri yayımladığınız olanak tanır. Başka bir geçerli etkin model kopyalamak için iyi bir uygulama olan [sürüm](luis-concept-version.md) modelde değişiklikler yapmadan önce uygulamanın. 

Sürümlerle çalışma için uygulamanızın adını seçerek açın **uygulamalarım** sayfasında ve ardından **Yönet** üst çubuktaki seçip **sürümleri** sol gezinti bölmesinde. 

Sürüm listesi hangi sürümlerin yayımlandığını, nerede yayımlandığını ve şu anda etkin olduğunu gösterir. 

[![Yönet bölümünün, sürümler sayfasını](./media/luis-how-to-manage-versions/versions-import.png "Yönet bölümünün, sürümler sayfasını")](./media/luis-how-to-manage-versions/versions-import.png#lightbox)

## <a name="clone-a-version"></a>Sürümü Kopyala

1. Ardından kopyalama istediğiniz sürümü seçin **kopya** araç çubuğundan. 

2. İçinde **kopya sürüm** iletişim kutusunda, "0.2" gibi yeni sürümü için bir ad yazın.

   ![Kopya sürüm iletişim kutusu](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)
 
     > [!NOTE]
     > Sürüm kimliği, yalnızca karakterlerinden basamak oluşabilir veya '.' ve 10 karakterden uzun olamaz.
 
   Belirtilen ada sahip yeni bir sürüm oluşturulur ve etkin bir sürüm olarak ayarlayın.

## <a name="set-active-version"></a>Etkin sürümü Ayarla

Listeden bir sürüm seçin ve ardından **olun etkin** araç çubuğundan. 

[![Bölümü, sürümleri yönetme sayfasını yönetin, sürüm eylemi yapın](./media/luis-how-to-manage-versions/versions-other.png "Bölümü, sürümleri yönetme sayfasını yönetin, sürüm eylemi yapın")](./media/luis-how-to-manage-versions/versions-other.png#lightbox)

## <a name="import-version"></a>Alma sürümü

1. Seçin **alma sürümü** araç çubuğundan. 

2. İçinde **yeni sürüm içeri aktarma** açılır penceresinde, yeni on karakter sürümü adını girin. JSON dosyasındaki sürüm uygulamada zaten varsa, bir sürüm kimliği ayarlamak yeterlidir.

    ![Bölümü yönetin, sürümler sayfası, yeni sürümü içeri aktarma](./media/luis-how-to-manage-versions/versions-import-pop-up.png)

    Bir sürümü içeri aktardığınızda, yeni sürümü etkin sürümü haline gelir.

### <a name="import-errors"></a>İçeri aktarma hataları

* Belirteç Oluşturucu hataları: İçeri aktarırken **Simgeleştirici hatası** alırsanız, uygulamanın Şu anda kullandığı farklı bir [Simgeleştirici](luis-language-support.md#custom-tokenizer-versions) kullanan bir sürümü içeri aktarmaya çalışıyorsunuz. Bu hatayı onarmak için bkz. [belirteç Oluşturucu sürümler arasında geçiş](luis-language-support.md#migrating-between-tokenizer-versions).

<a name = "export-version"></a>

## <a name="other-actions"></a>Diğer eylemler

* İçin **Sil** bir sürümü listeden bir sürüm seçin ve ardından **Sil** araç çubuğundan. **Tamam**’ı seçin. 
* İçin **Yeniden Adlandır** bir sürümü listeden bir sürüm seçin ve ardından **Yeniden Adlandır** araç çubuğundan. Yeni bir ad girin ve seçin **Bitti**. 
* İçin **dışarı** bir sürümü listeden bir sürüm seçin ve ardından **dışarı aktarma uygulama** araç çubuğundan. Yedekleme için dışarı aktarılacak JSON öğesini seçin, [BIR lusıs kapsayıcısında bu uygulamayı kullanmak](luis-container-howto.md)için **kapsayıcı için dışarı aktar** ' ı seçin.  

