---
title: Proje nasıl oluşturulur - Özel Çevirmen
titleSuffix: Azure Cognitive Services
description: Bu makalede, Azure Bilişsel Hizmetler Özel Çevirmeni'nde bir projenin nasıl oluşturulup yönetilen bir şekilde açıklanmaktadır.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 5b3a6f2e903d8178554b7a076ae1277633569353
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73836557"
---
# <a name="create-a-project"></a>Proje oluşturma

Proje, modeller, belgeler ve testler için bir kapsayıcıdır. Her proje, doğru dil çiftine sahip olan çalışma alanına yüklenen tüm belgeleri otomatik olarak içerir.

Proje oluşturma, bir model oluşturma yolunda ki ilk adımdır.

## <a name="create-a-project"></a>Proje oluşturma:

1.  Özel [Çevirmen](https://portal.customtranslator.azure.ai) portalında proje oluştur'u tıklatın.

    ![Proje oluşturma](media/how-to/how-to-create-project.png)

2.  İletişim kutusuna projenizle ilgili aşağıdaki ayrıntıları girin:

    a.  Proje adı (gerekli): Projenize benzersiz ve anlamlı bir ad verin. Başlıktaki dillerden bahsetmeye gerek yok.

    b.  Açıklama: Proje hakkında kısa bir özet. Bu açıklamanın Özel Çevirmen'in davranışı veya ortaya çıkan özel sisteminiz üzerinde hiçbir etkisi yoktur, ancak farklı projeler arasında ayrım yapabilirsiniz.

    c.  Dil çifti (gerekli): Çeviri yaptığınız dili seçin.

    d.  Kategori (gerekli): Projeniz için en uygun kategoriyi seçin. Kategori, çevirmeyi istediğiniz belgelerin terminolojisini ve stilini açıklar.

    e.  Kategori açıklaması: Çalıştığınız belirli alanı veya sektörü daha iyi açıklamak için bu alanı kullanın. Örneğin, kategoriniz tıpsa, belirli bir belgeyi, böyle bir ameliyatı veya pediatriyi ekleyebilirsiniz. Açıklamanın Özel Çevirmen'in davranışı veya ortaya çıkan özel sisteminiz üzerinde hiçbir etkisi yoktur.

    f.  Proje etiketi: [Proje etiketi,](workspace-and-project.md#project-labels) aynı dil çifti ve kategoriye sahip projeleri birbirinden ayırır. En iyi uygulama olarak, *yalnızca* aynı dil çifti ve aynı kategori için birden çok proje oluşturmayı planlıyorsanız ve bu projelere farklı bir CategoryID ile erişmek istiyorsanız bir etiket kullanın. Yalnızca bir kategori için sistemler oluşturuyorsanız bu alanı kullanmayın. Proje etiketi gerekli değildir ve dil çiftleri arasında ayrım yapmak yararlı değildir. Aynı etiketi birden çok proje için kullanabilirsiniz.

    ![Proje iletişim kutusu oluşturma](media/how-to/how-to-create-project-dialog.png)

3.  Oluştur’a tıklayın

## <a name="view-project-details"></a>Proje ayrıntılarını görüntüleme

Özel Çevirmen açılış sayfası, çalışma alanınızdaki ilk 10 projeyi gösterir. Proje adını, dil çiftini, kategorisini, durumunu ve BLEU puanını görüntüler.

Proje yi seçtikten sonra proje sayfasında aşağıdakileri görürsünüz:

- CategoryID: CategoryID, WorkspaceID, proje etiketi ve kategori kodunun biraraya gelerek oluşturulur. Özel çeviriler almak için Text Translator API ile CategoryID'yi kullanırsınız.

- Tren düğmesi: Bir modeli [eğitmek](how-to-train-model.md)için bu düğmeyi kullanın.

- Belge ekle düğmesi: [Belgeleri yüklemek](how-to-upload-document.md)için bu düğmeyi kullanın.

- Belgeleri filtrele düğmesi: Belirli belge(ler) için filtre leme ve arama yapmak için bu düğmeyi kullanın.

    ![Proje ayrıntılarını görüntüleme](media/how-to/how-to-view-project.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Projeyi nasıl arayacağınızı, düzenlemeyi, silmeyi](how-to-search-edit-delete-projects.md)öğrenin.
- Çeviri modelleri oluşturmak için [belgeyi nasıl yükleyin](how-to-upload-document.md) öğrenin.
