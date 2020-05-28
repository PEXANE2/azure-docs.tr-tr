---
title: Model-özel bir çevirici eğitme
titleSuffix: Azure Cognitive Services
description: Model eğitimi, bir çeviri modeli oluştururken önemli bir adımdır. Eğitim, söz konusu insanlar için seçtiğiniz belgelere göre yapılır.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 05/26/2020
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: c29a0b8b429705bb0315c37fc6fe63eb8d77511f
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83996712"
---
# <a name="train-a-model"></a>Modeli eğitme

Model eğitimi, bir eğitim olmadan bir çeviri modeli oluşturmak için önemli bir adımdır. Eğitim, seyahat için seçtiğiniz belgelere göre yapılır.

Bir modeli eğitme:

1.  Bir model oluşturmak istediğiniz projeyi seçin.

2.  Proje için veri sekmesi, proje dili çiftinin tüm ilgili belgelerini gösterir. Modelinize eğitebilmeniz için kullanmak istediğiniz belgeleri el ile seçin. Bu ekrandan eğitim, ayarlama ve test belgelerini seçebilirsiniz. Ayrıca, yalnızca eğitim kümesini seçersiniz ve özel çevirmenleriniz sizin için ayarlama ve test kümelerini oluşturur.

    -  Belge adı: belgenin adı.

    -  Eşleştirme: Bu belge bir Parallel veya monolingual belgesidir. Monolingual belgeler şu anda eğitim için desteklenmiyor.

    -  Belge türü: eğitim, ayarlama, test veya sözlük olabilir.

    -  Dil çifti: Bu, projenin kaynak ve hedef dilini gösterir.

    -  Kaynak cümleler: kaynak dosyadan ayıklanan Tümcelerin sayısını gösterir.

    -  Hedef cümleler: hedef dosyadan ayıklanan Tümcelerin sayısını gösterir.

    ![Modeli eğitme](media/how-to/how-to-train-model.png)

3.  Eğitme düğmesine tıklayın.

4.  İletişim kutusunda modelinize bir ad belirtin.

5.  Modeli eğitme ' ye tıklayın.

    ![Model eğitimi iletişim kutusu](media/how-to/how-to-train-model-2.png)

6.  Özel çevirici, eğitimi gönderir ve modeller sekmesinde eğitimin durumunu gösterir.

    ![Modeli eğitme sayfası](media/how-to/how-to-train-model-3.png)

>[!Note]
>Özel çevirmen, zaman içinde herhangi bir zamanda bir çalışma alanı içinde 10 eş zamanlı izleme destekler.


## <a name="edit-a-model"></a>Model düzenleme

Model Ayrıntıları sayfasındaki Düzenle bağlantısını kullanarak bir modeli düzenleyebilirsiniz.

1.  Kalem simgesine tıklayın.

    ![Modeli düzenle](media/how-to/how-to-edit-model.png)

2.  İletişim kutusu değiştiğinde,

    1.  Model adı (gerekli): modelinize anlamlı bir ad verin.

        ![Daha fazla Düzenle iletişim kutusu](media/how-to/how-to-edit-model-dialog.png)

3.  Kaydet’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

- [Model ayrıntılarının nasıl görüntüleneceğini](how-to-view-model-details.md)öğrenin.
