---
title: Sürümleri yönetme - LUIS
titleSuffix: Azure Cognitive Services
description: Sürümler, farklı modeller oluşturmanıza ve yayımlamanıza olanak sağlar. İyi bir uygulama, modelde değişiklik yapmadan önce geçerli etkin modeli uygulamanın farklı bir sürümüne klonlamaktır.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220902"
---
# <a name="use-versions-to-edit-and-test-without-impacting-staging-or-production-apps"></a>Hazırlama veya üretim uygulamalarını etkilemeden düzenlemek ve test etmek için sürümleri kullanın

Sürümler, farklı modeller oluşturmanıza ve yayımlamanıza olanak sağlar. İyi bir uygulama, modelde değişiklik yapmadan önce geçerli etkin modeli uygulamanın farklı bir [sürümüne](luis-concept-version.md) klonlamaktır. 

Sürümlerle çalışmak **için, Uygulamalarım** sayfasında adını seçerek uygulamanızı açın ve ardından üst çubukta **Yönet'i** seçin ve ardından sol daki **Sürümler'i** seçin. 

Sürümler listesi, hangi sürümlerin yayımlanacağını, nerede yayımlandıklarını ve hangi sürümün şu anda etkin olduğunu gösterir. 

> [!div class="mx-imgBorder"]
> [![Bölümü, sürümler sayfasını yönetme](./media/luis-how-to-manage-versions/versions-import.png "Bölümü, sürümler sayfasını yönetme")](./media/luis-how-to-manage-versions/versions-import.png#lightbox)

## <a name="clone-a-version"></a>Sürümü klonlama

1. Klonlamak istediğiniz sürümü seçin ve araç çubuğundan **Klon'u** seçin. 

2. Klon **sürümü** iletişim kutusunda, "0.2" gibi yeni sürüm için bir ad yazın.

   ![Klon Sürümü iletişim kutusu](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)
 
     > [!NOTE]
     > Sürüm Kimliği yalnızca karakterlerden, basamaklardan veya '.' dan oluşabilir ve 10 karakterden uzun olamaz.
 
   Belirtilen ada sahip yeni bir sürüm oluşturulur ve etkin sürüm olarak ayarlanır.

## <a name="set-active-version"></a>Etkin sürümü ayarlama

Listeden bir sürüm seçin ve araç çubuğundan **Etkinleştir'i** seçin. 

> [!div class="mx-imgBorder"]
> [![Bölümü, sürümsayfasını yönetme, sürüm eylemi yapma](./media/luis-how-to-manage-versions/versions-other.png "Bölümü, sürümsayfasını yönetme, sürüm eylemi yapma")](./media/luis-how-to-manage-versions/versions-other.png#lightbox)

## <a name="import-version"></a>Sürüm alma

Uygulamanızın bir `.json` sürümünü `.lu` veya bir sürümünü içe aktarabilirsiniz.

1. Araç çubuğundan **İçe Aktar'ı** seçin ve ardından biçimi seçin. 

2. Yeni **sürüm** açılır penceresine yeni on karakterli sürüm adını girin. Yalnızca dosyadaki sürüm uygulamada zaten varsa bir sürüm kimliği ayarlamanız gerekir.

    ![Bölümü, sürümler sayfasını yönetme, yeni sürümü alma](./media/luis-how-to-manage-versions/versions-import-pop-up.png)

    Bir sürümü içe aktardıktan sonra, yeni sürüm etkin sürüm olur.

### <a name="import-errors"></a>Alma hataları

* Belirteç hataları: İçe aktarırken bir **belirteç hatası** alırsanız, uygulamanın şu anda kullandığından farklı bir [belirteç](luis-language-support.md#custom-tokenizer-versions) kullanan bir sürümü almaya çalışıyorsunuz. Bunu düzeltmek için, [tokenizer sürümleri arasında geçiş](luis-language-support.md#migrating-between-tokenizer-versions)bkz.

<a name = "export-version"></a>

## <a name="other-actions"></a>Diğer eylemler

* Bir sürümü **silmek** için listeden bir sürüm seçin ve ardından araç çubuğundan **Sil'i** seçin. **Tamam**’ı seçin. 
* Bir sürümü **yeniden adlandırmak** için listeden bir sürüm seçin ve ardından araç çubuğundan **Yeniden Adlandır'ı** seçin. Yeni ad girin ve **Bitti'yi**seçin. 
* Bir sürümü **dışa aktarmak** için listeden bir sürüm seçin ve ardından araç çubuğundan **uygulamayı dışa** aktar'ı seçin. Yedekleme için dışa aktarmak için JSON'u seçin, [bu uygulamayı LUIS kapsayıcısında kullanmak](luis-container-howto.md)için kapsayıcı için **Dışa Aktar'ı** seçin.  

