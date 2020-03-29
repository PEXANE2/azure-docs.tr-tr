---
title: Belge nasıl yüklenir - Özel Çevirmen
titleSuffix: Azure Cognitive Services
description: Belge yükleme özelliği hizmete paralel belgeler (birinin kaynağı, diğerinin çevirisi olan iki belge) yükler.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: b2a249a40d8c782d54a12df43d33655f3409753c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73647377"
---
# <a name="upload-a-document"></a>Bir belgeyi karşıya yükleme

[Özel](https://portal.customtranslator.azure.ai)Çevirmen'de, çeviri modellerinizi eğitmek için paralel belgeler yükleyebilirsiniz. [Paralel belgeler,](what-are-parallel-documents.md) birinin diğerinin çevirisi olduğu belge çiftleridir. Çiftteki bir belge kaynak dilde cümleler, diğer belge ise hedef dile çevrilmiş bu cümleleri içerir.

Belgelerinizi yüklemeden önce, dosya biçiminizin Özel Çevirmen'te desteklendiğinden emin olmak için belge biçimlerini gözden geçirin [ve kural kılavuzunu adlandırın.](document-formats-naming-convention.md)

## <a name="how-to-upload-document"></a>Belge nasıl yüklenir?

[Özel Çevirmen](https://portal.customtranslator.azure.ai) portalından belgeler sayfasına gitmek için "Belgeler" sekmesine tıklayın.

![Belge yükleme bağlantısı](media/how-to/how-to-upload-1.png)


1.  Belgeler sayfasındaki Dosyaları Yükle düğmesine tıklayın.

    ![Belge sayfası yükleme](media/how-to/how-to-upload-2.png)

2.  İletişim kutusunda aşağıdaki bilgileri doldurun:

    a.  Belge türü:

    -  Eğitim: Bu belge(ler) eğitim seti için kullanılacaktır.
    -  Ayarlama: Bu belge(ler) ayar kümesi için kullanılacaktır.
    -  Test: Bu belge(ler) test kümesi için kullanılacaktır.
    -  İfade Sözlüğü: Bu belge(ler) ifade sözlüğü için kullanılacaktır.
    -  Cümle Sözlüğü: Bu belge(ler) cümle sözlüğü için kullanılacaktır

    b.  Dil çifti

    c.  Varsa belgeyi geçersiz kılma: Varolan belgeleri aynı ada sahip olarak yazmak istiyorsanız bu onay kutusunu seçin.

    d.  Paralel veri veya açılan veriler için ilgili bölümü doldurun.

    -  Paralel veriler:
        -  Kaynak dosya: Yerel bilgisayarınızdan kaynak dil dosyalarını seçin.
        -  Hedef dosya: Yerel bilgisayarınızdan hedef dil dosyanızı seçin.
        -  Belge adı: Yalnızca paralel dosyalar yüklerken kullanılır.

    - Açılan veriler:
        -  Açılan Dosya: Yerel bilgisayarınızdan açılan dosyayı seçin. Açılan dosyanızda hem kaynak hem de hedef dil cümleleriniz vardır. [Adlandırma kuralı](document-formats-naming-convention.md) açılan dosyalar için önemlidir.

    e.  Yükle'yi tıklatın

    ![Belge iletişim kutusunu yükleme](media/how-to/how-to-upload-dialog.png)

3.  Bu noktada, belgelerinizi inceliyoruz ve cümleleri çıkarmaya çalışıyoruz. Belgelerinizin işleme koyuldukları durumu kontrol etmek için "Yükleme İlerlemesini Görüntüle"yi tıklatabilirsiniz.

    ![Belge işleme iletişim kutusunu yükleme](media/how-to/how-to-upload-processing-dialog.png)

4.  Bu sayfa, yüklemenizdeki her dosya için durumu ve hataları görüntüler. "Yükleme geçmişi" sekmesine tıklayarak geçmiş yükleme durumunu istediğiniz zaman görüntüleyebilirsiniz.

    ![Belge geçmişi iletişim kutusunu yükleme](media/how-to/how-to-upload-document-history.png)


## <a name="view-upload-history"></a>Yükleme geçmişini görüntüleme

Yükleme geçmişi sayfasında belge türü, dil çifti, yükleme durumu vb. gibi tüm belge yükleme ayrıntılarının geçmişini görüntüleyebilirsiniz.

1. Özel [Çevirmen](https://portal.customtranslator.azure.ai) portalından geçmişi görüntülemek için Geçmişi Yükle sekmesini tıklatın.

    ![Geçmiş sekmesini yükleme](media/how-to/how-to-upload-history-1.png)

2. Bu sayfa, tüm geçmiş yüklemelerinizin durumunu gösterir. En sondan en sonyüklemelara kadar olan yüklemeleri görüntüler. Her yükleme için belge adını, yükleme durumunu, yükleme tarihini, yüklenen dosya sayısını, yüklenen dosya türünü ve dosyanın dil çiftini gösterir.

    ![Geçmiş sayfasını yükleme](media/how-to/how-to-document-history-2.png)

3. Herhangi bir yükleme geçmişi kaydına tıklayın. Yükleme geçmişi ayrıntıları sayfasında, yüklenen dosyaların bir parçası olarak yüklenen dosyaları, dosyanın yüklenen durumunu, dosyanın dilini ve hata iletisini (yüklemede herhangi bir hata varsa) görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Çıkarılan cümlelerin listesini gözden geçirmek için [belge ayrıntıları sayfasını](how-to-view-document-details.md) kullanın.
- [Nasıl bir model eğitmek için](how-to-train-model.md).
