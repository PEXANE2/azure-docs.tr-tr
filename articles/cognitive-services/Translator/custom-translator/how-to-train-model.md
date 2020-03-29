---
title: Bir model eğitin - Özel Çevirmen
titleSuffix: Azure Cognitive Services
description: Bir modeli eğitmek, çeviri modeli oluşturmak önemli bir adımdır. Eğitim, bu eğitimler için seçtiğiniz belgelere göre gerçekleşir.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: a875cc8f5e69be8b940f9d177d3b915bfaa2c973
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68595690"
---
# <a name="train-a-model"></a>Modeli eğitme

Bir modeli eğitmek bir çeviri modeli oluşturmak için önemli bir adımdır, çünkü eğitim olmadan, model inşa edilemez. Eğitim, eğitimler için seçtiğiniz belgelere göre gerçekleşir.

Bir modeli eğitmek için:

1.  Bir model oluşturmak istediğiniz projeyi seçin.

2.  Projenin Veri sekmesi, proje dil çifti için ilgili tüm belgeleri gösterir. Modelinizi eğitmek için kullanmak istediğiniz belgeleri el ile seçin. Bu ekrandan eğitim, atoalma ve test belgelerini seçebilirsiniz. Ayrıca sadece eğitim kümesini seçin ve Özel Çevirmen sizin için ayar ve test setleri oluşturmak var.

    -  Belge adı: Belgenin adı.

    -  Eşleştirme: Bu belge paralel veya tek dilli bir belgeyse. Tek dilli belgeler şu anda eğitim için desteklenmez.

    -  Belge türü: Eğitim, atoalma, test veya sözlük olabilir.

    -  Dil çifti: Bu, projenin kaynağını ve hedef dilini gösterir.

    -  Kaynak cümleler: Kaynak dosyadan çıkarılan cümle sayısını gösterir.

    -  Hedef cümleler: Hedef dosyadan çıkarılan cümle sayısını gösterir.

    ![Modeli eğitme](media/how-to/how-to-train-model.png)

3.  Tren düğmesini tıklatın.

4.  İletişim kutusunda, modeliniz için bir ad belirtin.

5.  Tren modeli'ni tıklatın.

    ![Model iletişim kutusunu eğit](media/how-to/how-to-train-model-2.png)

6.  Özel Çevirmen eğitimi gönderir ve modeller sekmesinde eğitimin durumunu gösterir.

    ![Tren modeli sayfası](media/how-to/how-to-train-model-3.png)

>[!Note]
>Custom Translator, herhangi bir zamanda bir çalışma alanı içinde 10 eşzamanlı eğitimi destekler.


## <a name="edit-a-model"></a>Bir modeli düzenleme

Model Ayrıntıları sayfasındaki Düzenleme bağlantısını kullanarak bir modeli düzenleme yapabilirsiniz.

1.  Kalem simgesine tıklayın.

    ![Modeli düzenle](media/how-to/how-to-edit-model.png)

2.  İletişim değişikliğinde,

    1.  Model Adı (gerekli): Modelinize anlamlı bir ad verin.

        ![Daha fazla iletişim kutusunu edin](media/how-to/how-to-edit-model-dialog.png)

3.  Kaydet’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

- [Model ayrıntılarını nasıl görüntüleyin](how-to-view-model-details.md)öğrenin.
