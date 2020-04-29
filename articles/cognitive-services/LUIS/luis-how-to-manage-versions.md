---
title: Sürümleri yönetme-LUSıS
titleSuffix: Azure Cognitive Services
description: Sürümler, farklı modeller oluşturmanıza ve yayımlamanıza olanak sağlar. Modelde değişiklik yapmadan önce geçerli etkin modeli uygulamanın farklı bir sürümüne kopyalamak iyi bir uygulamadır.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: diberry
ms.openlocfilehash: 138b84a9b7f54782fd6254304a3fdcf4dba83182
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79220902"
---
# <a name="use-versions-to-edit-and-test-without-impacting-staging-or-production-apps"></a>Hazırlama veya üretim uygulamalarını etkilemeden düzenlemek ve test etmek için sürümleri kullanın

Sürümler, farklı modeller oluşturmanıza ve yayımlamanıza olanak sağlar. Modelde değişiklik yapmadan önce geçerli etkin modeli uygulamanın farklı bir [sürümüne](luis-concept-version.md) kopyalamak iyi bir uygulamadır. 

Sürümlerle çalışmak için, **uygulamalarım** sayfasında adını seçerek uygulamanızı açın ve sonra üstteki çubukta **Yönet** ' i seçin ve ardından sol gezinti bölmesinde **sürümler** ' i seçin. 

Sürüm listesi hangi sürümlerin yayımlandığını, nerede yayımlandığını ve şu anda etkin olduğunu gösterir. 

> [!div class="mx-imgBorder"]
> [![Yönetme bölümü, sürümler sayfası](./media/luis-how-to-manage-versions/versions-import.png "Yönetme bölümü, sürümler sayfası")](./media/luis-how-to-manage-versions/versions-import.png#lightbox)

## <a name="clone-a-version"></a>Bir sürümü Kopyala

1. Kopyalamak istediğiniz sürümü seçin ve ardından araç çubuğundan **Kopyala** ' yı seçin. 

2. **Sürümü Kopyala** iletişim kutusunda, yeni sürüm için "0,2" gibi bir ad yazın.

   ![Sürümü Kopyala iletişim kutusu](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)
 
     > [!NOTE]
     > Sürüm KIMLIĞI yalnızca karakter, rakam veya '. ' karakterlerini içerebilir ve 10 karakterden uzun olamaz.
 
   Belirtilen ada sahip yeni bir sürüm oluşturulur ve etkin sürüm olarak ayarlanır.

## <a name="set-active-version"></a>Etkin sürümü ayarla

Listeden bir sürüm seçin ve ardından araç çubuğundan **Etkinleştir** ' i seçin. 

> [!div class="mx-imgBorder"]
> [![Bölümü, sürümleri yönetme sayfasını yönetin, sürüm eylemi yapın](./media/luis-how-to-manage-versions/versions-other.png "Bölümü, sürümleri yönetme sayfasını yönetin, sürüm eylemi yapın")](./media/luis-how-to-manage-versions/versions-other.png#lightbox)

## <a name="import-version"></a>İçeri aktarma sürümü

Uygulamanızın bir `.json` veya bir `.lu` sürümünü içeri aktarabilirsiniz.

1. Araç çubuğundan **Içeri aktar** ' ı seçin ve ardından biçimi seçin. 

2. **Yeni sürüm al** açılır penceresinde, yeni on karakter sürümü adını girin. Yalnızca dosyadaki sürüm uygulamada zaten varsa, bir sürüm KIMLIĞI ayarlamanız gerekir.

    ![Bölümü yönetin, sürümler sayfası, yeni sürümü içeri aktarma](./media/luis-how-to-manage-versions/versions-import-pop-up.png)

    Bir sürümü içeri aktardıktan sonra, yeni sürüm etkin sürüm olur.

### <a name="import-errors"></a>İçeri aktarma hataları

* Belirteç Oluşturucu hataları: içeri aktarırken **Simgeleştirici hatası** alırsanız, uygulamanın Şu anda kullandığı farklı bir [Simgeleştirici](luis-language-support.md#custom-tokenizer-versions) kullanan bir sürümü içeri aktarmaya çalışıyorsunuz. Bu hatayı onarmak için bkz. [belirteç Oluşturucu sürümler arasında geçiş](luis-language-support.md#migrating-between-tokenizer-versions).

<a name = "export-version"></a>

## <a name="other-actions"></a>Diğer eylemler

* Bir sürümü **silmek** için listeden bir sürüm seçin ve ardından araç çubuğundan **Sil** ' i seçin. **Tamam ' ı**seçin. 
* Bir sürümü **yeniden adlandırmak** için listeden bir sürüm seçin ve ardından araç çubuğundan **Yeniden Adlandır** ' ı seçin. Yeni ad girin ve **bitti**' yi seçin. 
* Bir sürümü **dışarı aktarmak** için listeden bir sürüm seçin ve ardından araç çubuğundan **uygulamayı dışarı aktar** ' ı seçin. Yedekleme için dışarı aktarılacak JSON öğesini seçin, [BIR lusıs kapsayıcısında bu uygulamayı kullanmak](luis-container-howto.md)için **kapsayıcı için dışarı aktar** ' ı seçin.  

