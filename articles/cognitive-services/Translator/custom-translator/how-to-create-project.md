---
title: Proje nasıl oluşturulur? -Özel Translator
titleSuffix: Azure Cognitive Services
description: Özel çevirmende bir proje nasıl oluşturulur?
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: a62cc6133fe01bf7478166c526e32b3215ceebf1
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68595758"
---
# <a name="create-a-project"></a>Proje oluşturma

Proje, modeller, belgeler ve testler için bir kapsayıcıdır. Her proje, doğru dil çiftine sahip olan bu çalışma alanına yüklenen tüm belgeleri otomatik olarak içerir.

Proje oluşturmak, model oluşturmaya yönelik ilk adımdır.

## <a name="create-a-project"></a>Proje oluşturun:

1.  [Özel çevirmen](https://portal.customtranslator.azure.ai) portalında proje oluştur ' a tıklayın.

    ![Proje oluşturma](media/how-to/how-to-create-project.png)

2.  İletişim kutusunda projenizle ilgili aşağıdaki ayrıntıları girin:

    a.  Proje adı (gerekli): Projenize benzersiz ve anlamlı bir ad verin. Başlık içindeki dillerin bahsetmek gerekli değildir.

    b.  Açıklama: Proje hakkında kısa bir Özet. Bu açıklamanın özel çevirmen veya ortaya çıkan özel sisteminizin davranışı üzerinde hiçbir etkisi yoktur, ancak farklı projeler arasında ayrım yapmanıza yardımcı olabilir.

    c.  Dil çifti (gerekli): Çeviri yaptığınız dili ve ' ı seçin.

    d.  Kategori (zorunlu): Projeniz için en uygun kategoriyi seçin. Kategori, çevirmek istediğiniz belgelerin terminolojisini ve stilini açıklar.

    e.  Kategori açıklaması: Üzerinde çalıştığınız belirli bir alanı veya sektöri daha iyi açıklayan bu alanı kullanın. Örneğin kategorinizin ilaç olması halinde, belirli bir belge (örneğin, bir Sury veya Pediatrics) ekleyebilirsiniz. Açıklamanın özel çevirmen veya elde edilen özel sisteminizin davranışı üzerinde hiçbir etkisi yoktur.

    f.  Proje etiketi: [Proje etiketi](workspace-and-project.md#project-labels) , aynı dil çiftine ve kategoriye sahip projeler arasında ayrım yapar. En iyi uygulama olarak, *yalnızca* aynı dil çifti ve aynı kategori için birden fazla proje oluşturmayı planlıyor ve bu projelere farklı bir CategoryID ile erişmek istiyorsanız bir etiket kullanın. Yalnızca bir kategori için sistem oluşturuyorsanız bu alanı kullanmayın. Proje etiketi gerekli değildir ve dil çiftlerini ayırt etmek için yararlı değildir. Birden çok proje için aynı etiketi kullanabilirsiniz.

    ![Proje oluştur iletişim kutusu](media/how-to/how-to-create-project-dialog.png)

3.  Oluştur’a tıklayın

## <a name="view-project-details"></a>Proje ayrıntılarını görüntüle

Özel çevirici giriş sayfası, çalışma alanınızdaki ilk 10 projeyi gösterir. Proje adı, dil çifti, kategori, durum ve BLEU Puanını görüntüler.

Proje seçildikten sonra proje sayfasında şunları görürsünüz:

- CategoryID Bir CategoryID, çalışma alanı kimliği, proje etiketi ve kategori kodu birleştirerek oluşturulur. Özel Çeviriler almak için metin Çeviricisi API 'SI ile CategoryID 'yi kullanırsınız.

- Eğitme düğmesi: [Bir model eğitimi](how-to-train-model.md)başlatmak için bu düğmeyi kullanın.

- Belge Ekle düğmesi: [Belge yüklemek](how-to-upload-document.md)için bu düğmeyi kullanın.

- Belgeleri filtrele düğmesi: Belirli belge (ler) i filtrelemek ve aramak için bu düğmeyi kullanın.

    ![Proje ayrıntılarını görüntüle](media/how-to/how-to-view-project.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Projeyi arama, düzenleme, silme hakkında](how-to-search-edit-delete-projects.md)bilgi edinin.
- Çeviri modellerini derlemek için [belgeyi karşıya yüklemeyi](how-to-upload-document.md) öğrenin.
